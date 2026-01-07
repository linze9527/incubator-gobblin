# java-webapp (Maven)

Simple Java Servlet web application (WAR) example.

Build:

```bash
mvn -f java-webapp/pom.xml clean package -DskipTests
```

Run (embedded Jetty):

```bash
cd java-webapp
mvn jetty:run
```

Then open http://localhost:8080/
