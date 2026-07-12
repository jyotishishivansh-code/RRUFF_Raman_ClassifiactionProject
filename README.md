# Raman Spectral Classification — RRUFF Mineral Dataset

Machine learning pipeline for classifying minerals from raw Raman spectra. Built from scratch on the publicly available RRUFF database — from raw `.txt` spectral files to a compiled dataset to trained classifiers.

**Status:** Work in progress — dataset pipeline complete, EDA and model training in progress.

---

## What this does

Raw Raman spectra from the RRUFF database come as individual `.txt` files, one spectrum per file, with inconsistent wavenumber axes and mixed metadata/data formatting. This project:

1. Parses raw RRUFF `.txt` files, handling mixed headers and non-numeric metadata lines
2. Resamples every spectrum onto a common wavenumber axis (100–1600 cm⁻¹, 2 cm⁻¹ step) using linear interpolation
3. Compiles a structured dataset: one row per spectrum, one column per wavenumber, with mineral label
4. Performs exploratory data analysis — class distribution, raw spectra visualisation, PCA
5. Trains and compares classical ML classifiers: Logistic Regression and SVM
6. Evaluates models using confusion matrices, precision, recall, and F1-score

---

## Dataset

**Source:** [RRUFF Project](https://rruff.info/) — open-access Raman spectra of minerals  
**Minerals included:** Actinolite, Adamite, Aegirine, Aenigmatite, Akermanite, Albite  
**Compiled dataset shape:** 384 spectra × 816 wavenumber features + 1 label column  
**Common wavenumber axis:** 100–1600 cm⁻¹ at 2 cm⁻¹ resolution

The raw RRUFF `.txt` files are not included in this repository. Download them directly from [rruff.info](https://rruff.info/) and place them in a local folder. Update the `folderpath` variable in the notebook accordingly.

---

## Pipeline

### 1. File parsing — `parse_rruff_file(filepath)`
Reads a single RRUFF `.txt` file. Skips all `##` header lines, blank lines, and non-numeric metadata lines (e.g. orientation descriptions, measurement notes). Returns wavenumber and intensity arrays as float64.

### 2. Interpolation — `interpolate_spectrum(wn, intensity, common_wn)`
Uses `scipy.interpolate.interp1d` with linear interpolation to resample a spectrum onto the common wavenumber axis. Points outside the measured range are filled with zero.

### 3. Dataset builder — `datasetbuilder(folderpath, minerals_to_include, common_wn)`
Loops over all `.txt` files in the specified folder. Filters to selected mineral classes. Calls parser and interpolator for each file. Returns a `pandas` DataFrame with wavenumber columns and a `label` column.

### 4. EDA *(in progress)*
- Class distribution and sample counts per mineral
- Raw spectra visualisation — overlaid spectra per mineral class
- Mean spectrum per class — characteristic peak comparison
- Intensity scale analysis — boxplot by class
- PCA scatter plot — 2D class separability visualisation
- Correlation structure of wavenumber channels

### 5. Model training *(upcoming)*
- StandardScaler normalisation
- Stratified train/test split (80/20)
- Logistic Regression and SVM training
- Confusion matrix and classification report for both
- Comparison table: accuracy, precision, recall, F1

---

## Requirements

```
python >= 3.8
pandas
numpy
matplotlib
seaborn
scipy
scikit-learn
```

Install:
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn
```

---

## Usage

1. Download RRUFF Raman spectra `.txt` files from [rruff.info](https://rruff.info/)
2. Place all files in a single local folder
3. Open `rruff_trial_project.ipynb` in Jupyter Notebook
4. Update `folderpath` in the dataset builder cell to point to your local folder
5. Run cells sequentially

---

## Project structure

```
RRUFF_Raman_ClassificationProject/
├── rruff_trial_project.ipynb    # Main notebook
└── README.md
```

Raw data files are not tracked in this repository.

---

## Relevance to research

This project is part of a broader effort to develop ML-assisted spectral classification pipelines for analytical spectroscopy applications, including LIBS and Raman-based material characterisation. Methods developed here — dataset compilation from raw spectral files, interpolation onto common axes, multi-class classification with chemometrics and deep learning baselines — are being extended toward publication-level work on ML-assisted LIBS analysis under Prof. G. Manoj Kumar at DIA-CoE, University of Hyderabad.
