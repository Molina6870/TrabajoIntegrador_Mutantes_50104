# ----------------------------------------------------------------------
# STAGE 1: BUILD (Compilación)
# Usamos una imagen de Maven con Java 21 para compilar el proyecto
# ----------------------------------------------------------------------
FROM maven:3.9.5-eclipse-temurin-21 AS build

# Establece el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copia los archivos de configuración de Maven
COPY pom.xml .

# Descarga las dependencias
RUN mvn dependency:go-offline

# Copia todo el código fuente del proyecto
COPY src ./src

# Compila y empaqueta la aplicación
RUN mvn clean package -DskipTests

# MUEVE el JAR a un directorio temporal llamado 'build_output'
# Esto hace que el JAR sea más fácil de encontrar para la siguiente etapa
RUN mkdir -p build_output && mv target/*.jar build_output/app.jar


# ----------------------------------------------------------------------
# STAGE 2: RUN (Ejecución)
# Usamos una imagen ligera de Java 21 para la ejecución
# ----------------------------------------------------------------------
FROM eclipse-temurin:21-jre-alpine

# Establece el puerto de exposición (el que usa Spring Boot)
EXPOSE 8080

# Establece el directorio de trabajo
WORKDIR /app

# Copia el JAR renombrado y movido a la nueva ubicación 'build_output'
COPY --from=build /app/build_output/app.jar .

# Define el punto de entrada para ejecutar el JAR
ENTRYPOINT ["java", "-jar", "app.jar"]