# Spring Security

## Introduction
1. Firstly, the `spring-boot-starter-security` needs to be added to the project. Example: `implementation 'org.springframework.boot:spring-boot-starter-security:3.3.0'`
2. Once the dependency is added, without the developer doing anything, Spring Security puts all the APIs behind a login (`/login` endpoint)
3. It generates a random password (it's different on every run) which is displayed in the run log. The default user is `user`. You can customize this in application properties if needed

## How it works out of the box?
1. How Spring Security does all of this (above mentioned steps) without developer intervention is by using `Filter`
2. Think of filters as layers that sit in between the incoming requests and their implementations. Filters intercept incoming requests
3. When Spring Security dependency is added, it adds its own filter called the `DelegatingFilterProxy`. If you're not working with a Spring Boot app, you'll need to add this filter manually
4. The `DelegatingFilterProxy` doesn't do the filtering itself. Instead, it delegates the task to filter classes (even the customly created ones) which do the filtering.
5. For us to create a custom filter, we need to create a Spring Bean which inherits the `jakarta.Servlet.Filter` interface

## Authentication
1. One of the ways in which authentication can be done is by using `Providers`
2. `AuthenticationProvider` specifically is what does the authentication. It's an interface with an [`authenticate()`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authentication/AuthenticationProvider.html#authenticate(org.springframework.security.core.Authentication)) method
3. Signature of the method: `Authentication authenticate(Authentication authentication)`
4. The `authenticate()` method takes in the `Authentication` object with credentials and returns the `Authentication` object with the principal (which is the currently logged in user)
5. That said, there are different ways in which a user can authenticate himself in an application. They might use form login, SSO, OAuth, etc. So, we can have multiple `AuthenticationProvider` instances in our application
6. To clarify which type of authentication an `AuthenticationProvider` supports, you need to override the [`supports()`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authentication/AuthenticationProvider.html#supports(java.lang.Class)) method

### How does Spring know which `AuthenticationProvider` to use?
1. There is an `AuthenticationManager` interface in Spring which helps with this
2. You'd need to create a custom class which implements the `AuthenticationManager` interface. This custom class would delegate the authentication to the appropriate `AuthenticationProvider`

### Retrieving the user data
1. Regardless of the type of authentication (OAuth, SSO, form login, etc), it's a very common pattern to get the user the username/email during authentication and do a lookup in some sort of storage (database, file, etc) and verify that the user exists and if it does, retrieve the user.
2. Spring Security found this common pattern and extracted it into an interface called `UserDetailsService`.
3. The sole purpose of this interface is to load a user object given a user identifier (like username). We need to implement the [`loadUserByUserName()`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/UserDetailsService.html#loadUserByUsername(java.lang.String)) method to search and retrieve the user object. 
4. Once the user object is retrieved, it is send back to the `AuthenticationProvider` and the authentication check is done


**Reference**: https://www.youtube.com/watch?v=caCJAJC41Rk
<img width="973" alt="Screenshot 2024-06-11 at 10 28 15 PM" src="https://github.com/shahlin/notes/assets/32275018/155b2222-2f1d-4aa1-b751-81c491de28a3">
