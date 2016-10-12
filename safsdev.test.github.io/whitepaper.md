---
layout: page
title: Whitepaper
---

# Test Automation Framework Whitepaper

> "When developing our test strategy, we must minimize the impact caused by changes in the applications we are testing, and changes in the tools we use to test them."
> --Carl J. Nagle

<a name="section_1.1" />
### 1.1 Thinking Past "The Project"

In today’s environment of plummeting cycle times, test automation becomes an increasingly critical and strategic necessity. Assuming the level of testing in the past was sufficient (which is rarely the case), how do we possibly keep up with this new explosive pace of web-enabled deployment while retaining satisfactory test coverage and reducing risk? The answer is either more people for manual testing, or a greater level of test automation. After all, a reduction in project cycle times generally correlates to a reduction of time for test.

With the onset and demand for rapidly developed and deployed web clients test automation is even more crucial. Add to this the cold, hard reality that we are often facing more than one active project at a time. For example, perhaps the team is finishing up Version 1.0, adding the needed new features to Version 1.1, and prototyping some new technologies for Version 2.0!

Better still; maybe our test team is actually a pool of testers supporting many diverse applications completely unrelated to each other. If each project implements a unique test strategy, then testers moving among different projects can potentially be more a hindrance rather than a help. The time needed for the tester to become productive in the new environment just may not be there. And, it may surely detract from the productivity of those bringing the new tester up to speed.

To handle this chaos we have to think past the project. We cannot afford to engineer or reengineer automation frameworks for each and every new application that comes along. We must strive to develop a single framework that will grow and continuously improve with each application and every diverse project that challenges us. We will see the advantages and disadvantages of these different approaches in [Section 1.2](#section_1.2).


<a name="section_1.1.1" />
### 1.1.1 Problems with Test Automation

Historically, test automation has not met with the level of success that it could. Time and again test automation efforts are born, stumble, and die. Most often this is the result of misconceived perceptions of the effort and resources necessary to implement a successful, long-lasting automation framework. Why is this, we might ask? Well, there are several reasons.

Foremost among this list is that automation tool vendors do not provide completely forthright demonstrations when showcasing the "simplicity" of their tools. We have seen the vendor’s sample applications. We have seen the tools play nice with those applications. And we try to get the tools to play nice with our applications just as fluently. Inherently, project after project, we do not achieve the same level of success.

This usually boils down to the fact that our applications most often contain elements that are not compatible with the tools we use to test them. Consequently, we must often mastermind technically creative solutions to make these automation tools work with our applications. Yet, this is rarely ever mentioned in the literature or the sales pitch.

The commercial automation tools have been chiefly marketed for use as solutions for testing an application. They should instead be sought as the cornerstone for an enterprise-wide test automation framework. And, while virtually all of the automation tools contain some scripting language allowing us to get past each tool’s failings, testers have typically neither held the development experience nor received the training necessary to exploit these programming environments.

> "For the most part, testers have been testers, not programmers. Consequently, the ‘simple’ commercial solutions have been far too complex to implement and maintain; and they become shelfware."

Most unfortunate of all, otherwise fully capable testers are seldom given the time required to gain the appropriate software development skills. For the most part, testers have been testers, not programmers. Consequently, the "simple" commercial solutions have been far too complex to implement and maintain; and they become shelfware.

Test automation must be approached as a full-blown software development effort in its own right. Without this, it is most likely destined to failure in the long term.


> ### Case Study: Costly Automation Failures
> 
> In 1996, one large corporation set out evaluating the various commercial automation tools that were available at that time. They brought in eager technical sales staff from the various vendors, watched demonstrations, and performed some fairly thorough internal evaluations of each tool.
> 
> By 1998, they had chosen one particular vendor and placed an initial order for over $250,000 worth of product licensing, maintenance contracts, and onsite training. The tools and training were distributed throughout the company into various test departments--each working on their own projects.
> 
> None of these test projects had anything in common. The applications were vastly different. The projects each had individual schedules and deadlines to meet. Yet, every one of these departments began separately coding functionally identical common libraries. They made routines for setting up the Windows test environment. They each made routines for accessing the Windows programming interface. They made file-handling routines, string utilities, database access routines--the list of code duplication was disheartening!
> 
> For their test designs, they each captured application specific interactive tests using the capture\replay tools. Some groups went the next step and modularized key reusable sections, creating reusable libraries of application-specific test functions or scenarios. This was to reduce the amount of code duplication and maintenance that so profusely occurs in pure captured test scripts. For some of the projects, this might have been appropriate if done with sufficient planning and an appropriate automation framework. But this was seldom the case.
> 
> With all these modularized libraries testers could create functional automated tests in the automation tool’s proprietary scripting language via a combination of interactive test capture, manual editing, and manual scripting.
> 
> One problem was, as separate test teams they did not think past their own individual projects. And although they were each setting up something of a reusable framework, each was completely unique--even where the common library functions were the same! This meant duplicate development, duplicate debugging, and duplicate maintenance. Understandably, each separate project still had looming deadlines, and each was forced to limit their automation efforts in order to get real testing done.
> 
> As changes to the various applications began breaking automated tests, script maintenance and debugging became a significant challenge. Additionally, upgrades in the automation tools themselves caused significant and unexpected script failures. In some cases, the necessity to revert back (downgrade) to older versions of the automation tools was indicated. Resource allocation for continued test development and test code maintenance became a difficult issue.
> 
> Eventually, most of these automation projects were put on hold. By the end of 1999--less than two years from the inception of this large-scale automation effort--over 75% of the test automation tools were back on the shelves waiting for a new chance to try again at some later date.

<a name="section_1.1.2" />
### 1.1.2 Some Test Strategy Guidelines

Past failings like these have been lessons for the entire testing community. Realizing that we must develop reusable test strategies is no different than the reusability concerns of any good application development project. As we set out on our task of automating test, we must keep these past lessons forefront.

In order to make the most of our test strategy, we need to make it reusable and manageable. To that end, there are some essential guiding principles we should follow when developing our overall test strategy:

- Test automation is a fulltime effort, not a sideline.
- The test design and the test framework are totally separate entities.
- The test framework should be application-independent.
- The test framework must be easy to expand, maintain, and perpetuate.
- The test strategy/design vocabulary should be framework independent.
- The test strategy/design should remove most testers from the complexities of the test framework.

These ideals are not earth shattering. They are not relatively new. Yet, it is seldom these principles are fully understood and instrumented.

So what do they mean?

<a name="section_1.1.3" />
### 1.1.3	Test automation is a fulltime effort, not a sideline

While not necessarily typical design criteria, it bears repeating. The test framework design and the coding of that design together require significant front-loaded time and effort. These are not things that someone can do when they have a little extra time here, or there, or between projects. The test framework must be well thought out. It must be documented. It should be reviewed. It should be tested. It is a full software development project like any other. This bears repeating--again.

Will our test framework development have all of these wonderful documentation, design, review, and test processes? Does our application development team?

We should continuously push for both endeavors to implement all these critical practices.

<a name="section_1.1.4" />
### 1.1.4	The test design and the test framework are totally separate entities

The test design details how the particular functions and features of our application will be tested. It will tell us what to do, how and when to do it, what data to use as input, and what results we expect to find. All of this is specific to the particular application or item being tested. Little of this requires any knowledge or care of whether the application will be tested automatically or manually. It is, essentially, the "how to" of what needs to be tested in the application.

On the other hand, the test framework, or specifically, the test automation framework is an execution environment for automated tests. It is the overall system in which our tests will be automated. The development of this framework requires completely different technical skills than those needed for test design.

<a name="section_1.1.5" />
### 1.1.5	The test framework should be application-independent

Although applications are relatively unique, the components that comprise them, in general, are not. Thus, we should focus our automation framework to deal with the common components that make up our unique applications. By doing this, we can remove all application-specific context from our framework and reuse virtually everything we develop for every application that comes through the automated test process.

> "We should focus our automation framework to deal with the common components that make up our unique applications."

Nearly all applications come with some form of menu system. They also have buttons to push, boxes to check, lists to view, and so on. In a typical automation tool script there is, generally, a very small number of component functions for each type of component. These functions work with the component objects independent of the applications that contain them.

Traditional, captured automation scripts are filled with thousands of calls to these component functions. So the tools already exist to achieve application independence. The problem is, most of these scripts construct the function calls using application-specific, hard coded values. This immediately reduces their effectiveness as application-independent constructs. Furthermore, the functions by themselves are prone to failure unless a very specific application state or synchronization exists at the time they are executed. There is little error correction or prevention built-in to these functions.

To deal with this in traditional scripts we must place additional code before and\or after the command, or a set of commands, to insure the proper application state and synchronization is maintained. We need to make sure our window has the current focus. We need to make sure the component we want to select, or press, or edit exists and is in the proper state. Only then can we perform the desired operation and separately verify the result of our actions.

For maximum robustness, we would have to code these state and synchronization tests for every component function call in our scripts. Realistically, we could never afford to do this. It would make the scripts huge, nearly unreadable, and difficult to maintain. Yet, where we forego this extra effort, we increase the possibility of script failure.

What we must do is develop a truly application-independent framework for these component functions. This will allow us to implement that extra effort just once, and execute it for every call to any component function. This framework should handle all the details of insuring we have the correct window, verifying the element of interest is in the proper state, doing something with that element, and logging the success or failure of the entire activity.

We do this by using variables, and providing application-specific data to our application-independent framework. In essence, we will provide our completed test designs as executable input into our automation framework.

Does this mean that we will never have to develop application-specific test scripts? Of course not. However, if we can limit our application-specific test scripts to some small percentage, while reusing the best features of our automation framework, we will reap the rewards project after project.

<a name="section_1.1.6" />
### 1.1.6 The test framework must be easy to expand, maintain, and perpetuate

One of our goals should be a highly modular and maintainable framework. Generally, each module should be independent and separate from all the other modules. What happens inside one is of no concern to the others.

With this modular black-box approach, the functionality available within each module can be readily expanded without affecting any other part of the system. This makes code maintenance much simpler. Additionally, the complexity of any one module will likely be quite minimal.

However, modularity alone will not be enough to ensure a highly maintainable framework. Like any good software project, our design must be fully documented and published. Without adequate, published documentation it will be very difficult for anyone to decipher what it is the framework is designed to do. Any hope of maintenance will not last far beyond the departure of the original framework designers. Our test automation efforts will eventually become another negative statistic.

To prevent this, we should define documentation standards and templates. Wherever possible, module documentation should be developed "in-context". That is, directly in the source code itself. Tools should be retained, or designed and developed, so that we can automatically extract and publish the documentation. This will eliminate the task of maintaining two separate sets of files: the source code, and its documentation. It will also provide those doing the code maintenance quite a ready reference. Nearly everything they need to know should exist right there in the code.

> "We must always remember: our ultimate goal is to simplify and perpetuate a successful automation framework."

We must always remember: our ultimate goal is to simplify and perpetuate a successful test automation framework. To put something in place that people will use and reuse for as long as it is technically viable and productive.

<a name="section_1.1.7" />
### 1.1.7	The test strategy/design vocabulary should be framework independent

As noted before, the framework refers to the overall environment we construct to execute our tests. The centerpiece is usually one of many commercially available automation tools. In good time, it may be more than one. In some rare circumstances, it might even be a proprietary tool developed or contracted specifically for our test automation needs.

The point is, different tools exist and some will work better for us than others in certain situations. While one tool might have worked best with our Visual Basic or C/C++ applications, we may need to use a different tool for our web clients. By keeping a specific tool consideration out of our test designs, we avoid limiting our tests to that tool alone.

The overall test strategy will define the format and *low-level* vocabulary we use to test all applications much like an automation tool defines the format and syntax of the scripting language it provides. Our vocabulary, however, will be independent of any particular test framework employed. The same vocabulary will migrate with us from framework to framework, and application to application. This means, for example, the syntax used to click a button will be the same regardless of the tool we use to execute the instruction or the application that contains the button.

The test design for a particular application, however, will define a high-level vocabulary that is specific to that application. While this high-level vocabulary will be application specific, it is still independent of the test framework used to execute it. This means that the high-level instruction to login to our website with a particular user ID and password will be the same regardless of the tool we use to execute it.

When we provide all the instructions necessary to test a particular application, we should be able to use the exact same instructions on any number of different framework implementations capable of testing that application. We must also consider the very likely scenario that some or all of this testing may, at one time or another, be manual testing. This means that our overall test strategy should not only facilitate test automation, it should also support manual testing.

Consequently, the format and vocabulary we use to test our applications should be intuitive enough for mere mortals to comprehend and execute. We should be able to hand our test over to a person, point to an area that failed, and that person should be able to manually reproduce the steps necessary to duplicate the failure.

> "A good test strategy can remove the necessity for both manual and automated test scripts. The same ‘script’ should suffice for both."

A good test strategy, comprised of our test designs and our test framework, can remove the necessity for both manual and automated test scripts for the same test. The same "script" should suffice for both. The important thing is that the vocabulary is independent of the framework used to execute it. And the test strategy must also accommodate manual testing.


<a name="section_1.1.8" />
### 1.1.8	The test strategy/design should remove most testers from the complexities of the test framework

In practice, we cannot expect all our test personnel to become proficient in the use of the automation tools we use in our test framework. In some cases, this is not even an option worth considering. Remember, generally, testers are testers--they are not programmers. Sometimes our testers are not even professional testers. Sometimes they are application domain experts with little or no use for the technical skills needed for software development.

Sometimes testers are application developers splitting time between development and test. And when application developers step in to perform testing roles, they do not want or need a complex test scripting language to learn. That is what you get with commercial automation tools. And that may even be counter-productive and promote confusion since some of these scripting languages are modified subsets of standard programming languages. Others are completely unique and proprietary.

Yet, with the appropriate test strategy and vocabulary as discussed in the previous section, there is no reason we should not be able to use all our test resources to design tests suitable for automation without knowing anything about the automation tools we plan to deploy.

The bulk of our testers can concentrate on test design, and test design only. It is the automation framework folks who will focus on the tools and utilities to automate those tests.

<a name="section_1.2" />
### 1.2 Data Driven Automation Frameworks

Over the past several years there have been numerous articles done on various approaches to test automation. Anyone who has read a fair, unbiased sampling of these knows that we cannot and *must not* expect pure capture and replay of test scripts to be successful for the life of a product. We will find nothing but frustration there.

Sometimes this manifesto is hard to explain to people who have not yet performed significant test automation with these capture\replay tools. But it usually takes less than a week, often less than a day, to hear the most repeated phrase: "It worked when I recorded it, but now it fails when I play it back!"

Obviously, we are not going to get there from here.


<a name="section_1.2.1" />
### 1.2.1 Data Driven Scripts

Data driven scripts are those application-specific scripts captured or manually coded in the automation tool’s proprietary language and then modified to accommodate variable data. Variables will be used for key application input fields and program selections allowing the script to drive the application with external data supplied by the calling routine or the shell that invoked the test script.

#### Variable Data, Hard Coded Component Identification:

These data driven scripts often still contain the hard coded and sometimes very fragile recognition strings for the window components they navigate. When this is the case, the scripts are easily broken when an application change or revision occurs. And when these scripts start breaking, we are not necessarily talking about just a few. We are sometimes talking about a great many, if not all the scripts, for the entire application.

Figure 1 is an example of activating a server-side image map link in a web application with an automation tool scripting language:

`Image Click "DocumentTitle=Welcome;\;ImageIndex=1" "Coords=25,20"`


<center> <div font-size="2px">Figure 1</div></center><br>

This particular scenario of clicking on the image map might exist thousands of times throughout all the scripts that test this application. The preceding example identifies the image by the title given to the document and the index of the image on the page. The hard coded image identification might work successfully all the way through the production release of that version of the application. Consequently, testers responsible for the automated test scripts may gain a false sense of security and satisfaction with these results.

However, the next release cycle may find some or all of these scripts broken because either the title of the document or the index of the image has changed. Sometimes, with the right tools, this might not be too hard to fix. Sometimes, no matter what tools, it will be frustratingly difficult.

Remember, we are potentially talking about thousands of broken lines of test script code. And this is just one particular change. Where there is one, there will likely be others.

#### Highly Technical or Duplicate Test Designs:

Another common feature of data driven scripts is that virtually all of the test design effort for the application is developed in the scripting language of the automation tool. Either that, or it is duplicated in both manual and automated script versions. This means that everyone involved with automated test development or automated test execution for the application must likely become proficient in the environment and programming language of the automation tool.

#### Findings:

A test automation framework relying on data driven scripts is definitely the easiest and quickest to implement if you have and keep the technical staff to maintain it. But it is the hardest of the data driven approaches to maintain and perpetuate and very often leads to long-term failure.


<a name="section_1.2.2" />
### 1.2.2 Keyword or Table Driven Test Automation

Nearly everything discussed so far defining our ideal automation framework has been describing the best features of "keyword driven" test automation. Sometimes this is also called "table driven" test automation. It is typically an application-independent automation framework designed to process our tests. These tests are developed as data tables using a keyword vocabulary that is independent of the test automation tool used to execute them. This keyword vocabulary should also be suitable for manual testing, as you will soon see.

#### Action, Input Data, and Expected Result ALL in One Record:

The data table records contain the keywords that describe the actions we want to perform. They also provide any additional data needed as input to the application, and where appropriate, the benchmark information we use to verify the state of our components and the application in general.

For example, to verify the value of a user ID textbox on a login page, we might have a data table record as seen in Table 1:

| Window | Component | Action | Expected Value |
| --- | --- | --- | --- |
| LoginPage | UserIDTextbox | VerifyValue | "MyUserID" |

<center> <div font-size="2px">Table 1</div></center><br>

#### Reusable Code, Error Correction and Synchronization:

Application-independent component functions are developed that accept application-specific variable data. Once these component functions exist, they can be used on each and every application we choose to test with the framework.

Figure 2 presents pseudo-code that would interpret the data table record from Table 1 and Table 2. In our design, the primary loop reads a record from the data table, performs some high-level validation on it, sets focus on the proper object for the instruction, and then routes the complete record to the appropriate component function for full processing. The component function is responsible for determining what action is being requested, and to further route the record based on the action.

**Framework Pseudo Code**

```
Primary Record Processor Module:

   Verify "LoginPage" Exists. (Attempt recovery if not)
   Set focus to "LoginPage".
   Verify "UserIDTextbox" Exists. (Attempt recovery if not)
   Find "Type" of component "UserIDTextbox". (It is a Textbox)
   Call the module that processes ALL Textbox components.

Textbox Component Module:

   Validate the action keyword "VerifyValue".
   Call the Textbox.VerifyValue function.

Textbox.VerifyValue Function:

   Get the text stored in the "UserIDTextbox" Textbox.
   Compare the retrieved text to "MyUserID".
   Record our success or failure.
```

#### Test Design for Man and Machine, With or Without the Application:

Table 2 reiterates the actual data table record run by the automation framework above:

| Window | Component | Action | Expected Value |
| --- | --- | --- | --- |
| LoginPage | UserIDTextbox | VerifyValue | "MyUserID" |

<center> <div font-size="2px">Table 2</div></center><br>

Note how the record uses a vocabulary that can be processed by both man and machine. With minimal training, a human tester can be made to understand the record instruction as deciphered in Figure 3:

``` 
On the LoginPage, in the UserIDTextbox,
Verify the Value is "MyUserID".
```

<center> <div font-size="2px">Figure 3</div></center><br>

Once they learn or can reference this simple vocabulary, testers can start designing tests without knowing anything about the automation tool used to execute them.

Another advantage of the keyword driven approach is that testers can develop tests without a functioning application as long as preliminary requirements or designs can be determined. All the tester needs is a fairly reliable definition of what the interface and functional flow is expected to be like. From this they can write most, if not all, of the data table test records.

Sometimes it is hard to convince people that this advantage is realizable. Yet, take our login example from Table 2 and Figure 3. We do not need the application to construct any login tests. All we have to know is that we will have a login form of some kind that will accept a user ID, a password, and contain a button or two to submit or cancel the request. A quick discussion with development can confirm or modify our determinations. We can then complete the test table and move on to another.

We can develop other tests similarly for any part of the product we can receive or deduce reliable information. In fact, if in such a position, testers can actually help guide the development of the UI and flow, providing developers with upfront input on how users might expect the product to function. And since the test vocabulary we use is suitable for both manual and automated execution, designed testing can commence immediately once the application becomes available.

It is, perhaps, important to note that this does not suggest that these tests can be executed *automatically* as soon as the application becomes available. The test record in Table 2 may be perfectly understood and executable by a person, but the automation framework knows nothing about the objects in this record until we can provide that additional information. That is a separate piece of the framework we will learn about when we discuss application mapping.

#### Findings:

The keyword driven automation framework is initially the hardest and most time-consuming data driven approach to implement. After all, we are trying to fully insulate our tests from both the many failings of the automation tools, as well as changes to the application itself.

To accomplish this, we are essentially writing enhancements to many of the component functions already provided by the automation tool: such as error correction, prevention, and enhanced synchronization.

Fortunately, this heavy, initial investment is mostly a one-shot deal. Once in place, keyword driven automation is arguably the easiest of the data driven frameworks to maintain and perpetuate providing the greatest potential for long-term success.

Additionally, there may now be commercial products suitable for your needs to decrease, but not eliminate, much of the up-front technical burden of implementing such a framework. This was not the case just a few years ago. We will briefly discuss a couple of these in [Section 1.2.4](#section_1.2.4).

<a name="section_1.2.3" />
### 1.2.3 Hybrid Test Automation (or, "All of the Above")

The most successful automation frameworks generally accommodate both keyword driven testing as well as data driven scripts. This allows data driven scripts to take advantage of the powerful libraries and utilities that usually accompany a keyword driven architecture.

The framework utilities can make the data driven scripts more compact and less prone to failure than they otherwise would have been. The utilities can also facilitate the gradual and manageable conversion of existing scripts to keyword driven equivalents when and where that appears desirable.

On the other hand, the framework can use scripts to perform some tasks that might be too difficult to re-implement in a pure keyword driven approach, or where the keyword driven capabilities are not yet in place.

<a name="section_1.2.4" />
### 1.2.4 Commercial Keyword Driven Frameworks

Some commercially available keyword driven frameworks are making inroads in the test automation markets. These generally come from 3rd party companies as a bridge between your application and the automation tools you intend to deploy. They are not out-of-the-box, turnkey automation solutions just as the capture\replay tools are not turnkey solutions.

They still require some up-front investment of time and personnel to complete the bridge between the application and the automation tools, but they can give some automation departments and professionals a huge jumpstart in the right direction for successful long-term test automation.

Two particular products to note are the TestFrameä product led by Hans Buwalda of CMG Corp, and the Certifyä product developed with Linda Hayes of WorkSoft Inc. These products each implement their own version of a keyword driven framework and have served as models for the subject at international software testing conferences, training courses, and user-group discussions worldwide. I’m sure there are others.

It really is up to the individual enterprise to evaluate if any of the commercial solutions are suitable for their needs. This will be based not only on the capabilities of the tools evaluated, but also on how readily they can be modified and expanded to accommodate your current and projected capability requirements.

