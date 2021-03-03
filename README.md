# Cartosat-1-DEM-Mosaic
Name of the program: Cartosat-1-DEM-mosaic

Title of the manuscript: A novel framework for seamless mosaic of Cartosat-1 DEM scenes

Author details:

1. Rajesh

Instructions to use this software:

A. SOFTWARE WITH SOURCE CODE:

The complete software with source code is provided in the folder "SourceCode". It consists of THREE sub-folders, namely,
DEMFilterLib
DEMFIlterRef
in_conv


1) DEMFilterLib: It defines separate functions for the DEM mosaic, namely, Mosaic_Average, Mosaic_Conventional, and Mosaic_Proposed, that correspond to average method, feathering-based blend method and Proposed method, respectively.

	1.1) Mosaic_Average function: It creates an virtual grid based on the extents of the input DEM scenes and assigns the elevation value to each cell of the grid, by computing the average of the elevation values in the overlapping regions wherever a cell contains more than one elevation value. Finally, gdal_translate is used to crop the result to ROI extent.

 	1.2) Mosaic_Conventional function: It is basically feathering-based blend method. This method considers two inputs at a time for the mosaic, according to its nature of the methodology (scalability property). It collects all the input DEM scene filenames in a list by lexicographic order and iterates by cascading the output generated from the first two inputs with the third input for the	mosaic process. Then, considering the resulted output with fourth input for the mosaic and so on. It computes the overlapping region and the valid-data(non-NODATA) cells along the scan and pixel direction for every iteration. It derives a weighting mechanism based on the number of valid-data cells and uses these weights in the mosaic process to compute a new elevation value. It does not use meta-data of the input scenes. 

	1.3) Mosaic_Proposed: Cartosat-1 mission has a unique path-row referencing scheme. This method makes use of this referencing-scheme which is available in the form of meta-data of each input DEM scene. It arranges the input scenes in a hierarchical manner and establishes a sequence. This method uses this sequence during mosaic process and creates a seamless mosaic. However, It also computes the overlapping region and the valid-data(non-NODATA) cells along the scan and pixel direction for every iteration. It derives a weighting mechanism based on the number of valid-data cells and uses these weights in the mosaic process to compute a new elevation value.

	
2) DEMFIlterRef: This folder contains the required dependencies. The DEMFIlterRef.cpp file has the main function to invoke the mosaic methods. All three mosaic methods are declared here. An automated build is provided in batch file "Build.bat".

3) in_conv: It reads the DEM in geotiff format into binary form along with the required meta-data population.

All the three mosaic methods use "gdal_translate" command to crop the mosaic output to an ROI extent. In this work, ROI extent is 1 degree x 1 degree (i.e., 110x 110 sq.km approximately).


B. FOLDER STURCTURE OF INPUT AND OUTPUT DATA:

   Sample input and output files are located in the folders TestData and OutputFiles, respectivley.
   The file name of an input DEM scene has the following naming convention:     P5008536_1P_20061202_556295
     where, P5 represents Sensor name followed by 6-digit orbit number,1P represents the payload followed by date in YYYYMMDD format; followed by path(3-digits) and row(last 3-digits);

Input files (DEMs) must be placed in a folder with a tile name, e.g., ABC_D81_E21 and place it under the folder "TestData".
   
The format of the inputs are in Geotiff and the output files generated by this software are also saved in Geotiff. 

This inputfolder and the outputfolder must be passed as the first and second arguments, respectively followed by the option which indicates mosaic method as explained in section C.

   In this repo, the output files provided in OutputFiles are in the png format to have glimpse of outputs and to save space.     
          
C. USAGE OF THIS SOFTWARE:

* Requirements:
.NET Framework v4.0.30319
Microsoft Visual Studio 2010

* Run Build.bat file for automated build.  
 A folder named Release will get generated in SourceCode folder. Find an executable file named DEMFIlterRef.exe in it. 

* Go to the location where DEMFIlterRef.exe is avaialble in command prompt.
Now, software can be executed through command prompt as mentioned below:   
   
Usage: DEMFIlterRef.exe {inputfolder} {outputfolder} {1 | 2| 3}
   
Example:
..\Cartosat-1-DEM-Mosaic-master\SourceCode\Release>DEMFIlterRef.exe E:\TestData E:\OutputFiles 1

* Details on input and output Commandline arguments
 inputfolder: Location of the parent folder of the tile folder (e.g., ABC_D81_E21). Tile folder contains input DEM scenes (in tif format).
  output folder: It must have read/write permission. It must be different from the inputfolder. The DEM mosaic outputs will be created in this folder. The ouputs with the following names, namely, ABC_D81_E21_average.tif, ABC_D81_E21_conventional.tif, and ABC_D81_E21_proposed.tif will be created by the mosaic process using average method, conventional feathering-based blend method, and the proposed method, respectively.
  {1|2|3}: option 1 for Proposed method; 2 for average method ;3 for conventional feathering-based blend method.
*   This software is compiled and executed in Microsoft visual studio 2010.
* This software is implemented with an intension to include these methods as static library for better reusage. 
