# FAF

## 1. API




## 2. Command Line tools

- sydInsertPlanarGeometricalMean

Usage: `sydInsertPlanarGeometricalMean <image>`

Create a new (mhd) image with the geometrical mean of planar images. `<image>`  must be a single image with 4 slices (ANT_EM, POST_EM, ANT_SC, POST_SC) for respectively the image from the anterior detector, the image from the posterior detector for the primary and the scattering. The calculation is:
```
sqrt[(ANT_EM - k*ANT_SC) * flip(POST_EM - k*POST_SC)]
```

- sydInsertAttenuationMap

Usage: `sydInsertAttenuationMap <image>`

Create a new (mhd) image with the conversion of CT's HU into attenuation coefficients.`<image>`  must be a CT image. The calculation is:

- sydInsertProjection

Usage: `sydInsertAttenuationMap <images>`

Create a new (mhd) image with the projected image along the dimension `d` of `<images>`. The resulted voxel is the sum of all voxel values along the dimension `d`. The tag ```--mean (or -m)``` can be set to compute the mean (eg.: for CT) instead of the sum (eg.: for SPECT)

- Registration ?

- sydInsertCorrectedImage

Usage `sydInsertCorrectedImage <images1> <images2>`

Create a new (mhd) image from the CT using the attenuation map. `<images1>` is a CT image and `<images2>` is an attenuation map. The result is computed as:

`CT corrected = CT * exp(...attenuationMap...)`

