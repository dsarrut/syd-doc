# API


- Initialization (db manager)
- Read a db
- StandardDatabase tables
- CRUD operations (Create/Read/Update/Delete) ; pointer/vector
- Dicom: insert
- Image: insert, tags, processing 



``` c++
  // Load plugin and retrieve pointer to the (unique) manager
  syd::PluginManager::GetInstance()->Load();
  syd::DatabaseManager* m = syd::DatabaseManager::GetInstance();

  // Get the database
  syd::StandardDatabase * db = m->Read<syd::StandardDatabase>(filename);
```

