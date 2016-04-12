

# Class Record

This is the base class of all elements recorded in the database. Any new table must inherit from this class.

**Public part**

It contains only one field: the ```id``` of the element.

It contains two types: ```pointer``` and ```vector```. A typical declaration is: ```syd::Record::pointer myrecord;``` (not allocated here) or ```syd::Record::vector myvector;```

It contains information about the table: ```GetTableName, GetSQLTableName, GetStaticTableName, GetStaticSQLTableName```. The static version is for use without instance.

Convenients functions, generally overloaded in daughter classes:
- ```ToString```: convert to a string. Use to print (std::cout << record;)
- ```Set(strings)```: set some elements (only for simple table)
- ```InitTable, DumpInTable```: helpers to print elements as an ascii table
- ```Check()```:

Must be set in inherited classes, thanks to the macro: ```TABLE_DEFINE``` or ```TABLE_DEFINE_I``` if the table inherit from another class than Record.

Every record store a pointer to the database where they are stored. This pointer is determined: either when the record is created with ```db->New```, or inserted with ```db->Ìnsert``` or queried with ```db->Query``` (thanks to the callback for the two lasts).


**Non-public part**

The function ```InitInheritance``` is used to store the tree of inherited sql table.

```Callbacks```: important to perform action when the record is created, deleted in the database. When a record become persistent (inserted in the db), the callback set the pointer to the database that contains the record. Database may thus be accessed with ```record->GetDatabase();```


# Class DatabaseDescription and TableDescription

Contains the list of tables/fields for a database, with correspondance between OO schema and sql schema. For example, fields that are vectors are stored with 2 tables in sql.

The SQL table name is stored with "" (because contains namespace in the name).

# CMakeLists.txt

Use:
```
find_package(SYD REQUIRED)
include(${SYD_USE_FILE})
```

The ```find_package```look for ```SYDConfig.cmake``` in binary folder. The variable ```SYD_DIR```could be used.

For every table (e.g. Patient), need to compile 3 files:
```
    sydPatient-odb.cxx
    sydPatient-schema.cxx
    sydPatient.cxx
```


# RecordWithHistory, RecordWithTags, RecordWithMD5Signature


A new table may (multiple) inherit from the following classes, that provide semi-automated functionalities. A class that define a new table need to always to inherit from ```syd::Record```. The odb concept wiich is used is "reused inheritance", it means that no additional table are created in the db, only the fields are inherited.

The callbacks must be explicitely called.

# Folder organisation

Folder ```core```:
 - Library: sydCore
 - Library: sydItkUtils
 - Library: sydFit

Folder ```common_db```:
  - Library: ```sydCommonDatabase```
  - Tools:

Folder ```std_db```:
  - Library: ```sydStandardDatabase```
  - Tools

# Migration

Change in ```sydVersion.h``` file. Automated migration should be done with any operation on a database.


- [Special case] When change a field from '' to 'UNIQUE', migration cannot be automated. Trial:
  - add new (unique) field
  - clear build folder, recompile
  - remove old (non unique) field
  - clear build folder, recompile
  - rename new field like old one, change version
  - clear build folder, recompile
  - NOT WORKING
