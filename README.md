# REM Architecture Analysis (Human, Mouse, and Rat)

This repository contains MATLAB live scripts used to generate manuscript figures and tables from preprocessed human, mouse, and rat sleep-cycle data of the paper: 

"A Data-Driven Measure of REM Sleep Propensity for Human and Rodent Sleep"
Naghmeh Akhavan, et al., 2026

## Repository layout

- `Tables_Figures_Codes/`: main figure and table scripts (`.mlx`)
- `DATA_files/combined_out/`: human combined input data files
- `DATA_files/Mouse light data/` and `DATA_files/Mouse dark data/`: mouse input data files (light and dark phases)
- `DATA_files/Rat light data/` and `DATA_files/Rat dark data/`: rat input data files (light and dark phases)
- `Human_DATA_codes/`: human preprocessing scripts (preparing human data table)
- `figures/`: exported figure images

## How to run the scripts

1. Open MATLAB.
2. Set the Current Folder to the repository root.
3. Open the script you want to run from `Tables_Figures_Codes/`.
4. At the top of the script, replace the hard-coded local paths (`C:\Users\...`) with paths inside this repository.
5. Run the live script.

Most scripts can use the following path block:

```matlab
repoRoot = pwd;

combinedFolder = fullfile(repoRoot, 'DATA_files', 'combined_out');
matR = fullfile(combinedFolder, 'Combined_BigResultTable.mat');
matD = fullfile(combinedFolder, 'Combined_BigStageDetailsTable.mat');

mouseLightCSV = fullfile(repoRoot, 'DATA_files', 'Mouse light data', 'BigResultTable_mouse_light.csv');
mouseDarkCSV  = fullfile(repoRoot, 'DATA_files', 'Mouse dark data',  'BigResultTable_mouse_dark.csv');
ratLightCSV   = fullfile(repoRoot, 'DATA_files', 'Rat light data',   'BigResultTable_rat_light.csv');
ratDarkCSV    = fullfile(repoRoot, 'DATA_files', 'Rat dark data',    'BigResultTable_rat_dark.csv');
```

## Script groups

- Human-only scripts use `DATA_files/combined_out/`.
- Rodent-only scripts use the CSV files in the mouse and rat folders under `DATA_files/`.
- All-species scripts use both the human files and the rodent CSV files.

## Notes

- The files in `Tables_Figures_Codes/` are MATLAB live scripts and can be run individually.
- Some scripts create output folders such as `tables_out` or `all_species_tables_out`.
- If a script points to a `.mat` file instead of a `.csv`, use the matching file from the same `DATA_files` subfolder.

## Author

Naghmeh Akhavan - March 2026

For questions, please refer to comments inside the MATLAB scripts or contact author Naghmeh Akhavan (`nakhavan@umich.edu`).
