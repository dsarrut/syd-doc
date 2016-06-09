
![syd](images/logo-syd.png "SYD")

**What is SYD ?**

SYD is a *command line* environment and a *C++ toolkit* to perform image processing tasks on a *oriented-object database* of medical images. SYD was initially developed to manage SPECT/PET/CT images in the field of Target Radionuclide Therapy and nuclear medicine. But it could also be useful in other domains.  

**Features**

- no server database (advantage / drawback)
- raw, file based access to images
- cmd line based
- develop OO --> forget sql
- manage image, link between images, results of computation
- linked with ITK, Elastix, Gate,


**Examples:**

Find all images with 'spect' and 'john' in the description:

```
sydFind Image spect john
```

Compute the mean (and other statistic) in a roi called 'liver' for all found images:

```
sydFind Image spect john -l | sydInsertRoiStatistic liver
```

Display the computed statistics:

```
sydFind RoiStatistic liver john
```
