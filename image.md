# Images management

(notes)

Image: 
- associated patient
- vector of tags
- pixel_value_unit
- associated syd::File to store mhd and raw
- associated dicoms (DicomSerie)
- frame_of_reference_uid: coordinate system

 
Command line tools
 - `sydInsertImage` *add tag, add unit*
 - `sydInsertImageFromDicom`
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