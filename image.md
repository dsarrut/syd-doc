# Images management

** Class table `syd:Image`** 

This table contains: 
- a patient
- a vector of tags
- a pixel_value_unit
- some ```syd::File``` to store mhd and raw
- some associated dicoms (```syd::DicomSerie```)
- a frame_of_reference_uid: coordinate system


---

**Command line tools to create a new image**

Image creation:
 - `sydInsertImage` : insert an image file in the db
 - `sydInsertImageFromDicom`: convert a DicomSerie to an image
 - `sydStitchDicom`: convert 2 dicom into a single stitched image
 - `sydInsertCalibratedImage`: scale an image according to a syd::Calibration object, in general from 'counts' to 'kBq/IA[MBq]' (IA for Injected Activity).
 - `sydInsertDecayCorrectedImage`: remove decay from the radionuclide associated with the injection of the images
 - `sydInsertSubstituteRadionuclideImage`: from decay corrected images, add radionuclide decay.
 - `sydInsertIntegratedActivityImage`: compute time integrated activity image from a set of images (lot of options). Frequently named 'tia'.
 - `sydCopyImage`: insert a copy of an image
 - `sydInsertMultiplyImage`: multiply two images and insert the results in the db.
 
Image modification: 
 - `sydUpdateDoseImage`: scale the dose according to the total nb of counts computed in the tia image, and the number of particles used in the Monte-Carlo simulation.
 - `sydUpdateImage`: update tags, pixel unit, scale
 - `sydCropImage`: Crop the image according to another image or a (lower) threshold

---
API
- `syd::ImageBuilder`: create, copy, rename an image in a database, set pixel value, 
- `syd::ScaleImageBuilder`
- `syd::CropImageBuilder`
- - 

 - API: builders, or functions ?
   - ImageBuilder
   - IntegratedActivityImageBuilder
   - DecayCorrectedImageBuilder
 - set Tags from command line
 - set Pixel_unit from command line
