# Lab 6 - Spring Data

## Intro

In this lab we will explore Spring Data. 

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\203 Spring Data`

You will be working in the spring-jpa-demo project you created in the last lab. If needed, you can copy this project from the repo at:

`springboot\end of section workspaces\202 Spring and JPA`

## 1. Create a repository

1. Create a **new package** called `repositories`.

2. Create a **new interface** called `CustomerRepository`.

3. This interface should **extend** `JpaRepository<Customer, Integer>`
(the second parameter is Integer because that is the data type of the Id field of the Customer object).

4. **Annotate** the Repository class with `@Component` – this tells Spring that it will be responsible for creating it.

```
@Component
public interface CustomerRepository extends JpaRepository<Customer, Integer> {
}
```

5. Go to the main class and remove the reference to the DAO – replace it with our repository.

```
CustomerRepository customerRepository = context.getBean(CustomerRepository.class);
```

6. Run the code and check it works!

7. Type in `customerRepository.` and explore what methods are available – try experimenting with some of them!


## 2. Create Custom Repository methods

1. Create a method in the CustomerRepository Interface called `findAllByCountry` – the method should take a string parameter, and return a List of countries

```
public List<Customer> findAllByCountry(String country);
```

Note that we do not implement this method, we just define it.

2. Call the method from the main function, to find and print out all customers with a country of "UK"

3. Run the code to check it works


## 3. Simplify the JPA configuratoin

1. **Remove** the `JpaConfig` and `CustomerDao` classes – these are no longer needed!

2. **Edit the file** called `application.properties` in the project’s resources folder – copy the example provided in the resources folder for this module. You may need to check the database name, username and password are correct.

3. Check the code still runs!

4. Review the project – we now have a simple configuration (application.properties) and almost no code to allow database access (the repository)


## 4. Create Entity Relationships

1. Create a **new class** called `Invoice`. Annotate it with `@Entity`.

2. Add the following **properties** to the class: 
 * id (Long)
 * amount (Double)
 * date (LocalDate)
 * customer (Customer)

3. Annotate the **id** field with `@Id` and `@GeneratedValue(strategy=GenerationType.IDENTITY)`.

4. Annotate the **customer** field with `@ManyToOne` (many invoices for each customer).

5. Run the code and review the structure that has been created in the database.

6. Edit the Customer class and add in a field called **invoices** – this is to be a List of Invoice objects. **Annotate** the field with:
  `@OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, fetch = FetchType.EAGER)`

This annotation means: one customer has many invoices. The mappedBy tells Spring which field in the invoice class relates back to the customer class. The  cascade setting tells Spring if we save a customer, save any changes to their invoices at the same time. The fetch setting tells Spring to load all invoices when loading customers.

**IMPORTANT: do not include the invoices in the Customer’s toString method.**

7. Create a **Repository** for the Invoice entity.

8. In the application’s main method write code to:
 * Get the customer with ID 81
 * Create a new invoice for this customer (with any data)
 * Save the invoice
 * Add the invoice to the customer’s invoices list
 * Save the customer
 * Load all invoices from the database and print them to the console

 
## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\203 Spring Data`

## 6. Explore a Spring Data application that uses MongoDB

1. Review the spring-mongodb-example in the resources folder for this module

Note:
 * The `pom.xml` file includes the MongoDB dependency (from start.spring.io)
 * The `application.properties` file includes the configuration of the MongoDB database
 * Classes to be persisted to the database are annotate with `@Document` (instead of `@Entity`)
 * The `@Id` field will be auto populated by the database (we don’t need the `@GeneratedValue` annotation) but it must be a String
 * We don’t define a repository – we can inject the `MongoTemplate` object


## Summary

In this lab we have:

* Seen how to use Spring Data to make interacting with databases from Spring projects easier
* **Configured JPA** using the application.properties file
* Created a **repository** interface to define the functions that we need to interact with the database - Spring magically creates an implementation of this interface, including for any custom methods we write
* Seen how to create entity relationships with annotations
* Explored a sample project that uses MongoDB as the database