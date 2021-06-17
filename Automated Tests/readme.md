# Services Used

## Runscope
Runscope is a service that allows us to run automated API tests to evaluate the uptime, performance and correctness of an API.

## Ghost Inspector
[Ghost Inspector](https://ghostinspector.com/) is an automated website testing and monitoring service that checks for problems with your website or application. It carries out operations in a browser, the same way a user would, to ensure that everything is working properly.

### Ghost Inspector Usage
Our [primary monitors](https://app.ghostinspector.com/folders/5c4763450bb17c1d6e1f5e9d) cover the website both on QA and Production. Since these tests are mostly duplicates, we prefer to use shared [Modules](https://ghostinspector.com/docs/modularizing-tests/) to import shared tests across environments. When creating a new test, it should first be created as a module within the [Test Modules](https://app.ghostinspector.com/suites/5deabb216635a209d98365bf) directory, and then introduced within both the QA & Production suites.

## Test Users
Some tests require a valid user to validate the API responses. Here's a list of test users being used in our apps:

| App | NorthstarId | role |
| -- | -- | -- |
Blink | `5af9a42fa0bfad6b4a7c67f8` | user
Northstar | `5a3acd23a0bfad43ec4542e6` | admin

> Emails and passwords can be found in LastPass.
