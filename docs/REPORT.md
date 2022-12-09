# Lab 6 report

In this project we have three apps:

- **Service discovery** (`registration` written in Kotlin):
  It launches an open source discovery server called `Eureka`
- **Account service** (`accounts` written in Kotlin):
  It is a standalone process that provides a RESTful server to a repository of accounts that will use the port 2222.
- **Web service** (`web` written in Java):
  It is a standalone process that provides an MVC front-end to the application of accounts that will use the port 3333.

## Registration service (Eureka server)

The registration server is a service to register the services that are running in the system. It's launched with the command:

```bash
./gradlew registration:bootRun
```

The dashboard of the registration server is exposed in `http://localhost:1111`.

![term-registration](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-registration.png?raw=true "Registration app running")

## Account service (Backend)

The account service is a service that provides a RESTful server to a repository of accounts. It's launched with the command:

```bash
./gradlew accounts:bootRun
```

![term-accounts-2222](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-accounts-2222.png?raw=true "Accounts app running on 2222 port")

And we can see how the backend account service is registered in the Eureka registration server:

![eureka-accounts](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/eureka-accounts-2222.png?raw=true "Eureka with accounts registered")

## Web service (Frontend)

The web service is a service that provides an MVC front-end to the application of accounts. It's launched with the command:

```bash 
./gradlew web:bootRun
```

![term-web](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-web.png?raw=true "web app running")

The web is exposed in `http://localhost:3333`.

And we can see again how the web service is registered in the Eureka registration server:

![eureka-web](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/eureka-web.png?raw=true "Eureka with web registered")

## Adding another account service

We can add another account service to the system. But first we need to change the port of the account service to another, for example 4444. We can do this in the `aplication.yaml` file of the account service:

```yaml
spring:
  application:
    name: accounts-service  # Identify this application

# HTTP Server
server:
  port: 4444   # HTTP (Tomcat) port
```

Then we can launch the new account service with the command:

```bash
./gradlew accounts:bootRun
```

![term-accounts-4444](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-accounts-4444.png?raw=true "Accounts app running on 4444 port")

And we can see how the new account service is registered in the Eureka registration server:

![eureka-accounts-4444](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/eureka-accounts-2222-4444.png?raw=true "Eureka with accounts on 4444 registered")

Now if we make a request from the web service (for example fetch an account) we can see that is using the account service in the port 2222:

![term-web-account-2222](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-web-account-222.png?raw=true "Account service in 2222 port received the request")

## Stopping the account service in the port 2222

We kill the account service in the port 2222 with `ctrl` `c`

![term-accounts-2222-killed](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-accounts-2222-killed.png?raw=true "Accounts on 2222 port killed")

And we can see in Eureka how the account service in the port 2222 is not registered anymore:

![eureka-accounts-2222-killed](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/eureka-accounts-2222-killed.png?raw=true "Accounts on 2222 port killed")

If we make a request from the web service (for example fetch an account) we can see that is using the account service in the port 4444, and it's still working:

![term-web-account-4444](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/term-web-account-4444.png?raw=true "Account service in 4444 port received the request")

We can see how the request has worked:

![get-account-4444](https://github.com/pikanachi/lab6-microservices/blob/work/docs/resources/get-account-4444.png?raw=true "Get account in 4444 port")

This happens because the web server searches for the account service in the Eureka server (where all the services are registered), and it finds the account service in the port 4444 so everything is still working even thought we have killed the accounts service on port 2222.