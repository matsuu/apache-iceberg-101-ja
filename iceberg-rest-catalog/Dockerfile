FROM gradle:jdk21-jammy as builder

COPY . /app/
WORKDIR /app/

RUN ["gradle", "build", "shadowJar"]

FROM gcr.io/distroless/java21-debian12

COPY --from=builder --chown=iceberg:iceberg /app/build/libs/iceberg-rest-catalog-all.jar /usr/lib/iceberg-rest/iceberg-rest-catalog-all.jar

ENV CATALOG_CATALOG__IMPL=org.apache.iceberg.jdbc.JdbcCatalog
ENV CATALOG_URI=jdbc:sqlite:file:/tmp/iceberg_rest?mode=memory
ENV CATALOG_JDBC_USER=user
ENV CATALOG_JDBC_PASSWORD=password
ENV REST_PORT=8181

EXPOSE $REST_PORT
USER nonroot:nonroot
ENV LANG en_US.UTF-8
WORKDIR /usr/lib/iceberg-rest
CMD ["iceberg-rest-catalog-all.jar"]