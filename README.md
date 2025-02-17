# Accelerating Natural Product Discovery with Linked MS-Genomics and Language/Transformer-Based Models
![alt text](https://github.com/SIBERanalytics/MS2-to-Structure/blob/main/overview_figure.png?raw=true)

## Overview

This repository contains the source data and python scripts for the MS/MS-to-structure processing portion of the Workflow for Intelligent Structural Elucidation (WISE). Raw LC-MS/MS data is obtained from MassIVE with the accession number MSV000092237 (https://doi.org/doi:10.25345/C53X83W53). If you are planning to use the dataset, please cite the paper Tay, D. W. P.;  Tan, L. L.;  Heng, E.;  Zulkarnain, N.;  Chin, E. J.;  Tan, Z. Y. Q.;  Leong, C. Y.;  Ng, V. W. P.;  Yang, L. K.;  Seow, D. C. S.;  Kanagasundaram, Y.;  Ng, S. B.;  Lim, Y. H.; Wong, F. T., Tandem mass spectral metabolic profiling of 54 actinobacterial strains and their 459 mutants. _Sci. Data_ **2024**, _11_ (1), 977. DOI: 10.1038/s41597-024-03833-9

The generated results and figures are presented in the manuscript titled "**Accelerating Natural Product Discovery with Linked MS-Genomics and Language/Transformer-Based Models**" https://www.biorxiv.org/content/10.1101/2025.01.20.633932v1

## Environment
The python packages in the conda environment for performing MS/MS-to-structure with WISE and recreating the heatmap figures are provided the yaml file (`environment.yml`).

## Directory Structure
- `notebooks/`: Contains Jupyter Notebooks with code to run both the MS/MS-to-structure and heatmap construction.
- `heatmaps/`: Contains sample heatmaps.
- `data/`: Contains both the input data and sample processed data after running MS/MS-to-structure as well as the reference MS/MS spectral library.
  + `input/`: data extracted from LC-MS/MS using Mestrenova.
  + `output/`: sample output data after running MS/MS-to-structure, also the input data for heatmap construction.
 
## Instructions
1. Setup environment (`environment.yml`).
```
conda env create -f environment.yml
conda activate wise   
```
2. Go to `notebooks/`.
3. Run the code under the notebook (`WISE_msms_to_structure.ipynb`).
  + You may select Valinomycin, Surfactin B, or Neomycin B, and declare it under the `molecule` variable.
  + Ensure that the input_file has the correct path to the desired input file in `data/input/`.
  + Ensure that reference library (`lib`) has the correct path to the sample reference library in `data/MSMS_reference_library.parquet`.
4. Run the code under the notebook (`plot_heatmaps.ipynb`) using the output from Step 3 OR use the sample data from `data/output/`.

## Using MS/MS-to-Structure On Your Own Data
In order to use the WISE MS/MS-to-Structure code on your own MS/MS data you will need to prepare the following:


- `MS/MS Input Data`: following the data structure of the sample input data in `data/input/` you will need to fill in the various fields with your own data to be queried. Only the `Precursor m/z` and `Expt MS2 Spectra` fields are mandatory. However, should you decide to not fill in other fields, some adjustments to the code in `WISE_msms_to_structure.ipynb` is necessary. Please note that `Expt MS2 Spectra` must be provided as a list of lists [[m/z, intensity], [m/z, intensity]...] with intensities binned to the nearest integer m/z.
  
  
- `Reference MS/MS Library`: you will need to replace `MSMS_reference_library.parquet` with your own reference library that follows the same data format. SMILES as string, PrecursorMZ as float, Mass_Spec_Rounded as list of lists [[m/z, intensity], [m/z, intensity]...] with intensities binned to the nearest interger m/z, Isotope_Prob as a list (you may calculate this by running the code in `notebooks/isotope_calculator.ipynb`). You may either use a free database (e.g. GNPS, MoNA), commercial database (e.g. NIST), or generate your own (e.g. using ICEBERG from https://github.com/samgoldman97/ms-pred).

## Notes
The input data was derived from the raw LC-MS/MS data using scripts executed in Mestrenova v15.0.1-35756 which is commercially available from Mestrelab Research S. L. (www.mestrelab.com).

Only a subset of the reference library is available here to recreate the results in the manuscript. The code for reference MS/MS generation as well as the pre-trained model is available from Github at https://github.com/samgoldman97/ms-pred. If you are planning to use the model, please cite the paper Goldman, S.;  Li, J.; Coley, C. W., Generating Molecular Fragmentation Graphs with Autoregressive Neural Networks. _Anal. Chem._ **2024**, _96_ (8), 3419-3428. DOI: 10.1021/acs.analchem.3c04654
 
