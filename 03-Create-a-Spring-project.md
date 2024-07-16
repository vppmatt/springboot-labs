# Lab 3 - Create a Spring Boot Project

## Intro

In this lab we will create our first Spring project.


## 1. Visit the Spring Initializr

1. Using your browser, visit https://start.spring.io 

2. Using the site, create a project with the following properties:

* Project: **Maven**
* Language: **Java **
* Spring Boot version: **(the latest verison not marked SNAPSHOT)**
* Group: **com.neueda**
* Artifact: **first-demo**
* Name: (will default to **first-demo**)
* Description: **demo Spring application**
* Package name: (will default to **com.neueda.first-demo**)
* Packaging: **Jar**
* Java version: **17**

3. Add the following dependencies:

* Spring Web
* Spring Boot DevTools 

(don’t add any others for now)

4. Generate the zip file, unzip it, open it in your IDE, and explore the contents. 

5. Use the maven pom to get the dependencies.

6. Explore the project structure. 

## 2. Add an HTML file

1. Create a file called hello.html in the `src/main/resources/static` folder.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
    <p>Hello World</p>
</body>
</html>
```

2. Run the application and visit http://localhost:8080/hello.html in your browser


## 3. Set up live reloading

Note this applies to IntelliJ - these settings are not needed in Eclipse. Other IDEs may require altnative configuration.

1. Configure IntelliJ to a support live reloading of code:
* Navigate the menus: File -> Settings –> Build, Execution, Deployment –> Compiler. Then select "Build project automatically"

* Press SHIFT+CTRL+A, search for and select “registry”. Now ensure “compiler.automake.allow.when.app.running” is ticked

## 4. Build a Jar file

1. Run the maven lifecyle step of package - you can do this by double-clicking on the package link in the Maven window in IntelliJ, or run the following command from a command prompt:

```mvnw package```

2. In a command window navigate to the target folder and run:
  
  ```java -jar first-demo-0.0.1-SNAPSHOT.jar```

3. Navigate to the URL in your browser to check it is working:
  http://localhost:8080/hello.html

4. Press CTRL+C to stop the application running


## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\102 Spring Boot`

## Summary

In this lab we have:

* Created a new Spring project using the Spring Initializr

* Built the application to a Jar file.
