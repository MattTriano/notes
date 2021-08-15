# SQLAlchemy


## Tables

A relational database table is a collection of related data consisting of rows (or records) and columns (or features). Relational database table columns necessarily have a defined data type (which tells the database management system how much memory to allocate to each value in that column). SQLAlchemy `Table` objects have the ability to create corresponding database tables, so a `Table` object must contain information on its `Columns` as well as those `Column`s' data types.  

## Joins

**methods**
* `select(left_table).join(right_table)`
* `Select.join_from(left_table, right_table)`
* `select(left_table).join(right_table, isouter=True)`
* `select(left_table).join(right_table, full=True)`


## Set Operations

**methods**
* `union()`
* `union_all()`
* `intersect()`
* `intersect_all()`
* `except_()`
* `except_all()`

## ORM workflow

### Declarative style

1. Create a `declarative_base` object (to connect `MetaData` to `registry`?)
    1. ```python
    from sqlalchemy.orm import declarative_base
    Base = declarative_base()
    ```
2. Write at least one class that defines a `Table` and inherits that `Base` object.
    1. ```python
    class User(Base):
    __tablename__ = 'user_account'

    id = Column(Integer, primary_key=True)
    name = Column(String(30))
    fullname = Column(String)
    ```
3. Create the tables defined by that set of classes.\
    1. ```python
    Base.metadata.create_all(engine)
    ```



## Glossary

* **Engine** object:
    * The sqlalchemy tool for managing all interactions with a database. When used, it creates a `Connection` to a database, which we can use to execute some interaction with that database (eg query data, insert data, etc. 
    * module: sqlalchemy.engine

* **MetaData** object: 
    * A `MetaData` object is a dictionary (that, interestingly, is not publicly mutable) of `Table` objects keyed to a string of tha table's name.
    * module: sqlalchemy.schema

* **Table** object: 
    * A `Table` object maps to a table in a database.  
    * module: sqlalchemy.schema
    
* **Table: target** term:
    * When defining a `ForeignKeyConstraint` for a given `Table`, you will have to reference one or more columns of another `Table`. A lot of documentation will refer to that other `Table` as the **target**.
    
* **UNION** concept (set operation):
    * Allows for the concatenation of SELECT statements so that 