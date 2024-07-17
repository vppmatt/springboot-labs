# Lab 9 part 2 - Spring and Rest

## Intro

In this lab we will start to create the main project for this course - we'll be constructing a REST API.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\402 Spring and Rest`

## 1. Open the sample application

1. **Open the project** in the resources section of this module. This contains:
  * A `Payment` entity in the model package
  * A `PaymentRepository` in the repositories package
  * The `Healthcheck` Rest end point. 
  * A `PaymentsService` interface and class which exposes some methods for managing payments
  * A `BootstrapService` : this is a `@Service` class, so Spring will instantiate it at the project start. It contains a method annotated `@PostConstruct` – this will be executed by Spring after all objects have been created.  The method will seed the database with some data.

2. Edit the `application.properties` file to **configure the database connection**. Check the database name, username and password.
 
3. **Run the application** and visit http://localhost:8080/health in your browser (or do curl –i http://localhost:8080/health ) to check it is working.

## 2. Create a REST endpoint

1. **Create a new controller class** called `PaymentsController`.

2. **Provide a rest end point** at `/api/payment` which returns a list of all payments in the system

3. Because all methods in this class will be at endpoints which start at /api/payments, **add a `@RequestMapping` annotation** to the class to specify this end point.

4. Ensure the methods can return data with **no CORS issues** if called from Javascript running in a browser.

5. **Use Constructor injection** to get an instance of the PaymentService (this is considered best practice)

6. Use a Curl command or the curlGenerator html page to **test the Rest EndPoint**

## 3. Add the Swagger Dependency

1. Add the **Swagger** Dependency to the project's pom.xml file.

```
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.6.0</version>
</dependency>
```

2. **Refresh the Maven dependencies** and restart the application

3. Visit http://localhost:8080/swagger-ui/index.html

## 4. Use a Path Variable

1. **Create a method** in the `PaymentsService` and its implementation to retrieve a single product by using its id.

2. Note that the automatically included repository method `.findById()` returns an `Optional<Product>` - this is because there may not be a product with the ID you specified. Use this method in your implementation, and for now return a null if there’s no matching payment found. 

3. Create a **new rest endpoint** with the mapping `/api/payment/{id}`

4. If no product was found, return null for now

<details>

<summary>
Expand to view the code sample
</summary>

```
@GetMapping("/{id}")
public Payment getPaymentById(@PathVariable("id") Long id) {
   return paymentsService.getById(id);
}
```

</details>

5. Try running the CURL command from the command line to get the data from url http://localhost:8080/api/payment/194

```
curl -i http://localhost:8080/api/payment/194
```

## 5. Use a Request Parameter

1. **Create a method** in the `PaymentsController` class which will find all payments with a particular country value.

2. This method should map against the url: /api/payment/search?country=xxx, where xxx is the country. 

NOTE this is not quite the best URL to use / doesn’t match the REST standards but we’ll fix that shortly!

3. Create all the required methods to implement this feature. This will require an additional method in the repostory!

4. Try running the CURL command from the command line to get the data:

```
curl –i http://localhost:8080/api/payment/search?country=irl
```

## 6. Overloading methods

1. Amend the code we have written to **provide the following endpoints** (the last one is new!):

  | endpoint | description |
  | --- | --- |
  | /api/payment | get all payments |
  | /api/payment/xxx | get payment with id xxx |
  | /api/payment?country=xxx |get all payments containing a country of xxx |
  | /api/payment?orderId=xxx | get all products containing an orderId of xxx |

Hint: To mark a request parameter as optional we can use the syntax:
  `@RequestParam(value="paramName" required=false) String paramName)`

<details>

<summary>
Expand to view the code sample
</summary>

```
@GetMapping
public List<Payment> getPayments(@RequestParam(value="country", required = false) String country, @RequestParam(value="orderId", required = false) String OrderId) {
  if (country != null) {
    return paymentsService.getAllByCountry(country);
  }
  if (OrderId != null) {
    return paymentsService.getAllByOrderId(OrderId);
  }
  return paymentsService.getAllPayments();
}
```    

</details>

2. Try running the CURL command from the command line to check:

```
curl -i http://localhost:8080/api/payment
curl -i http://localhost:8080/api/payment/126
curl -i http://localhost:8080/api/payment?country=irl
curl -i http://localhost:8080/api/payment?orderId=21216121
```

## 7. Implement XML data

1. **Add the dependency** to the pom.xml file:

```
<dependency>
  <groupId>com.fasterxml.jackson.dataformat</groupId>
  <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

2. Add the option to all Payment endpoints to **return XML or JSON** – put JSON as the first option to make it the default:

<details>

<summary>
Expand to view the code sample
</summary>

```
@GetMapping(produces={MediaType.APPLICATION_JSON_VALUE , MediaType.APPLICATION_XML_VALUE})

@GetMapping(value="/{id}", produces= {MediaType.APPLICATION_JSON_VALUE , MediaType.APPLICATION_XML_VALUE})
```

</details>

3. Try running a curl command to see what is returned:

```
curl -i http://localhost:8080/api/payment/126
```

4. Include a header request in the curl command to ask for a specific data type:

```
curl -i -H "Accept: application/xml" http://localhost:8080/api/payment/126
```

## 8. Return valid formats

1. Our **Healthcheck** end point is currently returning a String – amend this to return a map containing a key of "status" and a value of "ok". 

Test this from the curlGenerator page or by using a curl command.

## 9. Receiving data at the server

1. **Create a function** with the `@PostMapping` annotation to receive a new product and save it in the database.

2. The function should **return the full payment object**.

3. **Test the command** using Swagger or the CurlGenerator

<details>

<summary>
Expand to view the code sample
</summary>

```
@PostMapping
public Payment savePayment(@RequestBody Payment payment) {
  return paymentsService.save(payment);
}
```

</details>

## 10. Handle errors with Status codes

1. Repeat the last **CURL post** but use a string for the amount – it will fail due to a type mismatch (cannot deserialize a String to a Double)

2. Add the following to `application.properties` to prevent the stack trace and Java error being sent to the client:

```
server.error.include-stacktrace=on_param
server.error.include-exception=false
```

3. Run the application again and see what happens with the invalid new payment. 

4. Try adding ?trace=true to the end of the URL 

5. If you **try a GET request** for a payment by id, and there is no matching payment the application currently returns a 200 status code with a null value – let’s fix this!

6. Create a custom exception class and annotate it with `@ResponseStatus(code = HttpStatus.NOT_FOUND)`

7. Edit the method that retrieves a payment by ID to use this exception if no matching payment is found. 

## 11. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\402 Spring and rest`


## Summary

In this lab we have:

* Built a rest api
* implemented parameters in 2 different ways
* Created custom response codes using exceptions