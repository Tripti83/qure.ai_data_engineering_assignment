# qure.ai_data_engineering_assignment
## LIDC DICOM Data Ingestion, Validation, Metadata Extraction, and Database Loading

This project demonstrates a complete pipeline for handling a small subset of the LIDC-IDRI DICOM dataset. The workflow is implemented in the notebook `question1.ipynb` and covers the following stages:

## Table of Contents
- [Overview](#overview)
- [Setup & Requirements](#setup--requirements)
- [Pipeline Steps](#pipeline-steps)
  - [1. Data Ingestion](#1-data-ingestion)
  - [2. Extraction](#2-extraction)
  - [3. DICOM Validation](#3-dicom-validation)
  - [4. Metadata Extraction](#4-metadata-extraction)
  - [5. Data Organization](#5-data-organization)
  - [6. Visualization](#6-visualization)
  - [7. Database Loading](#7-database-loading)
- [Outputs](#outputs)
- [File Structure](#file-structure)
- [Notes](#notes)

## Overview
This notebook automates the process of downloading, extracting, validating, and organizing DICOM files, then extracting metadata and loading it into a SQLite database. It also provides visualizations and summary statistics for the dataset.

## Setup & Requirements
- Python 3.7+
- Required packages: `boto3`, `botocore`, `tqdm`, `pydicom`, `requests`, `matplotlib`, `seaborn`, `pandas`, `sqlite3`
- The notebook will automatically create necessary directories (`data/`, `logs/`)
- Internet connection required for downloading the dataset

## Pipeline Steps

### 1. Data Ingestion
- Downloads a ZIP file containing DICOM images from a presigned S3 URL.
- Validates the download and logs the process.

### 2. Extraction
- Extracts the ZIP file into the `data/lidc_extracted/` directory.
- Lists all extracted files for further processing.

### 3. DICOM Validation
- Iterates through all `.dcm` files.
- Validates each file using `pydicom`.
- Logs missing or corrupted files.
- Generates a CSV validation report (`logs/dicom_validation_report.csv`).

### 4. Metadata Extraction
- Parses metadata from each valid DICOM file:
  - Patient ID
  - Study Instance UID
  - Series Instance UID
  - SOP Instance UID
  - Slice Thickness
  - Pixel Spacing
  - Study Date
  - Acquisition Date
  - Instance Number
- Organizes files into a structured directory: `<PatientID>/<StudyUID>/<SeriesUID>/`
- Records metadata in a DataFrame and saves to `data/metadata_extracted.csv`.

### 5. Data Organization
- Copies DICOM files into organized folders to avoid duplicates.
- Counts slices per series and adds this info to the metadata.

### 6. Visualization
- Plots distribution of slice thickness using `matplotlib` and `seaborn`.
- Plots number of slices per series as a horizontal bar chart.
- Provides summary statistics (total studies, total slices, average slices per study).

### 7. Database Loading
- Creates a SQLite database (`data/mydatabase.db`).
- Defines a table `dicom_metadata` with appropriate columns.
- Loads metadata from CSV into the database.
- Validates table structure and column names.
- Provides queries to inspect the database (record counts, unique patients, null values).

## Outputs
- `data/lidc_small_dset.zip`: Downloaded dataset
- `data/lidc_extracted/`: Extracted DICOM files
- `logs/validation.log`: Log file for download and validation
- `logs/dicom_validation_report.csv`: DICOM validation report
- `data/metadata_extracted.csv`: Extracted metadata
- `data/mydatabase.db`: SQLite database with metadata

## File Structure
```
ques/
  question1.ipynb
  README.md
  data/
    lidc_small_dset.zip
    lidc_extracted/
      ...
    metadata_extracted.csv
    mydatabase.db
  logs/
    validation.log
    dicom_validation_report.csv
```

## Notes
- The notebook is modular; each step can be run independently.
- Logging is enabled for troubleshooting.
- The pipeline is robust to missing/corrupted files.
- The database schema matches the metadata fields extracted from DICOM files.
- Visualizations help understand the distribution of key features in the dataset.

---

