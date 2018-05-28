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

---

# Spring 
<img class="logo" src="resources/images/spring_logo.png" alt="spring_logo"/>

--

### Was ist Spring?

- Application Framework
- bringt IoC (Inversion of Control)
- bringt Dependency Injection

--

### Was mach Spring beim Start?

--

### [Spring Ökosystem](https://spring.io/projects)
- Spring ist modular
- alle Projekte sollten miteinander funktionieren
<img src="resources\images\spring_ecosystem.png"/>
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
- automatisch konfiguriere Spring, wann immer möglich<!-- .element: class="fragment fade-up" -->
- stellt "production ready" features wie Metrics, Health Checks und externe Konfiguration<!-- .element: class="fragment fade-up" -->
- keine XML Konfiguration oder Code-Gernerierung notwendig<!-- .element: class="fragment fade-up" -->

<small>https://projects.spring.io/spring-boot/</small>

--

<!-- .slide: style="text-align: left;" -->

### Wie baue ich eine Spring-Boot-Anwendung von <b>0</b> auf?
- [https://start.spring.io](https://start.spring.io)
  
[<img src="resources/images/start-spring-io.png" style="height:500px">](https://start.spring.io)

--

<p id="task">TASK</p>
Erstellt eine Spring-Boot Anwendung

- Maven
- Java8
- Web Dependency

---

# Docker

<img class="logo" src="resources/images/docker_logo.png" alt="docker_logo"/>

--

### Was ist Docker?

--

### Wie setze ich von 0 an eine Dockerumgebung auf?

--

### Wie verpacke ich meine Anwendung in Docker?

--

<p id="task">TASK</p>


--

### Docker best practices

--

### Java in Docker

---

# Testing Spring-Boot

<img class="logo" src="resources/images/junit_logo.png"/>
<img class="logo" src="resources/images/mockito_logo.png"/>