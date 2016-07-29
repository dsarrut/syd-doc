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

To insert dicom files in the db, use the class `syd::DicomSerieBuilder`.

```C++
  syd::StandardDatabase * db = ...     // get a db
  std::vector<std::string> files = ... // get some files
  syd::Patient::pointer patient = ...  // get a patient

  syd::DicomSerieBuilder builder(db);
  builder.SetPatient(patient); // patient is a syd::Patient::pointer
  builder.SetForcePatientFlag(true);
  for(auto file:files) builder.SearchDicomInFile(file);
  builder.InsertDicomSeries();
```

<!--
    I do not manage to make bidirectional DicomFile <-> DicomSerie
-->




## 4. Image insertion

```syd::Image``` is maybe the most important class. It contains information about an image and is linked to a file in the db. The image file format is mhd/raw (ascii header and raw binary).

There are several ways to insert `syd::Image` into the db, from a (mhd) file or from a dicom that has been inserted in the db.

To insert from a file, `syd::ImageHelper::InsertMhdFiles`. Other useful functions are available in `syd::ImageHelper` as simple static functions.

```C++
    syd::Image::pointer output;
    db->New(output); // empty image
    output->patient = patient; // required
    syd::ImageHelper::InsertMhdFiles(output, filename);
```

To insert from a `syd::DicomSerie`, use a `syd::ImageFromDicomBuilder`. The builder has options.

```C++
    syd::DicomSerie::pointer dicom_serie = ... // get a dicom serie
    syd::ImageFromDicomBuilder builder;
    builder.SetInputDicomSerie(dicom_serie, "float");
    builder.Update();
    syd::Image::pointer image = builder.GetOutput();
```



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
