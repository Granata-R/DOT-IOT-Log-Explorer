# DOT-IOT-Log-Explorer

## About this guide
This guide has been written by the author of DOT-IOT Log Explorer with the intent to show potential users its capabilities, and to guide current users as well.
As of march 26, the software is becoming quite big. therefore it's difficult to me to explain the meaning of each single button. i will probably do this in the future, updating this guide step by step. 
Also, I will make sure (but this is just a detail!) to put tips for the user while the cursor is above each button.

The three macro sections about the guide are about Preprocessing, Exploring the data in MATLAB, and averaging the results.
the preprocessing is made in python mostly. the second part describes how to use the main GUIs I have created to explore and save tailored analyses. finally, i describe how to verage the results from single experiments belonging to different groups, as described in the "averaging the results".

## Credits
The software was created by Rocco Granata with the inputs of Diletta Cavezza.

DOT-IOT-Log-Explorer is software for broad analysis of electrophysiological data. In short, it allows performing an analysis of spectral properties; single units (through external sorters, such as KiloSort, see:); comodulation between theta and gamma components of the EEG/LFP; inner feature of unwinding option for object exploration/epoch selection, with compatibility with the popular ANY-maze software; analysis of sleep segmentation (through AccuSleep, see).

The software has been designed to analyze data extracted from Tint + data extracted from .bin sources through SpikeInterface (see...).

This guide has the meaning of being a general overview of the program use. It is not detailed in all the specific handles for the moment.

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

# Explore preprocessed data in MATLAB
## DOT-IOT Log Explorer (main GUI)
Once you've completed preprocessing the relevant information, you can delve into it using DOT IOT Log Explorer, which is accessible via MATLAB either through AppDesigner or by directly executing the command "DOT_IOT_Log_Explorer" in the command window.

Upon launching, the main GUI appears. At the top left, you'll find the Load button, allowing you to select the .txt file containing your data. Take note of the check buttons to the right of the open button. Among them, the most crucial ones during this phase are LFPs and Spikes, enabling you to choose whether to import this additional information. Ensure that the LFP file is stored in the same folder as the desired .txt file. If spikes have been extracted, maintain the same folder structure, as parent folders contain information from Phy that may be necessary.

If the GUI loads correctly, you'll see an interface resembling this. The first column represents user actions, such as selecting a time window or determining which behavioral epochs to utilize. Most of these parameters are self-explanatory. The second column displays visualization output. Here, you can view the default trace, typically EEG or one of the channel LFPs if selected. These axes also display various comodulation metrics and signal components.

Directly below the EEG axis is the unit axis, showcasing a raster plot depicting units over time. Further specific metrics can be explored via the units module.

Continuing downward, you'll find the axis containing information regarding behavioral epochs. Currently, "behavior" encompasses high speed, manual unwound epochs (e.g., object exploration, interaction with a conspecific, or specific stereotypic behaviors in the animal), and sleep epochs.

Finally, below this is the spectrogram.

On the right column, you'll find some initial result outputs related to the explored data based on the selected time window and behavioral epochs. These results are presented using two pairs of colors: yellow versus magenta (for defining the selected time window versus all other times) and green versus red (for defining behavior "yes" versus behavior "no"). Bars are displayed to emphasize the activity of single neurons, and periodograms can be computed based on these filters.

## Analysis of movement and sleep patterns
Bly clicking on the "Movement and sleep" button in the main GUI, a submodule for this analysis is launched.
The module is composed of two tabs for movement and sleep, respectively. 

### Movement
Regarding movement, an overview of the speed time series and cumulative distance are provided. The module inherits the epoch filtering from main GUI: accordingly, it shows the distribution of the speed during these epochs alongside descriptive metrics in the form of box charts. 

The speed time series and the cumulative distance can be color coded on the basis of hard thresholds applied to the speed vector (and its accelleration). These hard thresholds span both in terms of value (cm/s or cm/s^2) and time and are
1) Movement (i.e., everything that is not immobility) [green]
2) Running (sustained high-velocity epochs) [red]
3) Burst (sustained high-accelleration epochs) [blue]
These hard thresholds can be applied to the user. The figure shows an example of color coded time series and of a fraction of it.

I added also a section for calculating thigmotaxis and for calculating an index of the head turning angles.

### Sleep
The software incorporates an edited version of the popular software AccuSleep (see:) for sleep segmentation.
Briefly, it allows to automatically assign the movement epochs (as defined by the user in the Movement tab) to wakefulness. Note that we use a setting by which the immobility is immobility only if it lasts only 1 second. therefore, artifacts due to the natural movement are in part excluded.
Please read the AccuSleep documentation for seeing how to use it.
Once the data is saved, this edited version generates a folder (in the same folder of the original .txt file) where the sleep labels are saved. thereupon they can be loaded by the main GUI for displaying them and to check, for example, the unit behavior during wakefulness, REM, NREM.

## Comodulation module
The comodulation module allows to perform analysis of theta vs. gamma components of the source signal, which can be EEG or LFP.

For the comodulatio I based mostly on three metrics which are phase locking value (PLV), MEAN VECTOR LENCGTH ( MVL), modulation index (MI). Furthermore, a fourth metric is the cross correlation. the two signals are the theta and gamma components of the original trace.

The GUI appears as follows.
### Comodulograms
The left panel encompass the possibility to compute a "comodulogram". this graph computes the comodulation with any of the three phase-amplitude coupling methods MVL, PLV or MI) between fixed bins of frequency. Specifically, in the hippocampus the "driver" is lower frequencies, while the "signal" is higher frequency. in the hippocampus they correspond to to theta and gamma, mostly.
the typical shape of the comodulogram is a cloud shape whose bounding box fall within the theta (x axis) and gamma (y axis) ranges.

the comodulogram can be computed as well upon filtering the original trace according to the time window or behavioral epochs.

### PAC-per-second
The rightmost panel are dedicated to the PAC per second. As the name suggests, this code computes these metrics for each second of the trace. The result is a bar that can be visualized in the original main GUI, by selecting the appropriate radio buttons.
The PAC per second metric is also endowed by a shuffling control. You can decide how many times you want to shuffle. Based on the shuffling repetitions a surrogate PAC-per-second is generated and subtracted statistically to the raw data.

On the right mostpanels, you can see the bars showing the mean raw or filtered scores during time or behavioral epochs. 

## Unit explorer module
The unit explorer module is still in development. For the moment, it allows the computation of the distributions of the spiking activity of single units. On the right panel, the place cell activity in space can be explored with different methods.

## Unwinding module
The unwinding for object exploration, or for any type of behavior the user wants to segment, can be done by clicking on the Unwind button in the main GUI. 
This button launches a dialog which basically is a time-stamped keylogger. Through this dialog, by pressing the keys 1 2 3 7 8 9 according to e.g, exloration of object 1 2 3 7 8 9, the user can create a vector with the object exploration epochs. after saving it, the output can be saved in a subfolder of the original text file. Then, it can be loaded in the main GUI by clicking this button.

### Exploring single keys
Once you have perfored your unwinding, you can see what happens to the periodogram while the animal was exploring all object, or a single object (just use the dropdown menu). furthermore you can limit the periodogram and unit computation to the first n seconds of exploration.
finally, since precise unwinding is difficult and you could not be as fast as you should, you can also shift everything on the left of n seconds usig this button. You can also move everything on the right if you want. it might be useful for "anticipatory" behavior!

## Saving the module analysis
The analysis can be conducted at different levels using the various modules. Regardless, to save the file, you need to click on the orange "Save" button after specifying the mouse and test IDs, which will generate the filename for the output file. The output file is stored in the Results folder. If multiple analyses are performed on the same file, with the same time window/behavior filters, then the filename will include specific labels just before the format. These labels might appear as "FILENAME_"[Spg][Mov][Sleep][Pac][Units]".mat" if the analysis includes spectrum, movement, sleep, comodulation, and single units.
