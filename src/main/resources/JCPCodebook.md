# JPA (Java Persistence API)

## Transaction Management with an Entity-Manager
---
entityManager.**getTransaction()**.**begin()**;  
entityManager.**persist(<some-entity>)**;  
entityManager.**getTransaction()**.**commit()**;  
entityManager.**clear()**;  

SomeEntity entity = entityManager.find(SomeEntity.class, 1);

---

## @OneToOne's fetch type is EAGER by default
Lists + Sets fetch type is LAZY by default

## Two types of Lazy Loading implementations
1. **Proxying the object** (default in Hibernate) by creating a Subclass of that object at runtime and overwrite the get methods.
   This is done by the JavaAssist lib.
2. **ByteCode Enhancement** (default in EclipseLink): Add special logic to the get methods inside the Java Bytecode

## LazyInitializationExcepiton


## Entity Lifecycle (EntityManager em)
- New -> em.**persist** -> Managed
- New -> em.**merge** -> Managed


- Managed ---> em.**remove** ---> Removed


- Managed ---> em.**find** ---> Managed
- Managed ---> query.**getResultList** ---> Managed
- Managed ---> query.**getSingleResult** ---> Managed


- Managed ---> em.**detach** ---> Detached
- Managed ---> em.**close** ---> Detached
- Managed ---> em.**clear** ---> Detached


- Detached ---> em.**merge** ---> Managed

### Transactions and Entity Manager

`Transactions` in JPA are managed using an `EntityManager`

## Query Data
- JDBC - Plain SQL statements
- JPQL - Query language based on Enitity names (not on table names)
- Criteria Query - Build a query with a Java based API
- Native Query - Plain SQL statements

#### Begin
`entityManager.getTransaction().begin();`  
Initiates a new transaction.

#### Persist
`entityManager.persist(<some-entity>);`  
Persists an entity to the database within the transaction.

#### Commit
`entityManager.getTransaction().commit();`  
Commits the transaction, making the changes permanent.

#### Clear
`entityManager.clear();`  
Clears the EntityManager from managed entities and detaches all entities from the persistence context.

### Fetch Types in Relationships

`@OneToOne`  
`EAGER`: associated entities are loaded immediately  
fetch type by default for *@OneToOne*  
`LAZY`: associated entities are loaded on demand  
fetch type by default for Lists and Sets in relationships

### Lazy Loading Implementations

#### Proxying the object
Default in Hibernate.  
It creates a subclass of the object at runtime, overriding get methods.  
Done by the JavaAssist library.
#### ByteCode Enhancement
Default in EclipseLink.  
Adds special logic inside Java bytecode's get methods for lazy loading.


### Entity Lifecycle
`New`: Newly created entity. Not yet associated with the database.  
`Managed`: Associated with an EntityManager.  
**em.persist** or **em.merge** transitions the entity to the managed state.  
`Removed`: Marked for removal from the database.
em.remove transitions the entity to the removed state.  
`Detached`: No longer associated with an EntityManager.

**em.detach**, **em.close**, or **em.clear** transitions the entity to the detached state.  


### Merging Detached Entities
Transitions detached entities back to the managed state using em.merge.

#### Entity State Specifics
There are some Managed State Advantages:

- Changes Automatically Tracked
- Only changes are written to the database on the next transaction commit.
- Lazy loading works.


#### Detached State: Lazy loading might not necessarily work.

### Entity Class Example: Address
```java
@Entity
@Table(name = "ADDRESS")
public class Address {
    Double latitude;
    Double longitude;
}
```

### OneToMany
```java
    // Delete dependent children, when the parent is going to be deleted (child-entites are orphans (=Waisen) then)
    @OneToMany(mappedBy="foo", orphanRemoval=true)
    private List<Bikes> bikes;
```

### ManyToMany
```java
// Corresponding relationship record deleted when entity record is deleted (relational integrity)
@Entity
public class Team {
    @ManyToMany(cascade = { CascadeType.MERGE, CascadeType.PERSIST }, mappedBy="teams")
    private List<Match> matches;
}
```

```java
@Entity
public class Match {
    @ManyToMany(cascade = { CascadeType.MERGE, CascadeType.PERSIST })
    @JoinTable(
            name="MATCH_TEAM",
            joinColumns={@JoinColumn(name="MATCH_ID", referencedColumnName="ID")},
            inverseJoinColumns={@JoinColumn(name="TEAM_ID", referencedColumnName="ID")}
    )
    private List<Team> teams;
}
```
