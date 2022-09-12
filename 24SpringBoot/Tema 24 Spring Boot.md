# Tema 24 Spring Boot

## SpringBoot

En el diccionario boot strapping tiene muchos significados, uno de ellos es: to start up with minimum resources. 

Esa definición parece que se adapta al espíritu de lo que es SpringBoot:

[Referencia muy interesante](https://www.ibm.com/cloud/learn/java-spring-boot)

Java Spring Framework (Spring Framework) is a popular, open source, enterprise-level framework for creating standalone, 
production-grade applications that run on the Java Virtual Machine (JVM).

Java Spring Boot (Spring Boot) is a tool that makes developing web application and microservices with Spring Framework faster and 
easier through three core capabilities:

- Autoconfiguration
- An opinionated approach to configuration
- The ability to create standalone applications

These features work together to provide you with a tool that allows you to set up a Spring-based application with minimal configuration 
and setup.

So the trick with Spring Boot is that many things happen implicitly. For instance, we use the `@SpringBootApplication` annotation, but 
it's a combination of three annotations:

~~~ java
@Configuration
@EnableAutoConfiguration
@ComponentScan
~~~
## Dependency injection and IoC

[Beans y IoC](https://www.baeldung.com/spring-bean)

Spring Framework offers a dependency injection feature that lets objects define their own dependencies that the Spring container later 
injects into them. This process is called Inversion of Control (IoC). This enables developers to create modular applications consisting 
of loosely coupled components that are ideal for microservices and distributed network applications.

Again, Inversion of Control (IoC) is a process in which an object defines its dependencies without creating them. This object 
delegates the job of constructing and managing such dependencies to an IoC container (in our case to an Spring IoC).

*A Spring bean is every object constructed/instantiated and managed by the IoC container. When working with Spring, we can annotate our 
classes in order to make them into Spring beans.*

*The Spring IoC container's management of beans includes several responsibilities. Perhaps the most significant of which include bean 
instantiation/assembly and the management of dependency injections.*

Veamos la IoC con un ejemplo. Usaremos esta clase:

~~~ java
public class Company {
    private Address address;

    public Company(Address address) {
        this.address = address;
    }

    // getter, setter and other properties
}
~~~

Vemos que depende de la clase `Address`:

~~~ java
public class Address {
    private String street;
    private int number;

    public Address(String street, int number) {
        this.street = street;
        this.number = number;
    }

    // getters and setters
}
~~~

En el código la dependencia se maneja de manera explícita invocando un constructor específico para crearla:

~~~ java
Address address = new Address("High Street", 1000);
Company company = new Company(address);
~~~

Pero si hay muchas dependencias, o las dependencias son muy complejas o hay muchos casos de uso, entonces es mejor usar la IoC para la 
gestión de dependencias. Instead of instantiating dependencies by itself, an object can retrieve its dependencies from an IoC container. 
All we need to do is to provide the container with appropriate configuration metadata.

Veamos el ejemplo anterior de nuevo, esta vez usando inyección de dependencias:

~~~ java
@Component
public class Company {
    // this body is the same as before
}
~~~

Este objeto sigue dependiendo de la clase `Address`:

~~~ java
public class Address {
    // this body is the same as before
}
~~~

Pero ahora esta dependencia la gestionamos con un fichero de configuración:

~~~ java
@Configuration
@ComponentScan(basePackageClasses = Company.class)
public class Config {
    @Bean
    public Address getAddress() {
        return new Address("High Street", 1000);
    }
}
~~~

*Las clases con anotaciones que el contenedor IoC sabe como interpretar se llaman beans*.

@Configuration significa que la clase es una clase de configuración que, en este ejemplo, se usa para instanciar beans de tipo 
`Address`.

[@ComponentScan](https://www.baeldung.com/spring-component-scanning) le dice al contenedor IoC que busque beans en la ruta especificada.

Ya tenemos los beans definidos. Ahora necesitamos un IoC container, para instanciar y gestionar los beans. El contenedor IoC se 
crea a partir de la configuración:

~~~ java
ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
~~~

y comprobamos que todo funciona:

~~~ java
Company company = context.getBean("company", Company.class);
assertEquals("High Street", company.getAddress().getStreet());
assertEquals(1000, company.getAddress().getNumber());
~~~
## Crear un proyecto SpringBoot con VSCode

[Referencia](https://code.visualstudio.com/docs/java/java-spring-boot)

Como ejemplo vamos a crear un proyecto web. Como gestor del proyecto usaremos Maven.

```{thumbnail} images/sb_maven.png
```

al seleccionar el tipo de proyecto se inicia un wizard en el que vamos respondiendo preguntas: seleccionamos la versión 
estable más reciente de Maven, Java como lenguaje del proyecto, com.opos como ID del grupo (o sea, nombre del paquete), sbweb como ID del 
artefacto, jar como tipo de empaquetado y la versión de Java más reciente disponible.

Después nos pide que seleccionemos las dependencias del proyecto. Elegimos Spring Web

```{thumbnail} images/sb_dependencias.png
```

Por último nos pregunta en que carpeta vamos a guardar el proyecto.

```{thumbnail} images/sb_project_folder.png
```

Una vez hecho esto ya podemos ver el proyecto en el panel izquierdo del VSCode.

```{thumbnail} images/sb_project_vscode.png
```

El fichero pom.xml resultante contiene toda la información que hemos introducido al crear el proyecto.

Además nos ayuda a entender la diferencia entre groupID y artifactID. Por ejemplo, en la sección de dependencias vemos que un
mismo groupID, org.springframework.boot, cuelgan dos artefactos, spring-boot-starter-web y spring-boot-starter-test. Es una
especie de estructura jerárquica en la que los componentes tienen funciones más específicas a medida que nos alejamos de la raíz.

El starter spring-boot-starter-web contiene los módulos necesarios para el desarrollo de la aplicación web y también un starter Tomcat
para que podamos ejecutar la aplicación sin tener que instalar un Tomcat.

El esqueleto de la aplicación ya está creado y además funciona. Si vamos a la clase anotada con @SpringBootApplication vemos en la
terminal como se ejecuta la aplicación:


      .   ____          _            __ _ _
    /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
    \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/
    :: Spring Boot ::                (v2.7.3)

    2022-09-11 11:31:34.462  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : Starting SbwebApplication using Java 18.0.2.1 on Perseus with PID 32855 (/home/vicent/Programming/Java/GettingStarted/SpringBoot/sbweb/target/classes started by vicent in /home/vicent/Programming/Java/GettingStarted/SpringBoot)
    2022-09-11 11:31:34.465  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : No active profile set, falling back to 1 default profile: "default"
    2022-09-11 11:31:35.217  INFO 32855 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
    2022-09-11 11:31:35.228  INFO 32855 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
    2022-09-11 11:31:35.228  INFO 32855 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.65]
    2022-09-11 11:31:35.318  INFO 32855 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
    2022-09-11 11:31:35.318  INFO 32855 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 812 ms
    2022-09-11 11:31:35.631  INFO 32855 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
    2022-09-11 11:31:35.641  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : Started SbwebApplication in 1.437 seconds (JVM running for 1.699)
    2022-09-11 11:33:57.338  INFO 32855 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
    2022-09-11 11:33:57.338  INFO 32855 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
    2022-09-11 11:33:57.339  INFO 32855 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
## Desarrollo del proyecto

Ahora tenemos un proyecto SpringBoot creado con Maven, así que tiene la misma estructura que todos los proyectos Maven, incluída una
clase principal. Pero la aplicación todavía no hace nada. El siguiente paso es crear el fichero de configuración.
