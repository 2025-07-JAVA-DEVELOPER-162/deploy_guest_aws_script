# 1단계: Gradle 이미지로 빌드 수행
FROM gradle:8.5.0-jdk21 AS build
WORKDIR /app

COPY gradle gradle
COPY gradlew .
COPY build.gradle* settings.gradle* gradle.properties* ./
RUN chmod +x ./gradlew

COPY src src
RUN ./gradlew clean bootJar -x test --no-daemon

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
ENV SPRING_PROFILES_ACTIVE=mysql-prod-group
ENTRYPOINT ["sh","-c","java -Dspring.profiles.active=${SPRING_PROFILES_ACTIVE} -jar /app/app.jar"]


# FROM eclipse-temurin:21-jre
# WORKDIR /app
# COPY /app.jar /app/app.jar

# # 기본값(필요하면 런타임에 덮어씀)
# ENV SPRING_PROFILES_ACTIVE=mysql-dev-group
# EXPOSE 8080

# ENTRYPOINT ["sh","-c","java -Dspring.profiles.active=${SPRING_PROFILES_ACTIVE} -jar /app/app.jar"]