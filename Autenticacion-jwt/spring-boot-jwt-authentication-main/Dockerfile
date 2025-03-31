# Etapa 1: construir el proyecto con Maven y JDK 17
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copiar el archivo pom.xml y resolver dependencias
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiar el resto del proyecto (src, etc.)
COPY src ./src

# Construir el .jar del proyecto sin ejecutar tests
RUN mvn clean package -DskipTests

# Etapa 2: generar imagen final más ligera solo con el JAR
FROM eclipse-temurin:17-jdk-jammy
WORKDIR /app

# Copiar el JAR desde la etapa de construcción
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto por defecto de Spring Boot
EXPOSE 8080

# Comando para iniciar la app
ENTRYPOINT ["java", "-jar", "app.jar"]
