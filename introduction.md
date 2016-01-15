
![syd](images/logo-syd.png "SYD")


Let's start with some command lines examples.

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
