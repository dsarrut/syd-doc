# Images management

<!-- ONLY API here, not command line info -->

Images are managed by the table `syd::Image`. This table basically contains meta-information about the images (patient, size, type, spacing, etc) and link to files in the database folder. Loading an image as an itk image object is simple. Helper functions grouped in the class `syd::ImageHelper` are provided to create and update images. Additional classes (tables) inherited from `syd::Image` allow to manage specific images types such as `syd::RoiMaskImage` or `syd::DicomSerie`.

** Class table `syd:Image`**

All images are linked to a `syd::Patient`, this is required. Images may be linked to injection, and dicoms.

When an image is stored in the database (persist), the file is usually in the folder with the patient name and named "<modality>_<id>.mhd" pattern. Warning, if the image is updated, for example by changing the modality, the filename is not updated.

When an image is deleted, the associated `syd::File` are also deleted. This is perform in `syd::Image::Callback`.


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


`syd::ImageBuilder`

- `NewMHDImage(filename)`: copy files (both mhd/raw into db). But the image is not yet persistent in the db.
