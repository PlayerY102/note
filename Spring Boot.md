# Spring Boot

## Annotation

* @RestController

  the class is a web @Controller, Spring considers it when handling incoming web requests.

* @RequestMapping

  which by default maps all HTTP operations, such as `GET`, `POST`, and so forth.

* @GetMapping

* @PostMapping

* @RequestParam(value="name", defaultValue="World") String name

  `@RequestParam` binds the value of the query string parameter `name` into the `name` parameter of the `greeting()` method. If the `name` parameter is absent in the request, the `defaultValue` of "World" is used.

* @SpringBootApplication

  is a convenience annotation that adds all of the following:

  @Configuration

  @EnableAutoConfiguration

  @ComponentScan

* @JsonIgnoreProperties

  Indicate that any properties not bound in this type should be ignored.
  
* @NotNull

  won’t allow a null value, which is what Spring MVC generates if the entry is empty

* @Size(min=2,max=30)

  will only allow names between 2 and 30 characters long

* @Min(18)

  won’t allow if the age is less than 18

* @Valid

  to gather the attribute filled out in the form you're about to build.
  
* @Transactional

  This method is tagged with `@Transactional`, meaning that any failure causes the entire operation to roll back to its previous state, and to re-throw the original exception.

* @Componentscan

  tells Spring to look for other components, configurations, and services in the `hello` package, allowing it to find the controllers.

  

   

  

## mvn

* mvn compile

* mvn package

* java -jar **.jar