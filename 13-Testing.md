# Lab 13 - Testing

## Intro

In this lab we will continue working on the main project for this course.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot. 

You should continue to work on the project you built in the previous lab. If you need to, you can use the sample proejct from the repository in the folder:

`springboot\end of section workspaces\502 AOP`

## 1. Create Unit Tests

Create the following unit tests:

1. Test that 2 Payments will be equal if they have the same ID (note this test will fail so you'll need to implement a fix!). 

Note that this doesn’t have any Spring dependencies so can be created as a regular Junit test with no extra annotations required.

<details>

<summary>
Expand to view the code sample

</summary>

```
public class DomainClassTests {

    @Test
    public void testEqualityOfPaymentsUsingIDOnly() {
        Payment p1 = new Payment();
        p1.setId(17L);
        Payment p2 = new Payment();
        p2.setId(17L);
        assertEquals(p1,p2);
    }

}

```
</details>


3. Test the `getById` method of the PaymentsService class throws an error when the payment isn’t found. You'll need to mock the repository and return an empty optional.

Note – for this test, you need to annotate your test class with:


```
@SpringBootTest
@ExtendWith(SpringExtension.class)
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
```

You also need to mock all other Service classes or the application won’t start, and you should also mock the Apsect classes so that these don’t run in your tests.

<details>

<summary>
Expand to view the code sample
</summary>

```
@SpringBootTest
@ExtendWith(SpringExtension.class)
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
public class ServiceClassTests {

    @Autowired
    PaymentsService paymentsService;

    @MockBean
    private PaymentsRepository paymentsRepository;

    @MockBean
    private UserService userService;

    @MockBean
    BootstrapService bootstrapService;

    @MockBean
    private RepoLoggingAspect repoLoggingAspect;

    @MockBean
    private ServiceLoggingAspect serviceLoggingAspect;

    @BeforeEach
    public void setup() {
        Mockito.when(paymentsRepository.findById(123L)).thenReturn(Optional.empty());
    }

    @Test
    public void testThatNoMatchingPaymentReturnsAnError() {
        assertThrows(NoMatchingPaymentException.class, () -> {
            paymentsService.getById(123L);
        });
    }
}
```
</details>
   

## 2. Test a controller using TDD techniques

1. Using TDD practices, create a controller that exposes a Get method on the /api/country endpoint. It should return a list of countries – this should be a unique list of countries, taken from the Payment database, and sorted alphabetically.

2. Create unit tests for any service or controller methods you create.

<details>

<summary>
Expand to view the code sample
</summary>

Additional method in ServiceClassTests:
```
    @Test
    public void testThatUniqueCountriesAreReturnedFromService() {

        Payment p1 = new Payment();
        p1.setCountry("CAN");

        Payment p2 = new Payment();
        p2.setCountry("USA");

        Payment p3 = new Payment();
        p3.setCountry("IRL");

        Payment p4 = new Payment();
        p4.setCountry("FRA");

        Payment p5 = new Payment();
        p5.setCountry("FRA");

        Payment p6 = new Payment();
        p6.setCountry("CAN");

        Mockito.when(paymentsRepository.findAll()).thenReturn(List.of(p1,p2,p3,p4,p5,p6));
        assertEquals(List.of("CAN", "FRA", "IRL", "USA"), paymentsService.getCountries());
    }
```

```
@ExtendWith(SpringExtension.class)
@SpringBootTest
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
public class ControllerTests {

    private CountryController countryController;

    @Autowired
    public ControllerTests(CountryController countryController) {
        this.countryController = countryController;
    }

    @MockBean
    private PaymentsService paymentsService;

    @MockBean
    private UserService userService;

    @Test
    public void testGetAllCountries() {
        Mockito.when(paymentsService.getCountries()).thenReturn(List.of("FRA","GBR", "USA"));
        List<String> result = countryController.getCountries();
        assertTrue(result.size() == 3);


    }
}
```

</details>

## 3. Integration tests

1. Create an integration test to check that when a post is made to submit a payment then the payment is saved to the database. 

2. You should verify that the repository save method was called, and that the orderId of the payment was passed through to the repository.

3. Note that you will need to mock various other objects for this to work - all other services (eg UserService, BootstrapService) and the Aspect classes.

<details>

<summary>
Expand to view the code sample
</summary>

```
@SpringBootTest
@ExtendWith(SpringExtension.class)
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
@AutoConfigureMockMvc
public class IntegrationTests {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    PaymentsRepository paymentsRepository;

    @MockBean
    UserService userService;

    @MockBean
    RepoLoggingAspect repoLoggingAspect;

    @MockBean
    ServiceLoggingAspect serviceLoggingAspect;

    @MockBean
    BootstrapService bootstrapService;

    @Test
    public void testThatPostingANewTransactionGetsAddedToTheDatabase() throws Exception {

        Payment payment = new Payment();
        payment.setOrderId("223344");
        payment.setCountry("USA");
        payment.setAmount(17.33);

        ObjectMapper om = new ObjectMapper();
        String json = om.writeValueAsString(payment);

        mockMvc.perform(post("/api/payment")
                        .contentType("application/json")
                        .content(json))
                .andExpect(status().isOk());

        ArgumentCaptor<Payment> capturedTransaction = ArgumentCaptor.forClass(Payment.class);

        Mockito.verify(paymentsRepository).save(capturedTransaction.capture());
        assertEquals("223344", capturedTransaction.getValue().getOrderId());

    }

}
```
</details>
 

## 4. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\601 Testing`


## Summary

In this lab we have:

* created various types of tests
* used Mockito to mock different layers of our application
* used MockMVC to simulate a user action