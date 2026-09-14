## Configurar la imagen

# Completar
 FROM maven:3.9.6-eclipse-temurin-17-alpine AS builder

 WORKDIR /build
 COPY pom.xml .
 RUN mvn -B dependency:go-offline

 COPY src ./src
 RUN mvn -B package -DskipTests

 #STAGE 2:

 FROM eclipse-temurin:17-jre-alpine AS runtime
 RUN addgroup -S appgroup && adduser -S appuser -G appgroup 
 WORKDIR /app

 RUN apk add --no-cache curl

 COPY --from=builder /build/target/*.jar app.jar
 RUN chown appuser:appgroup app.jar
 USER appuser 
 EXPOSE 8080 
 ENTRYPOINT ["java", "-jar", "app.jar"]
