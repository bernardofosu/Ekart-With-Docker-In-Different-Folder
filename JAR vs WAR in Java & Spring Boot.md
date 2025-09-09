# ⚖️ JAR vs WAR in Java & Spring Boot

📦 **.war (Web Application Archive)**

* Designed to be deployed on an **external servlet container** (like Apache Tomcat, JBoss, WebSphere).
* The WAR contains your app’s classes + web resources, but **not Tomcat itself**.
* The external Tomcat already knows how to load and run WAR files.
* Typical in older enterprise setups where many apps run on the same shared Tomcat.

---

🚀 **.jar (Spring Boot Executable JAR)**

* With Spring Boot, the build produces a **“fat JAR”** (includes all dependencies).
* This JAR bundles an **embedded Tomcat** (or Jetty/Undertow, depending on your setup).
* When you run:

  ```sh
  java -jar shopping-cart-0.0.1-SNAPSHOT.jar
  ```

  Spring Boot starts its **own Tomcat internally** and serves your app.
* No need to install Tomcat separately — it’s **inside your JAR**.

---

🗝️ **Key Difference**

* **WAR** → external Tomcat must exist; Tomcat runs your WAR.
* **JAR** → Spring Boot includes Tomcat; Java runs the JAR, and Tomcat is started inside.

So in your logs:

```
Tomcat started on port(s): 8070 (http)
```

That’s the **embedded Tomcat** packaged inside your JAR.

---

👉 **In short:**

* WAR = "Deploy me on a Tomcat server."
* JAR = "I bring my own Tomcat — just run me."

---

⚙️ **What happens when you add packaging JAR and run Maven**

```xml
<groupId>com.reljicd</groupId>
<artifactId>shopping-cart</artifactId>
<version>0.0.1-SNAPSHOT</version>
<packaging>jar</packaging>
```

When you run:

```sh
mvn package
```

👉 Maven will build a JAR file for your project, but whether it has “everything” (Spring Boot, Tomcat, dependencies) depends on what’s in your **pom.xml**.

---

🔑 **Key detail**

* You still need to declare Spring Boot dependencies in `pom.xml` (like `spring-boot-starter-web`, `spring-boot-starter-security`, etc.).
* Maven then downloads them from Maven Central (similar to how `pip` downloads packages from PyPI).
* The **Spring Boot Maven Plugin** repackages your JAR into an “executable fat JAR” containing:

  * Your code
  * All dependencies (Spring Boot, embedded Tomcat, etc.)
  * A launcher to run it with `java -jar`

So, it’s not that Maven magically includes Spring Boot/Tomcat by default — it’s that your **pom.xml** told Maven: “Hey, I need Spring Boot.” Maven fetched it, and the Boot plugin packaged it into the JAR.

---

✅ **Bottom line**

* **Without Spring Boot dependencies** → You just get a plain JAR with your code only. Running it won’t start a web server.
* **With Spring Boot dependencies (and Boot plugin)** → You get a **fat JAR** that includes Spring Boot + Tomcat. Running it (`java -jar app.jar`) will boot the app and start Tomcat.

---

📌 **The `<parent>` section in pom.xml**

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.3.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

This line is why everything “just works.”

* Provides **dependency management** → versions for Spring, Tomcat, Jackson, Hibernate, etc.
* Provides **plugin management** → especially the Spring Boot Maven Plugin that knows how to repackage your JAR into an executable fat JAR.
* Supplies **sensible defaults** (Java version, encoding, etc.).

---

🧩 **Under the hood**

You add starters like:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

This brings in:

* Spring MVC
* Jackson
* Tomcat (embedded)

The parent defines which exact versions of those libraries to use.

When you run:

```sh
mvn package
```

The Spring Boot Maven Plugin (inherited from the parent) takes your app + dependencies + Boot + Tomcat and wraps them all into:

```
shopping-cart-0.0.1-SNAPSHOT.jar
```

Running:

```sh
java -jar shopping-cart-0.0.1-SNAPSHOT.jar
```

Starts Spring Boot → Boot spins up embedded Tomcat → your app runs.

---

✅ **Summary**

* `<parent>` + `spring-boot-starter-*` dependencies in pom.xml → pull in Spring Boot & Tomcat.
* `<packaging>jar</packaging>` + Boot plugin → makes the JAR self-contained and executable.
* No need to install Spring Boot or Tomcat separately → Maven resolves and bundles them.

---

📑 **Minimal Spring Boot pom.xml Example**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Project coordinates -->
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.3</version> <!-- or latest stable -->
        <relativePath/> <!-- always fetch from repo -->
    </parent>

    <dependencies>
        <!-- Web starter (includes Spring MVC + Jackson + embedded Tomcat) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Optional: for writing unit tests -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Repackage into executable JAR -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

⚡ **What this gives you**

* `spring-boot-starter-parent` → manages versions + defaults.
* `spring-boot-starter-web` → pulls in Spring MVC and embedded Tomcat.
* `spring-boot-maven-plugin` → makes the fat JAR executable with `java -jar`.

So with just this file, plus a `DemoApplication.java` like:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

…you can do:

```sh
mvn package
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

And Tomcat will start on port **8080** (or another if you set `server.port`).
