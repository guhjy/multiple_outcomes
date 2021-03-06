# Overview

This repository contains all code required to reproduce analyses in: 
>http://www.citeulike.org/user/guhjy/article/14655055
>Mathur MB & VanderWeele TJ. XXX. 

Address any inquiries to mmathur [AT] stanford [DOT] edu.

# How to re-run the simulation study from scratch

Simulation scripts are parallelized and were run on a SLURM cluster. All files required to re-run the simulation study are in R code/For Sherlock:

- **functions.R** contains helper functions that can be run locally, as well as unit tests for each function. 

- **doParallel.R** runs a parallelized simulation study. Specifically, it runs sim.reps=10 simulation reps (each with B=2000 bootstrap iterates) on each computing node. Each scenario has 1000 total simulation reps that were spread across multiple sbatch files. For each simulation rep, doParallel.R generates an original dataset, resamples according to a user-specified algorithm, calls functions from functions.R to conduct analyses, and aggregates the joint test and null interval results. Output consists of two types of files: "Short" results files have one row per simulation rep (that aggregates over all B=2000 bootstraps), so should each have sim.reps=5 rows. "Long" results files have a row for every bootstrap rep.

- **gen_sbatch.R** automatically generates the sbatch files based on user-specified simulation parameters (from which gen_sbatch.R produces scen_params.csv, called later by doParallel.R). This script is specialized for our cluster and would likely need to be rewritten for other computing systems. 

- **stitch_on_sherlock.R** takes output files written by doParallel.R and stitches them into a single file, stitched.csv, that is used for analysis. 

- **analysis.R** uses stitched.csv and the scenario parameters, scen_params.csv, to create the plots and stats reported for the simulation study. 

Auxiliary files that are not necessary to re-run the simulation study: 

- **choose_scen_params.R** helps choose reasonable scenario parameters using theoretical calculations for the independent case. 

- **push_to_sherlock.txt** contains Unix commands to move local files to and from our cluster computing system. This file is highly specific to our file paths and cluster. 



# How to reproduce simulation analyses from our saved results

To reproduce the simulation analyses locally using our saved results without re-running the simulations themselves:

(SAVE THE SIMULATION RESULTS FOR PAPER IN THEIR OWN FOLDER)

These scripts produce both the extended simulation results in the Appendix and the short versions in the main text. 

# How to reproduce the applied example

Chronologically, the applied example can be reproduced as follows: 

1. Optionally, to start from the raw data, download the MIDUS data by XXX. Run the SAS script in Ying's data prep code/Extract dataset for MAYA.sas to produce the dataset flourish_new.sas7bdat. This SAS script reproduces inclusion/exclusion criteria from Chen et al. (2018) and randomly selects one sibling from each sibship. 

2. Run **data_prep_applied_example.R**, which does additional data cleaning to turn flourish_new.sas7bdat into the analysis-ready dataset in Prepped data/flourish_prepped.csv. Note that this script makes additional subject exclusions (e.g., removing subjects with incomplete data). This script calls the helper functions in **helper_applied_example.R**. 

3. Run **analyses_applied_example.R**, which uses the prepped dataset, flourish_prepped.csv, to produce all plots, stats, and tables reported in the paper. 


# How to reproduce the p-value scatterplot in the Appendix

Run the script in R code/Auxiliary code/Compare joint tests/plot_adjusted_pvalues.R.



