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

![Tape-image]("http://resources/registration.png")

## Account service (Backend)

The account service is a service that provides a RESTful server to a repository of accounts. It's launched with the command:

```bash
./gradlew accounts:bootRun
```

## Web service (Frontend)

The web service is a service that provides an MVC front-end to the application of accounts. It's launched with the command:

```bash 
./gradlew web:bootRun
```

The web is exposed in `http://localhost:3333`.