# Lab 1 - Decoupling

## Intro

An important principle when building Spring applications is decoupling. This is achieved through separating interfaces from their implementations and then "programming to the interface".

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\101 Starting Spring\01-DeCouplingExample`

## 1. Open and review the starting project

Open the starting project in your IDE. This is not a Spring project - it's a simple Java project containing the following classes:

* **Invoice**: This is a java object that is modelling a real world invoice
* **InvoiceData**: This is a class that simulates access to a database where invoices are stored
* **InvoiceUtilities**: This is a class that provides functionality to work with invoices, such as the ability to create a new invoice, mark an invoice as paid, or get a list of all invoices for a customer.
* **Main**: this is a class containing a runnable method. This method creates an invoice and then marks it as paid. 

Review the code in this project. Note that InvoiceUtilities requires an instance of InvoiceData to work - we say that InvoiceUtilities has a **dependency**. In order to use InvoiceUtilities, we must first create an instance of InvoiceData.

This code is **tightly coupled** - if we wanted to swap InvoiceData for a real database later on, that would be difficult to do.

## 2. Make Interfaces

To make it easier to swap the InvoiceData class with a new version later that uses a real database, we want to decouple it. 

1. Create an Interface called **InvoiceDataInterface**. Note that this is not a production standard name - we'll learn the regular naming convention later. The InvoiceData class should implement this interface.

Hint: If you are using an IDE like IntelliJ, try the **refactor/Extract Interface** feature.

2. Repeat the exercise to create an interface for the InvoiceUtilities class.

## 3. Program to an interface

1. Review the code and ensure that all references to InvoiceData and InvoiceUtilities are changed to reference their interfaces wherever possible. The only line of code that should reference the implementation of the class is where it is instantiated.

2. Run the code and check it compiles and runs as expected.

## 4. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\101 Starting Spring\01-DeCouplingExample`

## Summary

In this lab we have practiced the principle of loose coupling by adjusting some simple Java code so that we reference interfaces rather than instances of an interface wherever possible.

Having made this change, if we now wanted to replace InvoiceData with a new version that used a real database, note that the only change we would need to make to our code is in the one place where we instantiate InvoiceData.