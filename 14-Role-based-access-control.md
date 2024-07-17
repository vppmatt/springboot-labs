# Lab 14 - Role Based Access Control and Security

## Intro

In this lab we will continue working on the main project for this course.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\602 Security`

You should continue to work on the project you built in the previous lab. If you need to, you can use the sample proejct from the repository in the folder:

`springboot\end of section workspaces\601 Testing`

## 1. Implement Basic Authentication 

1. Add the **Spring Security dependencies** to the project

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.security</groupId>
   <artifactId>spring-security-test</artifactId>
   <scope>test</scope>
</dependency>
```

2. Copy the `SecurityConfig` **package** from the resources folder into the project.

3. Copy and replace the `UserServiceImpl` class from the resources folder.

4. Copy the `UserBootStrapService` class.

5. Delete any users previously created in the database

6. Add the `findByName` method to the `UserRepository`

7. Review all the code you have just added!

8. Test the application using the curlGenerator.html file

## 2. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\602 Security`


## Summary

In this lab we have:

* Implemented Basic authentication and configured our application to secure REST endpoints
* For a production standard approach, we need to go further and implement Bearer authentication!