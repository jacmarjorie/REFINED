# REFINED
This repository is made to share the REFINED (Representation of Features as Images with NEighborhood Dependencies). To use the REFINED to create images corresponding to your dataset you neet to use the following codes step by step. REFINED contains two main cores, initial MDS and hill climbing, where both are eucliadean distance based. First the initiall coordinates will be created by MDS initializer part, then the coordinates will be updated by the hill climbing. Multiple functions are being used in the process, hence a Toolpox.py is provided which includes all the function needed. 
**We added small subset of processed NCI60 data in the data folder for the users to be able to use the code. Note that due to the size of the dataset so the performance is much lower than the paper.**
## 1) Toolbox
The main functions provided in the toolbox are performing below tasks:
### a) two_d_eq:
The MDS function reduces dimensionality by preserving the distance in the new space as close as possible to the initial space. Therefore, when it is being used for generating coordinates in 2-D space for features, the coordinates will have arbitrary values. Hence, the first step to create images is bringing down the arbitrary values in the range [0,N], where N is the square root of the number features. This process is handled by **two_d_eq**.
### b) Assign_features_to_pixels:
As the provided coordinates generated by MDS extremely overlap, the generated image will be extremely sparse. Therefore we locate the overlapped features in a closest empty pixel.

## 2) Initial MDS
The Initial_MDS.py provides the none-overlaping coordinates generated by post MDS processing, plus euclidean distance matrix and feature lists (descriptor names) as input of the hill climbing. Note that the provided coordinates also can be used to generate image to train a CNN. The initial MDS uses some of the toolbox functions integrated with the following steps:

### a) Loading the data
This section loads the input data, no anottation is needed (unsupervised). The **"normalized_padel_feats_NCI60_672_small.csv"** is provided to test the code.

### b) MDS
This part of the code applies MDS and its post processing steps for transposed dimensionality reduction and creating inital image coordinates.

### c) Saving hill climbing inputs
As the hill climbing algorithms works based on minimizing the Euclidean distance, beside the the MDS image coordinates, a euclidean distance of features as matrice need to saved as inputs of the hill climbing. Therefore all the hill climbing required inputs are saved as a pickle.

## 3) Hill climbing
The hill climbing section of REFINED is written based on using Message Passing Interface (MPI) of python to use HPCC resource very efficiently (Thanks Ruibo!). To run this code make sure to install **mpi4py** library of Python. The hill climbing algorithm section was written based master-slave control process where the first processor is the master and other processors will slave. The master processor distribute, scatter and receive data from slave processors. Slave processors do the computational task. Some computational functions needed to run Hill climbing code should be imported from **paraHill.py**
To run the hill climbing algorithm one need to use the **mpiHill_Hardcoded.py**. 

### a) Input
Input of hill climbing is the initial MDS output saved as pickle file. It includes three parameter ** 1- gene_names: feature names, 2- dist_matr: Euclidean distance matrix of features in initial space, 3- init_map: feature's coordinate created by initial MDS.

### b) Parameters
The only parameter that user of this algorithm need to choose is number of iterations. Number of iterations is basically how many times the hill climbing goes over the entire features and check each feature exchange cost. Defaults is NI = 5.

### c) Output
The feature names, REFINED coordinates, and intial map will be save as the output of hill climbing in a pickle file. REFINED are coordinates are saved in **coords**.

## Cite
If you use REFINED for research, plase cite the following paper:

REFINED (REpresentation of Features as Images with NEighborhood Dependencies): A novel feature representation for Convolutional Neural Networks
O Bazgir, R Zhang, SR Dhruba, R Rahman, S Ghosh, R Pal
arXiv preprint arXiv:1912.05687

