# Lab 11 - Logging

## Intro

In this lab we will continue working on the main project for this course.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\501 Logging`

You should continue to work on the project you built in the previous lab. If you need to, you can use the sample proejct from the repository in the folder:

`springboot\end of section workspaces\403 Design patterns and conventions`

## 1. Impelment Logging

**Implement logging** in the `PaymentServiceImpl` class:

1. Create a local reference to the logger:

<details>

<summary>
Expand to view the code sample
</summary>

```
private Logger logger = LoggerFactory.getLogger(PaymentsServiceImpl.class);
```

</details>

2. Edit the `getAllPayments` method to **log at info level** that the request was made:

<details>

<summary>
Expand to view the code sample
</summary>

```
public List<Payment> getAllByCountry(String country) {
  logger.info("Getting all payments for country: " + country);
  return paymentsRepository.findAllByCountry(country);
}
```

</details>

3. Run the application and view the console


## 2. Configure logging

1. **Copy** the `logback.xml` file from the course resources to the application’s resources folder.

2. **Remove** the logging configuration from `application.properties` (if you added it!)

3. **Run the code** – there will be a lot less logging output now.


## 3. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\501 Logging`


## Summary

In this lab we have:

* Implemented custom logging functionality
* configured Logging levels using application.properties and an XML configuration file.