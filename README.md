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
For our preprocessing, we opted not to rely on manual clustering with Tint. Instead, we preferred utilizing more advanced spike sorters such as KiloSort. To utilize these external sorters, you need to first install them on your local workstation. Then, assuming you have Python installed (see requirements), you can utilize the "extract..." file, which is a Jupyter Notebook that automatically initiates the sorting process. Sorting can only occur if a .bin file is provided to the notebook; otherwise, it's not possible to load the spikes.

If you meet all the requirements, simply execute each block of the Jupyter Notebook to initiate the sorting. Note: the second block prompts the user with a dialog to choose the .bin file. The sorted data will be saved in a subfolder within the .bin file's directory.

A second step is then necessary. After sorting the data, you'll want to conduct manual clustering through Phy. Since the sorting was performed by SpikeInterface, the format of the output files will be standardized regardless of the sorter used. Once sorting and manual clustering are complete (ensure to save in Phy), the next step involves organizing the spiking information in a format compatible with MATLAB. This can be achieved through a second Jupyter Notebook, which performs two tasks:

1) Generates .mat files containing raster information of the spiking activity.
2) Allows previewing the coordinate information versus the spike information for place cell analysis. This latter option serves as a preview. In MATLAB, the dedicated module "Unit Explorer" enables deeper analysis.

Again, this second notebook is straightforward and can be executed block by block. Similar to the first notebook, it prompts the user with a dialog to select the desired file upon executing the second block.

### Extracting LFPs
To extract the LFPs for each channel, a third Jupyter notebook can be used.
This notebook, called "", requires the .bin file. Its output is a folder called "LFP" that contains .mat matrices with the LFPs.

## Explore preprocessed data in MATLAB
Once you've completed preprocessing the relevant information, you can delve into it using DOT IOT Log Explorer, which is accessible via MATLAB either through AppDesigner or by directly executing the command "DOT_IOT_Log_Explorer" in the command window.

Upon launching, the main GUI appears. At the top left, you'll find the Load button, allowing you to select the .txt file containing your data. Take note of the check buttons to the right of the open button. Among them, the most crucial ones during this phase are LFPs and Spikes, enabling you to choose whether to import this additional information. Ensure that the LFP file is stored in the same folder as the desired .txt file. If spikes have been extracted, maintain the same folder structure, as parent folders contain information from Phy that may be necessary.

If the GUI loads correctly, you'll see an interface resembling this. The first column represents user actions, such as selecting a time window or determining which behavioral epochs to utilize. Most of these parameters are self-explanatory. The second column displays visualization output. Here, you can view the default trace, typically EEG or one of the channel LFPs if selected. These axes also display various comodulation metrics and signal components.

Directly below the EEG axis is the unit axis, showcasing a raster plot depicting units over time. Further specific metrics can be explored via the units module.

Continuing downward, you'll find the axis containing information regarding behavioral epochs. Currently, "behavior" encompasses high speed, manual unwound epochs (e.g., object exploration, interaction with a conspecific, or specific stereotypic behaviors in the animal), and sleep epochs.

Finally, below this is the spectrogram.

On the right column, you'll find some initial result outputs related to the explored data based on the selected time window and behavioral epochs. These results are presented using two pairs of colors: yellow versus magenta (for defining the selected time window versus all other times) and green versus red (for defining behavior "yes" versus behavior "no"). Bars are displayed to emphasize the activity of single neurons, and periodograms can be computed based on these filters.

## Saving the data
The analysis can be conducted at different levels using the various modules. Regardless, to save the file, you need to click on the orange "Save" button after specifying the mouse and test IDs, which will generate the filename for the output file. The output file is stored in the Results folder. If multiple analyses are performed on the same file, with the same time window/behavior filters, then the filename will include specific labels just before the format. These labels might appear as "FILENAME_"[Spg][Mov][Sleep][Pac][Units]".mat" if the analysis includes spectrum, movement, sleep, comodulation, and single units.
