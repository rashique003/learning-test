<html>
  <head>
    <title>My first site</title>
  </head>
  <body>
    <h1>Access my first Site </h1>
    <a href = "https://rashique003.github.io/learning-test/site/">Click here</a><br><br>
    <h2><u>Notes</u></h2>
  </body>
</html>
Here’s a detailed, step-by-step guide to setting up and developing the Java Spring Boot backend with Selenium and Cucumber BDD in Eclipse and integrating it with a React.js frontend:


---

Backend Development in Eclipse

1. Install Prerequisites

Before starting, ensure you have:

Eclipse IDE for Java Developers (latest version).

Maven installed and configured.

Java 11 or later installed.

Google Chrome and ChromeDriver installed.


2. Create a Spring Boot Project

1. Open Eclipse.


2. Go to File > New > Maven Project.


3. In the wizard:

Select Create a simple project and click Next.

Enter Group ID (e.g., com.example), Artifact ID (e.g., selenium-bdd-app), and Version. Click Finish.





---

3. Add Dependencies in pom.xml

1. Open the pom.xml file in the project.


2. Add the following dependencies for Spring Boot, Selenium, and Cucumber:

<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- Selenium -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.14.0</version>
    </dependency>
    <!-- Cucumber -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-java</artifactId>
        <version>7.15.0</version>
    </dependency>
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-spring</artifactId>
        <version>7.15.0</version>
    </dependency>
</dependencies>


3. Save the file, and Eclipse will download the dependencies automatically.




---

4. Create a Feature File

1. Right-click on the src/test/resources folder, then select:

New > Folder, and name it features.



2. Inside the features folder:

Right-click and select New > File, name it test.feature.



3. Add the following content to the file:

Feature: Google Search

  Scenario: Verify Google Search Title
    Given I open the browser and navigate to "https://www.google.com"
    Then the page title should be "Google"




---

5. Create Step Definitions

1. Right-click on the src/test/java folder, then select:

New > Package, and name it com.example.stepdefinitions.



2. Inside this package:

Right-click and select New > Class, name it GoogleSteps.



3. Add the following code to the class:

package com.example.stepdefinitions;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class GoogleSteps {

    private WebDriver driver;

    @Given("I open the browser and navigate to {string}")
    public void iOpenTheBrowserAndNavigateTo(String url) {
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
        driver = new ChromeDriver();
        driver.get(url);
    }

    @Then("the page title should be {string}")
    public void thePageTitleShouldBe(String expectedTitle) {
        String actualTitle = driver.getTitle();
        assertEquals(expectedTitle, actualTitle, "Page title mismatch!");
        driver.quit();
    }
}

Note: Replace path/to/chromedriver with the actual path to your ChromeDriver executable.




---

6. Create the Cucumber Test Runner

1. Right-click on src/test/java and create a new package, com.example.runners.


2. Create a new class in this package, CucumberTestRunner.


3. Add the following code:

package com.example.runners;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
    features = "src/test/resources/features",
    glue = "com.example.stepdefinitions",
    plugin = {"pretty", "html:target/cucumber.html"}
)
public class CucumberTestRunner {
}




---

7. Create the REST API

1. Right-click on src/main/java and create a package, com.example.controller.


2. Inside this package:

Create a class TestController.



3. Add the following code:

package com.example.controller;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/tests")
public class TestController {

    @PostMapping("/run")
    public String runTests() {
        try {
            io.cucumber.junit.Cucumber.main(new String[]{
                "--glue", "com.example.stepdefinitions",
                "src/test/resources/features"
            });
            return "Tests executed successfully!";
        } catch (Exception e) {
            return "Test execution failed: " + e.getMessage();
        }
    }
}




---

Frontend Development in React.js

The React.js app can be developed separately using tools like VSCode.

Steps to Link Frontend and Backend

1. Create a React app:

npx create-react-app selenium-bdd-ui
cd selenium-bdd-ui


2. Add a component (App.js) to make REST API calls to the backend.



Refer to the React code shared in the previous response for the App.js component.


---

Running the Application

1. Start the Spring Boot backend:

Right-click on the main Spring Boot application class (@SpringBootApplication) in Eclipse and select Run As > Spring Boot App.

The backend will run on http://localhost:8080.



2. Start the React frontend:

Run npm start in the React project directory.

Open http://localhost:3000 in a browser.



3. Trigger tests:

Enter a URL in the React app (or click the button) to run Selenium tests via the backend.





---

Let me know if you’d like further clarification or assistance!

