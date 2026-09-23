# Raw Data

This directory contains the original survey datasets used in the analysis.

## Source

The data was obtained from Open Sourcing Mental Health (OSMH):

https://osmhhelp.org/research.html

OSMH provides historical Mental Health in Tech survey datasets for research and analysis.

## Survey Years

This project uses survey data from:

- 2014
- 2016
- 2017
- 2018
- 2019
- 2020
- 2021
- 2022
- 2023

The survey instruments and available questions changed across years. The ETL process harmonizes selected fields into a common analytical schema.

## Files

Place the following source files in this directory:

- `survey.csv`
- `mental-heath-in-tech-2016_20161114.csv`
- `OSMI Mental Health in Tech Survey 2017.csv`
- `OSMI Mental Health in Tech Survey 2018.csv`
- `OSMI 2019 Mental Health in Tech Survey Results - OSMI Mental Health in Tech Survey 2019.csv`
- `OSMI 2020 Mental Health in Tech Survey Results .csv`
- `OSMI 2021 Mental Health in Tech Survey Results .csv`
- `responses_2022.csv`
- `responses_2023.csv`

## Reproducing the Data Pipeline

1. Download the required survey datasets from the OSMH research page.
2. Place the files listed above in `data/raw/`.
3. Run `notebooks/1. OSMH ETL.ipynb`.
4. The notebook generates `data/processed/cleaned_data.csv`.

Raw survey files are excluded from this repository through `.gitignore`. They must be obtained separately from OSMH.