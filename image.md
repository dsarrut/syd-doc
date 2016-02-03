# Images management

(notes)



---

**Table `syd:Image`** 

This table contains: 
- associated patient
- vector of tags
- pixel_value_unit
- associated syd::File to store mhd and raw
- associated dicoms (DicomSerie)
- frame_of_reference_uid: coordinate system


---

**Command line tools to create a new image**

Image creation:
 - `sydInsertImage` : insert an image file in the db
 - `sydCopyImage`: insert a copy of an image
 - `sydInsertImageFromDicom`: convert a DicomSerie to an image
 - `sydStitchDicom`: convert 2 dicom into a single stitched image
 - `sydInsertIntegratedActivityImage`: compute time integrated activity image from a set of images (lot of options)
 - `sydInsertDecayCorrectedImage`: remove decay from the radionuclide associated with the injection of the images
 - `sydInsertSubstituteRadionuclideImage`: from decay corrected images, add radionuclide decay.

Image modification: 
 - `sydUpdateDoseImage`: scale the dose according to the total nb of counts computed in the tia image, and the number of particles used in the Monte-Carlo simulation.
 - `sydUpdateImage`: update tags, pixel unit, scale
 - sydCalibrateImage: update or insert ?
 - sydCropImage

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
