# This page contains additional information on how to use syd tools:



##  sydCalibrateImage
syd 0.2

Usage: sydCalibrateImage <image_ids>

Perform image calibration : consider the first image of the list (according to 
acquisition time), compute activity by detected counts (assuming whole activity 
in the image fov). Convert all images with the calibration factor. The pixel 
unit is changed to Bq.
  
\- <image_ids>      ids of the images to update

It can be adapted to use need by using --scale and --pixel_unit option.

The default pixel unit is in Bq. If the option --by_IA is used, new injection 
with 1 MBq injected activity is created and linked to the images.



|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-s|--scale=DOUBLE|Multiply the pixel's values by this value  (default=`1.0')|
|-n|--by_IA|Normalisation by injected activity (in MBq)  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydCheck
syd 0.1

Usage: sydCheck <table> <list_of_ids>

Check TODO

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-f|--folder=STRING|Folder to look for file|
|-r|--recurse|Recurse folder  (default=off)|
|-d|--delete|Delete files that are outside the db (dangerous)  (default=off)|
|-n|--no_loadbar|If 'on': do not display load bar  (default=off)|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  syd_clitkExtractPatient
syd 0.4

Usage: syd_clitkExtractPatient <list_of_image_ids>

Use clitkExtractPatient to build a mask image of images. The RoiType named 
'body' *must exist in the db.

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-p|--options=STRING|Option for clitkExtractPatient (in " ")  (default=`--verboseStep')|
|-o|--opening|Option final clitkMorphoMath opening  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydComputeActivityInMBqByDetectedCounts
syd 0.4

Usage: sydComputeActivityInMBqByDetectedCounts <image_ids> 

Compute the activity by detected counts, assuming whole injected activity is 
inside the image.

value =  total_counts_in_image / activity_at_acquisition
with the activity_at_acquisition = injected_activity * exp(-lambda * time);
with lambda = log(2.0)/half_life
with time = time between injection and acquisition


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-s|--scale=DOUBLE|Multiply the final result by this value  (default=`1.0')|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydCreateDatabase
syd 0.3

Usage: sydCreateClinicDatabase <schema> <filename.db> <folder>

Create a new *empty* database of the given schema. The <folder> will contains 
the images. The database schema (list of tables) is a name that corresponds to 
a dynamically loaded library (plugin).

Example:
sydCreateClinicDatabase StandardDatabase toto.db my_folder/

will load the library libStandardDatabase.so (or dylib) that contains the 
database schema. The variable SYD_PLUGIN is used to look fo those libraries.


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-f|--overwrite|Force overwrite even if the file exist  (default=off)|
|-l|--list|Print list of available DB schema (then stop)  (default=off)|
||--default_table=STRING|Create some default elements (radionuclides, tags etc). 'all', 'none', etc   (default=`all')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydDelete
syd 0.2

Usage: sydDelete <table> <list_of_ids>

Delete the given elements in a table.
  
\- <table>         name of the table
  
\- <ids>           list of elements id to delete. Can be 'all' to delete all 
elements.


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-f|--force|Dont ask confirmation. DANGEROUS. You have been warned.  (default=off)|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydDicomInfo
syd 0.3

Usage: sydDicomInfo <dicom_id>

Dump content of the dicom for the given dicom serie. If the serie
contains several files, only the first is displayed.

Alternatively, you can specify a simple file instead of dicom_id or
with the option --file, (the dicom_id is ignored),


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-f|--file=STRING|Read this file instead of a dicom_id|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  syd_elastix
syd 0.2

Usage: syd_elastix <elastix_ids> 

Parameters are:
  
\- <elastix_ids>  : ids of Elastix records in the db


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-o|--options=STRING|Option for elastix (in " ")  (default=`')|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydFind
syd 0.6

Usage: sydFind <table> <pattern> 

Find the ids of elements in the table that match the pattern. Parameters are:
  
\- <table>     name of the table
  
\- <pattern>   list of words to match, all words must match (logical 'AND').

If without any arg: print nb of elements by tables.
Case sensitive.
Use --exclude option to exclude words.


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-e|--exclude=STRING|Exclude pattern (multiple)|
|-l|--list|Dump list of ids only  (default=off)|
|-f|--field=STRING|Field format when dump results  (default=`default')|
|-p|--precision=FLOAT|Global precision for field (-1 for default)  (default=`-1')|
||--field_option=STRING|Set options for a given field, ie 'mean(2)' to set precision=2 for the field named 'mean'|
||--list_fields|Print the list of fields available for the given table and exit  (default=off)|
|-c|--check|Check files associated to found records  (default=off)|
||--noheader|Do not display table header  (default=off)|
||--nofooter|Do not display table footer  (default=off)|
||--single_line|All results in a single line  (default=off)|
|-d|--delete|Delete all found elements  (default=off)|
||--force|Dont ask confirmation if --delete. DANGEROUS. You have been warned.  (default=off)|
|-t|--tag=STRING|List of tag to match (all)|
|-i|--id=INT|Display the record with this id (maybe multiple)|
|-o|--oneOutput|The output is one and only one elements (An error occurs if there is zero or several elements)  (default=off)|
||--vv|Open images with vv (result must be image)  (default=off)|
||--vvs|Open images with vv --sequence (result must be images)  (default=off)|
|-s|--sort=STRING|Sort type default/name/date etc (depends on table) (default=`')|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  syd_gate
syd_gate 0.2

Usage: syd_gate <mac> <ct_id> <source_id> <rad_name>

Generate a Gate command line to exec the template macro with data from the 
database.
Alias are: CT_IMAGE PATIENT RADIONUCLIDE Z A SOURCE_IMAGE


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-f|--output_folder=STRING|Output folder when relative is on  (default=`data/')|
|-o|--output=STRING|Output file  (default=`')|
|-n|--N=DOUBLE|Nb of primaries  (default=`100')|
|-r|--run=INT|Execute the simulation with the given nb of thread  (default=`1')|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydIdentifyImageType
syd 0.4

Usage: sydIdentifyImageType <dicom_ids>

Create a new (mhd) image for all given dicoms
  
\- <dicom_ids>   list of dicom (serie) ids to identify


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInfo
syd 0.4

Usage: sydInfo

Print some information about the schema of the database (schema). Parameters 
are:
  
\- <table>     [optional] if given, list all the fields of the table


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsert
sydInsert 0.3

Usage: sydInsert <table> [<arg>]

Insert a new element in the table <table> of the db. The <args> is a list of 
strings depends on the table type.

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertAnonymizedDicom
syd 0.1

Usage: sydInsertAnonymizedDicom <dicom_ids>

Insert copy of dicom with some fields anonymized.

*WARNING* this is not a complete anonymization (only patient's name and 
birthdate)

*WARNING* some dicom tags could be lost.

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-d|--delete|Also delete the initial dicom (use with caution)  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertAttenuationCorrectedProjectionImage
syd 0.4

Usage: sydInsertAttenuationCorrectedProjectionImage <image1> <image2> <image3> -d [dimension]

Create a new (mhd) image from the 2D Geometric Mean (GM) using the projected 
attenuation map.
[image1] is a GM image id and [image2] is the projected attenuation map id. The 
spacing and the size along the d-axis given by --dimension (or -d) are computed 
thanks to the original 3D attenuation map [image3].


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-d|--dimension=INT|Dimension of projection|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertAttenuationImage
syd 0.4

Usage: sydInsertAttenuationImage <image> -c water,bone -s air1,water1,bone1,air2,water2,bone2 -w w1,w2

Create a new (mhd) attenuation map image computed from the CT image <image>.
The tag --attenuationCT (or -c) corresponds to the attenuation coefficients for 
water and bone (respect the order) for mean energy of the X-ray used for CT (2 
expected values)
The tag --attenuationSPECT (or -s) corresponds to the attenuation coefficients 
for air, water and bone (respect the order) for all targeted energies of the 
SPECT
The tag --weight (or -w) corresponds to the normalized weight for all targeted 
energies of the SPECT (sum =1). If there is just 1 targeted energy, this 
parameter is optional


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-c|--attenuationCT=DOUBLE|Attenuation for water and bone for kVeff|
|-s|--attenuationSPECT=DOUBLE|Attenuation for air, water and bone for all kEV|
|-w|--weight=DOUBLE|Pourcentage of each kEV emission|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertAveragedImages
syd 0.2

Usage: sydInsertAveragedImages <image_ids>

Create an image by averaging all given images (must be the same size).
  
\- <image_ids>      ids of the images to update


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertDicom
syd 0.3

Usage: sydInsertDicom <files or folders>

Insert the all dicom series in a db. Parameters are:
  
\- <files/folders>  list of files or folders to look for dicom series

If the option --patient <study_id> is given, the patient is forced. If not, the 
patient is guessed from the dicom id. If no patient was found, a new one is 
created. WARNING: this is the <study_id> not the <id> !



|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-p|--patient=STRING|Associate the dicom with this patient id or name|
|-u|--updatePatient|If true, update patient info from dicom  (default=off)|
|-f|--force|If DicomFile already exist, overwrite  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertElastixRegistration
syd 0.'

Usage: sydInsertElastixRegistration <config> <fixed_image_id> <moving_images_ids*> 

Parameters are:
  
\- <config>              : elastix config file
  
\- <fixed_image_id>      : id of the fixed image
  
\- <moving_images_ids*>  : ids of the moving image (multiple -> one registration 
by id)


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--fMask=STRING|Mask id or name for fixed image (table RoiMaskImage)|
||--mMask=STRING|Mask id or name for moving image (table RoiMaskImage)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertFlippedImage
syd 0.4

Usage: sydFlippedImage <image1> -a <xyzyxz>

Create a new (mhd) flipped image.
The axis of the flips are defined according the sequence -a


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-a|--axis=STRING|Sequence of axis|
||--origin|Flip around origin  (default=off)  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertGateOutput
sydInsertGateOutput 0.3

Usage: sydInsertGateOutput <gate_output_folder> <source_image_id>



Read the files in the <gate_output_folder> and look for mhd file in the form:
<patient>-<radionuclide>-Edep.mhd etc

The images (edep, dose, uncert) are inserted into the db according to the 
injection.

FIXME Dose are scaled according to the nb of particles (found in stat.txt file) 
and the total sum of value in the tia image.

FIXME Tags image are assumed to be : <study> <radionuclide> then tia, dose, 
edep


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--dry_run|Do not insert in the db, only check   (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertImage
syd 0.3

Usage: sydInsertImage <filename>

Insert a mhd image in the db
  
\- <filename>      image file to insert
if --like is not used, --patient is required


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-l|--like=INT|Id of the image to copy information (patient, dicoms, ...)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertImageFromDicom
syd 0.4

Usage: sydInsertImageFromDicom <list of dicom_ids>

Create a new (mhd) image for all given dicoms
  
\- <dicom_ids>     list of dicom id to insert

If the pixel_type is not 'auto', attempt will be made to convert the
dicom image into an image with this pixel_type.



|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-t|--pixel_type=STRING|Force the pixel type (i.e. float, short etc)  (default=`auto')|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertManualRegisteredImage
syd 0.4

Usage: sydInsertManualRegisteredImage <image1> -x [x] -y [y] -z [z] -o -c <image2>

Create a new (mhd) registered image.
if -c is passed, center [image1] and [image2] and then
The registration takes an image <image1> and translates it along x,y and z 
axis.


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-x|--x=DOUBLE|Displacement along x axis [mm]|
|-y|--y=DOUBLE|Displacement along y axis [mm]|
|-z|--z=DOUBLE|Displacement along z axis [mm]|
|-c|--center=INT|Align the centers of image1 and image2|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertPlanarGeometricalMean
syd 0.4

Usage: sydInsertPlanarGeometricalMean <images>

Create a new (mhd) image with the geometrical mean of planar images
  
\- <images>  must be a single image with 4 slices (ANT_EM, POST_EM, ANT_SC, 
POST_SC)


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-k|--k=DOUBLE|Weight parameters to remove scatter : EM-k*Sc  (default=`1.1')|
|-c|--crop|Crop the third dimension for 3D image with a one-size 3rd dimension (default = true)  (default=on)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertProjectionImage
syd 0.4

Usage: sydInsertProjectionImage <image>

Create a new (mhd) image with the projected image along the dimension d of 
<image>.
The resulted voxel is the sum of all voxel values along the dimension d.
The tag --mean (or -m) can be set to compute the mean (eg.: for CT) instead of 
the sum (eg.: for SPECT)


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-d|--dimension=DOUBLE|Projection along dimension d  (default=`0')|
|-m|--mean|Mean of voxels instead of sum  (default=off)|
|-f|--no-flip|Do not Flip & Rotate the image (conserve coordinates)  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertRegisterPlanarSPECT
syd 0.4

Usage: sydInsertRegisterPlanarSPECT <image1> <image2> <image3>

Create a new (mhd) attenuation map image from the non-registered attenuation 
map <image3>.
The registration takes the Planar image <image1> and the projected SPECT image 
<image2>, it centers around the x-axis and search for the best y-axis 
translation to maximize the correlation coefficient.


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertRoiBasedTimeIntegratedActivity
syd 0.4

Usage: sydInsertRoiBasedTimeIntegratedActivity <fitimage_ids> <roi_name>

Create a FitTimepoints that fit region based time activity 
  
\- <fitimages_ids>     list FitImages id to get the images list
  
\- <roi_name>                  list of roi_names (or 'all')


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertRoiMaskImage
sydInsertRoiMaskImage 0.4

Usage: sydInsertRoiMaskImage <roi> <image_id> <mask.mhd>

Create a new RoiMaskImage
  
\- <roi>          name of the roi type ('liver', 'body' etc. see `sydFind 
RoiType`)
  
\- <image_id>     id of the image to copy information from 
(frame_of_reference_uid etc)
  
\- <mask.mhd>     filename that will be copied in the db


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertRoiStatistic
syd 0.5

Usage: sydInsertRoiStatistic <roi_name> <list_of_ids>

Compute mean and other stat for image values in the roi mask image
  
\- <roi_name>            name of the roi (should have the same 
frame_of_reference_uid than the image)
  
\- <list_of_ids>         list of images or tia id

Options:
  
\- if --tia is given : the ids are TiaImage ids, if not, ids are Image ids.
  
\- If several images id are given, all the corresponding rois is searched for 
each image (same frame_of_reference_uid)
  
\- If <roi> is set to 'all', all RoiMaskImage with the same 
frame_of_reference_uid are considered.
  
\- If <roi> is set to 'null', statistics are computed on the whole image.
  
\- If --resampled_mask is set, the mask of the roi resampled to the image size 
is dumped on the disk.- If a RoiStatistic already exist for the same image and 
roi_name, it is skipped, except if the --force option is given.  


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-m|--resampled_mask=STRING|Resampled mask output filename  (default=`')|
|-t|--tia|Given ids are TiaImage ids, this string set the used output image  (default=off)|
||--fit_image_name=STRING|If tia, this string set the used output image  (default=`fit_auc')|
|-f|--force|Force a new RoiStatistic even if a RoiStatistic already exist  (default=off)|
|-u|--update|Update if a RoiStatistic already exist (default=on, use --update for toggle to off)  (default=on)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydInsertTimeIntegratedActivityImage
syd 0.4

Usage: sydInsertTimeIntegratedActivityImage <image_ids>

Create a new image with TIA in every pixels
  
\- <list_of_image_ids>     list of image id to integrate


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--min_activity=FLOAT|Dont fit if max activity over the n images is lower than this value  (default=`0')|
||--debug_images|Output some debug images  (default=off)|
||--mask=STRING|Mask name to use  (default=`body')|
||--r2_min=FLOAT|Min R2 threshold for model selection  (default=`0.9')|
|-i|--iterations=INT|Max nb of iterations  (default=`50')|
|-r|--restricted_tac|Fit only end of the curve (max to end)  (default=off)|
|-m|--model=STRING|Set the model list use to fit f2 f3 f4a etc|
|-a|--akaike=STRING|Set the Akaike criterion AIC or AICc  (default=`AIC')|
||--fit_verbose|Verbose fit tentative  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydStitchDicom
syd 0.4

Usage: sydStitchDicom <dicom_ids>

Create a new (mhd) Image for each given dicom
  
\- <dicom_ids>     the dicoms id to stitch together (will be paired by 
dicom_series_uid)


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-t|--t_cumul=DOUBLE|Threshold for cumul, to find the common slice  (default=`150000')|
|-s|--skip_slices=INT|Skip some slices at the end  (default=`4')|
|-d|--dry_run|Print what will be stitched, without stitching  (default=off)|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydSubstituteRadionuclide
syd 0.2

Usage: sydSubstituteRadionuclide <rad> <image_ids>

Insert copies of the given images where the radionuclide from the associated 
injection is replaced by the given radionuclide. A new injection is also 
created with 1 MBq.
  
\- <rad>          name of the radionuclide
  
\- <image_ids>    ids of the images



|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydTest
sydTest 0.3

Usage: sydTest DEBUG

DEBUG do not use

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  syd_transformix
syd 0.4

Usage: syd_transformix <image_ids>

Parameters are:
  
\- <image_ids>            : id of the images to transform

An Elastix id must be given for every image_id.



|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|-e|--elastix=STRING|Elastix ids withint " "|
|-p|--default_pixel=DOUBLE|Default pixel value  (default=`0')|
|-o|--options=STRING|Option for transformix (in " ")  (default=`')|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydUpdate
sydUpdate 0.1

Usage: sydUpdate <table> <field> <value> <id>

Update the field <field> of the element <id> in the table <table>.

|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydUpdateImage
syd 0.2

Usage: sydUpdateImage <image_ids>

Update information associated with an image, and/or scale.
  
\- <image_ids>      ids of the images to update


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--file=STRING|Content of the file will replace the image in the db|
|-s|--scale=DOUBLE|Multiply the pixel by this value|
||--pixel_type=STRING|Convert the current image to the given pixel_type|
||--fill_holes=INT|Fill holes according to neighboring voxels (radius)  (default=`0')|
||--fill_mask_image=INT|The 'holes' are given in this mask image  (default=`0')|
||--fill_mask_value=FLOAT|Values of the 'holes' in the mask image  (default=`0')|
|-g|--gauss=FLOAT|Smooth the image with a Gaussian filter, the parameter is the sigma in mm (0=no smooth)  (default=`0')|
||--copy|Make a copy of the image before update. The initial image is not modified  (default=off)|
||--rename|Rename internal file with patient info (advanced)  (default=off)|
|Resample/Crop info:|||
|-r|--resample_like=INT|Resample like the given image id|
|-i|--interpolation=INT|Interpolation type 0=NN, 1=Lin  (default=`1')|
|-d|--default_value=DOUBLE|default value  (default=`-1000')|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Image info option:|||
||--patient=STRING|Patient name or id|
||--pixel_unit=STRING|Name of pixel value unit to attach to the image|
||--modality=STRING|Modality type|
||--dicom=INT|Dicom id to link|
||--injection=STRING|Injection id or name to link|
||--acquisition_date=STRING|Acquisition date, ie '2011-11-03 12:34'|
||--frame_of_reference_uid=STRING|--frame_of_reference_uid=STRINGFrame of ref (for image coordinate system)|
||--flip_if_negative_spacing|--flip_if_negative_spacingFlip the image if negative spacing  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydUpdateRadionuclide
syd 0.4

Usage: sydUpdateRadionuclide <name>

Create or update a new radionuclide, by retrieving information from
http://www.nucleide.org/DDEP_WG/DDEPdata.htm


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
||--url=STRING|Database url server  (default=`www.nucleide.org')|
||--path=STRING|Database url path   (default=`/DDEP_WG/Nuclides/')|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)|


##  sydUpdateTags
syd 0.2

Usage: sydUpdateTags <table_name> <ids>

Update tags associated with record.
  
\- <table_name>   record table
  
\- <ids>          ids of the record to update


|little tag|tag|commment|
|---|---|---|
|-h|--help|Print help and exit|
|-V|--version|Print version and exit|
|Tags option:|||
||--add_tag=STRING|Add a tag (or a list if given in " ")|
||--rm_tag=STRING|Remove a tag (or a list if given in " ")|
||--rm_all_tags|Remove all tags  (default=off)|
|Comments option:|||
||--add_comment=STRING|Add a comment|
||--rm_comment=INT|Remove a comment|
||--db=STRING|Database file (use 'default' to read from $SYD_CURRENT_DB)  (default=`default')|
|-v|--verbose=INT|Verbosity level (0 for silence)  (default=`0')|
||--verboseSQL|Verbose SQL query (debug)  (default=off)|
||--config=STRING|Config file where to read options (overwritten by command line options)
