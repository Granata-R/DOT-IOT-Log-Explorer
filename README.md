# <img src="https://i.imgur.com/NY7RYu4.png" width="16" height="28"> DOT-IOT Log Explorer

DOT-IOT Log Explorer is a Python/MATLAB software for broad analysis of electrophysiological data from freely behaving animals. It can be used for visualization and quantitative analysis of spectral properties of electroencephalogram (EEG) and local field potentials (LFPs), movement patterns, sleep patterns, single neuron activity, and phase-amplitude coupling, during specified time windows and behaviorally relevant epochs. 

DOT-IOT Log Explorer has been created by Rocco Granata, with inputs from Diletta Cavezza, as tailored software for the neuropsychopharmacology laboratory of Dr. Elvira De Leonibus. Originally designed for an interactive analysis of hippocampal EEG recordings during memory tests, such as the different object task (DOT) and the identical object task (IOT)<sup>[1]</sup>, its versatility allows for potential application in a range of behavioral studies.

![DOT IOT Log Explorer](https://i.imgur.com/2qP9W6E.png)

**Reference**

<sup>[1]</sup>Sannino, S., Russo, F., Torromino, G., Pendolino, V., Calabresi, P., & De Leonibus, E. (2012). Role of the dorsal hippocampus in object memory load. In Learning &amp; Memory (Vol. 19, Issue 5, pp. 211–218). Cold Spring Harbor Laboratory. https://doi.org/10.1101/lm.025213.111 

# See it in action!

<a href="https://youtu.be/QhcdSRSdmRA" target="_blank">
    <img src="https://github.com/Granata-R/DOT-IOT-Log-Explorer/assets/114356527/4fdda6db-30b1-42c6-ac94-771fc65fca51" alt="See an overview on YouTube." width="50%">
</a>

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

   - **Statistics and Machine Learning Toolbox** (might be required for certain actions);
   
   - **Deep Learning Toolbox** (might be required for certain actions).

- A **spike sorter** of interest (optional). Some sorters (e.g., spyKING CIRCUS; Tridesclous) are directly installed with SpikeInterface. For other sorters, please refer to this [link](https://spikeinterface.readthedocs.io/en/latest/install_sorters.html).
  
- **Python (Jupyter) + SpikeInterface**. Python Jupyter notebooks and [SpikeInterface](https://github.com/SpikeInterface/spikeinterface/tree/main) are necessary for spike and LFP processing. One quick way to install all of them is to follow the [SpikeInterface beginner guide for Python installation](https://github.com/SpikeInterface/spikeinterface/tree/main/installation_tips).

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

The behavioral epochs might be referred to animal speed, object exploration, interaction with a conspecific, stereotypies, sleep epochs, and more. Behavioral epochs are always binarized in terms of *Behavior YES* or *Behavior NO*, according to GUI parameters.

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

## 3.2 <img src="https://i.imgur.com/RTwNlGx.png" width="16" height="28"> Analysis of movement and sleep patterns
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

***Movie 3.2.1.a***

In this section it is also possible to retrieve the arena borders (with some manual refinement) and calculate the tigmotaxis score (***Fig. 3.2.1.a***).

![Fig. 3.2.1.a](https://i.imgur.com/IjiM87j.png)

***Fig. 3.2.1.b***

In addition, metrics related to the head turn angles can be extracted as well both in the whole arena and in the center of it. These metrics might be employed to infer exploration of the open field and some stereotypies.

### 3.2.2 Sleep
In the *Sleep* tab there is a launcher (***Fig. 3.2.2.a***) for an edited version of [**AccuSleep**](https://github.com/zekebarger/AccuSleep)<sup>[1]</sup>.
AccuSleep is a popular MATLAB software to do automated and manual segmentation into wakefulness and rapid eye movement (REM), and non REM (NREM) sleep, basing on EEG and electromyogram (EMG). It comes already installed in DOT-IOT Log Explorer.

![Fig. 3.2.2.a](https://i.imgur.com/WtHKDlu.png)

***Fig. 3.2.2.a***

In our workflow, the AccuSleep viewer is used for manual sleep classification not requiring EMG. Mobility epochs, as defined in the **Movement** tab, can be automatically classified as wakefulness. 

The sleep labels can be then loaded on the main GUI and used to analyze what happens during wakefulness, REM, NREM.

**What if I don't have the EMG?**
A recent paper by H. Osanai et al<sup>[2]</sup> showed that it is possible to extract a simulated EMG from LFPs.
If you don't have EMG and are interested in automatic scoring, you can extract the LFPs (see **LFP extraction** section) and simulate the EMG through the *Sleep* module.
These functions need the Deep Learning toolbox (refer to **Requirements**) and haven't been tested yet. EMG quality appears to rely on electrode placement; shallower placement yields better results.

**References**

<sup>[1]</sup>Barger, Z., Frye, C. G., Liu, D., Dan, Y., & Bouchard, K. E. (2019). Robust, automated sleep scoring by a compact neural network with distributional shift correction. In G. F. Gilestro (Ed.), PLOS ONE (Vol. 14, Issue 12, p. e0224642). Public Library of Science (PLoS). https://doi.org/10.1371/journal.pone.0224642

<sup>[2]</sup>Osanai, H., Yamamoto, J., & Kitamura, T. (2023). Extracting electromyographic signals from multi-channel LFPs using independent component analysis without direct muscular recording. In Cell Reports Methods (Vol. 3, Issue 6, p. 100482). Elsevier BV. https://doi.org/10.1016/j.crmeth.2023.100482

## 3.3 <img src="https://i.imgur.com/1DBaLyw.png" width="16" height="28"> Comodulation module
The comodulation module (***Movie 3.3.a***) allows to calculate some metrics of phase-amplitude coupling (PAC) between the theta and gamma components of source signal.
Three currently available PAC metrics are mean vector length (MVL), phase locking value (PLV), and modulation index (MI).
For more information on these methods check the reference<sup>[1]</sup>.

![Movie 3.3.a](https://i.imgur.com/SYij7E7.gif)
***Movie 3.3.a***

Plus, cross-correlation between theta and gamma can also be calculated.

The PAC can be calculted in different ways, for example by taking into account filtering options like time window and behavior from the main GUI.

**Reference**

<sup>[1]</sup>Hülsemann, M. J., Naumann, E., & Rasch, B. (2019). Quantification of Phase-Amplitude Coupling in Neuronal Oscillations: Comparison of Phase-Locking Value, Mean Vector Length, Modulation Index, and Generalized-Linear-Modeling-Cross-Frequency-Coupling. In Frontiers in Neuroscience (Vol. 13). Frontiers Media SA. https://doi.org/10.3389/fnins.2019.00573 
   
### 3.3.1 Comodulograms
In the comodulation module it is possible to plot comodulograms (left panel). The comodulogram shows in color code the PAC between different pairs of frequencies. Usually, lower frequency range encompassing theta are on the x axis, and higher frequency range encopassing gamma range is on the y axis.

Comodulograms might be used for having an overview of the PAC in the frequency domain. 
You can save them for each experiment and average them in the **Results** module (see below).

### 3.3.2 PAC-per-second
It is also possible to calculate the PAC for each second. The result can be visualized as a bar graph in the original main GUI, in the EEG axis, by selecting the appropriate radio buttons.
This calculation is endowed by a shuffling control, which can be used to generate a surrogate distribution and filter the data. 

On the right panels, you can see the bars showing the mean raw or filtered scores during time window and behavioral epochs, according to main GUI. 

## 3.4 <img src="https://i.imgur.com/yKGFfgk.png" width="16" height="28"> Unit explorer module
The unit explorer module, still in development, can be used to assess place cell activity and spike distribution during a specified time window and behavior for each unit. The place cell analysis leverages algorithms published in Grieves et al., 2023[1]. Firing rate maps can be obtained by dividing the absolute spike map by the dwell map, which indicates how much time the animal spent in each location.

<img src="https://i.imgur.com/69Qt5ay.png" alt="Fig. 3.4.a">

***Fig. 3.4.a***

**Reference**

<sup>[1]</sup>Grieves, R. M. (2023). Estimating neuronal firing density: A quantitative analysis of firing rate map algorithms. In D. Bush (Ed.), PLOS Computational Biology (Vol. 19, Issue 12, p. e1011763). Public Library of Science (PLoS). https://doi.org/10.1371/journal.pcbi.1011763

# 4 Results averaging
The results module has the aim to get all the outputs from single experiments, average them, and perform some simple statistical test. 
To use it, you want to save data from single experiment first. 
## 4.1 Saving the analyses and organizing the _group_ folders
In the main GUI and in each child GUI, there is an orange *Save* button to export the output file, which will be stored in the *Results* folder. In the GUIs you can specify the mouse and test IDs.  
### 4.1.1 About the automated filename generation
The mouse and test IDs are inserted in the output filename.
Also, the filename automatedally retrieves information regarding channel used, time window/behavior epochs used, and type of analysis performed. If multiple analyses are performed on the same file, with the same time window/behavior filters, then the filename will include labels just before the format. For example:
_MOUSEID_TESTID_Channel_Behavior_**[Spg][Mov][Sleep][Pac][Units]**.mat_ will include inform abount periodograms, movement patterns, sleep states, comodulation, and single units.
### 4.1.2 Organizing _group_ folders
At the core of the results module there is the idea that the results obtained from main GUIs should be copied and organized in folders. Each folder may contain all the results sharing a particular experimental conditions (e.g., imagine to organize one folder with all animals treated with drug A, and a second folder with all animals treated with drug B. You can compare these two groups in a semi-automated manner). 
It is advided to do this by copying the files in the *Results* folder, keeping the original output in its place.
## 4.2 Using the Results module
### 4.2.1 <img src="https://i.imgur.com/tjlWoV3.png" width="16" height="28"> Results module
For the moment, you can use to perform the following things:
1) Periodogram averaging and nonparametric tests on desired frequency bands.
2) Quantification of sleep phases (REM, NREM): how much they last and how much are they scattered.
3) Quantitative analysis of movement. Half a dozen of metrics are available at the moment: they include running, bursts, thigmotaxis, head turn, circular variance... 
4) Comodulation analysis.
5) Spike analysis.

## 5 About this guide and credits
This short guide has been written by Rocco Granata on March 28th, 2024, with the idea of providing an overview of the current capabilities of DOT-IOT Log Explorer. As of now, the code has not been made available. For further details you can send me an [e-mail](mailto:rocc.granata@gmail.com).
