# Lab 2 part 2 - The Spring Container

## Intro

In this lab we will configure a simple Spring project to use **Inversion of Control (IoC)** with annotations.

## Pre-requisites

Before starting this lab, **you should have completed the exercise in Lab 2 part 1** from https://github.com/vppmatt/springboot. 

## 1. Remove the configuration class

1. Remove the Configuration class that you created in part 1 of this lab.
  
## 2. Implement Inversion of Control with annotations

1. Add the **@Component** annotation to the 2 classes that implement the interfaces. This will tell Spring that it should instantaite this class when the application starts. 

## 3. Implement Dependency Injection with annotations

1. Edit the InvoiceUtilities class and annotate the invoiceData field with the **@Autowired** annotation to tell Spring that it should find an instance of the object to inject. 

## 4. Simplify the runnable method

1. Update the runnable method in the **SpringContainerDemoApplication** class to remove all references to InvoiceData - now we can simply work with InvoiceUtilities. 

2. Run the application and check that it works.
 
<details>
<summary>
Expand to view the code sample

</summary>

```
@SpringBootApplication
public class SpringContainerDemoApplication {

	public static void main(String[] args) {
		ApplicationContext context = SpringApplication.run(SpringContainerDemoApplication.class, args);

		InvoiceUtilitiesInterface invoiceUtilities = context.getBean(InvoiceUtilitiesInterface.class);
		Invoice invoice = invoiceUtilities.generateInvoice();
		invoiceUtilities.payInvoice(invoice.getId());
	}

}

```
</details>

## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\101 Starting Spring\02-spring-container-demo with annotations`


## Summary

In this lab we have:

* Used a class with the **@Component** annotation - this tells Spring it must create an instance of this class when the application starts (Inversion of Control).

* Annotated a class level field with the **@Autowired** annotation. This tells Spring that when it instantiates this class, it needs to find a bean to inject here. (Dependency Injection)
