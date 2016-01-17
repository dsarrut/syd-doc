

# Quick start

-----------------------------------------------------
## 1. Create a database

First create a database. It is a single file associated with a single folder that contains the images (dicom or mhd/raw formats). This folder must be in the same directory than the database. Let's start with the default database schema named *StandardDatabase*:

```bash
sydCreateDatabase StandardDatabase test.db test-data/
```

See the structure of the database with:
```
sydFind -db test.db
```

Make this database the default database by setting a environment variable:
```bash
export SYD_CURRENT_DB=/home/foobar/test.db

 # Now you do not need to specify the database on the command line:
sydFind
```

The list of available tables is displayed.


Populate the database with some patients. The fields associated with a patient are the name, the study_id number, the weight (in kg). It will be possible later to add more patients or to update information.

```sh
sydInsert -v1 Patient john 1 70
sydInsert -v1 Patient smith 2 99
```

Before starting to include DICOM images, it is generally useful to define the injections associated with the PET or SPECT acquisitions. An injection is defined as a radionuclide, the date of the injection and a quantity (in MBq). The option `-v1`defined the verbosity. Level 1 (`-v1`) usually only display the result of a command ; level zero (by default or `-v0`) remove output. Higher level (usually) print more information.

```sh
sydUpdateRadionuclide In-111
sydUpdateRadionuclide Y-90
sydFind -d Radionuclide
sydInsert -v1 Injection john In-111 "2013-02-12 10:16" 206.43
sydInsert -v1 Injection smith Y-90  "2016-06-14 14:31" 80.12
```

Note that some radionuclide are available, but other may be created. Information associated with the injection may be modified later. We are now ready to insert DICOM images in the database, associated with the already defined patient and injection. This is performed with:

----------------------------------------------------------
## 2. Insert DICOM images

```sh
sydInsertDicom -v1 john In-111 folder_dicom/
```

In that example, the folder  `folder_dicom` is automatically parsed and the found dicom series are added to the patient. `john`, and associated with a Indium-111 injection (if several Indium injections exist for this patient, an error occur and you should specify which one). 

```sh
 # Print all patients
sydFind Patient

 # Print all injection
sydFind Injection

 # Print the tables in the database
sydFind

 # Find the dicomserie that contains the words "SPECT" and "TOMO" somewhere
 # in their description, but does not contain the word "PLANAR".
sydFind DicomSerie SPECT TOMO -e PLANAR

 # Same as previous but only display the ids as a raw line
sydFind DicomSerie SPECT TOMO -e PLANAR -l
```

----------------------------------------------------------
## Convert DICOM to raw images (mhd/raw)

The first key point is to keep the initial DICOM images and perform image processing tasks on copy images in mhd/raw file format. The second point is to label images with tags to retrieve them easily.

```
 # Convert the DicomSerie number 123 into a mhd image. Add two tags to the image
 sydInsertImageFromDicom -v1 123 --tag "spect" --tag "scaled"

 # Retrive some image
 sydFind Image spect
```

You can use `sydUpdateImage` to change the tags, the pixel unit, or perform basic operations.


----------------------------------------------------------
## Conclusion


* SYD manages a *single file* database, with raw image data in a single associated folder.
* Database is organized with tables such as `Patient`, `Injection`, `DicomSerie` and `Image`. Every records has a unique id. This id is and will be unique: if the record is deleted, they will never be a new record of the same table with the same id in the same database.
* Use ```sydFind``` to select some images and find their id.
* Images may be easily browsed in the file system.
* Date are managed by a string composed like "2016-06-14 14:31"


<!-- ## Select and convert DICOM to ITK ready -->

<!-- The previous step may be considered as a first step to define more or less "stable" data in the database. Now image processing and computation could be performed and will be also stored in the database. We consider that DICOM image are reaad-only, every process will be performed on image stored in another format. ITK Metafile (mhd/raw) is the default image format. -->
