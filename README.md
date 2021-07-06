## Rômulo Ferreira Paiva's Knowledge Base

Hi Folks!

I'm a Software Engineer that have been working with software development at about 25 years in Educational and Finantial industries.

My goal with this site is document and share articles, experiences, tips and tricks during my journey to build a knowledge base that could help me and others.

I hope you enjoy it!

Best Regards,

Rômulo Ferreira Paiva.

https://www.linkedin.com/in/romulofpaiva/

### Java
Java is a multi-platform programming language. It means that it allows writing programs that can be executed at any platform.

The secret power is the Java Virtual Machine (JVM). There are JVMs designed for most of platforms (Linux, Windows, etc.) that compile the bytecodes (pre-compiled Java code) for the Operational System (OS).

Because of it's characteristics, we can say that with Java you can "write once and run anywhere".

### Jackson Project
Do you know the Jackson Project (https://github.com/FasterXML/jackson)?

You can find cool tutorials at https://www.baeldung.com/jackson.

#### Useful Tips

##### Filtering properties
The @JsonFilter annotation allows specification of a filter to use during serialization.

So, it's possible to have different methods using the same entity and generating JSONs with some or all properties.

```
@Entity
@JsonFilter("filter")
public class Book {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @NotBlank
  private String title;

  @NotBlank
  @Length(min = 1, max = 500)
  private String summary;

  ...
}
  
  
public BookController {
  ...
  
  @GetMapping
  public Object listBooks() throws JsonProcessingException {

    TypedQuery<Livro> typedQuery = entityManager.createQuery("SELECT l FROM Book l", Book.class);

    FilterProvider filters = new SimpleFilterProvider()
        .addFilter("filter", SimpleBeanPropertyFilter.filterOutAllExcept("id", "title"));

    ObjectMapper mapper = new ObjectMapper();

    return mapper.writer(filters).writeValueAsString(typedQuery.getResultList());
  }

  @GetMapping("/{id}")
  public Object listOneBook(@PathVariable Long id) throws JsonProcessingException {

    Book book = entityManager.find(Book.class, id);

    if(book == null) {
      return ResponseEntity.notFound().build();
    }

    FilterProvider filters = new SimpleFilterProvider()
        .addFilter("filter", SimpleBeanPropertyFilter.serializeAll());

    ObjectMapper mapper = new ObjectMapper();

    return mapper.writer(filters).writeValueAsString(book);
  }
}
```


### Markdown
What about Markdown (https://guides.github.com/features/mastering-markdown/)?

