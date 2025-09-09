# 📦 Maven JAR Naming Notes

Maven takes the **artifactId** and **version**, merges them, and then
adds the `.jar` (or `.war`, depending on `<packaging>`).

So with your `pom.xml`:

``` xml
<groupId>com.reljicd</groupId>
<artifactId>shopping-cart</artifactId>
<version>0.0.1-SNAPSHOT</version>
<packaging>jar</packaging>
```

The output becomes:

    shopping-cart-0.0.1-SNAPSHOT.jar

------------------------------------------------------------------------

## 🛠️ Quick variations

### 🔹 Default naming

    artifactId-version.jar

### 🔹 Override with `<finalName>`{=html}

``` xml
<build>
    <finalName>ekart</finalName>
</build>
```

Produces → `ekart.jar`

### 🔹 Custom plugin packaging (Spring Boot)

If you're using the Spring Boot plugin, you often see:

``` xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

That can also affect the final executable JAR naming and add `-SNAPSHOT`
vs `-RELEASE`.
