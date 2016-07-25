# Readme

![syd](images/logo-syd.png "SYD")


SYD is a **C++ toolkit** and a **command line** environment to perform image processing tasks on a **oriented-object database** of medical images. 

SYD is initially developed to manage SPECT/PET/CT images in the field of Target Radionuclide Therapy and nuclear medicine. But it could also be useful in other domains. 

SYD allows to manage databases of images and related information (patient, acquisition date, associated injection etc). It can store original DICOM images, but also all intermediate images needed to perform computation (scaled image, cropped images, combined images etc). 


**Features**
- **Database = 1 file + 1 folder**. There is *no database server*. The db is a simple SQL file plus a raw folder of images. Hence, images may be accessed via the conventional file system. 
- The **images** are stored as DICOM or mhd/raw file formats
- **API**: Object-oriented (OO) database management (forget SQL). You can retrieve, update or store images, patient, injection etc in the db with simple OO concepts. 
- **Command line**: SYD also provide a number of command line tools to perform basic operations on the db, such as querying, inserting or deleting some items
- SYD try to no reinvent the wheel, it is linked with [ITK](www.itk.org) (image processing), [Elastix](http://elastix.isi.uu.nl/) (image registration), [Gate](www.opengatecollaboration.org) (image and dose simulation).

** Limitations**
- no GUI

**Examples:**

Find all images with 'spect' and 'john' in the description:

``` bash
$ sydFind Image spect john

```

Compute the mean (and other statistic) in a roi called 'liver' for all found images:

```
$ sydFind Image spect john -l | sydInsertRoiStatistic liver
$ 
```

Display the computed statistics:

```
$ sydFind RoiStatistic liver john
```


