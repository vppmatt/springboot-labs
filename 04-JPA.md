# Lab 4 - Exploring JPA

## Intro

In this lab we will explore Hibernate outside of Spring, then we’ll later see how to use Hibernate within Spring.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\201 Hibernate`

## 1. Create a new project

1. Create a **new project** in IntelliJ Idea

2. Select **Maven** as the project type

3. Choose the desired version of Java (suggested Java 17)

3. Give the project a **group id**, for example `com.neueda`. This must be entered in lower case and with no spaces.

4. Give the project an **artifact id**, for example `exploring-hibernate`.  This must be entered in lower case and with no spaces.

5. The **project name** should match the artifact id. 

6. Change the **file location** if required.

![new project settings](/images/jpa-project.jpg)

## 2. Add the JPA dependencies

1. Open the **pom.xml** file

2. Edit the pom.xml file to add in the dependencies and build plugin - see the sample pom.xml file in the resources folder of the repo. 

3. Click on **import changes** (the "refresh" icon)

4. Review the external libraries to see what has been added

## 3. Set up the database

1. **Import the database** provided in the resources folder of the repo by running the following command in a command prompt (run this from the folder location where the invoice_manager.sql file can be found.)

```
mysql -uroot –pc0nygre -h127.0.0.1 < invoice_manager.sql
```

## 4. Set up an entity

1. Create a **Customer class** in the project, containing fields that match the database table:
 * id (Long)
 * name (String)
 * address1 (String)
 * address2 (String)
 * address3 (String)
 * address4 (String)
 * country (String)
 * email (String)
 * phone (String)
 
2. Create **getters and setters** for each field.

3. Create a **custom constructor** that allows a customer to be created providing values for all the fields with the exception of the id field.

4. Create an **empty constructor**

5. Create a **toString** method.

6. Annotate the Customer class as an entity using the `@Entity` annotation. 

7. Add a table mapping to specify the table name being used in the database with the `@Table(name="xxx")` annotation. This isn’t actually required as it’s the expected name.

8. Annotate the id field with `@Id` (this makes it the primary key) and with `@GeneratedValue(strategy = GenerationType.IDENTITY)` (to specify it is created by the database for new records).

<details>
<summary>
Expand to view the code sample

</summary>

```
@Entity
@Table(name = "customer")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String address1;
    private String address2;
    private String address3;
    private String address4;
    private String country;
    private String email;
    private String phone;

    public Customer() {
    }


    public Customer(String name, String address1, String address2, String address3, String address4, String country, String email, String phone) {
        this.name = name;
        this.address1 = address1;
        this.address2 = address2;
        this.address3 = address3;
        this.address4 = address4;
        this.country = country;
        this.email = email;
        this.phone = phone;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddress1() {
        return address1;
    }

    public void setAddress1(String address1) {
        this.address1 = address1;
    }

    public String getAddress2() {
        return address2;
    }

    public void setAddress2(String address2) {
        this.address2 = address2;
    }

    public String getAddress3() {
        return address3;
    }

    public void setAddress3(String address3) {
        this.address3 = address3;
    }

    public String getAddress4() {
        return address4;
    }

    public void setAddress4(String address4) {
        this.address4 = address4;
    }

    public String getCountry() {
        return country;
    }

    public void setCountry(String country) {
        this.country = country;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "Customer{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", address1='" + address1 + '\'' +
                ", address2='" + address2 + '\'' +
                ", address3='" + address3 + '\'' +
                ", address4='" + address4 + '\'' +
                ", country='" + country + '\'' +
                ", email='" + email + '\'' +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```
</details>

## 5. Configure JPA

1. **Create a folder** in your project under `resources` called `META-INF` (note this is case sensitive).

2. Copy the provided **persistence.xml** file from the resources folder of the repo into this folder.

3. Edit the file as needed to match our requirements for the project - check the database name, username and password.

## 6. Create and use an EntityManager

1. Create a **new class** called `TestHarness1` with a `public static void main` method.

2. In this method, Create an `EntityManagerFactory` and from this create an `EntityManager` object.

<details>

<summary>
Expand to view the code sample
</summary>

```
EntityManagerFactory factory = Persistence.createEntityManagerFactory("invoiceManagerPersistenceUnit");
EntityManager em = factory.createEntityManager();
```
</details>

3. **Close** the entity manager and factory at the end of the file (note that the code you write in the remainder of this lab will need go BEFORE the objects are closed.)

<details>

<summary>
Expand to view the code sample
</summary>

```
em.close();
factory.close();
```

</details>

4. Create a query to retrieve data:
  * Retrieve all the customers from the database into a List of Customers.
  * Loop through the list printing out the customers to the console

<details>

<summary>
Expand to view the code sample
</summary>

```
TypedQuery<Customer> getAllCustomersQuery = em.createQuery("select c from Customer c", Customer.class);
List<Customer> customers = getAllCustomersQuery.getResultList();

for (Customer customer : customers) {
    System.out.println(customer.toString());
}
```  
</details>

5. Create a query to retreive a single customer:

  * Find a customer with a specific ID
  * print that customer out to the console

<details>

<summary>
Expand to view the code sample
</summary>

```
Customer customer81 = em.find(Customer.class, 81L);
System.out.println("Customer 81 is " + customer81);
```
</details>

6. Find all the customers (there is just 1) whose phone number starts with +33 and print that customer out to the console. 

<details>

<summary>
Expand to view the code sample
</summary>

```
TypedQuery<Customer> getSpecificCustomerQuery =
    em.createQuery("select c from Customer c where c.phone like '+33%'", Customer.class);
Customer foundCustomer = getSpecificCustomerQuery.getSingleResult();
System.out.println("Customer " + foundCustomer );
```

</details>

7. Create a new customer – use any values you like for the name, address, email etc. If any values are blank, use nulls. Note that for a NEW customer the database will generate the ID so set the ID to null. Don’t forget to use a transaction.

<details>

<summary>
Expand to view the code sample
</summary>

```
EntityTransaction tx = em.getTransaction();
tx.begin();
Customer newCustomer = new Customer("Fast Trains Ltd","10 Any Road","Any Town", null, "AB1 2CD", "UK", "someemail@notvalid.com", "+44 123456789");
em.persist(newCustomer);
tx.commit();

System.out.println("The new customer was given an ID of " + newCustomer.getId());
```

</details>

8. If you wish, practice updating and deleting a customer (optional). 

<details>

<summary>
Expand to view the code sample
</summary>

```
tx.begin();
newCustomer.setName("Slow Trains Ltd");
tx.commit();

tx.begin();
em.remove(newCustomer);
tx.commit();
```

</details>

## 7. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\201 Hibernate`

## Summary

In this lab we have:

* Created a project with the **JPA** dependency
* **Configured JPA** using an xml file. 
* Written regular **JPA code** to interact with a database.