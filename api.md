# API


- Initialization (db manager)
- Read a db
- StandardDatabase tables
- CRUD operations (Create/Read/Update/Delete) ; pointer/vector
- Dicom: insert
- Image: insert, tags, processing


## Start to open a db

To be able to use syd and load a database, the following line **must** be use prior to anything else.

``` c++
  syd::PluginManager::GetInstance()->Load();
```

The PluginManager looks for available database types that can be found in the ```SYD_PLUGIN``` environment variable.

To open a database, just call ```Open``` from the database manager (singleton):

``` c++
    syd::DatabaseManager* m = syd::DatabaseManager::GetInstance();
    // If you know the db type :
    syd::StandardDatabase * db = m->Open<syd::StandardDatabase>(filename);
    // If you don't know:
    syd::Database * gdb = m->Open(filename);
```
