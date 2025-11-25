# ----------------------------------------------------------------------
# STAGE 1: BUILD (Compilación)
# Usamos una imagen de Maven con Java 21 para compilar el proyecto
# ----------------------------------------------------------------------
FROM maven:3.9.5-eclipse-temurin-21 AS build

# Establece el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copia los archivos de configuración de Maven para descargar dependencias
COPY pom.xml .

# Descarga las dependencias primero (para aprovechar el cache)
RUN mvn dependency:go-offline

# Copia todo el código fuente del proyecto
COPY src ./src

# Compila y empaqueta la aplicación, generando el JAR ejecutable
RUN mvn clean package -DskipTests

# ----------------------------------------------------------------------
# STAGE 2: RUN (Ejecución)
# Usamos una imagen ligera de Java 21 para la ejecución
# ----------------------------------------------------------------------
FROM eclipse-temurin:21-jre-alpine

# Establece el puerto de exposición (el que usa Spring Boot)
EXPOSE 8080

# Establece el directorio de trabajo
WORKDIR /app

# Copia el JAR generado de la etapa de compilación
# El nombre del JAR debe coincidir con el de tu pom.xml
COPY --from=build /app/target/mutant-detector-0.0.1-SNAPSHOT.jar mutant-detector.jar

# Define el punto de entrada para ejecutar el JAR
ENTRYPOINT ["java", "-jar", "mutant-detector.jar"]