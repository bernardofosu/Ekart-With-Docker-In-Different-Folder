# ☕ Dockerfile Explained Line by Line

## 🏗️ Base Image

``` dockerfile
FROM openjdk:8u151-jdk-alpine3.7
```

Uses a lightweight Alpine Linux with OpenJDK 8 (update 151). Provides a
minimal Java environment.

------------------------------------------------------------------------

## 🌐 Expose Port

``` dockerfile
EXPOSE 8070
```

Indicates the app listens on **8070** inside the container.\
👉 Remember: you still need `-p 8070:8070` when running.

------------------------------------------------------------------------

## 📦 Environment Variable

``` dockerfile
ENV APP_HOME /usr/src/app
```

Defines `APP_HOME` variable to avoid repeating paths.\
Later used for `WORKDIR` and `COPY`.

------------------------------------------------------------------------

## 📂 Copy JAR

``` dockerfile
COPY target/shopping-cart-0.0.1-SNAPSHOT.jar $APP_HOME/app.jar
```

Copies your **Spring Boot JAR** into `/usr/src/app/app.jar` inside the
container.

------------------------------------------------------------------------

## 📍 Working Directory

``` dockerfile
WORKDIR $APP_HOME
```

Switches directory to `/usr/src/app`.\
All next commands run here.

------------------------------------------------------------------------

## 🚀 Entry Point

``` dockerfile
ENTRYPOINT exec java -jar app.jar
```

Runs your Spring Boot app.\
`exec` makes Java replace the shell → Docker handles stop/restart
properly.

------------------------------------------------------------------------

# ⚡ Ports & Configs

-   By default Spring Boot runs on **8080**.\
-   You changed it to **8070** in `application.properties`
    (`server.port=8070`).\
-   Dockerfile + app settings must match.

Run example:

``` sh
docker run -e SERVER_PORT=8070 -p 8070:8070 bofosu1/ekart:latest
```

✅ Ensures container listens & host publishes correctly.
