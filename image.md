# Images management

(notes)

Image: 
- tag, pixel_value_unit
- associated file in mhd
- patient
- associated dicoms (DicomSerie)
- frame_of_reference_uid: coordinate system

 
new image
 - sydInsertImage *add tag, add unit*
 - sydInsertImageFromDicom
 - sydInsertIntegratedActivityImage
 - sydInsertSubstituteRadionuclideImage
 - sydInsertDecayCorrectedImage
 - sydStitchDicom
 - sydCopyImage *TODO*

modify
 - sydUpdateImage: tag, unit, scale
 - sydUpdateDoseImage: need tia and N *TODO*
 - sydCalibrateImage: update or insert ?
 - sydCropImage
 - sydUpdateImageTag --> to remove. Change by sydUpdateTag

API
 - API: builders, or functions ?
   - ImageBuilder
   - IntegratedActivityImageBuilder
   - DecayCorrectedImageBuilder
 - set Tags from command line
 - set Pixel_unit from command line
