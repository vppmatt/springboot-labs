# Lab 5 - Spring and JPA

## Intro

In this lab we will explore Hibernate within a Spring project. 

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\202 Spring and JPA`

## 1. Create a new project

1. Use https://start.spring.io to **create a new Spring project**.

2. Give the project the **name** `spring-jpa-demo`

3. Add the following **dependencies** to the project:

  * Spring Data JPA
  * MySQL Driver
  * Spring Web
  * Spring Boot Dev Tools
  
4. Generate and unzip the project, then open it in your IDE. Review the pom.xml file.

**Do not try to run the project yet – it won’t work until we have configured JPA!**

## 2. Configure JPA

1. Copy the `JpaConfig` class provided in the  resources folder of the repo into your project.

2. Review the code in this file and make any changes required to suit your project (check the database name, username and password). 

4. Review the external libraries to see what has been added

## 3. Set up an entity

1. Copy the **Customer class** that you created in the previous lab exercise into the proejct. If you have not created this file, copy the one from the closing workspace for the previous section from the repo (`springboot\end of section workspaces\201 Hibernate`) 

2. Copy the following code into the main method of the Application, to test that JPA is working in Spring:

```
public static void main(String[] args) {
    ApplicationContext context = SpringApplication.run(SpringJpaDemoApplication.class, args);
    
    EntityManager entityManager = context.getBean(EntityManager.class);
    
    TypedQuery<Customer> getAllCustomersQuery = entityManager.createQuery("select c from Customer c", Customer.class);
    List<Customer> customers = getAllCustomersQuery.getResultList();
    
    for (Customer customer : customers) {
        System.out.println(customer.toString());
    }
}
```
## 4.  Create a DAO

1. Create a new class called `CustomerDao`. 

2. Annotate the class with `@Component` so that Spring creates the instance for us.

3. Declare a private class level variable of type `EntityManager`.

4. Annotate this variable with `@PersistenceContext` so that Spring can provide the bean using dependency injection.

5. Create a function called findAll() that uses the code we wrote earlier to return a list of Customers.

6. Edit the code in the main function to use the CustomerDao class.
 
## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\202 Spring and JPA`

## Summary

In this lab we have:

* Created a Spring project with the **JPA** dependency
* **Configured JPA** using Java. 
* Written regular **JPA code** to interact with a database.