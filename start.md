

# How to start ?

## Create a database

First create a database. It is a simple file associated with a folder that will contains the images. Note that this folder must be in the same directory than the database. Let's start with the default database type (or schema), named StandardDatabase:

```bash
sydCreateDatabase StandardDatabase test.db test-data/
```

Make this database the default database by setting a environment variable:
````bash
export SYD_CURRENT_DB=/home/foobar/test.db
```

Then populate the database with some patients. The fields associated with a patient are the name, the study_id number, the weight (in kg). It will be possible later to add more patients or to update information.

```sh
sydInsert -v1 Patient john 1 70
sydInsert -v1 Patient smith 2 99
```

Before starting to include DICOM images, it is generally useful to define the injections associated with the PET, SPECT acquisitions. An injection is defined as a radionuclide, the date of the injection and a quantity (in MBq). The option `-v1`defined the verbosity. Level 1 (`-v1`) usually only display the result of a command ; level zero (by default or `-v0`) remove output. Higher level (usually) print more information.

```sh
sydUpdateRadionuclide In-111
sydUpdateRadionuclide Y-90
sydFind -d Radionuclide
sydInsert -v1 Injection john In-111 "2013-02-12 10:16" 206.43
sydInsert -v1 Injection smith Y-90  "2016-06-14 14:31" 80.12
```

Note that some radionuclide are available, but other may be created. Information associated with the injection may be modified later. We are now ready to insert DICOM images in the database, associated with the already defined patient and injection. This is performed with:

```sh
sydInsertDicom -v1 john In-111 folder_dicom/
```

In that example, the patient is defined by his name (it can alternatively be his study_id, both are unique field). The radionuclide name is used to retrieve the injection associated with this patient. If several injections with the same radionuclide have been associated to the same patient, an error occur: the injection must in that case be specified by its id. Every records in the database have an associated, unique id. Type `sydDump Injection` to list the injections and their id.

The given `folder_dicom` is scanned for dicom files. Images are sorted to create DicomSerie  associated with the patient and the injection. All the files are *copied* to the database folder, so it could takes some times if a lot of images are found. Images are roughly sorted in this folder according to the following hierarchy: `patient_name/date/modality`. Initial filenames are conserved. The main idea here is to keep things simple: you can still navigate to the folder hierarchy with any DICOM viewer. Syd will the provide tools for analysis and conversion of the DICOM images.

Once your database has been populated with some data, you may display some information with `sydDump`and `sydFind`. `sydDump` display information on elements (records) of given table (Patient, Injection, DicomSerie etc). The elements that will be displayed are designated by their id. The `sydFind` tool perform a simple search like the shell command `grep` and retrieve the id of the elements that match the pattern. Some examples below:

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

You can delete a record by specifying its id and its table: `sydDelete test.db DicomSerie 2`. Such command will permanently delete the element ith id=2 in the database but also the associated files in the database folder. Like previously with `sydDump`, `sydDelete`may be piped after a `sydFind`command, allowing to delete the records that match a pattern.

*FIXME* how to update

Now, using the `sydCreateDatabase`, `sydInsert`, `sydInsertDicom`, `sydDump`, `sydFind`, `sydDelete`, we know how to create an database. Nothing really fun here except that, everything is now accessible via a database and a simple file system.

Delete:
- Patient:  delete also all elements that have a patient as FK (Injection->DicomSerie->DicomFile->File)
- Radionuclide: delete Injection->DicomSerie->DicomFile->File
- DicomSerie: delete DicomFile->File
- DicomFile: delete File
- File:



## Select and convert DICOM to ITK ready

The previous step may be considered as a first step to define more or less "stable" data in the database. Now image processing and computation could be performed and will be also stored in the database. We consider that DICOM image are reaad-only, every process will be performed on image stored in another format. ITK Metafile (mhd/raw) is the default image format.
