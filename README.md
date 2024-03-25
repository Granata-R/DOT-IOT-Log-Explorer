# DOT-IOT-Log-Explorer

## Introduction
DOT-IOT-Log-Explorer is software for broad analysis of electrophysiological data. In short, it allows performing an analysis of spectral properties; single units (through external sorters, such as KiloSort, see:); comodulation between theta and gamma components of the EEG/LFP; inner feature of unwinding option for object exploration/epoch selection, with compatibility with the popular ANY-maze software; analysis of sleep segmentation (through AccuSleep, see).

The software has been designed to analyze data extracted from Tint + data extracted from .bin sources through SpikeInterface (see...).

## Screenshots of the main GUIs

## Preprocessing in Tint and Python
The preprocessing of the data operates through Python Jupyter notebooks. For the moment, preprocessing involves two third-party software that are:
- Tint
- SpikeInterface

Plus, external spike sorters are required for performing unit extraction.

### About the possible data files
The recording file collected by Axona DacqUSB can be stored in two different formats. For this guide and analysis pipeline, two types of formats are taken into consideration: .bin (binary) files and .n (unit, e.g., .1 .2 .3, .4) files.

In short, they are used for different purposes. The .n file is used to be loaded in Tint and export the EEG .txt table. The bin file, instead, which comprehends the raw data, is used to extract units and LFPs through dedicated Jupyter Notebooks.

What is always necessary is the .n file (obtainable through Axona converter from the .bin file), because it is given to Tint and allows exporting the EEG table, containing EEG information, speed, coordinates, and other metrics such as the head turn angles. This table, in fact, is the first one to be processed.

### Extracting the table from Tint
By opening any .n file of the experiment into Tint, it is possible to retrieve some important information in the form of .txt table. To extract the table, open any .n file into Tint. Move all spikes to cluster 1. Then, export the table by making sure that the export sampling rate of the EEG, that is 250 Hz, is selected. For the rest, select the default values. Keep other default parameters in the export section.

The result is a quite large .txt file that contains the basic time series about the experiment. This .txt file is the first one that must be loaded in the main GUI.

### Extracting spikes
For our personal preprocessing, we preferred to not rely on the manual clustering with Tint. Instead, we preferred using some more advanced spike sorters such as KiloSort.
In order to use these external sorters, you must first install them on your local workstation. Then, provided that you have Python installed (see requirements), you can use the "extract..." file that is a Jupyter Notebook which automatically launches the sorting. The sorting can be made only if a .bin file is given to the notebook. Otherwise, it is not possible to load the spikes.

Given that you meet all the requirements, just execute each block of the Jupyter Notebook in order to launch the sorting. NB: the second block prompts the user with a dialog to choose the .bin file.
The sorted data will be saved in a subfolder placed in the folder of the .bin file. 

A second step is then needed.
After you have performed the sorting of the data, you want to do some manual clustering through Phy. As the sorting was carried out by SpikeInterface, the format of the output files will be standardized no matter which sorter you use. 
Once you are done with sorting and manual clustering (make sure to save in Phy), the second step involves organizing the spiking information in a form that MATLAB likes. To do so, you can simply do this via a second Jupyter Notebook, called "", that does two things: 
1) it generates .mat files containing raster information of the spiking activity
2) it allows previewing the coordinate information vs. the spike information for place cell analysis.
   this latter option is just a preview. In MATLAB, it is possible to use the dedicated module "Unit Explorer" for a deeper analysis.

Again, this second notebook is pretty straightforward and can be easily executed block by block.
Similarly to the first notebook, it prompts the user with a dialog to select the desired file upon executing the second block.

### Extracting LFPs
To extract the LFPs for each channel, a third Jupyter notebook can be used.
This notebook, called "", requires the .bin file. Its output is a folder called "LFP" that contains .mat matrices with the LFPs.

## Explore preprocessed data in MATLAB
Once you have preprocessed the information you are interested in, you want to explore it in DOT IOT Log Explorer, which can be run through MATLAB through AppDesigner or by simply running the command "DOT_IOT_Log_Explorer" in the command window.

Thereupon, the main GUI opens. On the top left, by clicking on the Load button, you can choose the .txt file containing your information. Please notice that on the right of the open button there are several check buttons. the most important ones in this phase are the LFPs and Spikes, by which you can decide if importing those information as well. Please make sure that the LFP file is placed in the same folder as the desired txt file. if spikes have been extracted, please keep the same folder structure. indeed, parent folders contain the information from Phy that can be retrieved if needed.

If the GUI loads correctly, you should have an interface like this.
the first column represents actions that the user can do. these encompass selecting a time window or deciding which behavioral epochs they want to use. most of these parameters are self-explanatory.
the second column represents the visualization output. here, you can browse in default conditions the selected trace, which is by default the EEG or one of the channel LFPs if you selected it. this axes are also the axes used for showing different comodulation metrics and signal components. see: comodulation for this.

immediately below the EEG axis, there is the unit axis. it will show a raster plot showing the units in time. more specific metrics can be explored via the units module.

still below, there is the axis which contains the information regarding behavioral epochs. so far, for "behavior" we can mean high speed; manual unwound epochs (for example, object exploration, or interaction with a conspecific, or a particular stereotypy in the animal).
