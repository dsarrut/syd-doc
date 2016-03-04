# API


- Initialization (db manager)
- Read a db
- StandardDatabase tables
- CRUD operations (Create/Read/Update/Delete) ; pointer/vector
- Dicom: insert
- Image: insert, tags, processing 


## Initialization

To be able to use syd and load a database, the following lines must be use prior to anything else. 

``` c++
  // Load plugin and retrieve pointer to the (unique) manager
  syd::PluginManager::GetInstance()->Load();
  syd::DatabaseManager* m = syd::DatabaseManager::GetInstance();
  // Get the database
  syd::StandardDatabase * db = m->Read<syd::StandardDatabase>(filename);
```

The PluginManager looks for available database types that can be found in the SYD_PLUGIN environment variable.

To open a database
