# x-platform-parent

`x-platform-parent` is the shared Maven parent POM for company platform services. It centralizes Java and Spring version settings, dependency versions, common dependencies, and plugin configuration so child projects can inherit a consistent baseline.

## Project structure

This repository currently contains a single root Maven project:

```text
.
├── pom.xml       # Parent POM and shared dependency/plugin configuration
├── README.md     # Project documentation
└── .gitignore    # Generated and machine-specific files excluded from Git
```

The root POM uses `pom` packaging and does not currently declare child modules or application source code. Service projects can use it as their Maven parent.

## Main technologies

- Java 21
- Apache Maven
- Spring Boot 4.0.8
- Spring Cloud Alibaba 2025.1.0.0 (dependency management)
- Spring AI 2.0.1 (dependency management)
- MyBatis-Plus 3.5.17
- Common managed dependencies for Spring MVC, Security, OAuth2, validation, Thymeleaf, Actuator, Nacos, Ollama/OpenAI model clients, MySQL, Lombok, UUID creation, and testing

The shared dependencies are declared in the parent POM and are inherited by child projects. The Spring Cloud Alibaba, Spring AI, MyBatis-Plus, and UUID versions are centrally managed there.

## Requirements

- JDK 21
- Maven 3.6.3 or newer (or a compatible Maven installation)

Confirm the tools are available:

```sh
java -version
mvn -version
```

## Configure and build

There is no standalone application to launch from this repository. To validate and install the parent POM into your local Maven repository, run from the project root:

```sh
mvn clean install
```

Child projects can inherit this parent with the following coordinates:

```xml
<parent>
    <groupId>com.lsxuan</groupId>
    <artifactId>x-platform-parent</artifactId>
    <version>1.0.0</version>
    <relativePath/>
</parent>
```

The `<relativePath/>` setting makes Maven resolve the parent from repositories, including the local repository populated by `mvn install`. Publish the parent to your organization's Maven repository for use by other developers and build agents.

No application-specific runtime configuration is needed here. A child service may need configuration for its database, Nacos server, authentication provider, or AI model endpoint, depending on which inherited starters it uses. Keep credentials and machine-specific settings outside version control.

## Notes

- The parent declares shared dependencies, including web, security, OAuth2 client, validation, Thymeleaf, Actuator, Nacos configuration, Spring AI Ollama and OpenAI clients, MyBatis-Plus, and the MySQL runtime driver.
- Lombok is optional and is excluded from Spring Boot executable archives by the managed plugin configuration.
- The root POM manages Spring Boot and compiler plugin configuration; child projects can activate plugins as needed.
- This repository currently has no modules, application entry point, or project-specific test suite.
