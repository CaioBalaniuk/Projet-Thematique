# EEG and AI Thematic Project — Preprocessing

EEG signal preprocessing notebook for the EEG and Artificial Intelligence thematic project.

## Contents

The notebook `Pré_processamento_Projet_Themátique_EEG_et_IA.ipynb` covers three main stages:

1. **Signal Exploration:** loading the 500 EEG signals, organizing datasets A–E, creating the MNE data structure, visualizing signals in the time domain, and performing spectral amplitude analysis.
2. **Filtering and Data Splitting:** applying a 40 Hz low-pass Butterworth filter (4th order, zero-phase filtering using `filtfilt`), validating the filtering process, and splitting the data into training and test sets using stratified cross-validation.
3. **Windowing and Normalization:** creating signal windows, applying normalization while avoiding data leakage, preparing PyTorch `Dataset` and `DataLoader` objects, and aggregating predictions at the signal level.

The dataset is split by **entire EEG signals before windowing**, ensuring that windows from the same signal cannot appear in different subsets.

## Required Data

The raw data is not included in this repository. The notebook expects the Bonn EEG dataset `.txt` files to be organized in the following Google Drive structure:

```text
MyDrive/
└── Projet Thématique/
    ├── raw/
    │   ├── A (Z)/
    │   ├── B (O)/
    │   ├── C (N)/
    │   ├── D (F)/
    │   └── E (S)/
    ├── processado/
    └── figuras/
```

Each of the five folders, from `A (Z)` to `E (S)`, must contain 100 `.txt` files.

> **Important:** The notebook uses the following path: `/content/drive/MyDrive/Projet Thématique/raw`. If the folder name or location in your Google Drive is different, update the `path` variable in the first stage.

## How to Run in Google Colab

1. Open the notebook in Google Colab.
2. Place the `Projet Thématique` folder inside your Google Drive's `MyDrive` directory, following the structure described above.
3. Run the notebook cells in order. The first stage mounts Google Drive.
4. If necessary, install the dependencies:

   ```python
   %pip install -r requirements.txt
   ```

   If the `requirements.txt` file is not accessible in the Colab environment, install the libraries directly:

   ```python
   %pip install numpy pandas matplotlib scipy scikit-learn mne torch
   ```

5. To save the processed results, set `SALVAR = True` in the parameter cell of the third stage. When `SALVAR = False`, the notebook does not save the final output file from the third stage.

The notebook was developed and tested with Python 3.13.18 in Google Colab. Colab already provides the `google.colab` integration used to mount Google Drive.

## Generated Files

When the corresponding cells are executed, the notebook generates:

- `figuras/`: plots used to analyze and document the preprocessing process.
- `processado/etapa2_filtrado_split.npz`: filtered signals, labels, identifiers, training/test indices, and cross-validation folds.
- `processado/etapa3_janelas.npz`: normalized windows, labels, signal identifiers, and preprocessing parameters (when `SALVAR = True`).

These files are generated outputs and do not need to be committed to GitHub if they are large or can be reproduced from the raw data.

## Dependencies

The Python libraries directly used in the notebook are listed in [`requirements.txt`](requirements.txt).

- **NumPy and Pandas:** data manipulation.
- **Matplotlib:** data visualization.
- **SciPy:** signal filtering and spectral analysis.
- **scikit-learn:** stratified splitting and cross-validation.
- **MNE:** EEG signal structuring and analysis.
- **PyTorch:** `Dataset`, `DataLoader`, and neural network input testing.