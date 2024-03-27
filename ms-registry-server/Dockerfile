# Use una imagen base de OpenJDK 17 slim
FROM openjdk:17-slim-buster AS base
WORKDIR /app

# Instala cualquier dependencia específica del sistema si es necesario

# Etapa de construcción
FROM maven:3.8.4-openjdk-17 AS build
WORKDIR /build

# Copiar solo el archivo de definición de dependencias para descargar las dependencias
COPY pom.xml ./
RUN mvn dependency:go-offline

# Copiar el resto del código fuente y construir la aplicación
COPY src ./src
RUN mvn -f pom.xml clean package -DskipTests

# Etapa final
FROM base AS final
WORKDIR /app

# Copiar el archivo JAR compilado desde la etapa de construcción
COPY --from=build /build/target/*.jar ./app.jar

# Comando para ejecutar la aplicación al iniciar el contenedor
CMD ["java", "-jar", "app.jar"]