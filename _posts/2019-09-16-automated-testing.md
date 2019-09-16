---
layout: post
title: "Automated Testing With JUnit, Selenium and Managing Database State"
tags:
- Java
- JUnit
- Selenium
- dbUnit
- testing
thumbnail_path: blog/thumbs/automated-testing-with-selenium.png
add_to_popular_list: true
---

{% include figure.html path="blog/thumbs/automated-testing-with-selenium.png" %}
Maven template for automated testing with JUnit, Selenium and managing database state with DbUnit.

[junit-selenium-dbunit](https://github.com/Marat555/junit-selenium-dbunit) - GitHub repository.

## Background
This week, one of my tasks was to create a basis for writing automated tests to test ui in different browsers. For this, I used the following:
1. [Selenium](https://github.com/SeleniumHQ/selenium) is an umbrella project encapsulating a variety of tools and libraries enabling web browser automation. Selenium specifically provides infrastructure for the W3C WebDriver specification — a platform and language-neutral coding interface compatible with all major web browsers.
2. [JUnit](https://github.com/junit-team/junit4) is a programmer-oriented testing framework for Java.
3. [DbUnit](http://dbunit.sourceforge.net/) is  is a JUnit extension (also usable with Ant) targeted at database-driven projects that, among other things, puts your database into a known state between test runs. This is an excellent way to avoid the myriad of problems that can occur when one test case corrupts the database and causes subsequent tests to fail or exacerbate the damage.
4. [Apache Maven](https://maven.apache.org/) is a software project management and comprehension tool.

In this post I would like to describe how to use this template in your project.

## DbUnit

{% include figure.html path="blog/automated-testing/database-management.jpg" %}

For managing database state I used [dbunit-plus](https://github.com/mjeanroy/dbunit-plus).

Add database connection configuration annotation `@DbUnitConnection` in `DriverBase` class.

Example:

```java
@DbUnitConnection(url = "jdbc:postgresql://localhost:5432/test", user = "deep", password = "123")
public class DriverBase {
  ...
}
```

Add a file with data set for insertion to the database to `/resources/dbunit/xml`. Example of file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<dataset>
    <person id="1" name="David"/>
    <person id="2" name="George"/>
    <person id="3" name="Freddy"/>
    <person id="4" name="Steven"/>
</dataset>
```

Here are the available annotations:

`@DbUnitDataSet`: define dataset (or directory containing dataset files) to load (can be used on package, entire class or a method).<br />
`@DbUnitInit`: define SQL script to execute before any dataset insertion (can be used on package or entire class).<br />
`@DbUnitSetup`: define DbUnit setup operation (can be used on package, entire class or a method).<br />
`@DbUnitTearDown`: define DbUnit tear down operation (can be used on package, entire class or a method).<br />

Example:

```java
@RunWith(SeleniumRunner.class)
public class CampaignsPageIT extends DriverBase {
    @BeforeClass
    public static void setUp() {
        instantiateDriverObject();
    }

    @AfterClass
    public static void tearDown() {
        closeDriverObjects();
    }

    @After
    public void afterEachMethod() {
        clearCookies();
    }

    private ExpectedCondition<Boolean> pageSizeEquals(final String size) {
        return driver -> driver.findElement(By.cssSelector("form#TemplateBackupForm > div:nth-of-type(2) > div:nth-of-type(2) > div > div:nth-of-type(2) > div:nth-of-type(4) > div > span:nth-of-type(2) > span > ul > li:nth-of-type(3) > a")).getText().equals(size);
    }

    @Test
    @DbUnitDataSet("/dbunit/xml/sampleData.xml")
    @DbUnitSetup(DbUnitOperation.INSERT)
    @DbUnitTearDown(DbUnitOperation.DELETE)
    public void pagingTest() throws Exception {
        WebDriver driver = getDriver();
        driver.get("https://10.20.3.138");
        AuthPage authPage = new AuthPage();
        authPage.fillLoginForm("default@user.com", "123").submitLogin();
        WebDriverWait wait = new WebDriverWait(driver, 30, 100);
        wait.until(pageSizeEquals("100"));
    }
}
```

## JUnit-Selenium 

{% include figure.html path="blog/automated-testing/selenium-icon.jpg" %}

As maven template, I used [Selenium-Maven-Template](https://github.com/Ardesco/Selenium-Maven-Template), but I made some changes in code and pom.xml replacing TestNg to JUnit, adding and fixing dependencies conflicts in pom.xml.

1. Open a terminal window/command prompt
2. Clone this project.
3. `cd junit-selenium-dbunit` (Or whatever folder you cloned it into)
4. `mvn clean verify`

All dependencies should now be downloaded and the example google cheese test will have run successfully in headless mode (Assuming you have Firefox installed in the default location)

### What should you know?

- To run any unit tests that test your Selenium framework you just need to ensure that all unit test file names end, or start with "test" and they will be run as part of the build.
- The maven failsafe plugin has been used to create a profile with the id "selenium-tests".  This is active by default, but if you want to perform a build without running your selenium tests you can disable it using:

        mvn clean verify -P-selenium-tests
        
- The maven-failsafe-plugin will pick up any files that end in IT by default.  You can customise this is you would prefer to use a custom identifier for your Selenium tests.

### Known problems...

- It looks like SafariDriver is no longer playing nicely and we are waiting on Apple to fix it... Running safari driver locally in server mode and connecting to it like a grid seems to be the workaround.

### Anything else?

You can specify which browser to use by using one of the following switches:

- -Dbrowser=firefox
- -Dbrowser=chrome
- -Dbrowser=ie
- -Dbrowser=edge
- -Dbrowser=opera

If you want to toggle the use of chrome or firefox in headless mode set the headless flag (by default the headless flag is set to true)

- -Dheadless=true
- -Dheadless=false

You don't need to worry about downloading the IEDriverServer, EdgeDriver, ChromeDriver , OperaChromiumDriver, or GeckoDriver binaries, this project will do that for you automatically.

You can specify a grid to connect to where you can choose your browser, browser version and platform:

- -Dremote=true 
- -DseleniumGridURL=http://{username}:{accessKey}@ondemand.saucelabs.com:80/wd/hub 
- -Dplatform=xp 
- -Dbrowser=firefox 
- -DbrowserVersion=44

You can even specify multiple threads (you can do it on a grid as well!):

- -Dthreads=2

You can also specify a proxy to use

- -DproxyEnabled=true
- -DproxyHost=localhost
- -DproxyPort=8080
- -DproxyUsername=fred
- -DproxyPassword=Password123

If the tests fail screenshots will be saved in ${project.basedir}/target/screenshots

If you need to force a binary overwrite you can do:

- -Doverwrite.binaries=true

### It's not working!!!

You have probably got outdated driver binaries, by default they are not overwritten if they already exist to speed things up.  You have two options:

- `mvn clean verify -Doverwrite.binaries=true`
- Delete the `selenium_standalone_binaries` folder in your resources directory

## Summary

I hope this post is useful to someone who plans to add auto tests to his projects.

## See more 

GitHub repository - [junit-selenium-dbunit](https://github.com/Marat555/junit-selenium-dbunit)