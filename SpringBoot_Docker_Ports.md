# 🔌 Spring Boot & Docker Port Configuration

## 🎯 What Controls the Port

-   Spring Boot port is set in
    `src/main/resources/application.properties` (or `.yml`).
-   You configured:

``` properties
server.port=8070
```

👉 This makes the app listen on **8070** inside the container.

------------------------------------------------------------------------

## 🐳 Docker EXPOSE

``` dockerfile
EXPOSE 8070
```

-   Purely **documentation** for the image.\
-   Does **not** change the app's port.\
-   Tells others: "this container expects 8070."

------------------------------------------------------------------------

## 🌍 Publishing to Host

When running the container, map ports:

``` sh
docker run -p 8070:8070 bofosu1/ekart:latest
# host:container
```

Now your app is accessible at **http://localhost:8070**.

------------------------------------------------------------------------

## ✅ Quick Checklist

-   [x] Keep `server.port=8070`
-   [x] Dockerfile → `EXPOSE 8070`
-   [x] Run with `-p 8070:8070`

Test endpoint:

``` sh
curl http://localhost:8070/actuator/health   # if actuator enabled
curl http://localhost:8070/                  # default app root
```

------------------------------------------------------------------------

## ⚠️ If It Still Starts on 8080

Possible overrides: 1. **Env vars**: `SERVER_PORT` or `PORT` at runtime.
2. **Profile-specific configs**: e.g. `application-prod.properties` with
a different port. 3. **Command-line args**: `--server.port=8080` when
launching.

------------------------------------------------------------------------

## 📝 Note

-   The correct file is `application.properties`, not
    `application.json`.

------------------------------------------------------------------------

## 🚀 One-Liner Override

Run with environment variable:

``` sh
docker run -e SERVER_PORT=8070 -p 8070:8070 bofosu1/ekart:latest
```

This ensures container + host both align on **8070**.
