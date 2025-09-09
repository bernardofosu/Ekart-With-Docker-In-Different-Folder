FROM openjdk:8u151-jdk-alpine3.7

EXPOSE 8070

ENV APP_HOME /usr/src/app

COPY target/shopping-cart-0.0.1-SNAPSHOT.jar $APP_HOME/app.jar

WORKDIR $APP_HOME

ENTRYPOINT exec java -jar app.jar


# A cleaner order (recommended)

# Put WORKDIR first and use a relative destination—clear and less error-prone:

# FROM openjdk:8u151-jdk-alpine3.7

# ENV APP_HOME=/usr/src/app
# WORKDIR $APP_HOME

# EXPOSE 8070
# COPY target/shopping-cart-0.0.1-SNAPSHOT.jar app.jar

# ENTRYPOINT ["java","-jar","app.jar"]