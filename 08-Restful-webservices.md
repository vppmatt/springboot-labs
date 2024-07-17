# Lab 8 - Restful Webservices

## Intro

In this lab we will explore a REST API.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  



## 1. Start a sample REST application

1. Use git to **clone the sample  project** (run this command in a terminal window)

```
git clone https://github.com/vppmatt/paymentgateway-standalone-17.git
```

2. **Open the project** in IntelliJ.

3. Use the Maven refresh icon to **get the dependencies**.

4. **Run** the application.

5. Open the `client.html` file in your browser (open the file don't navigate to it) and **try out the commands**.

6. Try running the **CURL commands** from the command line

Note that this project has a couple of features in that are not part of this course, and the javascript in the html page is also not part of this course. It has not necessarily been written to production standards or tested well!

7. **Clone the curl generator** file which will allow more general API testing by running the following command in a terminal window:

```
git clone https://github.com/vppmatt/curl-generator.git
```
 

## 2. View API Documentation

1. Ensure the application from part 1 of this lab is running.

2. Visit the following URL in your browser:
http://localhost:8080/swagger-ui.html

3. Use the swagger interface to explore the Rest API.

## Summary

In this lab we have:

* Experimented with communicating with a REST API from the browser and the command line
* Explored the **Swagger** UI.