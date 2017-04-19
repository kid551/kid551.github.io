---
layout: post
comments: true
title:  "MySQL and C# Dev Issues Summary"
excerpt: "Summary of tedious issues appearing in devlopment."
date:   2017-03-09
mathjax: true
---


### Use MySQL database in Visual Studio

1. You need to first add the MySQL-VS plugin from MySQL's official site.
2. Add *Reference* `MySql.Data` in your project. 
3. In class file, add `using MySql.Data.MySqlClient;` instead of `using System.Data.SqlClient;`.


### Assign Stored Procedure in MySQL

```sql
drop procedure if exists Proc_New_Role;

delimiter $$
create procedure Proc_New_Role()
begin
    select @MaxNum := ifnull(max(right(role_id, 5) * 1), 0) + 1 as 'MaxNo' from role_master;
    select CONCAT('BK', right(concat('00000', cast(@MaxNum as CHAR)), 5)) as 'RoleID';
end $$
delimiter ;

call Proc_New_Role();
```

Pay attension, in above code:

- Use `drop procedure if exists Proc_New_Role` instead of `drop procedure Proc_New_Role if exists`, which is MySQL's feature.
- the assign method operator should be used as `:=` instead of `=`.
- to cast data type, we can use `cast(right(role_id, 5) as signed)` instead of `cast(right(role_id, 5) as int)`, which may work in MS-SQL but fail in MySQL. Also, the `right(role_id, 5) * 1` is equivalent to cast type. In MySQL, the numerical environment will auto-cast element's data type. 
- I use `IFNULL` instead of `ISNULL` here. In MySQL, the `ISNULL` only accepts one parameter.



### Deal with multiple results of select operation

```c#
Connection conn = new Connection();
MySqlDataAdapter mda = new MySqlDataAdapter("Proc_New_Role", conn.ActiveCon());
mda.SelectCommand.CommandType = CommandType.StoredProcedure;

// Handle multiple sql resutls
DataSet ds = new DataSet();
mda.Fill(ds);
DataTableCollection dc = ds.Tables;
bookIDTextbox.Text = dc[dc.Count - 1].Rows[0][0].ToString();
```



### Expand Columns Space in DataGridView

In order to expand the columns space in **DataGridView**, we need to set the `AutoSizeMode` under `layout`. In Visual Studio, it appears as the title `Edit Columns...` .

### Can not add Foreign Key, error code: 1215

First you need to keep the type of column the same. Then, if you create a table with `Character Set = utf8;`, you need to keep both table with this format specifying.

### Unknown column in 'field list' error 

In C# code, when you try to write SQL command, you're required to use `'` to wrap the value used for inserting. 

```c#
string.Format("insert into {0}(id, name, tel, address, descp) " + 
                                        "values ('{1}', '{2}', '{3}', '{4}', '{5}')", 
										CUSTOMER_TABLE, id, name, tel, addr, comment);
```

In above code, if you don't use `'` in `'{1}'`, you will get the **Unknown column in 'field list' error**.

### Add 'Copy&Paste' feature for DataGridView Cells

In order to add this feature, you need to 

- add the `KeyDown` event for the windows form. You can double click the `Key -> KeyDown` section under the form's Properties part.
- change the `KeyPreview` property of this form as `true`.

Finally, add the simple code below can make it work:

```c#
public void CopyPasteDGCells(object sender, KeyEventArgs e, DataGridView dg)
{
	if ((e.Control && e.KeyCode == Keys.V) || (e.Shift && e.KeyCode == Keys.Insert))
	{
		BLL.UIUtils.Paste2Datagrid(dg);
	}
}


// ...


// BLL.UIUtils

// Comes from: 
// https://social.msdn.microsoft.com/Forums/windows/en-US/e9cee429-5f36-4073-85b4-d16c1708ee1e/how-to-paste-ctrlv-shiftins-the-data-from-clipboard-to-datagridview-datagridview1-c?forum=winforms
public static void Paste2Datagrid(DataGridView pastedDG)
{
	char[] rowSplitter = { '\r', '\n' };
	char[] columnSplitter = { '\t' };

	// Get the text from clipboard
	string stringInClipboard = (string)Clipboard.GetDataObject().GetData(DataFormats.Text);

	// Split it into lines
	string[] rowsInClipboard = stringInClipboard.Split(rowSplitter, StringSplitOptions.RemoveEmptyEntries);

	// Get the row and column of selected cell in grid
	int rowIndx = pastedDG.SelectedCells[0].RowIndex;
	int colIndx = pastedDG.SelectedCells[0].ColumnIndex;

	// Add rows into grid to fit clipboard lines
	if (pastedDG.Rows.Count < (rowIndx + rowsInClipboard.Length))
	{
		pastedDG.Rows.Add(rowIndx + rowsInClipboard.Length - pastedDG.Rows.Count);
	}

	// Loop through the lines, split them into cells and place the values in the corresponding cell.
	for (int iRow = 0; iRow < rowsInClipboard.Length; iRow++)
	{
		// Split row into cell values
		string[] valuesInRow = rowsInClipboard[iRow].Split(columnSplitter);

		// Cycle through cell values
		for (int iCol = 0; iCol < valuesInRow.Length; iCol++)
		{
			// Assign cell value, only if it within columns of the grid
			if (pastedDG.ColumnCount - 1 >= colIndx + iCol)
			{
				pastedDG.Rows[rowIndx + iRow].Cells[colIndx + iCol].Value = valuesInRow[iCol];
			}
		}
	}
}
```


