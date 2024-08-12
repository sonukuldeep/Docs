---
author: chatgpt
datetime: 2024-08-12
title: JMeter
slug: "jmeter"
featured: false
draft: false
tags:
  - jmeter testing
ogImage: ""
description: JMeter testing api.
---

# JMeter

Some important terminologies in JMeter that you should know:

### 1. **Test Plan**
   - **Definition**: The top-level container for all elements of a JMeter test. It defines what to test and how to test it.
   - **Significance**: A Test Plan is where you configure all the components, including Thread Groups, samplers, listeners, and more. It holds the structure of your performance test.

### 2. **Thread Group**
   - **Definition**: A set of virtual users that execute the same actions as defined by the samplers in the test plan.
   - **Significance**: It controls the number of threads (users), the ramp-up time, and the loop count. The Thread Group simulates user activity.

### 3. **Sampler**
   - **Definition**: Represents a single request sent to the server (e.g., HTTP Request, JDBC Request).
   - **Significance**: Samplers are the core of any JMeter test, as they define the actual actions (like sending an HTTP request) that the virtual users will perform.

### 4. **Listener**
   - **Definition**: Components that collect and present the results of a test execution (e.g., View Results Tree, Summary Report).
   - **Significance**: Listeners provide insights into the performance of the application by displaying the results of the requests.

### 5. **Assertion**
   - **Definition**: A condition or set of conditions that the response of a request must meet (e.g., Response Assertion).
   - **Significance**: Assertions are used to validate the correctness of responses, ensuring that the application behaves as expected under test conditions.

### 6. **Pre-Processor**
   - **Definition**: Elements that execute before a sampler (e.g., JSR223 PreProcessor).
   - **Significance**: Pre-Processors are used to modify requests, set variables, or perform other setup tasks before a request is sent.

### 7. **Post-Processor**
   - **Definition**: Elements that execute after a sampler (e.g., JSON Extractor).
   - **Significance**: Post-Processors are used to extract data from responses, perform actions based on the response, or clean up after a request is processed.

### 8. **Controller**
   - **Definition**: Logic controllers (e.g., If Controller, Loop Controller) that control the flow of requests in a test plan.
   - **Significance**: Controllers allow you to manage the execution flow, such as looping requests, conditionally executing requests, or grouping requests together.

### 9. **Timer**
   - **Definition**: Elements that introduce delays between requests to simulate real-world user behavior (e.g., Constant Timer).
   - **Significance**: Timers help in pacing requests, ensuring that the test realistically simulates how users interact with the application.

### 10. **Configuration Element**
   - **Definition**: Elements that provide default settings and variables for samplers and other components (e.g., HTTP Request Defaults, CSV Data Set Config).
   - **Significance**: Configuration Elements simplify the test setup by allowing you to define common settings once and reuse them across multiple samplers.

### 11. **Test Fragment**
   - **Definition**: A special type of element that contains a section of a test plan that can be referenced from other parts of the test (e.g., Module Controller).
   - **Significance**: Test Fragments allow for reusability of parts of the test plan, making it easier to manage and maintain large and complex tests.

### 12. **Set Up Thread Group**
   - **Definition**: A Thread Group that runs once before the main test, used for initialization tasks.
   - **Significance**: It’s used to prepare the test environment, such as logging in or setting up test data.

### 13. **Tear Down Thread Group**
   - **Definition**: A Thread Group that runs once after the main test, used for cleanup tasks.
   - **Significance**: It ensures that any cleanup, like logging out users or deleting test data, is done after the test execution.

### 14. **JSR223 Sampler**
   - **Definition**: A sampler that allows you to write custom scripts in languages like Groovy, JavaScript, or Beanshell.
   - **Significance**: JSR223 Samplers provide flexibility to implement complex logic, calculations, or data manipulation that standard components may not cover.

### 15. **Variables and Properties**
   - **Variables**:
     - **Definition**: Temporary data stored during the test execution, accessible within the current thread (e.g., `${variableName}`).
     - **Significance**: Variables are used to store data temporarily and are limited to the scope of the current Thread Group.
   - **Properties**:
     - **Definition**: Data stored that can be accessed globally across all threads (e.g., `props.get("propertyName")`).
     - **Significance**: Properties are used for data that needs to be shared between different Thread Groups or components in the test plan.

These terms are essential for understanding and effectively using JMeter to create, execute, and analyze performance tests.


## Thread Group, Setup Thread, Tear down Thread
### 1. **Thread Group**

- **Purpose**: The primary container for test scripts in JMeter. It defines the number of users (threads) and how they execute the test plan.
- **Use**: Used to simulate user activity by sending requests to the server and recording responses. You can define the number of threads, ramp-up period, and loop count (how many times the test runs).
- **Execution**: Executes as part of the main test scenario. All samplers and logic controllers are placed within a Thread Group.
- **Typical Use Case**: Running the core of your performance test, like simulating multiple users logging in and performing actions on your application.

### 2. **Set Up Thread Group**

- **Purpose**: A specialized thread group that runs before the main Thread Groups in your test plan.
- **Use**: Used to perform any setup actions required before the main test execution, such as logging in, initializing variables, or setting up test data.
- **Execution**: Runs once, before any other Thread Groups. It is not part of the main load test but is used to prepare the test environment.
- **Typical Use Case**: Logging in to the application and extracting a JWT token to be used in the main test, setting up initial test data, or configuring necessary test conditions.

### 3. **Tear Down Thread Group**

- **Purpose**: A specialized thread group that runs after all other Thread Groups have finished executing.
- **Use**: Used to perform cleanup actions after the test execution, such as logging out, cleaning up test data, or closing connections.
- **Execution**: Runs once, after all other Thread Groups have completed. It’s used for cleanup and post-test actions.
- **Typical Use Case**: Logging out users, removing test data, or closing connections to ensure the environment is reset after the test.

### Key Differences:

- **Execution Order**:
  - **Set Up Thread Group** runs first.
  - **Thread Group** runs after the setup.
  - **Tear Down Thread Group** runs last, after the main test is completed.
  
- **Purpose**:
  - **Set Up Thread Group** is for initializing and preparing the environment.
  - **Thread Group** is for the actual test execution.
  - **Tear Down Thread Group** is for cleaning up and finalizing the test.

- **Use Cases**:
  - **Set Up Thread Group**: Pre-test setup (e.g., logging in, preparing data).
  - **Thread Group**: Running the actual test load (e.g., simulating user actions).
  - **Tear Down Thread Group**: Post-test cleanup (e.g., logging out, data cleanup). 

Each of these thread groups serves a distinct purpose in ensuring your JMeter tests are well-organized, efficient, and clean.

## Processing JWT

**JSR223 PostProcessor** can be used to extract data from a response and set it as a JMeter property. This is a powerful feature because it allows the extracted data to be shared across different Thread Groups or be reused throughout the entire test plan.

### Example: Extracting a JWT Token and Setting it as a Property

Let's say you've received a JSON response containing a JWT token, and you want to extract this token and set it as a JMeter property.

### Steps:

1. **Add a JSR223 PostProcessor** as a child of the HTTP Request Sampler that receives the response.

2. **Use the following Groovy script** to extract the JWT token and set it as a property:

   ```groovy
   // Extract the response data (assuming it's JSON)
   def response = new groovy.json.JsonSlurper().parseText(prev.getResponseDataAsString())

   // Extract the JWT token from the JSON response
   def token = response.data.token // Adjust this path based on your JSON structure

   // Set the JWT token as a JMeter property
   props.put("shared_jwt", token)
   ```

### Breakdown of the Script:

- **`prev.getResponseDataAsString()`**: Fetches the response data from the previous sampler as a string.
- **`new groovy.json.JsonSlurper().parseText(response)`**: Parses the JSON response to allow you to extract specific data.
- **`response.data.token`**: Extracts the JWT token from the parsed JSON. (Adjust this based on the actual structure of your JSON response.)
- **`props.put("shared_jwt", token)`**: Sets the extracted JWT token as a JMeter property named `shared_jwt`.

### Accessing the Property in Other Components

Once set as a property, you can access the JWT token throughout your test plan using the following syntax:

- **In a JSR223 PreProcessor**:
  
  ```groovy
  def token = props.get("shared_jwt")
  vars.put("jwt_token", token)  // If you need to use it as a variable
  ```

- **In HTTP Request Headers**:

  ```
  Authorization: Bearer ${__property(shared_jwt)}
  ```

### Benefits:

- **Global Accessibility**: By setting the value as a property, it becomes globally accessible across all Thread Groups and components in the test plan.
- **Flexibility**: The JSR223 PostProcessor allows you to perform complex data manipulation and extraction that might not be possible with standard PostProcessors like the JSON Extractor.

This approach is particularly useful in scenarios where you need to share data, like tokens or session IDs, between different parts of your test plan.

**Alternate approach**

## Using JWT extractor
**Extract JWT using JWT extractor and JSR223**:

### 1. **Thread Group Setup**

   - **Thread Group**: This will contain your requests, including the login request where you'll extract the JWT.

### 2. **Login Request (HTTP Request Sampler)**

   - This is the HTTP request where you authenticate and receive the JWT token in the response.

### 3. **Extract JWT Token using JSON Extractor**

   - Add a **JSON Extractor** Post-Processor as a child of the login request.
   - Configure the JSON Extractor:
     - **Name of created variables**: `jwt_token`
     - **JSON Path Expressions**: `$.data.token` (assuming your JWT is located here)

### 4. **Share JWT Across Threads**

   - Add a **JSR223 PostProcessor** after the JSON Extractor to store the extracted token as a JMeter property, making it accessible across different Thread Groups.
   - In the JSR223 PostProcessor, add the following Groovy script:

     ```groovy
     def token = vars.get("jwt_token")
     props.put("shared_jwt", token)
     ```

   - This script fetches the `jwt_token` from the JSON Extractor and stores it in a JMeter property named `shared_jwt`.

### 5. **Using the JWT in Other Requests**

   - In subsequent HTTP requests, where you need to use the JWT, add a **JSR223 PreProcessor**.
   - In the JSR223 PreProcessor, use the following Groovy script:

     ```groovy
     def token = props.get("shared_jwt")
     vars.put("jwt_token", token)
     ```

   - This script retrieves the `shared_jwt` property and stores it in a JMeter variable `jwt_token`, which you can use in your HTTP request headers or elsewhere as `${jwt_token}`.

### 6. **Tear Down Thread Group**

   - If you want to perform any cleanup after all tests, you can use the **Tear Down Thread Group**.
   - Any variables or properties set during the test can be accessed and utilized here as well.

### Example: Using JWT in an HTTP Request Header

   - In the HTTP Request Sampler, set the `Authorization` header as follows:

     ```
     Authorization: Bearer ${jwt_token}
     ```

This setup ensures that the JWT token extracted from the login response is shared across all threads and can be used in subsequent requests throughout your JMeter test.
