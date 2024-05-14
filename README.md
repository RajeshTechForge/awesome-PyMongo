# Awesome PyMongo [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

Welcome to the comprehensive PyMongo tutorial repository! This guide is designed to help you master the art of working with MongoDB, using the official Python driver, PyMongo.

Inspired by [awesome-python](https://github.com/vinta/awesome-python).

- [Awesome PyMongo ](#awesome-pymongo-)
  - [Introduction to MongoDB and PyMongo](#introduction-to-mongodb-and-pymongo)
    - [What is MongoDB?](#what-is-mongodb)
    - [What is PyMongo?](#what-is-pymongo)
    - [Key Features of PyMongo](#key-features-of-pymongo)
  - [Installation](#installation)
    - [Installing MongoDB](#installing-mongodb)
    - [Installing PyMongo](#installing-pymongo)
  - [Connecting to MongoDB](#connecting-to-mongodb)
  - [Accessing Database and Collection](#accessing-database-and-collection)
  - [Inserting Document](#inserting-document)
  - [Document ID](#document-id)
  - [Find Document In Database](#find-document-in-database)
  - [Getting Specific Data](#getting-specific-data)
  - [Showing Databases and Collections](#showing-databases-and-collections)
  - [Updating Document](#updating-document)
  - [Deleting Document](#deleting-document)


## Introduction to MongoDB and PyMongo

### What is MongoDB?

[MongoDB](https://www.mongodb.com/) is a popular open-source NoSQL database management system. It is designed to be flexible, scalable, and high-performance, making it well-suited for modern web applications and data-intensive workloads. Unlike traditional relational databases, MongoDB stores data in flexible, JSON-like documents, allowing for dynamic and unstructured data models. This makes it a popular choice for developers working with rapidly changing application requirements. 

### What is PyMongo?

[PyMongo](https://pymongo.readthedocs.io/en/stable/) is the official Python driver for MongoDB, the popular NoSQL database. It provides a high-level API for interacting with MongoDB databases from within a Python application. PyMongo allows developers to perform various database operations such as inserting, querying, updating, and deleting data, as well as managing database connections and configurations.

### Key Features of PyMongo

1. **Easy Integration**: PyMongo seamlessly integrates with the Python programming language, allowing developers to leverage the full power of MongoDB within their Python applications.

2. **Comprehensive API**: The PyMongo API provides a wide range of methods and functions for performing common database operations, making it easy to interact with MongoDB databases.

3. **Connection Management**: PyMongo handles the complexities of managing database connections, including connection pooling and automatic reconnection, allowing developers to focus on their application logic.

4. **Query Optimization**: PyMongo optimizes database queries by automatically converting Python data structures into the appropriate MongoDB query formats, ensuring efficient data retrieval.

5. **Data Mapping**: PyMongo provides a flexible data mapping system that allows developers to map Python objects to MongoDB documents, simplifying the process of working with complex data structures.

6. **GridFS Support**: PyMongo includes support for GridFS, a specification for storing and retrieving large files (such as images, videos, or other binary data) in MongoDB.

7. **Asynchronous Support**: PyMongo supports asynchronous programming using the `asyncio` and `motor` libraries, enabling developers to build high-performance, scalable applications.



## Installation

### Installing MongoDB

1. Download and install MongoDB from the official website: [MongoDB Installation Guide](https://docs.odb.com/manual/installation/)
2. Ensure that Mon is running on your system.

### Installing PyMongo

You can install it simply by using pip:  
```bash
pip install pymongo
```

Working in _virtual environment_ is recommended
```bash
$ cd <path to your file>
$ python -m venv .env
```

Now your virtual environment is ready with the name `.env`. Let's activate the virtual-env & then start coding:
```bash
source .env/bin/activate
```



## Connecting to MongoDB

First import pymongo,
```python
from pymongo import MongoClient
```

Connecting client:
```python
client = MongoClient('localhost', 27017)
print(client)

# OR you can use URL format like,

client = MongoClient('mongodb://localhost:27017/')
print(client)
```

> Now if you run the code, the output will be like this,
> ```python
> >>> MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True)
> ```
> Congrats 🎉, you have passed the first chapter.



## Accessing Database and Collection

The way to **Creating** and **Opening** existing _Database_ and _Collection_ are the same. If the given database or collection name matches with an existing one then it will open it. Otherwise, it will create a new database with the same name.  

But still, there are slight differences.  

If you are creating a new database or collection then _you have to insert a document into them_. Otherwise, a database or collection will not be created.

**Database**
```python
# for opening or creating a database
db = client['test-database']
```

**Collection**

```python
# for opening or creating collection
collection = db['test-collection']
```
> **NOTE**: _You have to give collection name in the defined database; here 'db'_

**Inserting**

Now you have to insert a document into the collection if you are creating a new one as mentioned before,

```python
# First arrange the values in a dictionary
my_dict = {"name": "HONEY", "language": "Python", "Platform": "GitHub"}
```
Here I am using `insert_one` function to insert the values. Another function to insert the value I will explain in next step.

```python
# insert the value containing dictionary
collection.insert_one(my_dict)  # Using "insert_one" function
```



## Inserting Document



Now we will learn how to insert a document into a collection of any database. PyMongo has two methods to insert documents:  
- `insert_one` (_for inserting only one document_)  
- `insert_many` (_for inserting multiple documents at once_)  

1. **Insert One**   

    First, we need to arrange the values into a dictionary; for example:
    ```python
    # our sample dictionary
    my_dict = {"name": "HONEY", "language": "Python", "Platform": "GitHub"}

    # Using "insert_one" function
    collection.insert_one(my_dict)  # inserting a file in collection ( here our "my_dict" dictionary )
    ```

2. **Insert Many**

    For this, put multiple dictionaries into a list, like this:
    ```python
    files = [
        {'name':'A', "language": "Java", "Platform": "Git"},
        {'name':'B', "language": "Java", "Platform": "GitHub"},
        {"name":"C", "language": "Python", "Platform": "GitHub"}
    ]
    # using insert_many function
    collection.insert_many(files) # inserting the data
    ```



## Document ID

MongoDB sets a **document id** for every document by itself in the database to manage them. But you can also give your custom document id (But it should be unique). To do so just add **'_id'** key and its **value** in the dictionary: `{ '_id':'CustomID' }` 

```python
my_dict = {
    "_id":"customID",
    "name":"D", 
    "language": "Javascript", 
    "Platform": "Git"
    }

# NOTE: "_id":"customID"
```

Now we can just insert it into the collection as mentioned earlier.



## Find Document In Database

PyMongo has a function called `find_one()` to find any document in the database. `find_one()` returns only **one** document. It doesn't have any function to find multiple documents at the same time. You have to use **for loop** for it.  


1. **Finding random document**
    ```python
    doc = collection.find_one()
    print(doc)
    ```

2. **Finding Specific document**
    ```python
    _filter = {'name':'A'}  # filter
    doc = collection.find_one(_filter)
    print(doc)
    ```

3. **Counting the no. of documents matching the filter**
    ```python
    _filter = {"language": "Python"}
    # counting the number of document/s of matching the filter
    count = collection.count_documents(_filter)
    print(count)
    ```



## Getting Specific Data

When we import data from a database then we may want to get only the required data, not all data from documents. For that, we have to show only the required data and have to hide other things. We can do this by using PyMongo.  

If we want to hide some Key's value, then we can do this just by adding a second argument containing keys in a dictionary.

There, the key's value will be `0` or `1`  
`0` 👉 it not will print  
`1` 👉 it will print  

_If we make the specific key or keys **1**, then others key automatically become **0**, **but not `_id` key**

**Example:**

```python
import pymongo

# Connecting client
client = pymongo.MongoClient("mongodb://localhost:27017")
db = client['test-database']  # Database
collection = db['test-collection']  # Collection


# Finding Specific document using filter
_filter = {'name':'A'}

# Showing only specific keys
one = collection.find_one(_filter, {'name':1})
# NOTE: here only '_id' & 'name' key will show

two = collection.find_one(_filter, {'name':1, '_id':0})
# Here only 'name' will show

print(one)
print(two)
```

From the above example, we can see that we have to make `_id` key 0. Because it doesn't become `0` by default.



## Showing Databases and Collections

Anytime we may need to check the list of created databases and collections. Luckily, PyMongo has the functionality to do it.  

1. **Databases**
    ```python
    # For listing all Databases
    AllDatabases = client.list_database_names()
    print(AllDatabases)
    ```

2. **Collections**

    First select the _database_ and then,
    ```python
    db = client['test-database']  # selected database

    # For listing all collections
    AllCollections = db.list_collection_names()
    print(AllCollections)
    ```



## Updating Document

When we are working with a database then, we have to update documents with time. Here we can do that using PyMongo.

_Pymongo have functions that update documents,_  
- `update_one()`,
- `update_many()`

> Here also we have to arrange updated values into a dictionary. But here you have to put this dictionary into another dictionary of key `"$set"`. Like: `{"$set": {"key": "updated value"}}`  

1. **Update One**

    Example:
    ```python
    # Changing A's language to Python (Previously it was Java)
    filter = {"name": "A"}  # creating a filter
    updated_values = {"$set":{"language": "Python"}}

    collection.update_one(filter, updated_values)  # Updating
    ```

2. Update Many

    > It changes every document's value which matches the filter

    Example: 
    ```python
    # Changing everyone's language to Python, whose Platform is GitHub

    _filter = {"Platform": "GitHub"}  # filter
    updated_values = {"$set":{"age":18}}

    up = collection.update_many(_filter, updated_values)  # Updating
    ```
    For counting the number of documents updated
    ```python
    print(up.modified_count)
    ```



## Deleting Document

For deleting documents also there are two functions:  
- `delete_one()` 👉 for deleting one document
- `delete_many()` 👉 for deleting multiple documents

1. **Delete One**
    ```python
    _filter = {"name":"A"}  # filter
    collection.delete_one(_filter)  # deleting
    ```

2. **Delete Many**

    > It deletes all documents that match the filter.
    ```python
    filter_2 = {"Platform": "Git"}  # filter
    dle = collection.delete_many(filter_2)  # deleting
    ```

    For counting the number of documents deleted
    ```python
    print(dle.deleted_count)
    ```
