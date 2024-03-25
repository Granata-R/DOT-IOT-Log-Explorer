# DOT-IOT-Log-Explorer

## Introduction
DOT-IOT-Log-Explorer is a software for broad analysis of electrophysiological data. In short, it allows to perform an analysis of: spectral properteis; signle units (through external sorters, such as KiloSort, see:); comodulation between theta and gamma components of the EEG/LFP; inner feature of unwinding option for object exploration/epoch selection, with compatibility with the popular ANY-maze software; analysis of sleep segmentation (through AccuSleep, see).

The software has been designed to analyze data extracted from Tint + data extracted from .bin sources through SpikeInterface (see...).

## Screenshots of the main GUIs

## Preprocessing in Tint and Python
The preprocessing of the data operates thorugh Python Jupyter notebooks. For the moment the preoprocessing involes two third-party software that are:
- Tint
- SpikeInterface

Plus, external spike sorters are required for performing unit extraction.

### About the possible data files
The recording file as collected by Axona DacqUSB can be stored in two different formats. For this guide and analysis pipeline, two types of formats are took into consideration: .bin (binary) files and .n (unit, e.g., .1 .2 .3, .4) files.

In short, they are used for different purposes. The .n file is used to be loaded in Tint and export the EEG .txt table. The bin file, instead, which comprehends the raw data, is used to extract units and LFPs through dedicated Jupyter Notebooks.

What is always necessary necessary is the .n file (obtainable through Axona converter from the .bin file), because it is given to Tint and allows to export the EEG table, containing EEG information, speed, coordinates, and other metrics such as the head turn angles. This table in fact is the first one to be processed.

### Extracting the table from Tint
By opening any .n file of the experiment into Tint, it is possible to retrieve some important information in form of .txt table. To extract the table, open any .n file into Tint. Move all spikes to cluster 1. Then, export the table by making sure that the export sampling rate of the EEG, that is 250 hz, is selected. For the rest, select the default values. Keep other default parameters in the export section.

The result is a quite large .txt file which contains the basic time series about the exeriment. This .txt file is the first one that must be loaded in the main GUI.

### Extracting spikes
For our personal preprocessing we preferred to not rely on the manual clustering with Tint. Instead, we preferred using some more advanced spike sorters such as KiloSort.
In order to use these external sorters, you must first install them in your local workstation. Then, provided that you have Python installed (see requirements) you can use the "extract..." file that is a Jupyter Notebook which automatically launches the sorting. The sorting can be made only if a .bin file is given to the notebook. Otherwise, it is not possible to load the spikes.

Given that you meet all the requirements, just exectute each block of the Jupyter Notebook in order to launch the sorting. NB: the second block prompts the user with a dialog to choose the .bin file.
The sorted data will be saved in a subfolder placed in the folder of the .bin file. 

A second step is then needed.
After you have performed the sorting of the data, you want to do some manual clustering through Phy. As the sorting was carried out by SpikeInterface, the format of the output files will be standardized no matter which soerter you use. 
Once you are done with sorting and manual clustering (make sure to save in Phy), the second step involves organizing the spiking information in a form that MATLAB likes. To do so, you can simply do this via a second Jupyter Notebook, called "", that does two things: 
1) it generates .mat files containing raster information of the spiking activity
2) it allows to preview the coordinate information vs. the spike information for place cell analysis.
   this latter option is just a preview. In MATLAB, it is possible to use the dedicated module "Unit Explorer" for a deeper analysis.

Again, this second notebook is pretty straightforward and can easily executed block by block.
Similarly to the first notebook, it prompts the user with a dialog to select the desired file upon executing second block.

### Extracting LFPs
To extract the LFPs for each channel, a third Jupyter notebook can be used.
This notebook, called "", requires the .bin file. Its output is a folder called "LFP" that contains .mat matrices with the LFPs.

## Explore preprocessed data in MATLAB
Once you have preprocessed the information you are interested in, you want to explore it in DOT IOT Log Explorer, that can be run through MATLAB through AppDesigner o by simply running the command  "DOT_IOT_Log_Explorer" in the command window.

## Unwinding behaviorally-relevant epochs

## Analyze sleep epochs

## Unit analysis

## Comodulation analysis

## Results management

# Credits
