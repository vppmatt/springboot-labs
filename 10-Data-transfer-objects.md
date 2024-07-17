# Lab 10 - Create a Data Transfer Object

## Intro

In this lab we will continue working on the main project for this course.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\403 Design patterns and conventions`

You should continue to work on the project you built in the previous lab. If you need to, you can use the sample proejct from the repository in the folder:

`springboot\end of section workspaces\402 Spring and rest`

## 1. Create a DTO

1. **Create a new Entity class** for users of our system. It should be called `User` and contain the fields:
  * id : Long
  * name : String
  * role : enum (with values USER and ADMIN)
  * password: String

2. Create **getters and setters** for the `User` class. 

3. **Create a toString** method - don't include the `password` field in the toString method!

4. **Create a repository** for the User class

5. **Create a URL endpoint** of /api/user with functions to get all users, get a single user by id and post a new user. Create the required controllers and services to implement this.

6. Use Swagger to **create and save a new user** (note that we are saving the user’s password in plain text – that’s not production standard - we will fix it later!)

7. **Retrieve the user** via Swagger – we are seeing their password which is not great!

8. **Include in your project** the `UserDTO` class provided in the resources section – review the code in this class.

9. **Amend the methods** you created in the controller / service to return `UserDTO` objects.

## 2. Avoid JSON loops / exclude data from being returned by a REST API

1. We never want to return a user’s password, so we can **annotate the getter method** in the User entity class with `@JsonIgnore` – this is a good idea in case we accidently write code that returns a user in future!

## 3. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\403 Design patterns and conventions`


## Summary

In this lab we have:

* Created custom classes for use to transfer data into / out of a Rest API (data transfer objects)
* used the `@JsonIgnore` annotation to exclude entity fields from being returned in Rest requests. 