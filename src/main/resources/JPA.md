# JPA

## Introduzione

Quando vogliamo costruire servizi RESTfull con classi più complesse, riscontriamo diversi problemi in quanto
ci ritroviamo a gestire oggetti potenzialmente molto complessi, tabelle collegate e così via.

JPA (Java Persistence API) serve a semplificare la comunicazione tra un db a rappresentazione relazionale a fronte di 
un modello client orientato agli oggetti .

JPA si pone come obiettivo di implementare un `ORM - Object Relational Mapping` , con cui costruisco un oggetto separato dalla rappresentazione relazionale delle tabelle

## Annotations

Ci sono oltre 80 annotazioni che consentono di semplificare la comunicazione con il database e la creazione di servizi SOAP o REST.

Per gestire una **relazione molti-a-molti** tra utenti e ruoli nel contesto di classi Java, è consigliabile utilizzare
un approccio `ORM` (di mapping oggetti-relazionali) insieme a un framework di **persistenza dati** come `Hibernate` 
o `JPA` (Java Persistence API).

Questo permette di mappare in modo efficiente le relazioni complesse del database in oggetti Java.
Ti sarà più facile gestire le relazioni complesse tra le tue entità db, senza dover scrivere direttamente
le query SQL.

Mapping con annotations o file xml, più verboso. Dovrò comunque fare il *persistence.xml* che consente di separare i compiti e le logiche.  
Contiene anche le info dell'origine di dati , ovvero di connessione al db analogamente al **config.properties** visto in precedenza:
- properties : <property name = "javax." / "jakarta." 
- definizione driver

### Entity Classes: Utente and Ruolo

#### Utente Class:
```java
import javax.persistence.*;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "utenti")
public class Utente {

    @Id
    @GeneratedValue()
    @Column(name="id_utente")
    private Long idUtente;

    private String username;

    // Altri attributi e getter/setter

    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(name = "utenti_ruoli",
            joinColumns = @JoinColumn(name = "id_utente"),
            inverseJoinColumns = @JoinColumn(name = "id_ruolo"))
    private Set<Ruolo> ruoli = new HashSet<>();

    // Altri metodi
}
```
#### Ruolo Class:
```java
@Entity
@Table(name = "ruoli")
public class Ruolo {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long idRuolo;

    private String nomeRuolo;

    // Altri attributi e getter/setter

    @ManyToMany(mappedBy = "ruoli")
    private Set<Utente> utenti = new HashSet<>();

    // Altri metodi
}
```

In questo esempio osserviamo le seguenti annotations.

Nella classe Utenti :

`@Entity` : indica che le classi sono entità gestite da JPA, rappresenta il mapping con una tabella del database.  
Deve essere gestita con la persistenza, ovvero salvata in una o più tabelle di un db relazionale.  
Tutti gli attributi saranno persistenti, altrimenti per casi eccezionali va specificato. 
`@Table` : nome della tabella di riferimento

`@ManyToMany` : definisce la relazione molti-a-molti tra Utente e Ruolo

`@JoinTable` : definisce il mapping delle relazioni nella classe utente, specificando
- *name* = nome della tabella associativa
- *joinColumns* = `@JoinColumn` ( name = nome della colonna utenti )
- *inverseJoinColumns* = `@JoinColumn` ( name = nome della colonna ruoli )

Nella classe Ruolo :

`mappedBy` : direttiva che specifica l'attributo corrispondente nella classe Utente, in questo caso ruoli


Puoi in seguito creare DAO (Data Access Object) che siano in grado di sfruttare questa configurazione per effettuare
operazioni di persistenza e recupero dei dati relativi agli utenti e ai ruoli.

Successivamente, basandoti su questi DAO, puoi creare i servizi RESTful per accedere ai dati dell'utente basati su
queste classi.


`@Transient` : campo della classe che non sarà salvato sul nostro server db