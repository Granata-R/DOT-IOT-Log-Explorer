# DOT-IOT-Log-Explorer

DOT-IOT-Log-Explorer is a Python/MATLAB software for broad analysis of electrophysiological data from freely behaving animals. It can be used for visualization and quantitative analysis of spectral properties of electroencephalogram (EEG) and local field potentials (LFPs), movement patterns, sleep patterns, neuronal activity, and phase-amplitude coupling, during specified time windows or behaviorally relevant epochs. 

DOT-IOT-Log-Explorer was created by Rocco Granata as tailored software for the neuropsychopharmacology laboratory of Dr. Elvira De Leonibus. 
It is made of around 20k lines of custom Python/MATLAB code and leverages compatibility with state-of-the-art tools in systems neuroscience. These include [SpikeInterface](https://github.com/SpikeInterface/spikeinterface) (for standardized data extraction), [Kilosort](https://github.com/MouseLand/Kilosort) (for spike sorting), [AccuSleep](https://github.com/zekebarger/AccuSleep) (for semi-automated sleep segmentation), [ANY-maze](https://www.any-maze.com/) (for manual review of behavior through key logs). 

## Requirements and limitings factors

- So far, the workflow has been tested on **Windows** only.

- **Python (Jupyter) + SpikeInterface** is always needed to preprocess the recording files. Since the pipeline assumes to preprocess the recording files via **SpikeInterface (always needed)**, please use the [SpikeInterface guides](https://github.com/SpikeInterface/spikeinterface) to set up the environment.

- **MATLAB** is required. Any version above **R2019** should work fine.
Ensure that you have installed these add-ons:

- 1) **Image Processing Toolbox** (always required);

- 2) **Signal Processing Toolbox** (always required);

- 3) **Statistics and Machine Learning Toolbox** (might be required for certain actions).
   
- 5) **Deep Learning Toolbox** (might be required for certain actions).

If not, you can do this by following the official [Mathworks guide for installing Add-Ons](https://it.mathworks.com/help/matlab/matlab_env/get-add-ons.html).

- A **spike sorter** (optional). Some sorters (e.g., spyKING CIRCUS; Tridesclous) are directly installed with SpikeInterface. For other sorters, please refer to this [link](https://spikeinterface.readthedocs.io/en/latest/install_sorters.html).

- For the moment, the workflow is limited to data recorded through the [**Axona DacqUSB recording system**](http://www.axona.com/) and processed with Axona's sorter **TINT**. 

## Preprocessing
Data preprocessing is made with Axona TINT and custom Jupyter notebooks. 

**Note: about the possible data formats***
The recording files collected by Axona DacqUSB can be saved in two main formats, *.bin* (continuous) and *.n* (snippets. E.g., *.1*, *.2*, *.3*, ..., *.numberOfChannels*). 

Both formats are used in our workflow. The *.bin* format can be used to extract units and LFPs through dedicated Jupyter Notebooks. The *.n* format (which can be obtained from the *.bin* through Axona DacqUSB converter) is used to export basic information about EEG, speed, coordinates, and other metrics in form of a text table (see next section). The latter is always required to run DOT-IOT Log Explorer.

### TINT preprocessing
Among the other things, Axona TINT allows to export a *.txt* table file that contains basic information about EEG, speed, coordinates, and other metrics, from the original *.n* file. 
The goal of this step is to "simulate" a spike sorting in order to get this table. The table is always required for launching DOT-IOT Log Explorer. Actual spike sorting and LFPs can be added later by following the next sections of preprocessing.

**Steps for extracting the *.txt* table with TINT using the *.n* file**
1) First, make sure to have the *.n* files. If you have the *.bin* file only, you can convert it through Axona DacqUSB converter. If you encounter some limitations based on the sampling rate please check Axona DacqUSB manual.
2) Load any of the *.n* files of the experiment in TINT. It doesn't matter which channel you select, because TINT spikes are not used. 
3) Move all TINT spikes to *cluster 1* and *save* all.
4) Export the table through the *Export* section. Make sure that the export sampling rate of the EEG (that is 250 Hz), is selected. For the rest, keep the default values.

### Sorting spikes with SpikeInterface in Jupyter
Our current workflow does not consider the sorting made with TINT, favoring external sorters such as KiloSort. 

1) **Spike sorting**. An automated sorting can be made using the custom Jupyter Notebook *1a - SpikeSorting_AxonaDacqUSB.ipynb* with leverages SpikeInterface. The only thing required is the *.bin* recording. The output is standardized regardless of the sorter used.
2) **Manual curation**. Found units can be manually curated using [Phy](https://github.com/cortex-lab/phy). Phy should have already been installed via SpikeInterface.
3) **Creation of *.mat* files**. Once sorting and manual clustering are complete, the next step involves organizing the spiking information as *.mat* files for MATLAB. This can be achieved through a second custom Jupyter Notebook. This second notebook requires to select the original *.txt* table from TINT. It will process the TINT table by adding the spikes in the "cluster" columns.

Both the Jupyter Notebook mentioned are straightforward and thoroughly commented.

### Extracting LFPs
To extract the LFPs for each channel, a third custom Jupyter notebook can be used.
It requires the *.bin* file and, similarly to the spike one, its output is in *.mat* format.

## Explore preprocessed data in MATLAB
Once you've completed preprocessing the relevant information, you can delve into it using DOT IOT Log Explorer, which is accessible via MATLAB either through AppDesigner or by directly executing the command "DOT_IOT_Log_Explorer" in the command window.

## DOT-IOT Log Explorer
At the top left, you'll find the *Load* button, allowing you to select the *.txt* table from TINT/Jupyter. Among the check buttons on the right, the most crucial ones during this phase are *LFPs* and *Spikes*, enabling you to choose whether to import this additional information. This is how the graphical user interface (GUI) appears before loading the data.

The figure shows an example of the GUI after loading the data. The GUI might be divided in three columns or blocks. The left block contains handles for user control. These involve edit field to select a time window or to determine which behavioral epochs to use. The center block contains plot for data visualization. Going from top to bottom, there are:
1) EEG/LFP time series;
2) Units raster plot;
3) Behavioral epochs;
4) Spectrogram.

The behavioral epochs might be referred to animal speed, object exploration, interaction with a conspecific, stereotypies, sleep epochs, and more. Behavioral epochs are always binarized in terms of *Behavior YES* or *Behavior NO*, according to GUI paramters.

The right block shows some initial results related to the explored data, after filtering it with the selected time window and behavioral epochs. The mean activity of the units is on the top, while periodograms are on the bottom. All results are presented in two pairs of colors: yellow vs. magenta for discriminating the selected time window vs. all other times, respectively, and green vs. red for defining *Behavior YES* vs. *Behavior NO*, respectively. This color pattern is inherited by child apps, described below.

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

# Results averaging
## Saving the module analysis
The analysis can be conducted at different levels using the various modules.
The GUIs described above handle data regarding a single experiment.

Within each GUI, there is an orange "Save" button alongside edit fields for specifying mouse and test IDs, the latter being useful for generating the filename for the output file. The output file is stored in the Results folder. 

### About the automatic filename generation
For the moment, the filename contains information about: mouse ID, test ID, channel used, time window/behavior epochs used. If multiple analyses are performed on the same file, with the same time window/behavior filters, then the filename will include specific labels just before the format. These labels might appear as "FILENAME_"[Spg][Mov][Sleep][Pac][Units]".mat" if the analysis includes spectrum, movement, sleep, comodulation, and single units.

## Using the Results module
Finally, the results module has the aim to get all the outputs from single experiments and averaging them. This "averaging" works on multiple levels depending on the types of analysis that have been made. 
### Organizing group folders
At the core of the results module there is the idea that the results obtained from main GUIs should be copied and organized in folders. Each folder may contain all the results sharing a particular experimental conditions (e.g., you might have a folder where all male animal results are saved, and the other for females). 
It is probably better to do this by duplicating the files in the /Results/ folder. In this way the "raw" results will remain, while you can save isntead the group folder anywhere.

## Results module
The results module is still in development because at the moment is a bit messy.
For the moment, you can use to perform the following opeartions:
1) periodogram averaging
2) analysis of sleep phases (REM, NREM): how much they last and how much are they scattered
3) analysis of movement. Half a dozen of metrics are available at the moment! 
4) comodulation analysis
5) spike analysis (to be done)

The average results are computed both in terms of distributions and in terms of box chart. While generating box chart the program automatically computes non parametric tests.
While averaging the periodogram, also in that case non parameteric tests are used for each pyssiologically relevant interval of frequencies.
