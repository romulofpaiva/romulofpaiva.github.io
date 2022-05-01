### Jackson Project
[Jackson Project](https://github.com/FasterXML/jackson) provide a set of libraries for languages based on JVM platform for JSON parser/generator, data-binding and others.


#### Tips

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


<hr>

[Home](index.html)
