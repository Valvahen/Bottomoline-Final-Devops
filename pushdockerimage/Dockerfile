FROM eclipse-temurin:17-jre-alpine AS base
RUN addgroup --system appgroup && adduser --system appuser --ingroup appgroup  
WORKDIR /app
COPY target/hello-ci-*.jar app.jar
RUN chown -R appuser:appgroup /app
USER appuser:appgroup
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
