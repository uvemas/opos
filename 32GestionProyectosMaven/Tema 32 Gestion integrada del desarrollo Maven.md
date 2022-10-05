# Tema 32 Gestión Integrada del Desarrollo. Maven

## Maven

Maven es una herramienta para la gestión de desarrollo de proyectos de software. Utiliza patrones y promueve el uso de
buenas prácticas. Es muy utilizada en Java.

Maven está basado en el Project Object Model. En un fichero pom.xml se declara todo lo que necesita Maven para gestionar
el proyecto.

Maven permite gestionar:

- builds
    - generar el proyecto con una estructura estándar (todos los proyectos Maven tienen la misma estructura)
    - reconstruirlo
    - compilarlo
- documentation
- reporting: obtener información de calidad sobre el proyecto (referencias cruzadas en el código, informes de unit tests,
  listas de correo...) en un sitio web o en PDF
- dependencies: automatizar la gestión de dependencias
- gestionar las unidades de test
- gestionar los plugins
- releases
- distribution: empaquetarlo (.jar, .war)

Se puede usar por línea de comando pero la mayor parte de los IDEs lo soportan de manera bastante completa.

La estructura de un proyecto Maven es:

```{thumbnail} images/estructura.png
```
### Arquetipos

Por definición, un arquetipo es un patrón o modelo que se usa para alcanzar un objetivo siempre de la misma 
manera. En Maven, *un arquetipo es una plantilla de proyecto* que, combinada con user input, genera un proyecto Maven 
adaptado a las necesidades del usuario. El usuario decide, en función de las necesidades que tenga, que arquetipo 
utilizar (los arquetipos se pueden combinar). *La estructura del proyecto depende del arquetipo que se haya utilizado para generar
dicho proyecto*. La estructura del proyecto generada por los diferentes arquetipos puede
verse en [este enlace](https://maven.apache.org/archetypes/index.html)

Los arquetipos de Maven se agrupan en el toolkit Archetype. Cada arquetipo se identifica con un ArtifactID.

Para crear un proyecto basado en un arquetipo tenemos que invocar `mvn archetype:generate`.
### Guía rápida (CLI)

Versión instalada:

~~~ bash
$ mvn --version
Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
Maven home: /opt/maven
Java version: 11.0.15, vendor: Private Build, runtime: /usr/lib/jvm/java-11-openjdk-amd64
Default locale: es_ES, platform encoding: UTF-8
OS name: "linux", version: "5.15.0-37-generic", arch: "amd64", family: "unix"
~~~

Crear un proyecto:

~~~ bash
$ mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart 
-DarchetypeVersion=1.4 -DinteractiveMode=false
~~~

Esto usará el arquetipo estándar (disponible en el repositorio Maven) `maven-archetype-quickstart` para construir un
proyecto llamado `my-app` cuyo paquete raíz es `com.mycompany.app`. La estructura del proyecto será:

~~~ bash
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
~~~

Vemos que `${basedir}/src/main/java` contiene el código fuente del proyecto, mientras que `${basedir}/src/test/java` 
contiene las unidades de test. Al generar el proyecto se crean esqueletos tanto de la aplicación de entrada al proyecto 
como de sus tests.

En `${basedir}` se ha generado el `pom.xml` que es el corazón de la configuración del proyecto Maven.

Compilar las fuentes de un proyecto:

~~~ bash
$ mvn compile
~~~

Esto compila el proyecto y deja el resultado en la carpeta `myapp/target/classes`.

Compilar *y ejecutar* los tests:

~~~ bash
$ mvn test
~~~

Compilar *pero no ejecutar* los tests:

~~~ bash
$ mvn test-compile
~~~

Compilar y empaquetar:

~~~ bash
$ mvn package
~~~

Compila el proyecto y luego lo empaqueta en un `.jar` o un `.war` (si el proyecto es una aplicación web). El artefacto
resultante (el `.jar`) se almacena en `${basedir}/target`.

Ejecuta el proyecto empaquetado:

~~~ bash
$ java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
~~~

Instalar el `.jar` en el repositorio local de artefactos `$HOME/.m2/repository`:

~~~ bash
$ mvn install
~~~
### Fases del ciclo de vida del proyecto

Although hardly a comprehensive list, these are the most common default lifecycle phases executed.

- validate: validate the project is correct and all necessary information is available
- compile: compile the source code of the project
- test: test the compiled source code using a suitable unit testing framework. These tests should not require the code 
  be packaged or deployed
- package: take the compiled code and package it in its distributable format, such as a JAR.
- integration-test: process and deploy the package if necessary into an environment where integration tests can be run
- verify: run any checks to verify the package is valid and meets quality criteria
- install: install the package into the local repository, for use as a dependency in other projects locally
- deploy: done in an integration or release environment, copies the final package to the remote repository for sharing 
  with other developers and projects.

There are two other Maven lifecycles of note beyond the default list above. They are

- clean: cleans up artifacts created by prior builds
- site: generates site documentation for this project

Phases are actually mapped to underlying goals. The specific goals executed per phase is dependant upon the packaging 
type of the project. For example, package executes `jar:jar` if the project type is a JAR, and `war:war` if the project 
type is - you guessed it - a WAR.

An interesting thing to note is that phases and goals may be executed in sequence.

~~~ bash
$ mvn clean dependency:copy-dependencies package
~~~

This command will clean the project, copy dependencies, and package the project (executing all phases up to package, of 
course).

~~~ bash
$ mvn site
~~~

This phase generates a site based upon information on the project's pom. You can look at the documentation generated 
under `target/site`.
