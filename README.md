# SNR-Stratified Automatic Modulation Classification (AMC)

This project investigates automatic modulation classification using a 1D Convolutional Neural Network (CNN), a CNN enhanced with lightweight self-attention, and 1D Grad-CAM for model interpretability.

The experiments use the RadioML 2016.10a dataset to evaluate classification performance across different signal-to-noise ratio (SNR) levels.

## Research Question

Can lightweight self-attention improve the low-SNR performance of a VT-CNN2-style automatic modulation classification (AMC) model? Can 1D Grad-CAM help identify the temporal I/Q regions and modulation-class confusion pairs that contribute to model decisions and classification errors?

## Key Features

- Loads and validates the RadioML 2016.10a dataset.
- Supports 11 modulation classes and 20 SNR values.
- Normalizes I/Q signal samples.
- Creates reproducible, class-and-SNR-stratified training, validation, and test splits.
- Trains and compares CNN and CNN+attention models.
- Evaluates overall, low-SNR, high-SNR, and per-SNR classification accuracy.
- Generates confusion matrices and training curves.
- Visualizes accuracy versus SNR.
- Applies 1D Grad-CAM to investigate model predictions and selected confusion pairs.
- Performs a simple Grad-CAM deletion test.
- Visualizes average attention matrices.
- Exports evaluation metrics and plots for further analysis.

## Repository Structure

```text
snr-stratified-amc/
├── data/
│   └── README.md
├── notebooks/
│   └── AMC_Code.ipynb
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

**Note:** The RadioML dataset and generated experiment outputs are not included in the repository by default. The `amc_outputs/` directory is created when the notebook is executed.

## 1. Dataset Setup

This project uses the **RadioML 2016.10a** dataset, commonly distributed as a Python pickle file named:

```text
RML2016.10a_dict.pkl
```

Obtain the dataset from a trusted source and review its applicable license and usage terms before using or redistributing it.

Place the dataset in the following location:

```text
data/RML2016.10a_dict.pkl
```

If the dataset is stored elsewhere, set the `RADIOML_DATA_PATH` environment variable to its full path before launching Jupyter.

**Security note:** Python pickle files can execute arbitrary code during deserialization. Only load dataset files from trusted sources.

## 2. Python Environment Setup

Python 3.10 or 3.11 is a reasonable starting point.

### Windows (PowerShell)

Run the following commands from the repository root:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

If PowerShell blocks virtual environment activation, run the environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
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

After Jupyter starts, open:

```text
notebooks/AMC_Code.ipynb
```

Run the notebook cells from top to bottom.

### Google Colab / Kaggle

1. Upload or open the notebook in your environment.
2. Add the RadioML 2016.10a dataset to the runtime.
3. Update `DATA_PATH` or set `RADIOML_DATA_PATH` to the actual dataset location.
4. Install any missing dependencies listed in `requirements.txt`.
5. Run the notebook cells in order.

GPU acceleration is optional but recommended for faster training.

## 3. Running the Experiments

The notebook performs the following steps:

1. Loads the dataset and checks the expected 11 modulation classes and 20 SNR values.
2. Normalizes each I/Q example.
3. Creates reproducible, class-and-SNR-stratified training, validation, and test splits.
4. Trains the baseline CNN model.
5. Trains the CNN+attention model using a comparable training budget.
6. Evaluates overall classification accuracy and accuracy at different SNR levels.
7. Generates confusion matrices, training curves, and accuracy-versus-SNR plots.
8. Applies 1D Grad-CAM to selected confusion pairs and SNR values.
9. Performs a simple deletion test to investigate Grad-CAM explanations.
10. Visualizes average attention matrices.

By default, the notebook uses a single model seed to keep the runtime manageable.

For more reliable estimates, consider using multiple seeds:

```python
SEEDS = [0, 1, 2]
```

Multiple seeds increase training time. Use the same data split and comparable training budgets for both models to ensure a fair comparison.

## 4. Results and Outputs

The notebook saves generated experiment artifacts to:

```text
amc_outputs/
```

On Kaggle, the output directory may instead be:

```text
/kaggle/working/amc_outputs/
```

Depending on the notebook configuration, generated artifacts include:

- CSV files containing evaluation metrics.
- Confusion matrices.
- Training and validation curves.
- Accuracy-versus-SNR plots.
- Grad-CAM visualizations.
- Attention matrix visualizations.
- An `amc_outputs.zip` archive.

These outputs are generated by running the notebook and are not provided as precomputed results in this repository.

**Important:** Do not present illustrative figures from a project proposal as experimental findings. Report only results obtained by running the code on the dataset. Document the software environment, hyperparameters, data split, and random seeds used.

## 5. Evaluation Strategy

The experiments compare the baseline CNN and CNN+attention models using the following measures:

| Evaluation | Purpose |
|---|---|
| Overall accuracy | Measures aggregate classification performance. |
| Low-SNR accuracy | Evaluates performance under noisier signal conditions. |
| High-SNR accuracy | Evaluates performance under cleaner signal conditions. |
| Per-SNR accuracy | Examines how performance changes across SNR values. |
| Confusion matrices | Identifies frequently confused modulation classes. |
| 1D Grad-CAM | Investigates temporal I/Q regions associated with predictions. |
| Deletion test | Provides a simple perturbation-based check of Grad-CAM explanations. |
| Attention matrices | Helps visualize learned attention patterns. |

The central comparison is whether lightweight self-attention improves classification performance, particularly at low SNR, and whether Grad-CAM provides useful insight into model behavior.

## 6. Reproducibility and Limitations

Experimental results may vary with random seeds, hardware, library versions, preprocessing choices, and training configurations.

For a fair model comparison:

- Use the same dataset partition for both models.
- Keep preprocessing consistent.
- Use comparable training budgets and evaluation procedures.
- Report results across the same SNR values.
- Consider multiple random seeds when computational resources permit.
- Interpret Grad-CAM and attention visualizations as diagnostic tools rather than definitive proof of causal model behavior.

The project is intended to investigate both classification performance and model interpretability.

## 7. Research Contribution

This project investigates two complementary aspects of automatic modulation classification:

1. **Classification performance:** Whether lightweight self-attention improves the performance of a VT-CNN2-style CNN, especially under low-SNR conditions.
2. **Model interpretability:** Whether 1D Grad-CAM can help examine temporal I/Q regions associated with model predictions and reveal patterns in frequently confused modulation classes.

The experiments aim to provide a reproducible framework for comparing model performance and exploring potential explanations for classification errors.

## 8. Citation and References

The accompanying project proposal identifies the relevant literature. Verify the original publication details, author names, publication venues, and DOIs before citing these works in an academic report.

When publishing or extending this project, cite the original RadioML dataset source and the relevant publications for the model architecture, attention mechanism, and Grad-CAM methodology.

---

**Disclaimer:** This repository provides code and experimental procedures for research and educational purposes. Results should be reported only after the experiments have been executed and independently verified.
