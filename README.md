This guide walks you through creating a "hello world" [RESTful web service][u-rest] with Spring.

What you'll build
-----------------

The service will accept HTTP GET requests at:

    http://localhost:8080/greeting

and respond with a [JSON][u-json] representation of a greeting:

```json
{"id":1,"content":"Hello, World!"}
```

You can customize the greeting with an optional `name` parameter in the query string:

    http://localhost:8080/greeting?name=User

The `name` parameter value overrides the default value of "World" and is reflected in the response:

```json
{"id":1,"content":"Hello, User!"}
```


What you'll need
----------------

 - About 15 minutes
 - A favorite text editor or IDE
 - [JDK 6][jdk] or later
 - [Maven 3.0][mvn] or later

[jdk]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[mvn]: http://maven.apache.org/download.cgi


How to complete this guide
--------------------------

Like all Spring's [Getting Started guides](/guides/gs), you can start from scratch and complete each step, or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

To **start from scratch**, move on to [Set up the project](#scratch).

To **skip the basics**, do the following:

 - [Download][zip] and unzip the source repository for this guide, or clone it using [git](/understanding/git):
`git clone https://github.com/springframework-meta/gs-rest-service.git`
 - cd into `gs-rest-service/initial`.
 - Jump ahead to [Create a resource representation class](#initial).

**When you're finished**, you can check your results against the code in `gs-rest-service/complete`.
[zip]: https://github.com/springframework-meta/gs-rest-service/archive/master.zip


<a name="scratch"></a>
<<<<<<< HEAD
## Set up the project

=======
Set up the project
------------------
>>>>>>> Regenerate README.md based on updates

First you set up a basic build script. You can use any build system you like when building apps with Spring, but the code you need to work with [Maven](https://maven.apache.org) and [Gradle](http://gradle.org) is included here. If you're not familiar with either, refer to [Building Java Projects with Maven](/guides/gs/maven) or [Building Java Projects with Gradle](/guides/gs/gradle/).

### Create the directory structure

In a project directory of your choosing, create the following subdirectory structure; for example, with `mkdir -p src/main/java/hello` on *nix systems:

    └── src
        └── main
            └── java
                └── hello

### Create a Maven POM

`pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-rest-service</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>0.5.0.BUILD-SNAPSHOT</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>http://repo.springsource.org/libs-snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>http://repo.springsource.org/libs-snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
    </pluginRepositories>
</project>
```

This guide is using [Spring Boot's starter POMs](/guides/gs/spring-boot/).

Note to experienced Maven users who are unaccustomed to using an external parent project: you can take it out later, it's just there to reduce the amount of code you have to write to get started.


<a name="initial"></a>
<<<<<<< HEAD
## Create a resource representation class
=======
Create a resource representation class
--------------------------------------
>>>>>>> Regenerate README.md based on updates

Now that you've set up the project and build system, you can create your web service.

Begin the process by thinking about service interactions.

The service will handle `GET` requests for `/greeting`, optionally with a `name` parameter in the query string. The `GET` request should return a `200 OK` response with JSON in the body that represents a greeting. It should look something like this:

```json
{
    "id": 1,
    "content": "Hello, World!"
}
```

The `id` field is a unique identifier for the greeting, and `content` is the textual representation of the greeting.

<<<<<<< HEAD
To model the greeting representation, you create a _resource representation class_. To do this, you simply create a plain old java object with fields, constructors, and accessors for the `id` and `content` data:
=======
To model the greeting representation, you create a resource representation class. Provide a plain old java object with fields, constructors, and accessors for the `id` and `content` data:
>>>>>>> Regenerate README.md based on updates

`src/main/java/hello/Greeting.java`
```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```

<<<<<<< HEAD
> **Note:** As you'll see in steps below, Spring will use the _Jackson_ JSON library to automatically marshal instances of type `Greeting` into JSON.
=======
> **Note:** As you see in steps below, Spring uses the [Jackson JSON][jackson] library to automatically marshal instances of type `Greeting` into JSON.
>>>>>>> Regenerate README.md based on updates

Next you create the resource controller that will serve these greetings.


Create a resource controller
------------------------------

<<<<<<< HEAD
In Spring's approach to building RESTful web services, HTTP requests are handled by a _controller_. These components are easily identified by the [`@Controller`][] annotation, and the `GreetingController` below handles `GET` requests for `/greeting` by returning a new instance of the `Greeting` class:
=======
In Spring's approach to building RESTful web services, HTTP requests are handled by a controller. These components are easily identified by the [`@Controller`][] annotation, and the `GreetingController` below handles `GET` requests for `/greeting` by returning a new instance of the `Greeting` class:
>>>>>>> Regenerate README.md based on updates

`src/main/java/hello/GreetingController.java`
```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public @ResponseBody Greeting greeting(
            @RequestParam(value="name", required=false, defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```

This controller is concise and simple, but there's plenty going on under the hood. Let's break it down step by step.

The `@RequestMapping` annotation ensures that HTTP requests to `/greeting` are mapped to the `greeting()` method.

<<<<<<< HEAD
> **Note:** The above example does not specify `GET` vs. `PUT`, `POST`, and so forth, because `@RequestMapping` maps _all_ HTTP operations by default. Use `@RequestMapping(method=GET)` to narrow this mapping.
=======
> **Note:** The above example does not specify `GET` vs. `PUT`, `POST`, and so forth, because `@RequestMapping` maps all HTTP operations by default. Use `@RequestMapping(method=GET)` to narrow this mapping.
>>>>>>> Regenerate README.md based on updates

`@RequestParam` binds the value of the query string parameter `name` into the `name` parameter of the `greeting()` method. This query string parameter is not `required`; if it is absent in the request, the `defaultValue` of "World" is used.

The implementation of the method body creates and returns a new `Greeting` object with `id` and `content` attributes based on the next value from the `counter`, and formats the given `name` by using the greeting `template`.

A key difference between a traditional MVC controller and the RESTful web service controller above is the way that the HTTP response body is created. Rather than relying on a view technology (such as [JSP][u-jsp]) to perform server-side rendering of the greeting data to HTML, this RESTful web service controller simply populates and returns a `Greeting` object. The object data will be written directly to the HTTP response as JSON.

To accomplish this, the [`@ResponseBody`][] annotation on the `greeting()` method tells Spring MVC that it does not need to render the greeting object through a server-side view layer, but that instead that the greeting object returned _is_ the response body, and should be written out directly.

<<<<<<< HEAD
The `Greeting` object must be converted to JSON. Thanks to Spring's _HTTP message converter_ support, you don't need to do this conversion manually. Because [Jackson 2][jackson] is on the classpath, Spring's [`MappingJackson2HttpMessageConverter`][] is automatically chosen to convert the `Greeting` instance to JSON.
=======
The `Greeting` object must be converted to JSON. Thanks to Spring's HTTP message converter support, you don't need to do this conversion manually. Because [Jackson 2][jackson] is on the classpath, Spring's [`MappingJackson2HttpMessageConverter`][] is automatically chosen to convert the `Greeting` instance to JSON.
>>>>>>> Regenerate README.md based on updates


Make the application executable
-------------------------------

<<<<<<< HEAD
Although it is possible to package this service as a traditional _web application archive_ or [WAR][u-war] file for deployment to an external application server, the simpler approach demonstrated below creates a _standalone application_. You package everything in a single, executable JAR file, driven by a good old Java `main()` method. And along the way, you use Spring's support for embedding the [Tomcat][u-tomcat] servlet container as the HTTP runtime, instead of deploying to an external instance.

### Create a main class
The `main()` method defers to the [`SpringApplication`][] helper class, providing `Application.class` as an argument to its `run()` method. This tells Spring to read the annotation metadata from `Application` and to manage it as a component in the _[Spring application context][u-application-context]_.
=======
Although it is possible to package this service as a traditional [WAR][u-war] file for deployment to an external application server, the simpler approach demonstrated below creates a standalone application. You package everything in a single, executable JAR file, driven by a good old Java `main()` method. Along the way, you use Spring's support for embedding the [Tomcat][u-tomcat] servlet container as the HTTP runtime, instead of deploying to an external instance.

### Create an Application class
>>>>>>> Regenerate README.md based on updates

`src/main/java/hello/Application.java`
```java
package hello;

import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.SpringApplication;
import org.springframework.context.annotation.ComponentScan;

@ComponentScan
@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

<<<<<<< HEAD
<<<<<<< HEAD
```java
package hello;

import org.springframework.bootstrap.SpringApplication;
import org.springframework.bootstrap.context.annotation.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;

@ComponentScan
@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
=======
The `main()` method defers to the [`SpringApplication`][] helper class, providing `Application.class` as an argument to its `run()` method. This tells Spring to read the annotation metadata from `Application` and to manage it as a component in the _[Spring application context][u-application-context]_.

>>>>>>> Refactor to new {!include} syntax
=======
The `main()` method defers to the [`SpringApplication`][] helper class, providing `Application.class` as an argument to its `run()` method. This tells Spring to read the annotation metadata from `Application` and to manage it as a component in the [Spring application context][u-application-context].

>>>>>>> Regenerate README.md based on updates
The `@ComponentScan` annotation tells Spring to search recursively through the `hello` package and its children for classes marked directly or indirectly with Spring's [`@Component`][] annotation. This directive ensures that Spring finds and registers the `GreetingController`, because it is marked with `@Controller`, which in turn is a kind of `@Component` annotation.

The [`@EnableAutoConfiguration`][] annotation switches on reasonable default behaviors based on the content of your classpath. For example, because the application depends on the embeddable version of Tomcat (tomcat-embed-core.jar), a Tomcat server is set up and configured with reasonable defaults on your behalf. And because the application also depends on Spring MVC (spring-webmvc.jar), a Spring MVC [`DispatcherServlet`][] is configured and registered for you — no `web.xml` necessary! Auto-configuration is a powerful, flexible mechanism. See the [API documentation][`@EnableAutoConfiguration`] for further details.

<<<<<<< HEAD
=======
### Build an executable JAR

>>>>>>> Regenerate README.md based on updates
Now that your `Application` class is ready, you simply instruct the build system to create a single, executable jar containing everything. This makes it easy to ship, version, and deploy the service as an application throughout the development lifecycle, across different environments, and so forth.

Add the following configuration to your existing Maven POM:

`pom.xml`
```xml
    <properties>
        <start-class>hello.Application</start-class>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

The `start-class` property tells Maven to create a `META-INF/MANIFEST.MF` file with a `Main-Class: hello.Application` entry. This entry enables you to run it with `mvn spring-boot:run` (or simply run the jar itself with `java -jar`).

The [Spring Boot maven plugin][spring-boot-maven-plugin] collects all the jars on the classpath and builds a single "über-jar", which makes it more convenient to execute and transport your service.

Now run the following command to produce a single executable JAR file containing all necessary dependency classes and resources:

```sh
$ mvn package
```

[spring-boot-maven-plugin]: https://github.com/SpringSource/spring-boot/tree/master/spring-boot-maven-plugin

> **Note:** The procedure above will create a runnable JAR. You can also opt to [build a classic WAR file](/guides/gs/convert-jar-to-war/) instead.

Run the service
-------------------
Run your service using the spring-boot plugin at the command line:

```sh
$ mvn spring-boot:run
```


Logging output is displayed. The service should be up and running within a few seconds.


Test the service
----------------

Now that the service is up, visit <http://localhost:8080/greeting>, where you see:

```json
{"id":1,"content":"Hello, World!"}
```

Provide a `name` query string parameter with <http://localhost:8080/greeting?name=User>. Notice how the value of the `content` attribute changes from "Hello, World!" to "Hello User!":

```json
{"id":2,"content":"Hello, User!"}
```

This change demonstrates that the `@RequestParam` arrangement in `GreetingController` is working as expected. The `name` parameter has been given a default value of "World", but can always be explicitly overridden through the query string.

Notice also how the `id` attribute has changed from `1` to `2`. This proves that you are working against the same `GreetingController` instance across multiple requests, and that its `counter` field is being incremented on each call as expected.


Summary
-------

<<<<<<< HEAD
Congrats! You've just developed a RESTful web service with Spring. This of course is just the beginning, and there are many more features to explore and take advantage of. Be sure to check out Spring's support for [securing](TODO), [describing](TODO) [managing](TODO), [testing](TODO) and [consuming](/gs-consuming-rest) RESTful web services.
=======
Congratulations! You've just developed a RESTful web service with Spring. 

>>>>>>> Regenerate README.md based on updates


[u-rest]: /understanding/REST
[u-json]: /understanding/JSON
[u-jsp]: /understanding/JSP
[jackson]: http://wiki.fasterxml.com/JacksonHome
[u-war]: /understanding/WAR
[u-tomcat]: /understanding/Tomcat
[u-application-context]: /understanding/application-context
[`@Controller`]: http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/stereotype/Controller.html
[`SpringApplication`]: http://static.springsource.org/spring-bootstrap/docs/0.5.0.BUILD-SNAPSHOT/javadoc-api/org/springframework/bootstrap/SpringApplication.html
[`@EnableAutoConfiguration`]: http://static.springsource.org/spring-bootstrap/docs/0.5.0.BUILD-SNAPSHOT/javadoc-api/org/springframework/bootstrap/context/annotation/SpringApplication.html
[`@Component`]: http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/stereotype/Component.html
[`@ResponseBody`]: http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseBody.html
[`MappingJackson2HttpMessageConverter`]: http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/http/converter/json/MappingJackson2HttpMessageConverter.html
[`DispatcherServlet`]: http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/web/servlet/DispatcherServlet.html
