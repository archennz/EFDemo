# EF 6

EF is a Object-Relational Mapping Framework (ORM).

## Why use ORM?
* SQL sucks - its easier to manipulate domain objects, LINQ
* Database Abstraction Layer: provide interface for the datastore, not coupled with the "implementation"
* Abstractions makes it easier to write correct code: migrations, transactions, tripping over SQL

## How does EF work?
* Model: Shape of your entity classes
* Context Object (`DBContext`): 
  * session with the DB
  * query and save data

### DbContext: 
* model
* represents a unit of work
* change tracker: add when returned from query or added/attached to the context

### Model:

Example Contoso University:

![Untitled](data-model-diagram.png)

* Consists of POCO's with some configuration as Data annotations / fluent API
* By convention, a property named Id or <typename>Id = primary key of entity 

```c#
    [Key]
    public string LicensePlate { get; set; }

// OR 
    modelBuilder.Entity<Car>()
      .HasKey(c => c.LicensePlate);
```
* exclude columns, different column names, length restrictions
* configuration dictates what gets retrieved you query from your DbContext

### Querying in DbContext:
* We can query using LINQ
* When we retrieve an entity, we can add this to the change tracker, 
if tracked, any changes will be saved when `SaveChanges` are called. 
* Tracking can be a performance hit, but cache is a performance boost?
* If the entity is not found in the context then EF will create new entity 
and attach it to the context
* 