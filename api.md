# API


- Initialization (db manager)
- Read a db
- CRUD operations (Create/Read/Update/Delete) ; pointer/vector
- Dicom: insert
- Image: insert, tags, processing 



``` c++
  // Load plugin
  syd::PluginManager::GetInstance()->Load();
  syd::DatabaseManager* m = syd::DatabaseManager::GetInstance();

  // Get the database
  syd::StandardDatabase * db = m->Read<syd::StandardDatabase>(args_info.db_arg);
```

