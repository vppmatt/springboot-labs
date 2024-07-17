# Lab 7 - Spring Boot Basics

## Intro

In this lab we will explore some of the basic fundamentals of Spring Boot applications, and understand some of the Spring conventions.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  

You will be working in the spring-jpa-demo project you created in the last lab. If needed, you can copy this project from the repo at:

`springboot\end of section workspaces\203 Spring Data`

## 1. Dependency Injection

1. Create a **new class** called `CustomerService` in a **package** called `service`.

2. **Annotate** this class with `@Component` so that Spring will instantiate it.

3. Create a **private class level variable** of type `CustomerRepository` - try out the different syntaxes for dependency injection:

FIELD INJECTION:
```
@Autowired
private CustomerRepository customerRepository;
```

SETTER INJECTION:
```
private CustomerRepository customerRepository;

@Autowired
public void setCustomerRepository(CustomerRepository customerRepository) {
  this.customerRepository = customerRepository;
}
```

CONSTRUCTOR INJECTION:
```
private CustomerRepository customerRepository;

public CustomerService(CustomerRepository customerRepository) {
   this.customerRepository = customerRepository;
}
```

4. **Create methods** in this class to retrieve all the customers, to find all customers by country, find a customer by id, and to save a customer.

```
public List<Customer> findAll() {
   return customerRepository.findAll();
}

public Customer findById(int id) {
  return customerRepository.findById(id).get();
}

public List<Customer> findAllByCountry(String country) {
  return customerRepository.findAllByCountry(country);
}

public Customer save(Customer customer) {
  return customerRepository.save(customer);
}
```

5. Edit the code in the main method to use the new CustomerService class

```
CustomerService customerService = context.getBean(CustomerService.class);
```


## 2. Applying Spring Boot conventions

1. **Replace the annotation** on the CustomerService class with `@Service`.

2. This should be decoupled, so **refactor it to implement an interface**, then adjust the code in our main function to use the interface

3. Run the application and check the code still works

**Note that we have written code in the main function just to explore / learn how Spring works – we won’t do that in a real application – instead we will have controllers!**

4. Ensure the 2 Repository classes are annotated with `@Repository` instead of `@Component`

## 3. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\301 Spring Boot basics`

## Summary

In this lab we have:

* Seen how to use the correct Spring annotations for different classes that Spring will instantiate
* Explored the different sytnaxes for dependency injection 
* Understood the correct way to structure a typcial Spring project. 