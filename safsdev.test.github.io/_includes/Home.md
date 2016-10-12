# Software Automation Framework Support

Welcome to the SAFSDEV project!    See [What's New](http://safsdev.github.io/Default.htm), including our latest releases for:

- [SeleniumPlus Release](https://github.com/SAFSDEV/SeleniumPlus-Plugin/releases) for Advanced HTML5/SAPUI5 testing with SAFS/Selenium RemoteWebDriver!
- [SAFS Win Release](https://github.com/SAFSDEV/Core/releases) --- includes the full breadth of SAFS support on Windows!
- [Robotium Remote Control Release](https://sourceforge.net/projects/safsdev/files/Robotium%20RemoteControl/) for Android testing with Robotium 5.2.1! (no SAFS)
- [SAFS Mac Release](https://sourceforge.net/projects/safsdev/files/SAFS%20Mac/) for Apple iOS testing!

### Mission

Our focus is on implementing tools and frameworks for strategic, long-term success in functional test automation.  To that end we work on `keyword-driven` engines, also sometimes called `data-driven` engines.  What we are dealing with, actually, are tools and full implementations of testing frameworks for `action-based testing` or "the action word approach" for functional test automation.  Read more about the benefits of keyword-driven testing in the [Test Automation Frameworks Whitepaper](http://safsdev.github.io/FRAMESDataDrivenTestAutomationFrameworks.htm).

### Keyword-driven Testing

As an extension of keyword-driven testing, we also provide [JSAFS Support](http://safsdev.github.io/sqabasic2000/UsingJSAFS.htm) and [SeleniumPlus](http://safsdev.github.io/selenium/doc/SeleniumPlus-Welcome.html) for Java-savvy developers and testers that want to write `tool-independent` tests in Java that leverage the same keyword-driven framework environments.

Larger portions of our tools development are intended to benefit functional test automation in general.  This is an effort to provide functionality and tools that augment, or can be used independent of, the commercial automation solutions used by many automators.  These tools are not only suitable for the keyword-driven engines that are the focal point of our effort, but they are also useful for any number of other software test automation projects.

We have dubbed the `general-purpose` nature of our work, **Software Automation Framework Support**, or **SAFS**.

We take these general-purpose tools and apply them to specific keyword-driven engine implementations we call, `SAFS Engines`.  Our primary goal is to implement a common keyword-driven engine design across multiple tools and testing platforms for robust automation that can be immune to the course of any one particular testing tool.  This helps us realize the concept:

> "We must minimize the impact caused by changes in the applications we are testing,
> and changes in the tools we use to test them."

This strategy allows us to separate our tests from the automation tools that will execute them.  So, we can actually migrate tests from one automation tool to another, or even use multiple automation tools simultaneously.  For example, we could use *Selenium WebDriver* and *IBM Rational Functional Tester* at the same time.  Or *Rational Functional Tester* and *AQA TestComplete*!  We could have *Rational Functional Tester* execute a test on Windows but use a different tool to run the same test on another platform. 

### Supported Tools and Platforms

We are experiencing explosive development in many different areas and platforms right now which makes frequent **official** releases very difficult.


**List of tool implementations currently supported**

- [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/)
- [Android Test Automation](http://developer.android.com/sdk/index.html)
- [Apple OSX iOS UIAutomation](http://developer.apple.com/library/ios/)
- [Robotium Remote Control](http://safsdev.github.io/doc/com/jayway/android/robotium/remotecontrol/solo/Solo.html).  (See also [Robotium Remote Control Concept](https://github.com/robotiumtech/robotium))
- [IBM Rational Functional Tester](http://www.ibm.com/software/awdtools/tester/functional/support/)
- [SmartBear TestComplete](http://smartbear.com/product/testcomplete/overview/)
- [SAFS Image-Based Testing](http://safsdev.github.io/sqabasic2000/SAFSImageBasedRecognition.htm)
- [JSAFS Support](http://safsdev.github.io/sqabasic2000/UsingJSAFS.htm)
- [IBM Rational Robot](http://www.ibm.com/software/awdtools/tester/robot/support/index.html)


**List of implementations actively in development**

- [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) also supporting SeBuilder scripts.
- [SAFS Robotium Remote Control](http://safsdev.github.io/doc/com/jayway/android/robotium/remotecontrol/solo/Solo.html).  (See also [Robotium Remote Control Concept](https://github.com/robotiumtech/robotium))
- [Android Test Automation](https://developer.android.com/studio/index.html)  (The Android SDK and Robotium Remote Control as a [SAFS Android Engine](http://safsdev.github.io/doc/org/safs/tools/engines/SAFSDROID.html)!)
- [Apple OSX iOS UIAutomation](https://developer.apple.com/library/content/navigation/) (Apple XCode/iOS SDK and SAFS as a [SAFS iOS Engine](http://safsdev.github.io/doc/org/safs/tools/engines/SAFSIOS.html)!)
- [IBM Rational Functional Tester](http://www-947.ibm.com/support/entry/portal/Software/Rational/Rational_Functional_Tester/Overview)
- [SAFS Image-Based Testing](http://safsdev.github.io/sqabasic2000/SAFSImageBasedRecognition.htm)
- [JSAFS Support](http://safsdev.github.io/sqabasic2000/UsingJSAFS.htm)
- [SmartBear TestComplete](http://smartbear.com/product/testcomplete/overview/)
- [IBM Rational Robot](http://www-947.ibm.com/support/entry/portal/Software/Rational/Rational_Robot/Overview)


**List of partially developed support needing development resources**
- Mercury's Quick Test Pro (need resources)
- Perl API
- Python API
- TCL API

We want to acknowledge and present kudos to [STAF](http://staf.sourceforge.net/), another SourceForge project that has enabled us to develop many of our tool-independent pieces with minimal effort.  This is a wonderful project to investigate for ANY kind of test automation framework you might be considering or developing.

Thanks for visiting!  If you'd like to find out more, drop in on our [discussion forums](http://safsdev.freeforums.net/).

<br>

<p style="text-align:right"><i>The SAFSDEV Users and Developers</i></p>

---
