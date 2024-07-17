# Lab 12 - Aspected Oriented Programming

## Intro

In this lab we will continue working on the main project for this course.

## Pre-requisites

Before starting this lab, **you should have cloned the repository for the Spring Boot course** from https://github.com/vppmatt/springboot.  The code files needed for this lab can be found in this folder inside the repository:

`springboot\resources\502 AOP`

You should continue to work on the project you built in the previous lab. If you need to, you can use the sample proejct from the repository in the folder:

`springboot\end of section workspaces\501 Logging`

## 1. Create an Aspect

1. **Remove the logging code** we added previously to the service class

2. Create a new **package** called `aspects`

3. Create a new **class** called `ServiceLoggingAspect` and annotate it with the `@Aspect` annotation and the `@Component` annotation

<details>

<summary>
Expand to view the code sample
</summary>

```
@Component
@Aspect
public class ServiceLoggingAspect { 
}
```

</details>

## 2. Create a Pointcut

1. In the `ServiceLoggingAspect` class, **create a pointcut** called `logServiceMethods`.

2. Set the pointcut to be executed on any method in the service package.

<details>

<summary>
Expand to view the code sample
</summary>

```
@Pointcut("execution(* com.neueda.payments.service.*.* (..))")
public void logServiceMethods() {}
```

</details>

## 3. Create an advice

1. In the `ServiceLoggingAspect` class, **create 2 advices** that will run with the `logServiceMethods` pointcut.

2. The first advice should run **before** the method and the second should run **after**.

3. In each advice **log at info level** the phrase "Starting a method" or "Ending a method"

<details>

<summary>
Expand to view the code sample
</summary>

```
@Before("logServiceMethods()")
public void beforeLogServiceMethods() {
  logger.info("Starting a method");
}

@After("logServiceMethods()")
public void afterLogServiceMethods() {
  logger.info("Ending a method");
}
```

</details>

## 4. Create more advice

1. **Update the two advice methods** that log the calls to the methods in the service package to log the method name and the parameters supplied to that method.

<details>

<summary>
Expand to view the code sample
</summary>

```
private String getArgsAsString(Object[] args) {
  try {
    String[] stringArgs = Arrays.copyOf(args, args.length, String[].class);
    return String.join(",", stringArgs);
  }
  catch (Exception e) {
    return "[unknown]";
  }
}

@Before("logServiceMethods()")
public void beforeLogServiceMethods(JoinPoint joinPoint) {
  logger.info("Starting method {} with arguments {}" , joinPoint.getSignature().getName() , getArgsAsString(joinPoint.getArgs()));
}

@After("logServiceMethods()")
public void afterLogServiceMethods(JoinPoint joinPoint) {
  logger.info("Ending method {} with arguments {}" , joinPoint.getSignature().getName() , getArgsAsString(joinPoint.getArgs()));
}
```

</details>

3. **Create a new pointcut** to be applied to all methods in all classes in the repository package - do this in a class called `RepoLoggingAspect`

4. **Create an around advice** to apply to this pointcut, logging the names of each method as they are entered and exited. 

5. Ensure you capture the return value of the proceedingJoinPoint and return it from the advice method (the advice method should return an object of type Object).

<details>

<summary>
Expand to view the code sample
</summary>

```
@Component
@Aspect
public class RepoLoggingAspect {

    Logger logger = LoggerFactory.getLogger(RepoLoggingAspect.class);

    @Pointcut("execution(* com.neueda.payments.repositories.*.* (..))")
    public void logRepoMethods() {}

    @Around("logRepoMethods()")
    public Object aroundLogRepoMethods(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        logger.info("Entering repository method {}", proceedingJoinPoint.getSignature().getName());
        Object returnValue = proceedingJoinPoint.proceed();
        logger.info("Exiting repository method {}", proceedingJoinPoint.getSignature().getName());
        return returnValue;
    }
}
```

</details>

## 5. Review the sample solution

A sample solution for this challenge can be found in the course repository at 

`springboot\end of section workspaces\502 AOP`


## Summary

In this lab we have:

* Used Aspect Oriented Programming functionality to inject extra code into our application