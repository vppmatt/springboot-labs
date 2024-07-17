# Lab 9 part 1 - Spring and Rest

## Intro

In this lab we will start to create the main project for this course - we'll be constructing a REST API.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\402 Spring and Rest`



## 1. Create a new application and a REST endpoint

1. Using https://start.spring.io, **create a new project** called `payments-application`

2. Add in the following **dependencies**:
  * Spring Data JPA
  * MySQL Driver
  * Spring Web
  * Spring Boot Dev Tools

3. **Generate** the project, open it in your IDE and run the process to get the **Maven dependencies**.

4. **Create a database** in MySQL called "payments"

5. Edit the `application.properties` file to **configure the database connection**. The format is shown below - check the database name, username and password.

```
spring.datasource.url=jdbc:mysql://localhost:3306/payments
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.hbm2ddl.auto=update
```

6. **Create a package** called `control`.

7. Inside this package **create a class** called `HealthcheckController`.

8. **Annotate** the class with `@RestController`.

9. **Create a function** which returns the string "ok". The name of this function is not important.

10. **Annotate** this function with `@GetMapping("/health")`

11. **Run the application** and visit http://localhost:8080/health in your browser (or do curl –i http://localhost:8080/health )

<details>
<summary>
Expand to view the code sample

</summary>

```
@RestController
public class HealthcheckController {

    @GetMapping("/health")
    public String healthcheck() {
        return "OK";
    }
}
```

</details> 

## 2. Support Cross Origin Resource Sharing (CORS)

1. Ensure the application from part 1 of this lab is running.

2. If you have not previously done so, **clone the curl generator** example web page by running the following command in a terminal window:

```
git clone https://github.com/vppmatt/curl-generator.git
```

3. **Open the html page** from the curl generator repo in a browser.

4. Attempt to **perform a get request** to the http://localhost:8080/health endpoint. View the browser console to see the error. 

5. **Add the `@CrossOrigin` annotation** to the controller class. If you have not configured your IDE to auto-restart the proejct, stop and restart the application.

6. **Access the health endpoint** using the Curl Generator page again- note that you will still get an error as the code is not returning valid JSON but you’ll see a 200 status!

## Summary

In this lab we have:

* Created a simple rest end point
* Implemented CORS (in a non production-standard way!)