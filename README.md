# DOT-IOT Log Explorer

DOT-IOT Log Explorer is a Python/MATLAB software for broad analysis of electrophysiological data from freely behaving animals. It can be used for visualization and quantitative analysis of spectral properties of electroencephalogram (EEG) and local field potentials (LFPs), movement patterns, sleep patterns, single neuron activity, and phase-amplitude coupling, during specified time windows or behaviorally relevant epochs. 

DOT-IOT Log Explorer has been created by Rocco Granata, with inputs from Diletta Cavezza, as tailored software for the neuropsychopharmacology laboratory of Dr. Elvira De Leonibus. Originally designed for hippocampal recordings during memory tests, such as the different object task (DOT) and the identical object task (IOT)<sup>[1]</sup>, its versatility allows for potential application in a range of behavioral studies.

![DOT IOT Log Explorer](https://i.imgur.com/2qP9W6E.png)

**Reference**

<sup>[1]</sup>Sannino, S., Russo, F., Torromino, G., Pendolino, V., Calabresi, P., & De Leonibus, E. (2012). Role of the dorsal hippocampus in object memory load. In Learning &amp; Memory (Vol. 19, Issue 5, pp. 211–218). Cold Spring Harbor Laboratory. https://doi.org/10.1101/lm.025213.111 

# Movie

***[This short movie](https://i.imgur.com/tTH4YRX.gif)*** shows an example of interaction with the main graphical user interface (GUI). Several child modules can then be launched from this main GUI, as described in the instruction below.

# Quick installation and use guide 
## 1 Requirements

The software leverages compatibility with state-of-the-art computational tools in systems neuroscience. These include [SpikeInterface](https://github.com/SpikeInterface/spikeinterface) (for standardized data extraction), [Kilosort](https://github.com/MouseLand/Kilosort) (for spike sorting), [AccuSleep](https://github.com/zekebarger/AccuSleep) (for semi-automated sleep segmentation), [ANY-maze](https://www.any-maze.com/) (for manual review of behavior through key logs). 
Basic requirements are below.

- **Windows** operating system.

- [**Axona DacqUSB**](http://www.axona.com/) as recording system + Axona's sorter **TINT**.

- **MATLAB**. Any version above **R2019** should work fine.
Ensure that you have installed these add-ons. 
If not, you can do this by following the official [Mathworks guide for installing Add-Ons](https://it.mathworks.com/help/matlab/matlab_env/get-add-ons.html).

   - **Image Processing Toolbox** (always required);

   - **Signal Processing Toolbox** (always required);

   - **Statistics and Machine Learning Toolbox** (might be required for certain actions).
   
   - **Deep Learning Toolbox** (might be required for certain actions).

- A **spike sorter** of interest (optional). Some sorters (e.g., spyKING CIRCUS; Tridesclous) are directly installed with SpikeInterface. For other sorters, please refer to this [link](https://spikeinterface.readthedocs.io/en/latest/install_sorters.html).
  
- **Python (Jupyter) + SpikeInterface**. Python Jupyter notebooks and [SpikeInterface](https://github.com/SpikeInterface/spikeinterface/tree/main) are necessary for spike and LFP processing. One quick way to install all these requirements is to follow the [SpikeInterface beginner guide for Python installation](https://github.com/SpikeInterface/spikeinterface/tree/main/installation_tips).

## 2 Preprocessing
Data preprocessing is made with Axona TINT and custom Jupyter notebooks. 

**Note: about the possible data formats**

The recording files collected by Axona DacqUSB can be saved in two main formats, *.bin* (continuous) and *.n* (snippets. E.g., *.1*, *.2*, *.3*, ..., *.numberOfChannels*). 

Both formats are used in our workflow. The *.bin* format can be used to extract units and LFPs through dedicated Jupyter Notebooks. The *.n* format (which can be obtained from the *.bin* through Axona DacqUSB converter) is used to export basic information about EEG, speed, coordinates, and other metrics in form of a text table (see next section). The latter is always required to run DOT-IOT Log Explorer.

### 2.1 TINT preprocessing
Among the other things, Axona TINT allows to export a *.txt* table file that contains basic information about EEG, speed, coordinates, and other metrics, from the original *.n* file. 
The goal of this step is to "simulate" a spike sorting in order to get this table. The table is always required for launching DOT-IOT Log Explorer. Actual spike sorting and LFPs can be added by following the next sections of preprocessing.

**Steps for extracting the *.txt* table with TINT using the *.n* file**
1) First, make sure to have the *.n* files. If you have the *.bin* file only, you can convert it through Axona DacqUSB converter. If you encounter some limitations based on the sampling rate please check Axona DacqUSB manual.
2) Load any of the *.n* files of the experiment in TINT. It doesn't matter which channel you select, because TINT spikes are not used. 
3) Move all TINT spikes to *cluster 1* and *save* all.
4) Export the table through the *Export* section. Make sure that the export sampling rate of the EEG (that is 250 Hz), is selected. For the rest, keep the default values.

### 2.2 Sorting spikes with SpikeInterface in Jupyter
Our current workflow overlooks TINT's sorting, preferring Kilosort and manual clustering in Phy.

1) **Spike sorting**. An automated sorting can be made using the custom Jupyter Notebook *1a - SpikeSorting_AxonaDacqUSB.ipynb* with leverages SpikeInterface. The only thing required is the *.bin* recording. The output is standardized regardless of the sorter used.
2) **Manual curation**. Found units can be manually curated using [Phy](https://github.com/cortex-lab/phy). Phy should have already been installed via SpikeInterface.
3) **Creation of *.mat* files**. Once sorting and manual clustering are complete, the next step involves organizing the spiking information as *.mat* files for MATLAB. This can be achieved through a second custom Jupyter Notebook. This second notebook requires to select the original *.txt* table from TINT. It will process the TINT table by adding the spikes in the "cluster" columns.

Both the Jupyter Notebook mentioned are straightforward and thoroughly commented.

### 2.3 Extracting LFPs
To extract the LFPs for each channel, a third custom Jupyter notebook, *LFPExtraction_AxonaDacqUSB.ipynb*, can be used.
It requires the *.bin* file and, similarly to the spike one, its output is in *.mat* format.

## 3 Explore preprocessed data in MATLAB
Once you've completed preprocessing the relevant information, you can delve into it using DOT IOT Log Explorer, which is accessible via MATLAB either through AppDesigner or by directly executing the command "DOT_IOT_Log_Explorer" in the command window.

## 3.1 DOT-IOT Log Explorer
At the top left, you'll find the *Load* button, allowing you to select the *.txt* table from TINT/Jupyter. Among the check buttons on the right, the most crucial ones during this phase are *LFPs* and *Spikes*, enabling you to choose whether to import this additional information. ***Fig. 3.1.a*** shows the GUI before loading the data.

![Fig. 3.1.a](https://i.imgur.com/jODL3vH.png)

***Fig. 3.1.a***

***Fig. 3.1.b*** shows an example of the GUI after loading the data. The GUI might be divided in three columns or blocks. The left block contains handles for user control. These involve edit field to select a time window or to determine which behavioral epochs to use. The center block contains plot for data visualization. Going from top to bottom, there are:
1) EEG/LFP time series.
2) Units raster plot.
3) Behavioral epochs.
4) Spectrogram.

![Fig. 3.1.b](https://i.imgur.com/nxvrIqL.png)

***Fig. 3.1.b***

The behavioral epochs might be referred to animal speed, object exploration, interaction with a conspecific, stereotypies, sleep epochs, and more. Behavioral epochs are always binarized in terms of *Behavior YES* or *Behavior NO*, according to GUI paramters.

The right block shows some initial results related to the explored data, after filtering it with the selected time window and behavioral epochs. The mean activity of the units is on the top, while periodograms are on the bottom. All results are presented in two pairs of colors: yellow vs. magenta for discriminating the selected time window vs. all other times, respectively, and green vs. red for defining *Behavior YES* vs. *Behavior NO*, respectively. This color pattern is inherited by child apps, described below.

### 3.1.1 Key-logging behavioral states
The unwinding module is used for custom time-stamped key logging of behavioral states. It is conceived to be used in parallel with the video recording of the experiment. 

During the unwinding, you can press the keys *1, 2, 3, 7, 8, 9* for each behavioral state.
You can use this module for defining epochs of active object exploration, stereotypies, grooming, social interaction, etc. For the moment it is limited to 6 behavioral states.

If you have ANY-maze, you can import both coordinates and logged information through the _ANY-maze_ button.

### 3.1.2 Exploring single keys
Once you have perfored your unwinding, you can see what happens to the periodogram, units, and some of the metrics described below, during these behavioral states. With the dropdown menu in the main GUI you can select wheter to join epochs corresponding to all keys, or to browse just a single key. You can consider the first N seconds for each epoch if you want to.
Last, you can shift the behavioral state vector on the left or right of N seconds. This latter option can be used to adjust delay in logging, or to investigate what just before or after a certain behavior. ***Fig. 3.1.2.a*** shows the GUI upon loading and selecting manually logged object exploration epochs.

![Fig. 3.1.2.a](https://i.imgur.com/2qP9W6E.png)

***Fig. 3.1.2.a***

## 3.2 Analysis of movement and sleep patterns
Bly clicking on the *Movement and sleep* button in the main GUI, a submodule for this analysis is launched.

### 3.2.1 Movement
Regarding movement, in the _Movement_ tab, an overview of the speed time series and cumulative distance is provided. The module inherits the epoch filtering from main GUI. It shows the speed/distance distribution during these epochs, alongside descriptive metrics, in the form of box charts. 

The speed time series and the cumulative distance can be segmented into movement states. They are:
1) Movement (i.e., everything that is not immobility)
2) Running (sustained high-velocity epochs)
3) Bursts (sustained high-accelleration epochs)

The movement state segmentation is set with two hard thresholds. With the first one the user can select the minimum cm/s (or cm/s^2 for bursts). The second is minimal duration in seconds.
***Movie 3.2.1.a*** shows an example of color coded speed and distance series.

![Movie 3.2.1.a](https://i.imgur.com/prtLXns.gif)

***Fig. 3.2.1.a***

In this section it is also possible to retrieve the arena borders (with some manual refinement) and calculate the tigmotaxis score (***Fig. 3.2.1.a***).

![Fig. 3.2.1.a](https://i.imgur.com/IjiM87j.png)

***Fig. 3.2.1.b***

In addition, metrics related to the head turn angles can be extracted as well both in the whole arena and in the center of it. These metrics might be employed to infer exploration of the open field and some stereotypies.

### 3.2.2 Sleep
In the *Sleep* tab there is a launcher (***Fig. 3.2.2.a***) for an edited version of [**AccuSleep**](https://github.com/zekebarger/AccuSleep).
AccuSleep is a popular MATLAB software to do automatic and manual segmentation into wakefulness and rapid eye movement (REM), and non REM (NREM) sleep, basing on EEG and electromyogram (EMG). It comes already installed in DOT-IOT Log Explorer.

![Fig. 3.2.2.a](https://i.imgur.com/WtHKDlu.png)

***Fig. 3.2.2.a***

If you don't have EMG you can use the TINT speed vector to aid yourself in manual sleep segmentation. DOT-IOT Log Explorer can automatically assign the movement epochs (as defined by the user in the *Movement* tab) to the wakefulness epochs in the AccuSleep GUI. 

Once the data is saved, this edited version generates a *Sleep* folder (in the same folder of the original *.txt* file) where the sleep labels are saved. Thereupon, these labels can be loaded by the main GUI for displaying them and to check, for example, the unit metrics during wakefulness, REM, NREM.

**Bonus: what if I want to do the automatic segmentation of sleep but I do not have the EMG?**
A recent paper by H. Osanai et al, [Extracting electromyographic signals from multi-channel LFPs using independent component analysis without direct muscular recording](https://www.cell.com/cell-reports-methods/fulltext/S2667-2375(23)00105-4), showed that it is possible to extract a simulated EMG from LFPs.

If you have the *.bin* recording you can use it for LFP extraction (see **LFP extraction** section), simulate the EMG through the *Sleep* module, and run the automated version of AccuSleep. Note that these functions require the Deep Learning toolbox (see **Requirements**) and have not been tested yet. 
The quality of the extracted EMG seems to be dependent on the placement of the electrodes. The shallower they are, the better it is for this purpose.

## 3.3 Comodulation module
The comodulation module (***Movie 3.3.a***) allows to calculate some metrics of phase-amplitude coupling (PAC) between the theta and gamma components of source signal.
Three currently available PAC metrics are:
1) Mean vector length (MVL)
2) Phase locking value (PLV)
3) Modulation index (MI)

![Movie 3.3.a](https://i.imgur.com/SYij7E7.gif)
***Movie 3.3.a***

Plus, corr-correlation between theta and gamma can also be calculated.

The PAC can be calculted in different ways, for example by taking into account filtering options like time window and behavior from the main GUI.

### 3.3.1 Comodulograms
In the comodulation module it is possible to plot comodulograms (left panel). The comodulogram shows in color code the PAC between different pairs of frequencies. Usually, lower frequency range encompassing theta are on the x axis, and higher frequency range encopassing gamma range is on the y axis.

Comodulograms might be used for having an overview of the PAC in the frequency domain. 
You can save them for each experiment and average them in the **Results** module (see below).

### 3.3.2 PAC-per-second
It is also possible to calculate the PAC for each second. The result can be visualized as a bar graph in the original main GUI, in the EEG axis, by selecting the appropriate radio buttons.
This calculation is endowed by a shuffling control, which can be used to generate a surrogate distribution and filter the data. 

On the right panels, you can see the bars showing the mean raw or filtered scores during time window and behavioral epochs, according to main GUI. 

## 3.4 Unit explorer module
The unit explorer module (still in development) is designed for the analysis of place cell activity. It is possible to plot the spike activity (***Fig. 3.4.a***) in space and compute the distribution of single unit activity.

# 4 Results averaging
The results module has the aim to get all the outputs from single experiments, average them, and perform some simple statistical test. 
To use it, you want to save data from single experiment first. 
## 4.1 Saving the analyses and organizing the _group_ folders
In the main GUI and in each child GUI, there is an orange *Save* button to export the output file, which will be stored in the *Results* folder. In the GUIs you can specify the mouse and test IDs.  
### 4.1.1 About the automatic filename generation
The mouse and test IDs are inserted in the output filename.
Also, the filename automatically retrieves information regarding channel used, time window/behavior epochs used, and type of analysis performed. If multiple analyses are performed on the same file, with the same time window/behavior filters, then the filename will include labels just before the format. For example:
_MOUSEID_TESTID_Channel_Behavior_**[Spg][Mov][Sleep][Pac][Units]**.mat_ will include inform abount periodograms, movement patterns, sleep states, comodulation, and single units.
### 4.1.2 Organizing _group_ folders
At the core of the results module there is the idea that the results obtained from main GUIs should be copied and organized in folders. Each folder may contain all the results sharing a particular experimental conditions (e.g., imagine to organize one folder with all animals treated with drug A, and a second folder with all animals treated with drug B. You can compare these two groups in a semi-automated manner). 
It is advided to do this by copying the files in the *Results* folder, keeping the original output in its place.
## 4.2 Using the Results module
### 4.2.1 Results module
For the moment, you can use to perform the following things:
1) Periodogram averaging and nonparametric tests on desired frequency bands.
2) Quantification of sleep phases (REM, NREM): how much they last and how much are they scattered.
3) Quantitative analysis of movement. Half a dozen of metrics are available at the moment: they include running, bursts, thigmotaxis, head turn, circular variance... 
4) Comodulation analysis.
5) Spike analysis.

# About this guide and credits
This guide has been written by Rocco Granata in March 28th, 2024 to provide a quick overview of what DOT-IOT Log Explorer can do.
In the future, it will be implemented with more details about how to use specific handles.
