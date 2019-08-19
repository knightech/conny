# Spring Boot Microservices with Kubernetes

This example shows how to create a microservices architecture and deploy it with Kubernetes. 

Please see [Build a Microservice Architecture with Spring Boot and Kubernetes](https://developer.okta.com/blog/2019/04/01/spring-boot-microservices-with-kubernetes) for a tutorial that shows you how to build this example. 

**Prerequisites:** [Java 8](https://adoptopenjdk.net/).

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

* [Getting Started](#getting-started)
* [Help](#help)
* [Links](#links)
* [License](#license)

## Getting Started

To install this example application, run the following commands:

```bash
git clone https://github.com/oktadeveloper/okta-spring-boot-microservice-kubernetes.git
cd okta-spring-boot-microservice-kubernetes
```

### Create an Okta Developer Account

If you don't have one, [create an Okta Developer account](https://developer.okta.com/signup/). After you've completed the setup process, log in to your account and navigate to **Applications** > **Add Application**. Click **Web** and **Next**. On the next page, enter a name for your app (e.g., "Spring Boot GKE"), add `https://oidcdebugger.com/debug` as a Login redirect URI, select `Implicit (hybrid)` under Grant types,  and click **Done**. 

Copy your issuer (found under **API** > **Authorization Servers**) into `src/main/resources/application.properties`.

```properties
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://{yourOktaDomain}/oauth2/default
````

Start the Spring Boot app using the following command:

```bash
./gradlew bootRun
```

Go to the [OIDC Debugger](http://oidcdebugger.com/). You'll need your Client ID from your Okta OIDC application.

* Fill in the Authorize URI: https://{yourOktaDomain}/oauth2/default/v1/authorize
* Fill in your Client ID.
* Put `abcdef` for the state.
* At the bottom, click **Send Request**.

Copy the generated token, and store it in a shell variable for convenience:

```bash
TOKEN=eyJraWQiOiI4UlE5REJGVUJOTnJER0VGaEExekd6bWJqREp...
```

Run a GET on the `/kayaks` endpoint with the token:

```bash
http :8080/kayaks Authorization:"Bearer $TOKEN"
```

**NOTE the double quotes!** Single quotes won’t work because the variable won’t be expanded in the string.

You should get:

```bash
HTTP/1.1 200 OK
cache-control: no-cache, no-store, max-age=0, must-revalidate
content-type: application/json;charset=UTF-8
...
[
  {
    "makeModel": "NDK",
    "name": "sea",
    "owner": "Andrew",
    "value": 300.12
  },
  {
    "makeModel": "Piranha",
    "name": "creek",
    "owner": "Andrew",
    "value": 100.75
  },
  {
    "makeModel": "Necky",
    "name": "loaner",
    "owner": "Andrew",
    "value": 75
  }
]
```


## Links

This example uses the following open source projects:

* [Kubernetes](https://kubernetes.io/)
* [Spring Boot](https://spring.io/projects/spring-boot)
* [Spring Security](https://spring.io/projects/spring-security)

## Help

Please post any questions as comments on this repo's [blog post](https://developer.okta.com/blog/2019/04/01/spring-boot-microservices-with-kubernetes), or visit our [Okta Developer Forums](https://devforum.okta.com/). 

## License

Apache 2.0, see [LICENSE](LICENSE).
