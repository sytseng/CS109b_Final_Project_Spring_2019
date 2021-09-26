# CS109b Final Project: Microbiome Dynamics

### Introduction
- This is a final project for the Spring 2019 course in [Data Science II](https://harvard-iacs.github.io/2019-CS109B) (CS109B) at [Harvard John A. Paulson School of Engineering and Applied Sciences](https://iacs.seas.harvard.edu).
- Group 12 members are Shih-Yi Tseng (FAS student), Eunice Yeh (FAS student), Xihan Zhang (FAS student), and Michel Atoudem (DCE student). 
- The goal of our project is to apply the data science techniques and tools that we have learned in this course on the following topic: *microbiome dynamics*.
  + Motivation: The microbiome has been an increasingly hot topic over the years, especially in public health research. One of the key challenges to understanding the microbiome is in how dynamic the microbial abundances can be even within an individual over a short timespan, which imposes several practical issues when conducting statistical analysis and inference of these data.
- Time-series gut microbiome data were graciously provided by Dr. Travis Gibson and Dr. Georg Gerber from the [Massachusetts Host-Microbiome Center](https://metagenomics.partners.org).
- All of our code is provided across multiple Jupyter notebooks, which have been organized into directories described in the next section.

### Directory Structure and Contents
Within each of the following directories, there will be a subdirectory called `/supplementary` containing Jupyter notebooks currently in development that investigate alternatives or extensions to our final notebooks. Do not expect these notebooks to be well-documented as they are not part of our final analysis as presented in the upper-level directory (you can expect those to be well-documented), but we still wanted a way to share with you some of the work that we are further diving into - any feedback would be greatly appreciated!

1. __EDA__ (Exploratory Data Analysis)
  - `EDA_milestone2.ipynb` contains the exploratory data analysis previously done for the project proposal (see milestone #2 under `/Reports`). 
  - `EDA_Qiime.ipynb` contains additional data analysis performed to answer some exploratory questions we raised in the project proposal and to guide our data processing decisions in the subsequent steps of our project. Spoilers: there are some quite interesting scientific results in this notebook, as discussed briefly with Dr. Gerber! Note that this notebook contains images that are provided under the subdirectory `\images`.

2. __Clustering__ - The purpose of clustering is to reduce the high dimensionality of multi'omics data. In our case, each of our microbiome datasets contains about 200 microbes over 73-75 time points for each of the 3-4 samples. We also want to be able to take into account the biological and functional similarities and differences among the 200+ microbes over time when modeling their dynamics.

  - `phylo_clustering.ipynb` explores a biological way to clustering the microbes based on their sequences via the [`Bio.Phylo`](https://biopython.org/wiki/Phylo) module. Similar to the k-means clustering algorithm we learned in class, we used the k-medoids (or PAM) algorithm here.
  - `temporal_clustering.ipynb` considers the temporal relationships among the microbes by clustering them based on the correlations of their abundance trajectories over time. The k-medoids (or PAM) algorithm was also used here.

3. __Data Augmentation__ - As mentioned in clustering, our datasets contain too many covariates for too little samples. This would cause our machine learning models to overfit. So to complement clustering, the purpose of data augmentation is to increase our sample size based on the current data at hand (since we do not own a time machine to go back in time and increase the number of mice during the experiment; time and cost are the two common practical limitations to having sufficient sample size).

  - `Bootstrap_augmentation.ipynb` uses the clustering assignments created from the clustering notebooks to bootstrap about 30 more samples (for each dataset) of microbial abundances within each cluster in order to simulate the microbial abundances as carefully as possible without losing their inherent biological or temporal relationships with one another.
  - `DTWDBA_augmentation.ipynb` considers a time-series classification technique called Dynamic Time Warping (DTW) with Barycentric Averaging (DBA) to generate more samples in each of our two datasets.
  
4. __Modeling__ - Since we learned both machine learning and Bayesian modeling in this course, we decided to attempt both types in this project for modeling the microbial dynamics over time. Exciting!

  - `RNN_models.ipynb` trains and tests two different Recurrent Neural Network (RNN) models:
    + One with Long Short Term Memory (LSTM) units that take the time series of microbial abundances (or clustered traces) as input to predict the microbial abundances in the next time point for every given time point.
    + One with a Sequential LSTM Variational Autoencoder (VAE) that learns both the microbial dynamics and the representation of health states.
     
  - `PyJAGS_simplified.ipynb` implements a simplified version of Drs. Gibson and Gerber's full Bayesian non-parametric model of microbiome dynamics (as presented in their [paper](https://arxiv.org/abs/1805.04591)) using PyJAGS as we learned in the course. We also modified the simplified model to incorporate additional features that our datasets contain, such as perturbations.

5. __Reports__
  - `Milestone2_Proposal-EDA.pdf` is the project proposal and EDA submitted to achieve milestone #2 of this project.
  - `Milestone3_Final-Report.pdf` is the final written report completed by Michel as part of the milestone #3 requirement for the DCE students.


Our project is summarized in a 6-minute presentation [here](https://docs.google.com/presentation/d/1vdqMISQLsjKAlXUCjdU0ueCAjJRDKy0dXQw5otXvcqk/edit?usp=sharing).
