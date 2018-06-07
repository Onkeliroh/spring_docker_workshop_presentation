---
# theme : "Solarized"
transition: "slide"
highlightTheme: "darkula"
slideNumber: true
progress: true
customTheme: "custom"
---

# Spring & Docker Workshop

### What is Spring and how to use Docker?

<small>Created by Daniel Pollack</small>

--

# Disclaimer
- keine Zertifikate
- kein XML
- learning by doing Ansatz
- keine Kaufentscheidungen wegen mir
- Spring/Docker entwickelt sich zu schnell um aktuell zu sein

---

# Spring 
<img class="logo" src="resources/images/spring_logo.png" alt="spring_logo"/>

--

### Was ist Spring?

- Application Framework<!-- .element: class="fragment fade-up" -->
- bringt IoC (Inversion of Control)<!-- .element: class="fragment fade-up" -->
- bringt Dependency Injection<!-- .element: class="fragment fade-up" -->

--

### Was macht Spring beim Start?
- erstellen aller notwendigen Beans<!-- .element: class="fragment fade-up" -->
- Autowiring/Injecting der Dependencies<!-- .element: class="fragment fade-up" -->
- was immer der Entwickler will<!-- .element: class="fragment fade-up" -->

--

### Bean Lifecycle
1. Framework factory lädt Bean Definition u. erstellt sie<!-- .element: class="fragment fade-up" -->
2. Befüllen der Properties<!-- .element: class="fragment fade-up" -->
3. Erfüllen von Abhängigkeiten<!-- .element: class="fragment fade-up" -->

--

### Beans erstellen
```
@Component
public class Foo {}
```

```
@Configuration
public class SomeConfigurationClass{
  @Bean
  public SomeClass produceThisClass(){return new SomeClass();}
}
```

--

### Alternativen zu _@Component_
- `@Service` 
- `@Repository`
- `@Controller`
- `@RestController`
- etc.
note: Service ist Alias für Component

--

### Bean Scopes
- Singleton (**_default_**)
- Prototype
- Request
- Session
- GlobalSession

--

### Abhängigkeiten bestimmen
```
@Autowired
private ISomeClass someClass
```

````
public class SomeOtherClass{

  private ISomeClass someClass;

  @Autowired
  SomeOtherClass(ISomeClass someClass){
    this.someClass=someClass;
  }
}
```

--

### Autowire Varianten
1. byName 
  - Field Name ~= Bean Class Name
2. byType 
  - Field Type == Bean Type 
3. byConstructor 
  - byName | byType als Constructor Param

--

### Property Injection
```
@Value("${some.path.value}")
public Long solutionToSomething;
```

```
# src/java/resources/application.yml
some:
  path:
    value: 42.0
```

```
# src/java/resources/application.properties
some.path.value=42.0
```

--

### [Spring Ökosystem](https://spring.io/projects)
- Spring ist modular
- alle Projekte sollten miteinander funktionieren
[<img src="resources\images\spring_ecosystem.png"/>](https://spring.io/projects)

---

# Spring-Boot

<img class="logo" src="resources/images/spring-boot_logo.png" alt="spring_logo"/>

--

### Was ist Spring-Boot?
>"Takes an <strong>opinionated</strong> view of building production-ready Spring applications. Spring Boot favors <strong>convention over configuration</strong> and is designed to get you up and running as quickly as possible."
<small>[https://projects.spring.io/spring-boot/](https://projects.spring.io/spring-boot/)</small>

--

<!-- .slide: style="text-align: left;" -->
### Features
- stand-alone Spring applications<!-- .element: class="fragment fade-up" -->
- embedded Tomcat (keine WAR files nötig)<!-- .element: class="fragment fade-up" -->
- <p>stellt <strong>opinionated 'starter' POMs</strong> um Maven Konfiguration zu vereinfachen</p><!-- .element: class="fragment fade-up" -->
- konfiguriert Spring automatisch, wann immer möglich<!-- .element: class="fragment fade-up" -->
- stellt "production ready" Features wie Metrics, Health Checks und externe Konfiguration<!-- .element: class="fragment fade-up" -->
- keine XML Konfiguration oder Code-Generierung notwendig<!-- .element: class="fragment fade-up" -->

--

<!-- .slide: style="text-align: left;" -->
### Wie baue ich eine Spring-Boot-Anwendung von <b>Null</b> auf?
- [https://start.spring.io](https://start.spring.io)  
[<img src="resources/images/start-spring-io.png" style="height:500px">](https://start.spring.io)

--

<!-- .slide: style="text-align: left;" -->
<p id="task">TASK</p>
Erstellt eine Spring-Boot Anwendung
- Maven
- Java8
- Dependencies:
  - Web

--

<!-- .slide: style="text-align: left;" -->
<p id="task">TASK</p>
- erstellt 
  - einen `@RestController`
  - eine Methode die mit HTTP Get aufgerufen wird (`@GetMapping`)
    - return `"Hello World"`

Zusatz:
- Controller bezieht Grußformel aus der application property (`@Value`)

---

# Testing Spring-Boot

<img class="logo" src="resources/images/junit_logo.png"/>
<img class="logo" src="resources/images/mockito_logo.png"/>

--

- Unit-Test
- Integration Test

--

### Unit-Test
```
@Test
public void stuff(){assertTrue(true);}
```

--

### Integration Test
````
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestClass{
  @Test
  public void someTest(){
    ...
  }
}
```

--

- **Mock** - eine fake Implementation einer Klasse
- **Spy** - zum Teil ein Mock, erlaubt das überladen von Methoden der Klasse

--

### Run Tests
- Unit Tests laufen mit Surefire Plugin (enabled by default)
  - führt alle Tests in Files mit Test Sufix aus (`*Test.java`)
- Integration Tests laufen mit Failsafe Plugin (missing by default)
  - führt alle Tests in Files mit Sufix IT oder IntegerationTest aus 
  (`*IT.java` | `*IntegrationTest.java`)

--

### Failsafe Plugin
```
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-failsafe-plugin</artifactId>
  <version>2.20</version>
  <configuration>
    <includes>
      <include>**/*IT.java</include>
    </includes>
    <additionalClasspathElements>
      <additionalClasspathElement>${basedir}/target/classes</additionalClasspathElement>
    </additionalClasspathElements>
    <parallel>none</parallel>
  </configuration>
  <executions>
    <execution>
      <goals>
        <goal>integration-test</goal>
        <goal>verify</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

--

<!-- .slide: style="text-align: left;" -->

<p id="task">TASK</p>
- Testet den Controller

--

```
        <dependency>
            <groupId>com.jayway.restassured</groupId>
            <artifactId>rest-assured</artifactId>
            <version>RELEASE</version>
            <scope>test</scope>
        </dependency>
```
```
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@RunWith(SpringRunner.class)
public class HelloControllerIT {

	@LocalServerPort
	public int port;

	@Before
	public void init() {
		RestAssured.port = port;
	}

	@Test
	public void testHelloControllerResponse() {
		...
	}
}
```

--

### Nützliche Annotationen für Tests
- `@Test(expected=RuntimeExceptio.class)`
- `@RunWith(SpringRunner.class)`
- `@TestConfiguration`
- `@MockBean`
- `@SpyBean`
- `@ActiveProfiles`
- `@ContextConfiguration`
- `@DirtiesContext`
- `@Transactional`

---

# Docker

<img class="logo" src="resources/images/docker_logo.png" alt="docker_logo"/>

--

### Was ist Docker?

--


<!-- .slide: data-background="resources/images/works_on_dev.png" data-background-size="auto 100%" -->

--

### Container vs. VM

<img src="resources/images/DockerVsVm.png"/ style="with:100%;heigth:100%">

--

<!-- .slide: style="text-align: left;" -->
<p id="task">TASK</p>

`$ docker run hello-world`

`$ docker run -p 8080:80 nginx`<!-- .element: class="fragment fade-up" -->

--

<!-- .slide: style="text-align: left;" -->
### Begriffe
#### Image & Layer

 > "A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the very last one is read-only."

<small>https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/</small>

--

<!-- .slide: style="text-align: left;" -->
### Begriffe
#### Registry

 > "A registry is a storage and content delivery system, holding named Docker images, available in different tagged versions."

<small>https://docs.docker.com/registry/introduction/</small>

--

<!-- .slide: style="text-align: left;" -->
### Begriffe
#### Container

> "The major difference between a container and an image is the top writable layer."

<small>https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/#container-and-layers</small>

--

<!-- .slide: style="text-align: left;" -->
### Begriffe
#### Docker Compose:
- Tool zum Definieren und Ausführen von Multi-Container Definitionen


#### Docker Machine:
- Docker Host auf mehren Plattformen starten

#### Docker Hub:
- Dockers registry für Images

--

### Was macht Docker beim Start?
- Resourcen reservieren
- Host Kernel ankoppeln
- Befehl ausführen

--

### Wie setze ich von Null an eine Dockerumgebung auf?
- Installiere Docker auf einem Computer
- Besorge eine Registry

--

### Wie verpacke ich meine Anwendung in Docker?
- In einem Dockerfile


--

### Dockerfile
```
FROM ibmjava:8-sfj-alpine

CMD ["echo","hello world"]
```

--

```
FROM ibmjava:8-sfj-alpine

ADD target/demo.jar demo.jar

EXPOSE 8080

CMD ["java","-jar","demo.jar"]
```

--

### Dockerfile build
- `$docker build .` <!-- .element: class="fragment fade-up" -->
- `$docker build . -t greeter` <!-- .element: class="fragment fade-up" -->
- `$docker build . -t greeter:stable` <!-- .element: class="fragment fade-up" -->

--

--

### Docker Kommandos
- docker run
- docker build
- docker logs
- docker exec
- docker stop
- docker kill

--

### Docker Kommandos
- docker rm
- docker ps
- docker top

--

<!-- .slide: style="text-align: left;" -->

<p id="task">TASK</p>
- erstellt ein `Dockerfile` und verpackt eure App
```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <finalName>${project.name}</finalName>
        <goal>packaging</goal>
        <mainClass>de.workshop.demo.DemoApplication</mainClass>
    </configuration>
</plugin>
```

--

### Docker best practices?
- Dockerfiles versionieren
- .dockerignore File nutzen
- nur einen Prozess/Befehl pro Container starten
- zustandslos halten
- schmale BaseImages nutzen

--

<!-- .slide: style="text-align: left;" -->
### Java in Docker
#### Problem:

JVM weiß nichts von Containern und sieht den Docker Host und seine Resourcen

--

<!-- .slide: style="text-align: left;" -->
#### Lösung:
- JVM Parameter 
- Seit Java8 optional container aware

--> 

---

# Docker-Compose

--

### Problem
```
docker image pull redis:alpine
docker image pull russmckendrick/moby-counter
docker network create moby-counter
docker container run -d --name redis --network moby-counter redis:alpine
docker container run -d --name moby-counter --network moby-counter \
  -p 8080:80 russmckendrick/moby-counter
```

--

# Lösung

````
version: "3"
services:
  redis:
    image: redis:alpine
    volumes:
    - redis_data:/data
    restart: always
  mobycounter:
    depends_on:
      - redis
    image: russmckendrick/moby-counter
    ports:
      - "8080:80"
    restart: always
    volumes:
      redis_data:
```

--

```
#build all images defined within compose file
docker-compose build
docker-compose build --no-cache

#run all images defined within compose file
docker-compose up
docker-compose up -d

#stop all running images
docker-compose down

#pull all images from remote registry
docker-compose pull

#push all images to remote registry
docker-compose push
```

---

# Spring Security
<img class="logo" src="resources/images/spring-security_logo.png" alt="docker_logo"/>

--

### Authorization
Darf der Nutzer das?

### Authentication
Hier bin ich und ich darf das.

--

### Spring-Security
- bietet default Einstellungen
  - Login Page
  - User Roles

--

<!-- .slide: style="text-align: left;" -->

<p id="task">TASK</p>
- bindet Spring-Security in eure App ein
- legt einen weiteren Nutzer und meldet euch an

---

# Træfik 
<img class="logo" src="resources/images/traefik.logo.png" alt="spring_logo"/>

--

## Was ist Træfik?

--

- HTTP reverse Proxy
- Loadbalancer
- unterstützt Docker

--

<!-- .slide: style="text-align: left;" -->

<p id="task">TASK</p>

- besorgt Træfik: `docker pull traefik`
  - tag ist egal
- erweitert euer docker-compose file

_Powershell TIP_
`$Env:COMPOSE_CONVERT_WINDOWS_PATHS=1`
