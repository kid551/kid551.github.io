---
layout: post
comments: true
title:  "Create Temporary File in Java"
excerpt: "It's not as easy as it seems."
date:   2019-11-27
mathjax: true
---

During these days, I accepted one common task: abstract a list of filtered data, and send these data by email as Excel attachment file. Sounds pretty easy and classic. What I need to do is just create one temporary Excel file and send it as email attachment.

I use [POI](http://poi.apache.org/index.html) to generate Excel file, which requires a `File` object to finish the generating. But concurrent problems appear here at once:
- Where should we store the temporary Excel file?
- How to avoid the situation that one thread is sending the Excel file while another thread is trying to delete it?
- How to avoid one thread is sending while another is updating it?

One potential solution is use Java lock `synchronized` to control the concurrency conflict. If for any specified filtered condition, we only need to generate one copy of Excel file. We can use `synchronized` to guarantee only one copy Excel file being generated, and the following request can send the same file.

One hard part of this solution is the Excel file you generated is out of the control of Java lock `synchronized`. This can be solved by introducing one internal static variable as the locked object to control the atomic operations of creating new Excel file, where use double-check method as in _Singleton Pattern_.

```java
import org.apache.commons.collections.CollectionUtils;
import org.apache.commons.lang.StringUtils;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;

public class POIUtil {

    private static final Object LOCK_FLAG = new Object();

    private POIUtil() {
        throw new AssertionError();
    }

    public static File generateExcelIfNotExist(String fileNameWithoutExtension, String dirStr, List<List<String>> content) {

        if (StringUtils.isBlank(fileNameWithoutExtension)) {
            throw new IllegalArgumentException("File name should not be blank");
        }
        if (CollectionUtils.isEmpty(content)) {
            throw new IllegalArgumentException("Content should not be empty");
        }

        String excelFileName = fileNameWithoutExtension + ".xls";
        File excelFile = new File(dirStr + excelFileName);
        if (excelFile.exists()) {
            return excelFile;
        }


        // 1. create Excel workbook and sheet
        HSSFWorkbook workbook = new HSSFWorkbook();
        HSSFSheet sheet = workbook.createSheet();


        // 2. fill the content data
        int rowIndex = 0;
        for (List<String> row : content) {
            if (CollectionUtils.isEmpty(row)) {
                continue;
            }

            HSSFRow eRow = sheet.createRow(rowIndex++);

            int colIndex = 0;
            for (String col : row) {
                eRow.createCell(colIndex++).setCellValue(PmStringUtil.removeBlankStr(col));
            }
        }


        // 3. Export the Excel file
        if (!excelFile.exists()) {
            synchronized (LOCK_FLAG) {
                if (!excelFile.exists()) {
                    try {

                        if (!excelFile.getParentFile().exists()) {
                            excelFile.getParentFile().mkdirs();
                        }
                        if (!excelFile.getParentFile().exists()) {
                            throw new IOException("Failed to create excel file dir: " + dirStr);
                        }

                        boolean res = excelFile.createNewFile();
                        if (res) {
                            workbook.write(new FileOutputStream(excelFile));
                        } else {
                            excelFile.deleteOnExit();
                            throw new IOException("Failed to create excel file: " + excelFileName);
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                        excelFile.deleteOnExit();
                        throw new UnknownError();
                    }
                }
            }
        }

        return excelFile;
    }

}
```

This solution may provide a way to manage access control. But once the Excel file is generated, it'll always return the same error file. There's no chance to fix incorrect generating. And once we want to update the data in Excel file, there's no chance to make it done.

So maybe we need to consider another solution. If each thread at each time can generate a different Excel file, i.e., one Excel file per thread. And after sending this file, we delete this temporary file. This may sound like a valid solution. One intuitive solution is using random number for each thread when creating an Excel file. But the problem is: *how to guarantee each random number is different*?

When I consider this problem, one method in `File` class comes to my eys sight: `File#createTempFile(String prefix, String suffix, File directory)`. Its Java doc is almost stating what we need:

> Creates a new empty file in the specified directory, using the given prefix and suffix strings to generate its name. If this method returns successfully then it is guaranteed that:
> 1. The file denoted by the returned abstract pathname did not exist before this method was invoked, and
> 2. Neither this method nor any of its variants will return the same abstract pathname again in the current invocation of the virtual machine.
> 
> This method provides only part of a temporary-file facility. To arrange for a file created by this method to be deleted automatically, use the deleteOnExit method.

So, the solution is obvious: just use `File#createTempFile()` method to generate the Excel file per thread. But how does this method to solve the concurrency conflict problem we stated above? Let's dig into its source code.

```java
public static File createTempFile(String prefix, String suffix,
                                      File directory)
    throws IOException
{
    if (prefix.length() < 3)
        throw new IllegalArgumentException("Prefix string too short");
    if (suffix == null)
        suffix = ".tmp";

    File tmpdir = (directory != null) ? directory
                                        : TempDirectory.location();
    SecurityManager sm = System.getSecurityManager();
    File f;
    do {
        f = TempDirectory.generateFile(prefix, suffix, tmpdir);

        if (sm != null) {
            try {
                sm.checkWrite(f.getPath());
            } catch (SecurityException se) {
                // don't reveal temporary directory location
                if (directory == null)
                    throw new SecurityException("Unable to create temporary file");
                throw se;
            }
        }
    } while ((fs.getBooleanAttributes(f) & FileSystem.BA_EXISTS) != 0);

    if (!fs.createFileExclusively(f.getPath()))
        throw new IOException("Unable to create temporary file");

    return f;
}
```

Pretty simple and elegant code: get the directory of generated file, and generate it by `TempDirectory.generateFile()` method. And let's check what this method's duty:

```java
// file name generation
private static final SecureRandom random = new SecureRandom();
static File generateFile(String prefix, String suffix, File dir)
    throws IOException
{
    long n = random.nextLong();
    if (n == Long.MIN_VALUE) {
        n = 0;      // corner case
    } else {
        n = Math.abs(n);
    }

    // Use only the file name from the supplied prefix
    prefix = (new File(prefix)).getName();

    String name = prefix + Long.toString(n) + suffix;
    File f = new File(dir, name);
    if (!name.equals(f.getName()) || f.isInvalid()) {
        if (System.getSecurityManager() != null)
            throw new IOException("Unable to create temporary file");
        else
            throw new IOException("Unable to create temporary file, " + f);
    }
    return f;
}
```

The core part is it generates one specified temporary file name by `String name = prefix + Long.toString(n) + suffix;`, where the `n` is the random number generated by `SecureRandom`. So, the whole secret is it uses the `SecureRandom` to generate one sole specified number.

But why does the `SecureRandom` class have such magic power? Let's read its docs.

> This class provides a cryptographically strong random number generator (RNG).
> 
> A cryptographically strong random number minimally complies with the statistical random number generator tests specified in [FIPS 140-2, Security Requirements for Cryptographic Modules](https://csrc.nist.gov/publications/detail/fips/140/2/final), section 4.9.1. Additionally, SecureRandom must produce non-deterministic output. Therefore any seed material passed to a SecureRandom object must be unpredictable, and all SecureRandom output sequences must be cryptographically strong, as described in [RFC 1750: Randomness Recommendations for Security](http://www.ietf.org/rfc/rfc1750.txt).

It implements strong random number generator (RNG) algorithm to provide that magic feature.
