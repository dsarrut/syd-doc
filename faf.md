# FAF

## 1. API




## 2. Command Line tools


- **sydInsertPlanarGeometricalMean**

Usage: `sydInsertPlanarGeometricalMean <image> -k 0.5`

Create a new (mhd) image with the geometrical mean of planar images. `<image>`  must be a single image with 4 slices (ANT_EM, POST_EM, ANT_SC, POST_SC) for respectively the image from the anterior detector, the image from the posterior detector for the primary and the scattering. The calculation is:
$$
\sqrt{(ANT_{EM} - k*ANT_{SC}) * flip(POST_{EM} - k*POST_{SC})}
$$

- **sydInsertAttenuationImage**

Usage: `sydInsertAttenuationImage <image>`

Create a new (mhd) image with the conversion of CT's HU into attenuation coefficients.`<image>`  must be a CT image. The calculation is: **TODO**


- **sydInsertProjectionImage**

Usage: `sydInsertProjectionImage <image> --dimension 2`

Create a new 2D (mhd) image with the projected image along the dimension `d`. The resulted voxel is the sum of all voxel values along the dimension `d`. The tag ```--mean (or -m)``` can be set to compute the mean (eg.: for CT) instead of the sum (eg.: for SPECT)

- Registration ?
Input: 2D geometrical mean image + 2D projection image (from attenuation)
Output: 2D translations only
Solution1: could it be possible to automatically find translations from the input images information ? 
Solution2: allows users to change by something like sydTranslateImage <image> x y z ?


- **sydInsertScatterCorrectedProjectionImage**

Usage `sydInsertScatterCorrectedProjectionImage <images1> <images2>`

Create a new (mhd) image from the CT using the attenuation map. `<images1>` is a CT image and `<images2>` is an attenuation map. The result is computed as:

$$CT_{corrected} = CT * \exp{...attenuationMap...}$$

- **sydCalibrateImage**

Usage `sydCalibrateImage <image_ids>`.

Perform image calibration: consider the first image of the list (according to
acquisition time), compute activity by detected counts (assuming whole activity
in the image fov). Convert all images with the calibration factor. The pixel
unit is changed to Bq.
It can be adapted to use need by using --scale and --pixel_unit option.
The default pixel unit is in Bq. If the option --by_IA is used, new injection
with 1 MBq injected activity is created and linked to the images.



