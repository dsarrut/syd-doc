# Command line tools (standard database)

Here is the list of tools:

- Dicom
 - `sydInsertDicom`
 - `sydDicomInfo`

- Images
 - `sydInsertImage`
 - `sydInsertImageFromDicom`
 - `sydUpdateImage`
 - `sydStitchDicom`
 - `sydInsertPlanarGeometricalMean`
 
 - Others
  - `sydUpdateRadionuclide`

<!--
 -  - `sydInsertMultiplyImage`
 - `sydInsertCalibratedImage`
 - `sydInsertDecayCorrectedImage`
 - `sydInsertSubstituteRadionuclideImage`
 - `sydStitchDicom`
 - `sydInsertRoiMaskImage`
 - `sydInsertIntegratedActivityImage`

- Insert for specific tables
 - `sydInsertTimePoints`
 - `sydInsertCalibration`
 - `sydInsertRoiStatistic`

- Processing
 - `sydCopyImage`
 - `sydUpdateImage`
 - `sydUpdateDoseImage`
 - `sydCropImage`
 - `sydUpdateDicomSerie`
 - `sydUpdateRadionuclide`


- With external tools
 - `syd_clitkExtractPatient`
 - `sydInsertGateOutput`
 - `syd_elastix`
 - `syd_transformix`
-->

----------------------------------------------------------
## ```sydInsertDicom```: insert DICOM images

```
    sydInsertDicom -v1 john folder_dicom/
```

In that example, the patient is defined by his name (it can alternatively be his study_id, both are unique field). The given `folder_dicom` is scanned for dicom files. Images are sorted to create DicomSerie  associated with the patient. All the files are *copied* to the database folder, so it could takes some times if a lot of images are found. Images are roughly sorted in this folder according to the following hierarchy: `patient_name/date/modality`. Initial filenames are conserved. The main idea here is to keep things simple: you can still navigate to the folder hierarchy with any DICOM viewer. Syd will the provide tools for analysis and conversion of the DICOM images.

Once your database has been populated with some data, you may display some information with `sydFind`. `sydFind` display information on elements (records) of given table (Patient, DicomSerie etc). The elements that will be displayed are designated by their id. The `sydFind` tool perform a simple search like the shell command `grep` and retrieve the id of the elements that match the pattern. Some examples below:

```sh
    # Print all patients
    sydFind Patient

    # Print the tables in the database
    sydFind

    # Find the dicomserie that contains the words "SPECT" and "TOMO" somewhere
    # in their description, but does not contain the word "PLANAR".
    sydFind DicomSerie SPECT TOMO -e PLANAR

    # Same as previous but only display the ids as a raw line
    sydFind DicomSerie SPECT TOMO -e PLANAR -l
```

You can delete a record by specifying its id and its table: `sydDelete DicomSerie 2`. Such command will permanently delete the element ith id=2 in the database but also the associated files in the database folder. `sydDelete` may use after a `sydFind` command, allowing to delete the records that match a pattern.


----------------------------------------------------------
## ```sydInsertImageFromDicom```: convert DICOM to image

The first key point is to keep the initial DICOM images and perform image processing tasks on copy images in mhd/raw file format. The second point is to label images with tags to retrieve them easily.

```
    # Convert the DicomSerie number 123 into a mhd image. Add two tags to the image
    sydInsertImageFromDicom -v1 123 --tag "spect" --tag "scaled"

    # Retrive some image
    sydFind Image spect
```

You can use `sydUpdateImage` to change the tags, the pixel unit, or perform basic operations.


----------------------------------------------------------
# ```sydStitchDicom```




<!--

## TODO Tools

    sydInsertDicom

    sydInsertImageFromDicom
    sydInsertImage
    sydInsertMultiplyImage               --> in InsertImage ?
    sydInsertCalibratedImage             --> in InsertImage ?
    sydInsertDecayCorrectedImage         --> in InsertImage ?
    sydInsertSubstituteRadionuclideImage --> in InsertImage ?
    sydStitchDicom ---> to rename ? sydInsertStitchDicomImage
    sydInsertRoiMaskImage
    sydInsertIntegratedActivityImage

    sydInsertTimePoints
    sydInsertCalibration
    sydInsertRoiStatistic

    sydCopyImage
    sydUpdateImage
    sydUpdateDoseImage
    sydCropImage
    sydUpdateDicomSerie
    sydDicomInfo
    sydUpdateRadionuclide


    syd_clitkExtractPatient
    sydInsertGateOutput
    syd_elastix
    syd_transformix
-->