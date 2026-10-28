# SNR-Stratified and Interpretable Deep Learning for Automatic Modulation Classification

A third-year ECE deep-learning project for **Automatic Modulation Classification (AMC)** using raw I/Q samples from RadioML 2016.10a.

## Project overview

This project compares two models under the same data split and evaluation protocol:

- **Baseline:** VT-CNN2-style 1D CNN.
- **Proposed model:** 1D CNN + lightweight self-attention.
- **Interpretability:** 1D Grad-CAM on temporal I/Q samples.
- **Evaluation:** per-SNR accuracy from -20 dB to +18 dB in 2 dB steps, aggregate accuracy, low-SNR accuracy (SNR < 0 dB), high-SNR accuracy (SNR >= 0 dB), confusion matrices, and CNN-vs-attention comparison.
- **Confusion-pair focus:** 16-QAM vs 64-QAM and QPSK vs 8PSK.

The project contribution is the controlled combination of SNR-stratified evaluation, low-SNR failure analysis, lightweight attention, and 1D Grad-CAM. It does **not** claim that attention-based AMC is a new invention.

## Repository structure

```text
AMC_GitHub_Ready/
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
├── data/
│   └── README.md
└── notebooks/
    └── AMC_Code.ipynb
```

## 1. Get the dataset

The notebook expects the **RadioML 2016.10a** dataset in Python pickle/dictionary format, commonly named:

`RML2016.10a_dict.pkl`

Download it only from a source you trust and check the dataset's applicable terms before redistributing it. The dataset itself is intentionally **not included** in this GitHub repository.

Place the file here:

```text
data/RML2016.10a_dict.pkl
```

If the file is stored somewhere else, set the `RADIOML_DATA_PATH` environment variable to its full path before starting Jupyter. The notebook uses `pickle.load`; only load pickle files from trusted sources.

## 2. Set up Python

Python 3.10 or 3.11 is a reasonable starting point. Create and activate a virtual environment from the repository root.

### Windows (PowerShell)

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

If PowerShell blocks activation, you can instead run the environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyter lab
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

Open `notebooks/AMC_Code.ipynb` in Jupyter and run cells from top to bottom.

### Google Colab / Kaggle

Upload or open the notebook in your environment, add the RadioML dataset to the runtime, and update `DATA_PATH` (or set `RADIOML_DATA_PATH`) to the actual dataset path. GPU is optional but recommended for faster training.

## 3. Run the experiment

The notebook performs these main steps:

1. Loads and checks the 11 modulation classes and 20 SNR values.
2. Normalizes each I/Q example and makes a reproducible, class-and-SNR-stratified train/validation/test split.
3. Trains the CNN and CNN+attention models.
4. Reports aggregate, low-SNR, high-SNR, and per-SNR accuracy.
5. Saves confusion matrices, training curves, and accuracy-vs-SNR plots.
6. Generates 1D Grad-CAM comparisons for selected confusion pairs and SNR values.
7. Runs a simple deletion test for Grad-CAM and visualizes average attention matrices.

By default, the notebook uses one model seed to keep the runtime manageable. For a more reliable estimate, consider setting `SEEDS = [0, 1, 2]`, understanding that this increases training time. Keep the same data split and training budget for both models.

## 4. Outputs

Generated files are written to `amc_outputs/` (or `/kaggle/working/amc_outputs/` on Kaggle), including CSV metrics and PNG plots. The notebook also creates an `amc_outputs.zip` archive. These are generated when you execute the notebook; they are not included as precomputed results in this starter repository.

Do not report illustrative figures from the proposal as measured results. Only report results produced by running this code on the dataset, and document the environment, hyperparameters, split, and seeds.

## 5. Upload this project to GitHub

### Option A — GitHub website (no command line)

1. Sign in at [GitHub](https://github.com/).
2. Click **+** (top-right) → **New repository**.
3. Repository name suggestion: `snr-stratified-amc`.
4. Add a short description, for example: `Raw-IQ automatic modulation classification using 1D CNN, lightweight self-attention, and 1D Grad-CAM`.
5. Choose **Public** or **Private**. If this is coursework, check your institution's rules before making it public.
6. Do not upload the RadioML pickle dataset, virtual environment, checkpoints, or large generated outputs.
7. Create the repository.
8. In the repository page, choose **Add file** → **Upload files**.
9. Upload `README.md`, `requirements.txt`, `.gitignore`, `LICENSE`, and the `notebooks` folder (including `AMC_Code.ipynb`). GitHub's web upload interface may not upload an empty folder, so `data/README.md` documents where to put the dataset locally.
10. Enter a commit message such as `Add AMC project notebook and setup instructions`, then click **Commit changes**.

### Option B — Git command line (recommended)

Install Git, then open a terminal in this repository folder:

```bash
git init
git add .
git status
git commit -m "Initial commit: AMC CNN and attention project"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/snr-stratified-amc.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username and use the exact repository URL shown by GitHub. If Git asks you to authenticate, follow GitHub's sign-in/browser or token instructions; do not put passwords or access tokens into the notebook or repository.

If Git says `remote origin already exists`, run:

```bash
git remote -v
git remote set-url origin https://github.com/YOUR-USERNAME/snr-stratified-amc.git
git push -u origin main
```

## 6. Before publishing

- Confirm the notebook runs from a clean environment after adding the dataset.
- Check that no personal file paths, API keys, passwords, or private information remain.
- Keep large datasets and model checkpoints out of Git.
- Add your project proposal PDF only if you have permission to publish it.
- Replace this README's title/description if your final submitted project title changes.
- Add actual results and screenshots only after you run the experiment.
- Verify all references/DOIs against the original publications before academic submission.

## Research question

Can lightweight self-attention improve the low-SNR performance of a VT-CNN2-style AMC classifier, and can 1D Grad-CAM help investigate which temporal I/Q regions and confusion pairs contribute to model decisions and failures?

## Citation note

The accompanying proposal lists the relevant literature. Verify publication details against original sources before using them in a final academic submission.
