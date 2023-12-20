# Data Serialization

## Aggiunta della dipendenza di Jackson nel file `pom.xml` (utilizzando Maven)

Aggiungi la dipendenza di Jackson al tuo progetto nel file `pom.xml`:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.3</version>
</dependency>
```

Modifica la tua classe Spesa ( classe `POJO`) aggiungendo l'annotazione `@JsonFormat` su qualsiasi campo Date che desideri formattare nel JSON in un formato specifico. Ad esempio
```java
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;

public class Spesa {
private int idSpesa;

    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
    private Date giorno;

    // Altri campi e metodi della classe Spesa
}
```
Con questa modifica, quando oggetti Spesa vengono serializzati in JSON, il campo giorno verrà formattato nel formato specificato ("yyyy-MM-dd").