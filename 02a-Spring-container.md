# Lab 2 part 1 - The Spring Container

## Intro

In this lab we will configure a simple Spring project to use **Inversion of Control (IoC)** by adding a configuration class.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\101 Starting Spring\02-spring-container-demo`

## 1. Open and review the starting project

Open the starting project in your IDE. This is a Spring project, although it doesn't follow some of the standard conventions in Spring just yet... we have recreated the example project from lab 1 as a Spring project. 

Note that the class called **SpringContainerDemoApplication** contains the runnable method. This method contains the same code as before - we are instantiating the classes we want. In this lab we'll be changing this so that Spring instantiates them for us. 

1. Download the project's dependencies. If you are using an IDE like IntelliJ, you can open the file called **pom.xml**, and then click on the Maven refresh icon. Alternatively you can run the following command from a command prompt / terminal window: 

``mvnw package``

2. Run the applicaiton and check it works. 
     

## 2. Implement Inversion of Control with a Configuration class

1. Create a new class called **Config.java**.

2. Annotate this class with **@Configuration**.

3. Create a function to **instantiate and return an instance of the InvoiceDataInterface**. This function can have any name you like (we will never call it).

4. Annotate the function with **@Bean** to tell Spring that it should run this method and therefore be responsible for instantiating the object.

<details>
<summary>
Expand to view the code sample

</summary>

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Config {

    @Bean
    InvoiceDataInterface invoiceData() {
        return new InvoiceData();
    }

}
```
</details>

## 3. Implement Dependency Injection with a Configuration class

1. Create a function to **instantiate and return an instance of the InvoiceUtilitiesInterface**. THis function will take a parameter of type **InvoiceDataInterface**.

2. Annotate the function with **@Bean** to tell Spring that it should run this method and therefore be responsible for instantiating the object.

3. Annotate the parameter to the method with  **@Autowired** to tell Spring that it should find an instance of the object to inject. 

<details>
<summary>
Expand to view the code sample

</summary>

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Config {

    @Bean
    InvoiceDataInterface invoiceData() {
        return new InvoiceData();
    }

    @Bean
    InvoiceUtilitiesInterface invoiceUtilities(@Autowired InvoiceDataInterface invoiceData) {
        InvoiceUtilities invoiceUtilities = new InvoiceUtilities();
        invoiceUtilities.setInvoiceData(invoiceData);
        return invoiceUtilities;
    }
}

```
</details>

## 4. Use the Beans Spring has created

1. Update the runnable method in the **SpringContainerDemoApplication** class to remove any code that instantiates the objects and instead use the beans that Spring created.

Hint - the first line of code returns an object of type **ApplicationContext** - this object contains a **.getBean()** method.
 
 2. Run the application and check that it works.
 
<details>
<summary>
Expand to view the code sample

</summary>

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class SpringContainerDemoApplication {

	public static void main(String[] args) {
		ApplicationContext context = SpringApplication.run(SpringContainerDemoApplication.class, args);

		InvoiceDataInterface invoiceData = context.getBean(InvoiceDataInterface.class);
		InvoiceUtilitiesInterface invoiceUtilities = context.getBean(InvoiceUtilitiesInterface.class);
		invoiceUtilities.setInvoiceData(invoiceData);

		Invoice invoice = invoiceUtilities.generateInvoice();
		invoiceUtilities.payInvoice(invoice.getId());

	}
}
```
</details>

## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\101 Starting Spring\02-spring-container-demo with configuration`

## Summary

In this lab we have:

* Used a class with the **@Configuration** annotation - this tells Spring it must execute the methods in this class when the application starts.

* Created methods with an **@Bean** annotation. This tells Spring that the methods return an object that Spring is responsible for creating (Inversion of Control)

* Created a method parameter with an **@Autowired** annotation. This tells Spring that when it executes this method it needs to find a bean to inject here. (Dependency Injection)
