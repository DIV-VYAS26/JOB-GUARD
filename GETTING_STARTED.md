# JobGuard foundation

JobGuard targets Java 21 and uses Spring Boot 3.5.16. The only direct
dependencies are Spring Web (HTTP/JSON and embedded Tomcat) and Spring Boot Test
(test scope only). There is no job processing or database integration yet.

## Run

From this directory on macOS or Linux:

```sh
./mvnw spring-boot:run
```

On Windows, use `mvnw.cmd` instead of `./mvnw`. The Maven Wrapper downloads
Maven 3.9.11 on first use; no global Maven installation is required. Initial
builds need internet access for Maven and dependencies.

Check the running application from another terminal:

```sh
curl -i http://localhost:8080/health
```

Expected: HTTP 200 with `{"status":"UP"}`. This endpoint confirms the HTTP
application is responding; it does not check any external services. Stop the
application with Ctrl+C. To use another port:

```sh
PORT=8081 ./mvnw spring-boot:run
```

## Test and package

```sh
./mvnw test
./mvnw clean verify
java -jar target/jobguard-0.0.1-SNAPSHOT.jar
```

The integration test starts the complete application on a random port and
checks `/health` for HTTP 200, JSON content type, and status `UP`.

## Java compatibility

The POM sets `maven.compiler.release` to 21. This restricts Java language
features and standard Java APIs to Java 21 and produces Java 21 class files,
even when compiling with a newer JDK. Spring Boot 3.5.16 supports Java 17–25:
https://docs.spring.io/spring-boot/3.5/system-requirements.html

Check the JDK actually selected by your terminal and Maven:

```sh
java -version
./mvnw -version
/usr/libexec/java_home -V
```

If macOS detects an installed JDK 25, select it for the current terminal:

```sh
export JAVA_HOME=$(/usr/libexec/java_home -v 25)
export PATH="$JAVA_HOME/bin:$PATH"
./mvnw test
```

The setup session detected JDK 23.0.2 and Java 8, with JDK 23 active. JDK 25
was not detected, so this session's execution checks use JDK 23.

## Files

- `pom.xml`: Maven coordinates, Java 21 compilation, dependencies, executable JAR plugin.
- `mvnw`, `mvnw.cmd`: official Maven Wrapper launchers for Unix and Windows.
- `.mvn/wrapper/maven-wrapper.properties`: pins the Maven distribution.
- `src/main/java/com/jobguard/JobGuardApplication.java`: starts Spring Boot and component scanning.
- `src/main/java/com/jobguard/health/HealthController.java`: serves the JSON health response.
- `src/main/resources/application.yml`: application name and configurable server port.
- `src/test/java/com/jobguard/JobGuardApplicationTests.java`: application startup and HTTP health integration test.
- `.gitignore`: excludes Maven's generated `target/` directory.
- `GETTING_STARTED.md`: these JobGuard instructions, separate from the existing README.

The existing repository `README.md` is preserved.
