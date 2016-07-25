# API

- Initialization (db manager)
- Read a db
- StandardDatabase tables
- CRUD operations (Create/Read/Update/Delete) ; pointer/vector

- Dicom: insert
- Image: insert, tags
- Processing:


## 1. Open a database

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

## 2. Basics operations

CRUD operations (Create/Read/Update/Delete) are:

### Create and insert new records

``` c++
    syd::Patient::pointer p; // declare a pointer
    db->New(p); // Allocate

    // Allocate with table name
    syd::Record::pointer im = db->New("Image");
```

Warning: when the record is allocated by ```New```, it is not yet in the database (we say it is not persistent), and will be lost once the program ends. To insert in the database (to make it persistent), use the ```Insert``` function: ```db->Insert(p)```.

Every record inherit from the ```syd::Record``` class and has an ```id``` field.

The ```id``` of an record is only valid when the record is inserted.

Every record has a ```pointer``` type and a ```vector``` type. The first is a ```std::shared_ptr``` and the second ```std::vector```.


### Read some records

## 3. Dicom insertion

## 4. Image insertion

## 5. Image processing


# Common Database

- Record
- RecordWithHistory
- RecordWithTags
- RecordWithMD5Signature
- File *should be with MD5 ?*
- Tag


# Standard Database

- Patient
- Injection
- Radionuclide

- Dicom tables
 - DicomFile *should inherit from File ?*
 - DicomSerie

- Image tables
 - Image
 - PixelValueUnit
 - Calibration
 - ImageTransform

- Roi tables
 - RoiMaskImage [<-- Image]
 - RoiType
 - RoiStatistic

- TAC tables
 - TimePoints
 - FitResult

## Policy for empty field

When a field is not set, if it is a string, it must be set to `empty_value`. When the field is a NULL pointer, it is also printed with `empty_value`. Defined in `syd::Record` and `syd::PrintTable`.

By default all string in the db are set to `empty_value` with the pragam in `syd::Record.`.
