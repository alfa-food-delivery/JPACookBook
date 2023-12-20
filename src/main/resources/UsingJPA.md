# Using JPA

## Database Connection

### JPA Entities Interfaces
JPA implementation defines classes that implement these interfaces.  
When you use ObjectDB you work with instances of ObjectDB classes that implement these interfaces.  
Standard JPA interfaces are used your application is portable.

Java Persistence API (JPA) uses the following interfaces:
### EntityManagerFactory emf  
Supports instantiation of em instances.  
Manages resources efficiently (pool of sockets).  
Efficient way to construct multiple EntityManager instances for the specific database it was constructed for.  
Instantiation is a one time operation, then emf serves the entire application.  
### EntityManager - manager 
Connection to database represented by an EntityManager instance, that also provides functionalities
to perform db operations.  
Usually applications require multiple connections, so we can establish a separate db connection
using a *separate* em instance, for every HTTP request.  
Functions as a factory for Query instances, needed for executing queries on the database.
### EntityTransaction - transaction 
Operations to modify the content of the database needs active transactions and so et instances.  
Obtained by the EntityManager.

Connection to database represented by an EntityManager instance, that also provides functionalities to perform db operations.  
Usually applications require multiple connections, so we can establish a separate db connection
using a *separate em instance for every HTTP request*.  
Funzione create di esempio :
```java
public class EntityJPA implements IDao<User,Long>{

    private static final EntityManagerFactory emFactoryObj;
    private static final String PERSISTENCE_UNIT_NAME = "esempioPU";
    static { emFactoryObj = Persistence.createEntityManagerFactory(PERSISTENCE_UNIT_NAME); }

    // Metodo statico usato per ritornare un 'EntityManager' Object
    public static EntityManager getEntityManager() { return emFactoryObj.createEntityManager(); }
    //CRUD BASICS - Create
    public Long create(User user) throws DaoException {
        if (user == null || user.getEmail() == null || user.getPasswordHash() == null) {
            throw new DaoException("User, mail e password sono obbligatori");
        }
        EntityTransaction transaction = null;
        EntityManager manager = null;
        try { //ASSEGNO manager e transaction
            manager = getEntityManager();
            transaction = manager.getTransaction();
            transaction.begin(); //TX BEGIN
            // ... operazioni
            user.setId(null); // Assicuro che l'ID sia nullo per garantire la generazione automatica
            manager.persist(user);
            transaction.commit(); //TX COMMIT
        } catch (Exception e) {
            if (transaction != null && transaction.isActive()) { transaction.rollback(); }
            e.printStackTrace();
            throw new DaoException(e);
        } finally { manager.close(); /*EM CLOSE*/}

        return user.getId();
    }
}
```

### @JoinColumn annotation

#### @ManyToOne - Difference between name and referencedColumnName
Supponi di avere due tabelle
```roomsql 
-- DIPARTIMENTI
CREATE TABLE dipartimenti (
	id_dipartimento INT PRIMARY KEY AUTO_INCREMENT,
	nome_dipartimento VARCHAR(100) NOT NULL UNIQUE
);
-- DIPENDENTI
CREATE TABLE dipendenti (
	id_dipendente INT PRIMARY KEY AUTO_INCREMENT,
	id_dipartimento_fk INT NOT NULL,
	salario DECIMAL(10,2),
	FOREIGN KEY (id_dipartimento_fk) REFERENCES dipartimenti(id_dipartimento),
);
```

Allora le tue classi Dipendente e Dipartimento avranno la seguente forma: 
```java
@Entity
@Table(name = "dipendenti")
public class Dipendente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id_dipendente")
    private int idDipendente;

    private double salario;

    @ManyToOne
    @JoinColumn(name = "id_dipartimento_fk", referencedColumnName = "id_dipartimento")
    private Dipartimento dipartimento;

    // Constructors, getters, and setters
    // ...
}
```

```java
import javax.persistence.*;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "dipartimenti")
public class Dipartimento {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id_dipartimento")
    private int idDipartimento;

    @Column(name = "nome_dipartimento")
    private String nomeDipartimento;

    @OneToMany(mappedBy = "dipartimento")
    private Set<Dipendente> dipendenti = new HashSet<>();

    // Constructors, getters, and setters
    // ...
}
```

### OneToMany(mappedBy = "something")
Indica il nome del campo nella **classe associata** che gestisce la relazione con la classe corrente.  
`private Dipartimento dipartimento;` è il campo nella classe Dipendente  che gestisce la relazione tra le due entità.