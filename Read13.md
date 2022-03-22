# Working with Relationships in Spring Data REST
## One-to-One Relationship
- we will define two entity classes **Library** and **Address** having a **one-to-one** relationship, using the **@OneToOne** annotation. The association is owned by the Library end of the association:   
   ```
   @Entity
   public class Library {

       @Id
       @GeneratedValue
       private long id;

       @Column
       private String name;

       @OneToOne
       @JoinColumn(name = "address_id")
       @RestResource(path = "libraryAddress", rel="address")
       private Address address;
    
       // standard constructor, getters, setters
   }
   ```
   
   ```
   @Entity
   public class Address {

       @Id
       @GeneratedValue
       private long id;

       @Column(nullable = false)
       private String location;

       @OneToOne(mappedBy = "address")
       private Library library;

       // standard constructor, getters, setters
   }
   ```
- For that relation, we need to create a foreign key for **address_id** parameter in Library table. **OneToOne** annotation will create this relation for us. We should add this annotation to both entity classes that’s because this is a bidirectional relationship.  
- We want to create a column in Library table. So, we will use **JoinColumn** annotation in Library class. On the Address class, simply put **OneToOne** annotation and fill **mappedBy** field by variable name on the owning side. In this example, this value will be **‘address’**.  
- Rerun the application and you will see the address_id in columns and the foreign key in the properties of the Library table in the database.

## One-to-Many Relationship  
- A one-to-many relationship is defined using the @OneToMany and @ManyToOne annotations and can have the optional @RestResource annotation to customize the association resource.  
- Relationship between parent category and children categories.
- in our example we will put the **@ManyToOne** annotation in the Book class to create a foreign key **"library_id"** in this class by using **@JoinColumn** annotation.  
- And the Library class hold **@OneToMany** annotation and fill **mappedBy** field by variable name on the owning side. in this example this value will be **"library"**. 
   ```
   @Entity
   public class Book {

       @Id
       @GeneratedValue
       private long id;
    
       @Column(nullable=false)
       private String title;
    
       @ManyToOne
       @JoinColumn(name="library_id")
       private Library library;
    
       // standard constructor, getter, setter
   }
   ```
   ```
   public class Library {
 
      //...
 
      @OneToMany(mappedBy = "library")
      private List<Book> books;
 
      //...
 
   }
   ```

## Many-to-Many Relationship  
- A many-to-many relationship is defined using **@ManyToMany** annotation, to which we can add **@RestResource**.
   ```
   @Entity
   public class Author {

       @Id
       @GeneratedValue
       private long id;

       @Column(nullable = false)
       private String name;

       @ManyToMany(cascade = CascadeType.ALL)
       @JoinTable(name = "book_author", 
         joinColumns = @JoinColumn(name = "book_id", referencedColumnName = "id"), 
         inverseJoinColumns = @JoinColumn(name = "author_id", 
         referencedColumnName = "id"))
       private List<Book> books;

       //standard constructors, getters, setters
   }
   ```
   ```
   public class Book {
 
       //...
 
       @ManyToMany(mappedBy = "books")
       private List<Author> authors;
 
       //...
   }
   ```
   
- In this relation, we need to create a new table to handle ManyToMany relationship. This new table will hold foreign keys for both book_id and author_id fields. We will follow the relationship between books and authors by this table.  
- After running the code, a new table called **book_author** will be exist and specified foreign keys will be shown in properties of this new table.  

# Testing the Endpoints With TestRestTemplate  
- we will create a test class that injects a TestRestTemplate instance and defines the constants we will use:  
   ```
   @RunWith(SpringRunner.class)
   @SpringBootTest(classes = SpringDataRestApplication.class, 
     webEnvironment = WebEnvironment.DEFINED_PORT)
   public class SpringDataRelationshipsTest {

       @Autowired
       private TestRestTemplate template;

       private static String BOOK_ENDPOINT = "http://localhost:8080/books/";
       private static String AUTHOR_ENDPOINT = "http://localhost:8080/authors/";
       private static String ADDRESS_ENDPOINT = "http://localhost:8080/addresses/";
       private static String LIBRARY_ENDPOINT = "http://localhost:8080/libraries/";

       private static String LIBRARY_NAME = "My Library";
       private static String AUTHOR_NAME = "George Orwell";
   }
   ```
## Testing the One-to-One Relationship  
- Let's create a @Test method that saves Library and Address objects by making POST requests to the collection resources.  
- Then it saves the relationship with a PUT request to the association resource and verifies that it has been established with a GET request to the same resource:  
   ```
   @Test
   public void whenSaveOneToOneRelationship_thenCorrect() {
       Library library = new Library(LIBRARY_NAME);
       template.postForEntity(LIBRARY_ENDPOINT, library, Library.class);
   
       Address address = new Address("Main street, nr 1");
       template.postForEntity(ADDRESS_ENDPOINT, address, Address.class);
    
       HttpHeaders requestHeaders = new HttpHeaders();
       requestHeaders.add("Content-type", "text/uri-list");
       HttpEntity<String> httpEntity 
         = new HttpEntity<>(ADDRESS_ENDPOINT + "/1", requestHeaders);
       template.exchange(LIBRARY_ENDPOINT + "/1/libraryAddress", 
         HttpMethod.PUT, httpEntity, String.class);

       ResponseEntity<Library> libraryGetResponse 
         = template.getForEntity(ADDRESS_ENDPOINT + "/1/library", Library.class);
       assertEquals("library is incorrect", 
         libraryGetResponse.getBody().getName(), LIBRARY_NAME);
   }
   ```
## Testing the One-to-Many Relationship  
- Let's create a @Test method that saves a Library instance and two Book instances, sends a PUT request to each Book object's /library association resource, and verifies that the relationship has been saved:  
  ```
  @Test
  public void whenSaveOneToManyRelationship_thenCorrect() {
      Library library = new Library(LIBRARY_NAME);
      template.postForEntity(LIBRARY_ENDPOINT, library, Library.class);

      Book book1 = new Book("Dune");
      template.postForEntity(BOOK_ENDPOINT, book1, Book.class);

      Book book2 = new Book("1984");
      template.postForEntity(BOOK_ENDPOINT, book2, Book.class);

      HttpHeaders requestHeaders = new HttpHeaders();
      requestHeaders.add("Content-Type", "text/uri-list");    
      HttpEntity<String> bookHttpEntity 
        = new HttpEntity<>(LIBRARY_ENDPOINT + "/1", requestHeaders);
      template.exchange(BOOK_ENDPOINT + "/1/library", 
        HttpMethod.PUT, bookHttpEntity, String.class);
      template.exchange(BOOK_ENDPOINT + "/2/library", 
        HttpMethod.PUT, bookHttpEntity, String.class);

      ResponseEntity<Library> libraryGetResponse = 
        template.getForEntity(BOOK_ENDPOINT + "/1/library", Library.class);
      assertEquals("library is incorrect", 
        libraryGetResponse.getBody().getName(), LIBRARY_NAME);
  }
  ```
## Testing the Many-to-Many Relationship  
- For testing the many-to-many relationship between Book and Author entities, we will create a test method that saves one Author record and two Book records.  
- Then it sends a PUT request to the /books association resource with the two Books‘ URIs and verifies that the relationship has been established:  
   ```
   @Test
   public void whenSaveManyToManyRelationship_thenCorrect() {
       Author author1 = new Author(AUTHOR_NAME);
       template.postForEntity(AUTHOR_ENDPOINT, author1, Author.class);

       Book book1 = new Book("Animal Farm");
       template.postForEntity(BOOK_ENDPOINT, book1, Book.class);

       Book book2 = new Book("1984");
       template.postForEntity(BOOK_ENDPOINT, book2, Book.class);

       HttpHeaders requestHeaders = new HttpHeaders();
       requestHeaders.add("Content-type", "text/uri-list");
       HttpEntity<String> httpEntity = new HttpEntity<>(
         BOOK_ENDPOINT + "/1\n" + BOOK_ENDPOINT + "/2", requestHeaders);
       template.exchange(AUTHOR_ENDPOINT + "/1/books", 
         HttpMethod.PUT, httpEntity, String.class);

       String jsonResponse = template
         .getForObject(BOOK_ENDPOINT + "/1/authors", String.class);
       JSONObject jsonObj = new JSONObject(jsonResponse).getJSONObject("_embedded");
       JSONArray jsonArray = jsonObj.getJSONArray("authors");
       assertEquals("author is incorrect", 
         jsonArray.getJSONObject(0).getString("name"), AUTHOR_NAME);
   }
   ```

### Resources:  
- [Working with Relationships in Spring Data REST](https://www.baeldung.com/spring-data-rest-relationships)
- [Integration Testing in Spring](https://www.baeldung.com/integration-testing-in-spring)
