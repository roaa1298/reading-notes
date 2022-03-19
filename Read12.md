# Accessing Data with JPA

- we will build an application that uses Spring Data JPA to store and retrieve data in a relational database.
## 1- Define a Simple Entity
- In our example, we will store Customer objects, each annotated as a JPA entity. The following listing shows the Customer class   
   ```
   package com.example.accessingdatajpa;

   import javax.persistence.Entity;
   import javax.persistence.GeneratedValue;
   import javax.persistence.GenerationType;
   import javax.persistence.Id;

   @Entity
   public class Customer {

     @Id
     @GeneratedValue(strategy=GenerationType.AUTO)
     private Long id;
     private String firstName;
     private String lastName;

     protected Customer() {}

     public Customer(String firstName, String lastName) {
       this.firstName = firstName;
       this.lastName = lastName;
     }

     @Override
     public String toString() {
       return String.format(
           "Customer[id=%d, firstName='%s', lastName='%s']",
           id, firstName, lastName);
     }

     public Long getId() {
       return id;
     }

     public String getFirstName() {
       return firstName;
     }

     public String getLastName() {
       return lastName;
     }
   }
   ```
- Here we have a **Customer** class with three attributes: **id**, **firstName**, and **lastName**.
- We also have two constructors. The default constructor exists only for the sake of JPA. You do not use it directly, so it is designated as **protected**. The other constructor is the one you use to create instances of **Customer** to be saved to the database.  
- The **Customer** class is annotated with **@Entity**, indicating that it is a JPA entity (Because no **@Table** annotation exists, it is assumed that this entity is mapped to a table named **Customer**.).  
- The **Customer** object’s **id** property is annotated with **@Id** so that JPA recognizes it as the object’s ID. The **id** property is also annotated with **@GeneratedValue** to indicate that the ID should be generated automatically.
- The other two properties, **firstName** and **lastName**, are left unannotated. It is assumed that they are mapped to columns that share the same names as the properties themselves.
- The convenient **toString()** method print outs the customer’s properties.  

## 2- Create Simple Queries
- Spring Data JPA focuses on using JPA to store data in a relational database.  
- Its most compelling feature is the ability to create repository implementations automatically, at runtime, from a repository interface.  
- then we will create a repository interface that works with Customer entities.  
   ```
   package com.example.accessingdatajpa;

   import java.util.List;

   import org.springframework.data.repository.CrudRepository;

   public interface CustomerRepository extends CrudRepository<Customer, Long> {

     List<Customer> findByLastName(String lastName);

     Customer findById(long id);
   }
   ```
- **CustomerRepository** extends the **CrudRepository** interface.
- The type of entity and ID that it works with, **Customer** and **Long**, are specified in the generic parameters on **CrudRepository**.
- By extending **CrudRepository**, **CustomerRepository** inherits several methods for working with Customer persistence, including methods for saving, deleting, and finding Customer entities.   
- Spring Data JPA also lets you define other query methods by declaring their method signature. For example, **CustomerRepository** includes the **findByLastName()** method.  
- In a typical Java application, you might expect to write a class that implements **CustomerRepository**. However, that is what makes Spring Data JPA so powerful: You need not write an implementation of the repository interface. Spring Data JPA creates an implementation when you run the application.  

## 3- Create an Application Class
- Spring Initializr creates a simple class for the application.
   ```
   package com.example.accessingdatajpa;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class AccessingDataJpaApplication {

     public static void main(String[] args) {
       SpringApplication.run(AccessingDataJpaApplication.class, args);
     }

   }
   ```
- **@SpringBootApplication** is a convenience annotation that adds all of the following:  
   - **@Configuration**: Tags the class as a source of bean definitions for the application context.  
   - **@EnableAutoConfiguration**: Tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings. For example, if **spring-webmvc** is on the classpath, this annotation flags the application as a web application and activates key behaviors, such as setting up a **DispatcherServlet**.  
   - **@ComponentScan**: Tells Spring to look for other components, configurations, and services in the **com/example** package, letting it find the controllers.  
- The **main()** method uses Spring Boot’s **SpringApplication.run()** method to launch an application.  
- This web application is 100% pure Java and you did not have to deal with configuring any plumbing or infrastructure.  
- Now you need to modify the simple class that the Initializr created for you. To get output (to the console), you need to set up a logger. Then you need to set up some data and use it to generate output. The following listing shows the finished **AccessingDataJpaApplication** class.  
   ```
   package com.example.accessingdatajpa;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.context.annotation.Bean;

   @SpringBootApplication
   public class AccessingDataJpaApplication {

     private static final Logger log = LoggerFactory.getLogger(AccessingDataJpaApplication.class);

     public static void main(String[] args) {
       SpringApplication.run(AccessingDataJpaApplication.class);
     }

     @Bean
     public CommandLineRunner demo(CustomerRepository repository) {
       return (args) -> {
         // save a few customers
         repository.save(new Customer("Jack", "Bauer"));
         repository.save(new Customer("Chloe", "O'Brian"));
         repository.save(new Customer("Kim", "Bauer"));
         repository.save(new Customer("David", "Palmer"));
         repository.save(new Customer("Michelle", "Dessler"));

         // fetch all customers
         log.info("Customers found with findAll():");
         log.info("-------------------------------");
         for (Customer customer : repository.findAll()) {
           log.info(customer.toString());
         }
         log.info("");

         // fetch an individual customer by ID
         Customer customer = repository.findById(1L);
         log.info("Customer found with findById(1L):");
         log.info("--------------------------------");
         log.info(customer.toString());
         log.info("");

         // fetch customers by last name
         log.info("Customer found with findByLastName('Bauer'):");
         log.info("--------------------------------------------");
         repository.findByLastName("Bauer").forEach(bauer -> {
           log.info(bauer.toString());
         });
         // for (Customer bauer : repository.findByLastName("Bauer")) {
         //  log.info(bauer.toString());
         // }
         log.info("");
       };
     }

   }
   ```
- The AccessingDataJpaApplication class includes a demo() method that puts the CustomerRepository through a few tests.   
- First, it fetches the CustomerRepository from the Spring application context. Then it saves a handful of Customer objects, demonstrating the save() method and setting up some data to work with. Next, it calls findAll() to fetch all Customer objects from the database. Then it calls findById() to fetch a single Customer by its ID. Finally, it calls findByLastName() to find all customers whose last name is "Bauer". The demo() method returns a CommandLineRunner bean that automatically runs the code when the application launches.  

## 3- Build an executable JAR
- You can run the application from the command line with Gradle or Maven. You can also build a single executable JAR file that contains all the necessary dependencies, classes, and resources and run that. Building an executable jar makes it easy to ship, version, and deploy the service as an application throughout the development lifecycle, across different environments, and so forth.  
- If you use Gradle, you can run the application by using **./gradlew bootRun**. Alternatively, you can build the JAR file by using **./gradlew build** and then run the JAR file, as follows:  
   ```
   java -jar build/libs/gs-accessing-data-jpa-0.1.0.jar
   ```
- If you use Maven, you can run the application by using **./mvnw spring-boot:run**. Alternatively, you can build the JAR file with **./mvnw clean package** and then run the JAR file, as follows:  
   ```
   java -jar target/gs-accessing-data-jpa-0.1.0.jar
   ```
- When you run your application, you should see output similar to the following:  
   ```
   == Customers found with findAll():
   Customer[id=1, firstName='Jack', lastName='Bauer']
   Customer[id=2, firstName='Chloe', lastName='O'Brian']
   Customer[id=3, firstName='Kim', lastName='Bauer']
   Customer[id=4, firstName='David', lastName='Palmer']
   Customer[id=5, firstName='Michelle', lastName='Dessler']

   == Customer found with findById(1L):
   Customer[id=1, firstName='Jack', lastName='Bauer']

   == Customer found with findByLastName('Bauer'):
   Customer[id=1, firstName='Jack', lastName='Bauer']
   Customer[id=3, firstName='Kim', lastName='Bauer']
   ```
# Spring Data Repositories  
- we will start with the **JpaRepository** – which extends **PagingAndSortingRepository** and, in turn, the **CrudRepository**.  
- Each of these defines its own functionality:  
   - **CrudRepository** provides CRUD functions.  
   - **PagingAndSortingRepository** provides methods to do pagination and sort records.  
   - **JpaRepository** provides JPA related methods such as flushing the persistence context and delete records in a batch.  
- And so, because of this inheritance relationship, the **JpaRepository** contains the full API of **CrudRepository** and **PagingAndSortingRepository**.  
- When we don't need the full functionality provided by JpaRepository and PagingAndSortingRepository, we can simply use the CrudRepository.  
- We'll start with a simple Product entity:  
   ```
   @Entity
   public class Product {

       @Id
       private long id;
       private String name;

       // getters and setters
   }
   ```
- And let's implement a simple operation – find a Product based on its name:  
   ```
   @Repository
   public interface ProductRepository extends JpaRepository<Product, Long> {
       Product findByName(String productName);
   }
   ```
## CrudRepository
- the code for the CrudRepository interface:  
   ```
   public interface CrudRepository<T, ID extends Serializable>
     extends Repository<T, ID> {

       <S extends T> S save(S entity);

       T findOne(ID primaryKey);

       Iterable<T> findAll();

       Long count();

       void delete(T entity);

       boolean exists(ID primaryKey);
   }
   ```
- the typical CRUD functionality:  
   - save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch.  
   - findOne(…) – get a single entity based on passed primary key value.  
   - findAll() – get an Iterable of all available entities in database.  
   - count() – return the count of total entities in a table.  
   - delete(…) – delete an entity based on the passed object.  
   - exists(…) – verify if an entity exists based on the passed primary key value.  

## PagingAndSortingRepository  
- this is the repository interface, which extends CrudRepository:  
   ```
   public interface PagingAndSortingRepository<T, ID extends Serializable> 
     extends CrudRepository<T, ID> {

       Iterable<T> findAll(Sort sort);

       Page<T> findAll(Pageable pageable);
   }
   ```
- This interface provides a method findAll(Pageable pageable), which is the key to implementing Pagination.  
- When using Pageable, we create a Pageable object with certain properties and we've to specify at least:  
   1- Page size  
   2- Current page number  
   3- Sorting  
- So, let's assume that we want to show the first page of a result set sorted by lastName, ascending, having no more than five records each. This is how we can achieve this using a PageRequest and a Sort definition:  
   ```
   Sort sort = new Sort(new Sort.Order(Direction.ASC, "lastName"));
   Pageable pageable = new PageRequest(0, 5, sort);
   ```

## JpaRepository  
- this is the JpaRepository interface:  
   ```
   public interface JpaRepository<T, ID extends Serializable> extends
     PagingAndSortingRepository<T, ID> {

       List<T> findAll();

       List<T> findAll(Sort sort);

       List<T> save(Iterable<? extends T> entities);

       void flush();

       T saveAndFlush(T entity);

       void deleteInBatch(Iterable<T> entities);
   }
   ```
- **findAll()** – get a List of all available entities in database  
- **findAll(…)** – get a List of all available entities and sort them using the provided condition  
- **save(…)** – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch  
- **flush()** – flush all pending task to the database   
- **saveAndFlush(…)** – save the entity and flush changes immediately  
- **deleteInBatch(…)** – delete an Iterable of entities. Here, we can pass multiple objects to delete them in a batch  
- Clearly, above interface extends PagingAndSortingRepository which means it has all methods present in the CrudRepository as well.



   




- 
