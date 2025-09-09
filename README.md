# Spring Boot Shopping Cart Web App

## About

This is a demo project for practicing Spring + Thymeleaf. The idea was to build some basic shopping cart web app.

It was made using **Spring Boot**, **Spring Security**, **Thymeleaf**, **Spring Data JPA**, **Spring Data REST and Docker**. 
Database is in memory **H2**.

There is a login and registration functionality included.

Users can shop for products. Each user has his own shopping cart (session functionality).
Checkout is transactional.

## Configuration

### Configuration Files

Folder **src/resources/** contains config files for **shopping-cart** Spring Boot application.

* **src/resources/application.properties** - main configuration file. Here it is possible to change admin username/password,
as well as change the port number.

## How to run

There are several ways to run the application. You can run it from the command line with included Maven Wrapper, Maven or Docker. 

Once the app starts, go to the web browser and visit `http://localhost:8070/home`

Admin username: **admin**

Admin password: **admin**

User username: **user**

User password: **password**

## First let build the application
```sh
git clone <githublink>
cd <name>

mvn package
```

Test error
```sh

Results :

Tests in error:
  ShoppingCartApplicationTests.contextLoads » IllegalState Failed to load Applic...

Tests run: 1, Failures: 0, Errors: 1, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  21.810 s
[INFO] Finished at: 2025-09-09T09:52:00Z
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test (default-test) on project shopping-cart: There are test failures.
[ERROR]
[ERROR] Please refer to /home/ubuntu/Ekart-With-Docker-In-Different-Folder/target/surefire-reports for the individual test results.
[ERROR] -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException
```
### We will skip test stage
```sh
# 1️⃣ -DskipTests=true

mvn package -DskipTests=true
mvn package -DskipTests


[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.845 s
[INFO] Finished at: 2025-09-09T09:59:05Z
[INFO] ------------------------------------------------------------------------
```
- What it does: Compiles the tests but does not run them.
- The test classes are still built and included in the output JAR/WAR if your packaging requires them.
- Faster than running tests, but still slower than a full skip because it compiles tests.

### We will Check jar artifacts
```sh
cd target
```
[Where to find the jar file](./maven_jar_notes.md)

```sh
# 2️⃣ -Dmaven.test.skip=true

mvn package -Dmaven.test.skip=true
```
- What it does: Completely skips both compiling and running tests.
- Maven will not even generate test classes.
- This gives the fastest build, but can be risky if you forget to test before deploying.


### ✅ When to use which
- Use -DskipTests=true if you want a normal build but don’t want to waste time running tests right now (e.g., testing packaging).
- Use -Dmaven.test.skip=true if you’re only interested in producing the artifact as fast as possible and don’t care about compiling or running tests at all (e.g., quick builds in CI/CD or debugging builds).

> [!NOTE] 
> If you can skip tests and Maven will build the JAR/WAR, but your app may still fail at runtime.
Your failing test is contextLoads, which means the Spring Boot application context didn’t start. If the same cause exists in your real run, the app will also crash when you java -jar it.


### How to build the image
```sh
cd ~/Ekart-With-Docker-In-Different-Folder
docker build -t bofosu1/ekart:latest -f docker/Dockerfile .
```

```sh
docker run -d -p 8070:8070 --name ekart bofosu1/ekart:latest

docker ps

docker logs <id>

http://98.81.164.3:8070/home
```

### Maven Wrapper

#### Using the Maven Plugin

Go to the root folder of the application and type:
```bash
$ chmod +x scripts/mvnw
$ scripts/mvnw spring-boot:run
```

#### Using Executable Jar

Or you can build the JAR file with 
```bash
$ scripts/mvnw clean package
``` 

Then you can run the JAR file:
```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

### Maven

Open a terminal and run the following commands to ensure that you have valid versions of Java and Maven installed:

```bash
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

```bash
$ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T16:41:47+00:00)
Maven home: /usr/local/Cellar/maven/3.3.9/libexec
Java version: 1.8.0_102, vendor: Oracle Corporation
```

#### Using the Maven Plugin

The Spring Boot Maven plugin includes a run goal that can be used to quickly compile and run your application. 
Applications run in an exploded form, as they do in your IDE. 
The following example shows a typical Maven command to run a Spring Boot application:
 
```bash
$ mvn spring-boot:run
``` 

#### Using Executable Jar

To create an executable jar run:

```bash
$ mvn clean package
``` 

To run that application, use the java -jar command, as follows:

```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

To exit the application, press **ctrl-c**.

### Docker

It is possible to run **shopping-cart** using Docker:

Build Docker image:
```bash
$ mvn clean package
$ docker build -t shopping-cart:dev -f docker/Dockerfile .
```

Run Docker container:
```bash
$ docker run --rm -i -p 8070:8070 \
      --name shopping-cart \
      shopping-cart:dev
```

##### Helper script

It is possible to run all of the above with helper script:

```bash
$ chmod +x scripts/run_docker.sh
$ scripts/run_docker.sh
```

## Docker 

Folder **docker** contains:

* **docker/shopping-cart/Dockerfile** - Docker build file for executing shopping-cart Docker image. 
Instructions to build artifacts, copy build artifacts to docker image and then run app on proper port with proper configuration file.

## Util Scripts

* **scripts/run_docker.sh.sh** - util script for running shopping-cart Docker container using **docker/Dockerfile**

## Tests

Tests can be run by executing following command from the root of the project:

```bash
$ mvn test
```

## Helper Tools

### HAL REST Browser

Go to the web browser and visit `http://localhost:8070/`

You will need to be authenticated to be able to see this page.

### H2 Database web interface

Go to the web browser and visit `http://localhost:8070/h2-console`

In field **JDBC URL** put 
```
jdbc:h2:mem:shopping_cart_db
```

In `/src/main/resources/application.properties` file it is possible to change both
web interface url path, as well as the datasource url.
