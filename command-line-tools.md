
All tools start with 'syd'. Verbosity is managed by `-v` flag. You can increase the verbosity by increasing the number `-v1`, `-v2` etc ...

Most of the tool (except sydCreateDatabase) requires a database, set by the flag `--db filename.db` or implicitly with the environment variable `SYD_CURRENT_DB`. 

Help for most of the tools may be displayed with the `-h` flag. 


# sydCreateDatabase

Usage: `sydCreateClinicDatabase <schema> <filename.db> <folder>`. It creates a database name *filename.db* associated with a folder where the images will be stored. The filename and the folder must be in the same folder. 

The database *filename.db* is a sqlite file that can be viewed or browsed with any database viewer, such as [sqlitestudio](http://sqlitestudio.pl) 

The schema is *StandardDatabase*. A schema define the list of tables the database contains. Other types of schema will be proposed in the future. Database schemas are plugin libraries that can be dynamically found in the `SYD_PLUGIN` folder. 



# sydFind

This is probably the most use tool. Just give the name of the table to display the records in the table. For example: 

```
sydFind Image

 #Table: RoiMaskImage. Number of rows: 74
 #  id  p       acqui_date                                 tags        size           spacing dicom       unit           ref_frame          roi
 2059 gg 2012-02-29 11:11                               no_tag    13x17x12             1x1x1  1773      label ...01.2983962137656     lesion01
 1974 gg 2012-02-29 11:11                init_dicom,ct,dir_reg 240x165x399             2x2x2  1773         HU ...01.2983962137656            -
  499 gg 2012-02-29 11:11                                 mask   59x68x110             1x1x1  1773      label ...01.2983962137656  bone_marrow
  498 gg 2012-02-29 11:11                                 mask   80x65x102             1x1x1  1773      label ...01.2983962137656 right_kidney
  497 gg 2012-02-29 11:11                                 mask   69x66x115             1x1x1  1773      label ...01.2983962137656  left_kidney
  197 gg 2012-02-29 11:11                init_dicom,ct,dir_reg 480x330x798             1x1x1  1773         HU ...01.2983962137656            -
  493 gg 2012-02-29 11:11                                 mask 480x330x798             1x1x1  1773      label ...01.2983962137656         body
  494 gg 2012-02-29 11:11                                 mask 200x181x175             1x1x1  1773      label ...01.2983962137656        liver
  495 gg 2012-02-29 11:11                                 mask   99x117x98             1x1x1  1773      label ...01.2983962137656       spleen
  496 gg 2012-02-29 11:11                                 mask  148x114x85             1x1x1  1773      label ...01.2983962137656        heart
 2083 gg 2012-02-29 11:33                           tia,In-111 130x130x171 4.664x4.664x4.664   184 Bq.h_by_IA ...01.2983962137656            -
 2124 gg 2012-02-29 11:33         At-211,dose,results.3mEt,old 240x165x399             2x2x2   184        cGy ...01.2983962137656            -
 2163 gg 2012-02-29 11:33                            tia,Sc-47 130x130x171 4.664x4.664x4.664   184 Bq.h_by_IA ...01.2983962137656            -
 2156 gg 2012-02-29 11:33                           tia,Sm-153 130x130x171 4.664x4.664x4.664   184 Bq.h_by_IA ...01.2983962137656            -
 2155 gg 2012-02-29 11:33 Sm-153,dose_uncertainty,results.vuYW 240x165x399             2x2x2   184          % ...01.2983962137656            -
 ...
```

Give some words after the table name to filter the results and only display records that contains those words. The flag `--noheader` remove the two first lines that start with '#'. 


```
sydFind Image gg spect

#Table: Image. Number of rows: 11
#  id  p       acqui_date                     tags        size           spacing dicom     unit           ref_frame
  164 gg 2012-02-29 11:33 init_dicom,spect,dir_reg 130x130x171 4.664x4.664x4.664   184   counts ...01.2983962137656
 1953 gg 2012-02-29 11:33                 spect_dc 130x130x171 4.664x4.664x4.664   184 Bq_by_IA ...01.2983962137656
  165 gg 2012-03-01 11:21         init_dicom,spect 130x130x171 4.664x4.664x4.664   198   counts ...17.2984445870938
 1954 gg 2012-03-01 11:21                 spect_dc 130x130x171 4.664x4.664x4.664   198 Bq_by_IA ...01.2983962137656
 1950 gg 2012-03-01 11:21 init_dicom,spect,dir_reg 130x130x171 4.664x4.664x4.664   198   counts ...01.2983962137656
 1951 gg 2012-03-02 11:09 init_dicom,spect,dir_reg 130x130x171 4.664x4.664x4.664   209   counts ...01.2983962137656
  166 gg 2012-03-02 11:09         init_dicom,spect 130x130x171 4.664x4.664x4.664   209   counts ...10.2984504706766
 1955 gg 2012-03-02 11:09                 spect_dc 130x130x171 4.664x4.664x4.664   209 Bq_by_IA ...01.2983962137656
 1952 gg 2012-03-05 11:26 init_dicom,spect,dir_reg 130x130x171 4.664x4.664x4.664   221   counts ...01.2983962137656
  167 gg 2012-03-05 11:26         init_dicom,spect 130x130x171 4.664x4.664x4.664   221   counts ...76.2984504706766
 1956 gg 2012-03-05 11:26                 spect_dc 130x130x171 4.664x4.664x4.664   221 Bq_by_IA ...01.2983962137656

```

If several words are given, it behave like an logical 'and' (all words must be in the records to be displayed). 

If a word is preceded by the flag `-e` (like *exclude*), the records will not be displayed if it contains the word. 

Using `sydFind` without table name displays the list of tables of the database. 

Using the flag `-l` list the ids of the matched records. It is useful when `sydFind` is piped to another command. 

The flag `-f` allows to specify a (basic) output format. Use `-f help` to get a list of format, depending on the table name. For example for the table *Image*, you can use `-f filelist` to display a list of matched image filenames:

```
sydFind Image gg spect ini -f filelist

db/data/gg/NM_164.mhd db/data/gg/NM_165.mhd db/data/gg/NM_1950.mhd db/data/gg/NM_166.mhd db/data/gg/NM_1951.mhd db/data/gg/NM_167.mhd db/data/gg/NM_1952.mhd

```

It could be useful to view the images, for example with [vv](http://vv.creatis.insa-lyon.fr): 

```
vv --sequence `sydFind Image gg spect ini -f filelist`

```




