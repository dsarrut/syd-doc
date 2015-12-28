
![syd](images/syd.png "SYD")


SYD is a *command line* environnement and a *C++ toolkit* to perform image processing tasks on a database of medical images. It is initially developed to manage SPECT/CT images and perform tasks such as computing activity or integrated activity in various ROIs, in the field of Target Radionuclide Therapy and nuclear medicine.

**Examples:**

Find all images with 'spect' and 'john' in the description:

```
sydFind Image spect john
```


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
