# FAF

## 1. Command Line tools


- **sydInsertPlanarGeometricalMean**

`sydInsertPlanarGeometricalMean <image> -k 0.5`

Create a new (mhd) image with the geometrical mean of planar images. `<image>`  must be a single image with 4 slices (ANT_EM, POST_EM, ANT_SC, POST_SC) for respectively the image from the anterior detector, the image from the posterior detector for the primary and the scattering. The calculation is:
$$
\sqrt{(ANT_{EM} - k*ANT_{SC}) * flip(POST_{EM} - k*POST_{SC})}
$$

- **sydInsertAttenuationImage**

`sydInsertAttenuationImage <image>`

Create a new (mhd) image with the conversion of CT's HU into attenuation coefficients.`<image>`  must be a CT image. The calculation is: **TODO**


- **sydInsertProjectionImage**

`sydInsertProjectionImage <image> --dimension 2 [--mean] [--flip]`

Take a 3D image to create a new 2D (mhd) image with the projected image along the dimension ```--dimension (or -d)```. The resulted voxel is the sum of all voxel values along the dimension `-d`. The tag ```--mean (or -m)``` can be set to compute the mean (eg.: for CT) instead of the sum (eg.: for SPECT). The algorithm uses ```itk::SumProjectionImageFilter``` to project the image but the axes are flipped in the resulting image. Set the flag ```--flip (or -f)``` to have the the head at the top and the feet at the bottom.

- Registration ?
Input: 2D geometrical mean image + 2D projection image (from attenuation)
Output: 2D translations only
Solution1: could it be possible to automatically find translations from the input images information ? 
Solution2: allows users to change by something like sydTranslateImage <image> x y z ?


- **sydInsertScatterCorrectedProjectionImage**

`sydInsertScatterCorrectedProjectionImage <images1> <images2>`

Create a new (mhd) image from the CT using the attenuation map. `<images1>` is a CT image and `<images2>` is an attenuation map. The result is computed as:

$$CT_{corrected} = CT * \exp{...attenuationMap...}$$

- **sydCalibrateImage**

`sydCalibrateImage <image_ids>`.

Perform image calibration: consider the first image of the list (according to
acquisition time), compute activity by detected counts (assuming whole activity
in the image fov). Convert all images with the calibration factor. The pixel
unit is changed to Bq.
It can be adapted to use need by using --scale and --pixel_unit option.
The default pixel unit is in Bq. If the option --by_IA is used, new injection
with 1 MBq injected activity is created and linked to the images.


## 2. API

In file `std_db/sydImageHelper.h\cxx`

- ```double ComputeActivityInMBqByDetectedCounts(syd::Image::pointer image)```

- ```void syd::ScaleImage(syd::Image::pointer image, double s)```

- ```syd::Image::pointer syd::InsertImageGeometricalMean(const syd::Image::pointer input, double k)```

- ```syd::Image::pointer InsertProjectionImage(const syd::Image::pointer input, double dimension=0, bool mean=false, bool flip=false);```


In file `core/sydImageGeometricalMean.txx.h\txx`

- ```template<class ImageType> typename ImageType::Pointer syd::GeometricalMean(const ImageType * ant_em, const ImageType * post_em, const ImageType * ant_sc, const ImageType * post_sc, double k)``` : Main function to compute geometrical mean and call the next functions. Start correcting the scattering for both the ant and the post images, flip the post image and then compute the geometrical mean between these two images.

- ```template<class ImageType> typename ImageType::Pointer syd::RemoveScatter(const ImageType * em, const ImageType * sc, double k)``` : Remove the scattering. Compute $$ em - k*sc $$

- ```template<class ImageType> typename ImageType::Pointer syd::GeometricalMean(const ImageType * ant, const ImageType * post)``` : Compute the geometrical mean $$ \sqrt{ant * post} $$



In file `core/sydProjectionImage.h\txx`

- ```template<class ImageType, class OutputImageType> typename OutputImageType::Pointer Projection(const ImageType * input, double dimension, bool mean, bool flip)``` : Call the next function to project the input image, and then compute the mean and flip if necessary

- ```template<class ImageType, class OutputImageType> typename OutputImageType::Pointer Projection(const ImageType * input, double dimension);``` : 
Call ```itk::SumProjectionImageFilter``` to project the input image along the dimension `dimension`


