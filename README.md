# Readme

![syd](images/logo-syd.png)

(See on [github](https://github.com/OpenSyd/syd))

SYD is a **C++ toolkit** and a **command line** environment to perform image processing tasks on a **oriented-object database** of medical images.

SYD is initially developed to manage SPECT/PET/CT images in the field of [Target Radionuclide Therapy](https://www.google.fr/search?q=targeted+radionuclide+therapy) and [nuclear medicine](https://en.wikipedia.org/wiki/Nuclear_medicine). But it could also be useful in other domains.

SYD allows to manage databases of images and related information \(patient, acquisition date, associated injection etc\). It can store original DICOM images, but also all intermediate images needed to perform computation \(scaled image, cropped images, combined images etc\).

---

**Main features: **

* The **database = 1 file + 1 folder**. There is _no database server_. The db is a simple SQL file plus a raw folder of images. Hence, images may be accessed via the conventional file system.
* The **images** are stored as DICOM or mhd+raw file formats
* **API**: Object-oriented \(OO\) database management \(forget SQL\). You can retrieve, update or store images, patient, injection etc in the db with simple OO concepts.
* **Command line**: SYD also provide a number of command line tools to perform basic operations on the db, such as querying, inserting or deleting some items
* SYD try to no reinvent the wheel, it is linked with [ITK](www.itk.org) \(image processing\), [Elastix](http://elastix.isi.uu.nl/) \(image registration\), [Gate](www.opengatecollaboration.org) \(image and dose simulation\).

** Limitations: **

* The [installation](install.md) process is tedious because of the dependencies
* no GUI
* the db structure is mostly static. It means that in order to add tables or fields, you need to use code it in C++ and recompile.
* It is limited to small size db. It is not a PACS. It is intended to perform studies on limited set of data
* no db server, so no sharing capabilities nor collaboration

---

**API examples:**

``` C++
// open the db
syd::StandardDatabase * db = m->Open<syd::StandardDatabase>("mydb.db");

syd::Image::pointer image; // Declare an image pointer
db->QueryOne(image, 12); // Get the image with id=12
std::cout << image << std::endl; // Print image information
std::cout << image->patient << std::endl; // Print associated patient info
std::cout << image->acquisition_date << std::endl; // Do you really need help ?

typedef itk::Image<float,3> ImageType;
ImageType::Pointer itk_image;
syd::ReadImage<ImageType>(image->GetAbsolutePath()); // Read associated itk image
```

**Command line examples:**

Find all images with 'spect' and 'john' in the description:

``` bash
sydFind Image spect john
```

Compute the mean \(and other statistic\) in a roi called 'liver' for all found images:

``` bash
sydFind Image spect john -l | sydInsertRoiStatistic liver
```

Display the computed statistics:

``` bash
sydFind RoiStatistic liver john
```
