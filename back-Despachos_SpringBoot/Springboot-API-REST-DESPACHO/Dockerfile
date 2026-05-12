# Etapa 1: Construcción (Multi-stage build)
FROM maven:3.9-eclipse-temurin-17-alpine AS builder
WORKDIR /app

# Copiar solo el pom.xml primero para aprovechar la caché de Docker
# y descargar las dependencias sin tener que compilar todo el código de nuevo
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copiar el código fuente y compilar el proyecto
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Ejecución (Imagen más ligera)
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Mínimo privilegio: Crear y usar un usuario que no sea root
RUN addgroup -S devopsgroup && adduser -S devopsuser -G devopsgroup
USER devopsuser

# Copiar solo el archivo .jar generado en la etapa anterior (limpieza de capas)
COPY --from=builder /app/target/*.jar app.jar

# Exponer el puerto (cambia el 8080 si tu backend usa otro por defecto)
EXPOSE 8080

# Comando de inicio
ENTRYPOINT ["java", "-jar", "app.jar"]