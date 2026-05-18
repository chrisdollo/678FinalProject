# CMSC 678 Final Project: BCI Motor Imagery Classification

4-class EEG motor imagery classification on [BCI Competition IV Dataset 2a](https://www.bbci.de/competition/iv/). We benchmark FBCSP, EEGNet, ATCNet, MI-Mamba, and BYOL-pretrained EEGNet with and without Segmentation and Recombination (S&R) augmentation, with a focus on whether these approaches help BCI-inefficient users (low performers).

---

## Repository layout

| File | Purpose |
|------|---------|
| [driver.ipynb](driver.ipynb) | Main notebook to run everything from here |
| [utils.py](utils.py) | Data loading and preprocessing |
| [normal_results.py](normal_results.py) | Runs all model training conditions |
| [EEGNet.py](EEGNet.py) | EEGNet model |
| [ATCNet.py](ATCNet.py) | ATCNet model |
| [Mamba.py](Mamba.py) | MI-Mamba model |
| [FBCSP_Multiclass.py](FBCSP_Multiclass.py) | FBCSP pipeline |
| [FBCSP_V4.py](FBCSP_V4.py) | FBCSP variant |
| [EEGNetBYOL.py](EEGNetBYOL.py) | BYOL pretraining and fine-tuning |
| [ablation.py](ablation.py) | Leave-one-out BYOL ablation |
| [sr_augmentation.py](sr_augmentation.py) | S&R data augmentation |
| [plot.py](plot.py) | All plotting functions |
| [requirements.txt](requirements.txt) | Python dependencies |

---

## Getting the data

You need **27 files** in a single folder (`DATA_DIR`):

- **18 signal files** — `A01T.npz` – `A09T.npz` (training) and `A01E.npz` – `A09E.npz` (evaluation)
- **9 label files** — `A01E.mat` – `A09E.mat` (true evaluation labels)

### Download Through Shared Google Drive folder

The preprocessed data is available in a shared Drive folder:

**[Open shared data folder](https://drive.google.com/drive/folders/1n7tAowp8rQdGrV1lA_4O__Ex_fLi1AJz)**

Steps:
1. Open the link above and sign in with your Google account.
2. Click the dropdown arrow next to the folder name → **"Add shortcut to My Drive"** → choose a location you'll remember (e.g., `My Drive/bci_data`).
3. In `driver.ipynb` cell 2, update `DATA_DIR` to match where you placed the shortcut:
   ```python
   DATA_DIR = '/content/drive/MyDrive/bci_data'
   ```
4. You can skip the **Download data** cell entirely since the files are already there.

---

## How to run

> **Requires Google Colab**: a GPU is needed for experiments 2–10. A100 is recommended; T4 works but is slower.

1. Upload the entire repository folder to your Google Drive.
2. Open [driver.ipynb](driver.ipynb) in **Google Colab** (File → Open → Google Drive).
3. In **cell 2**, update the two path variables:
   ```python
   HOME_FOLDER = '/content/drive/MyDrive/path/to/678FinalProject'
   DATA_DIR = '/content/drive/MyDrive/bci_data'
   ```
4. Run all cells top to bottom (Runtime → Run all).

Results for each experiment are saved to `HOME_FOLDER/results/` as pickle files. If you stop and resume, re-running a cell loads from cache instead of retraining from scratch.

### Approximate runtimes on A100

| Experiments | Content | Time |
|-------------|---------|------|
| 1–7 | FBCSP + supervised models (with/without S&R) | 2–3 hours total |
| 8–9 | BYOL pretrain + fine-tune | 30–60 min each |
| 10 | Leave-one-out ablation (32 runs) | Several hours and is checkpointed/safe to interrupt |

---

## Installation

All experiments run in Google Colab, so most dependencies are pre-installed. For local use:

```bash
pip install -r requirements.txt
pip install mamba-ssm --no-build-isolation
```

Key dependencies: `torch`, `torchvision`, `numpy`, `scipy`, `scikit-learn`, `matplotlib`, `mne`, `braindecode`, `mamba-ssm`

---

## Results

Pre-computed results are included in [results/](results/) so you can run the plotting cells without retraining. Delete any `.pkl` file to force a fresh run for that experiment.

The ablation summary is in [results/ABLATION_RESULTS.txt](results/ABLATION_RESULTS.txt).
