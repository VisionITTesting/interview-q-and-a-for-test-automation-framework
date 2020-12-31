# Interview Questions and Answers for Test Automation Framework

---
#### Description:
* This repo contains the answers to some very basic Automation Test Framework Questions.
* A person going to an Automation Testing Interview should have very detailed answer to all of these questions. 
* I will be referring 'cucumber-selenium-fw' to answer some of the questions below. Link of this FW: https://github.com/VisionITTesting/cucumber-selenium-fw

---
#### Author:
* Created By: <b>Akash Tyagi.</b> 
* Date: Jan-2020
* Reach me out at: akashdktyagi@gmail.com

---

### Execution Related:

* How do you configure your pack?
    * We have different types of configuration settings available for our framework:
        * URLs:
            * We have different environment and for each env we have different urls.
            * We pass this info from the maven command line argument. For example: ```mvn clean test -Denv=dev.ecommerce.com```
            * ```env``` variable value is then caught in the code and taken for execution.
            * This way we can run the same pack on different envs using different urls.
        * Browser:
            * We can execute the same test pack on multiple browsers by passing the argument from maven command line statement.
            * To execute in chrome, we write: ```mvn clean test -Dbrowser=chrome```
            * To execute in firefox, we write: ```mvn clean test -Dbrowser=firefox```
            * ```browser``` acts as a system variable and directly is passed to the code and can be accessed using ```String browser = System.getProperty("browser")``` statement.
        * Tags: 
            * We can pass cucumber tags from maven command line to execute only the specific test scenarios.
            * We have below tags in our framework, which we can pass from maven command line:
                * Generic tags: ```@smoke```, ```@sanity```, ```@ui```, ```@api```, ```@regression```
                * Business Specific Tags: ```@search```, ```@add_to_cart```, ```@login_signup```, ```@checkout``` etc
            * We can pass these tags to control the execution flow and to control which test cases to execute.
            * ```mvn clean test -Dcucumber.options=--tags "@search"```. This will execute the search test cases only.
        * Profile:
            * We can also make use of maven profiles to execute and configure the pack based on existing values.
            * To execute any specific profiles, we can make use of statment: ```mvn clean -Prun-api```.
            * This will run feature files marked with cucumber tag as ```@api```
            * (For more details on this approach check this repo: https://github.com/akashdktyagi/VisionIT_Batch8/blob/master/Batch8CucumberSeleniumAPI/pom.xml
            * ```xml
              	<profiles>

                      <profile>
                        <id>run-api</id>
                        <properties>
                            <cucumber.options>--tags "@api"</cucumber.options>
                        </properties>
                      </profile>
                
                      <profile>
                        <id>run-ui</id>
                        <properties>
                            <cucumber.options>--tags "@ui"</cucumber.options>
                        </properties>
                      </profile>

                </profiles>
                ```
* How do you execute your pack?
    * We used maven command line to execute our pack.
    * We also pass command line arguments to customize and configure the pack.
    * Full statement to execute looks like below. This runs the pack on chrome browser, on dev env and only the search test cases will be executed.
        * ```mvn clean test -Dbrowser=chrome -Denv=dev.server_name.com -Dcucumber.options=--tags @search``` 

* How frequently do you run your pack?
    * We wait for deployments on the dev and qa server and as soon as we have the deployment on these servers, we "manually" trigger our pack on Virtual machines.
    * We do not have a working CI/CD pipeline yet. So deployment team notifies us via email and then we go ahead and trigger the pack using maven command line tool on our VMs.
    * Our team has hired few dev ops engineer who is working towards developing the CI pipeline, once that is up , we will not need to trigger it manually. With each dev change and deployment, our test automation framework will get triggered.
    * We run our pack on the daily basis, mostly end of the day, even if there is no deployment. So we continuously execute the test pack on both the environments.

* How many environment you are running against?
    * We have two env. One is called "dev" and other one is "qa"
    * Dev env is mostly used by developers to consolidate their work and see its working. 
    * Once the code looks fine, deployment team moves the code from Dev env to QA env.
    * Manual testers then comes in to picture and tests the code and new features in the QA env.
    * But we execute our automation test pack on both the env. As soon as deployment happens in qa or dev env, we trigger the test automation pack.
* How many test cases do you have, in regression and smoke?
    * Regression: We have a total of 1500 + test cases automated out 2000 total test cases.
    * These 1500 test cases we execute after each code deployment on Dev or QA env.
    * Our smoke test pack is nothing but a small subset of regression test pack. 
        * We have tagged them as "@smoke". 
        * It contains around 20 test cases. 
        * We execute them after the code deployment to test the overall health of the environment.
        * Once the smoke tests are passed we trigger the regression pack.
    * So in a nutshell this is what we do:
        * Once deployment happens on dev we run: ```mvn clean test -Dbrowser=chrome -Denv=dev.server.com -Dcucumber.options="--tags @smoke"``` on VMs. 
            * If result of this is negative, then we notify the deployment team, devs that the deployment has failed and raise defects if required in jira.
        * If smoke tests are passed then we run the regression pack using command: ```mvn clean test -Dbrowser=chrome -Denv=dev.server.com -Dcucumber.options="--tags @regression"``` on VMs.
        * Based on the regression execution above, we notify the team with the results and reports. We raise defects if something fails.
        * Post this execution and reporting, we judge whether this code base is to be moved to QA env, if every thing is ok and if there are no major issues to be fixed, deployment team mmoves the code to the QA env.
        * Once the code is deployed to the QA env, we repeat the same activity. This happens daily.
        * This is what we execute in QA env:
            * ```mvn clean test -Dbrowser=chrome -Denv=qa.server.com -Dcucumber.options="--tags @smoke"``` Smoke test(20 test cases only) on VM
            * ```mvn clean test -Dbrowser=chrome -Denv=qa.server.com -Dcucumber.options="--tags @regression"``` Full Regression tests (All 1500 test cases) on VM
          
* How much is the Execution time for regression?
    * It takes around 3-4 hours for full regression test cases execution.
* How much is the Execution time for Smoke?
    * It takes around 4-5 mins for smoke test cases execution.
* Parallel or Sequential execution?
    * We are using maven sure fire plugin to execute our test pack in parallel.
    * Note: Check the URL to know more on parallel execution in cucumber framework: https://github.com/VisionITTesting/code-tutorial-execute-cucumber-test-in-parallel
* How frequently your execution catch functionality defects?
    * Since our application is still under development and we have lot of changes happening, we find some issues in almost every every day.
* How frequently your execution catch env stability defects?
    * Our deployment team is actually quite good, so our smoke tests which we use to check the stability of the application hardly fails.
    * Our Smoke test is also very robost and if deployment fails due to any issue, we quickly catch it and report it.
---
### Framework Related:

* Are you using any existing Framework ?
    * Yes we are using existing framework. We are using Cucumber with Junit.
* What tool set you are using: Maven, TestNG, Log4j, Git etc?
    * Our project is a maven based project.
    * We are using Git to contribute, collaborate and version control our automation test framework.
    * We are using Log4J library for logging.
    * We are using default cucumber reports for High level HTML reports.
    
* How mature is the Framework, robustness, synchronization, Generic method libraries, like, xl, db, xml, json common libraries?
  
* How do you store Locators, locators strategies, like page object model etc?
    * We use page object model to store the locators.
    * We mostly use xpath to identify the elements
* How do generate low level logs?
    * For log level logs we are using Log4J2 library.
    * Log level logs are used for debugging and troubleshooting purposes.
    * There-fore we log every event that happens in our test fw.
    
* How do you generate high level logs?
    * For high level logs we are using default cucumber html reports.
    * We can also use Extent report but our client did not require very sophisticated reports and thats why default cucumber html reports works fine for us.
    * But if required we can quickly implement Extent report by using Cucumber Extent report adapter, which does not require any changes in the code and uses same, scn.write statement to generate the report.
        * For more on how to implement Extent report cucumber adapter check the framework: https://github.com/akashdktyagi/VisionIT_Batch8
            * Check this video on how to implement it: https://youtu.be/zCH1ZUbrEx0
    * Latest cucumber version 6 and above also provides a way to generate the reports and publish it using a new cucumber options tag called as ```publish=true```. However, this is still in beta but in the future we are planning to use it. Check the link for more details on this one: https://github.com/akashdktyagi/cucumber-new-report-feature-demo
    
* What is your screen shots strategy? Test case level, step level?
    * We take screen shots after end of each test scenario.
    * Using After cucumber hook to take screen shots.
    * Inside the After cucumber hook method we use Scenario.attach method to attach the screen shot with the cucumber report.
* Where do you store Historical reports for Audit purposes?
    * We store all the reports for each run in the shared drive.
---
### Framework User Related:

* How easy it is to create the test scripts?
    * We have very re-usable steps which we have written.
    * Every time there is a new feature comes, we only have to write the steps for it and we reuse the previously written gherkin steps.
* How much expertise is required to write test scripts? (depends on maturity of the framework)
    * Since we already have cucumber steps written and they are in english language, any one with a very limited knowledge can create a test scenario.
    * Only when there is a new feature is introduced, then it requires some technical knowledge to create a new step def.
* Who write the automation test, only automation tester or functional testers as well.( a functional tester can write better automated tests)?
    * In our team our whole QA team is capable of doing manual as well as automation test case writing. So we interchange the role of manual and automation work between ourselves.
    * So basicall, we all do both manual and automation of our assigned user stories.
* How much test cases per day you can create using the current Framework?
    * It depends on the complexity of the test case.
    * If feature is small then we can create 5-10 test scenarios.
    * If user story is complex, like it requires navigation to many pages or it requires interaction with DB, or dynamic content in the pages then 1-3 test cases per day.

---
### Failures Related:

* What is the current fail rate? And what are the primary reasons?
    * When we run our test pack usually we have around 5-10 percent failures. Not all the failures are because of issues. Some failures are because of bad, flaky test cases.
    * We analyze all the failed test cases to check if we have genuine failed test. For genuine failure we raise a defect and the tests which are failing due to some sync issues etc, we try to re-exeucte them. 
    * If on re-execution they pass, they are considered good tests but if they continuously fail, then we have to fix such test cases.
    * Also, some test cases are failed due to feature being changed. We identify such test cases and we fix them as well.
* How much time is spent in fixing the failed scripts and what priority it holds?
    * Depending on what is the cause of the failure.
    * Tests can fail because of three reasons:
        * Genuine fail: Fail due to defect in the app.
        * Feature Change: Tests failing because of functionality is changed.
        * Flaky Test: Bad tests, written badly. 
    * So depending on the cause and complexity we have to fix these test cases.

---

### Review and Quality Check Related: 
>(i.e. how do you control the FW robustness and Test case quality)

#### For Full details on this flow, please check the tutorial. This is a very comprehensive guide on reviews, PR, git, merging, branching etc.
Link: https://github.com/VisionITTesting/tutorial-git-advanced-demo

* What is the current code review strategy?
    * For every new test addition or fw change we create a branch.
    * We create a Pull request and ask for code review from Team Lead and team members.
    * We fix the issues based on the review comments.
    * Team Lead then approve the PR (pull request).
    * The branch is then merged in to the master branch.
* What is the branching and code merge strategy?
  * Explained above and check the link mentioned above.
* Do you have review check list for the reviewers?
    * We have a list, which we use to give the code comments. (<<<more to write on this.>>>)
* Number of mandatory approvals for the code merge in the final build
    * We have a total of 2 mandatory approvals required in order to merge the pull request.
    
---

### Some other general questions:
#### Processes Related:

* What is your automation sprint cycle?
* Do you have daily scrum meetings?
* How do you notify about new enhancement in the Framework and generic methods with in the team?
* Do you have regular demos?
* Do you have regular retrospectives?
* How much are you delivering per sprint?
* How much is your current back log?
* What percentage of test cases are currently automated?
* How frequently you update your back log and how do you pick which test cases holds priority in that sprint?
* Do you have CI integration for test Automation pack?
* What percent of your automation is back end and in compliance with Agile testing pyramid?