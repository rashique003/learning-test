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
<![CDATA[
To ensure a robust and efficient implementation of the Java Spring Boot backend with Selenium BDD and React frontend, here’s a refined step-by-step process using best practices. This involves structured project organization, dynamic configurations, detailed error handling, and extensibility for future needs.


---

Backend Development: Spring Boot

1. Project Setup

1. Open Eclipse IDE.


2. Create a new Maven project:

Go to File > New > Maven Project.

Check Create a simple project.

Enter Group ID as com.example, Artifact ID as selenium-bdd-service, and click Finish.




2. Add Dependencies to pom.xml

Add the latest versions of required dependencies:

<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- Selenium WebDriver -->
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
    <!-- Testing Framework -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version>
    </dependency>
</dependencies>

3. Configure ChromeDriver Path

Use application.properties for dynamic configurations:

chromedriver.path=path/to/chromedriver


---

4. Define the REST Controller

Create a REST API to trigger tests dynamically:

1. Create a new package com.example.controller.


2. Add a class TestController:



package com.example.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/tests")
public class TestController {

    @Value("${chromedriver.path}")
    private String chromeDriverPath;

    @PostMapping("/run")
    public String runTests() {
        System.setProperty("webdriver.chrome.driver", chromeDriverPath);

        try {
            io.cucumber.junit.Cucumber.main(new String[]{
                "--glue", "com.example.stepdefinitions",
                "src/test/resources/features"
            });
            return "Test execution completed!";
        } catch (Exception e) {
            return "Error during test execution: " + e.getMessage();
        }
    }
}


---

5. Feature Files

Create a directory src/test/resources/features:

1. Add a file google.feature:



Feature: Google Search

  Scenario: Verify Google Search Title
    Given I open the browser and navigate to "https://www.google.com"
    Then the page title should be "Google"


---

6. Step Definitions

Create a package com.example.stepdefinitions:

1. Add a class GoogleSteps:



package com.example.stepdefinitions;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class GoogleSteps {

    private WebDriver driver;

    @Given("I open the browser and navigate to {string}")
    public void openBrowserAndNavigate(String url) {
        driver = new ChromeDriver();
        driver.get(url);
    }

    @Then("the page title should be {string}")
    public void verifyPageTitle(String expectedTitle) {
        assertEquals(expectedTitle, driver.getTitle(), "Page title mismatch!");
        driver.quit();
    }
}


---

7. Test Runner

Create a package com.example.runners:

1. Add a class CucumberTestRunner:



package com.example.runners;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
    features = "src/test/resources/features",
    glue = "com.example.stepdefinitions",
    plugin = {"pretty", "html:target/cucumber-reports.html"}
)
public class CucumberTestRunner {
}


---

Frontend Development: React.js

1. Create React App

Initialize a React project:

npx create-react-app selenium-bdd-ui
cd selenium-bdd-ui

2. Install Axios

Install Axios for REST API calls:

npm install axios

3. Implement the Test Runner Interface

Modify App.js:

import React, { useState } from "react";
import axios from "axios";

function App() {
  const [result, setResult] = useState("");

  const runTests = async () => {
    try {
      const response = await axios.post("http://localhost:8080/api/tests/run");
      setResult(response.data);
    } catch (error) {
      setResult("Error: " + error.message);
    }
  };

  return (
    <div style={{ padding: "20px", fontFamily: "Arial" }}>
      <h1>Test Automation Runner</h1>
      <button
        onClick={runTests}
        style={{
          padding: "10px 20px",
          backgroundColor: "#007BFF",
          color: "#FFF",
          border: "none",
          borderRadius: "5px",
          cursor: "pointer",
        }}
      >
        Run Tests
      </button>
      <div style={{ marginTop: "20px" }}>
        <strong>Result:</strong>
        <p>{result}</p>
      </div>
    </div>
  );
}

export default App;


---

Key Best Practices

1. Dynamic Configurations:

Store ChromeDriver path and other variables in application.properties.

Use environment variables for sensitive data.



2. Logging and Error Handling:

Replace System.out.println with a logging framework (e.g., Logback).

Provide detailed error messages in API responses.



3. Extensible Design:

Add support for multiple test scenarios.

Allow dynamic feature file selection via frontend.



4. Enhance Reporting:

Integrate tools like Allure or Cucumber HTML Reports for detailed reporting.



5. Frontend Improvements:

Add a status indicator (e.g., spinner) during test execution.

Display test execution logs dynamically.





---

Execution Steps

1. Run the Spring Boot Backend:

In Eclipse, right-click the main class annotated with @SpringBootApplication and select Run As > Spring Boot App.



2. Run the React Frontend:

Navigate to the React project folder and run:

npm start



3. Trigger Tests:

Open the frontend at http://localhost:3000.

Click Run Tests. Observe the backend API logs in Eclipse and the result in the React UI.





---

This approach ensures a clean, scalable, and maintainable solution. Let me know if you'd like assistance with any step!
]]>
