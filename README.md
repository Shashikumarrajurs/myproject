# myproject
# Project Overheat: Predictive Maintenance AI

**AeroLogistics | Data Science Capstone Project**

Predict catastrophic motor failure **24 hours in advance** using IoT sensor telemetry and a class-weighted Random Forest model.

## Project overview

Unexpected motor failures can stop automated sorting operations and cause costly downtime. This project explores a proactive maintenance workflow using hourly temperature, vibration, and voltage readings.

The notebook follows the assignment requirements:
1. Load and validate the IoT telemetry dataset.
2. Sort records chronologically.
3. Engineer 12-hour rolling features:
   - `Temp_Rolling_Mean_12h`
   - `Vibration_Rolling_Std_12h`
4. Create a 24-hour-ahead target with `shift(-24)`.
5. Use the first 80% of the timeline for training and the final 20% for testing.
6. Train a `RandomForestClassifier` with `class_weight="balanced"`.
7. Evaluate failure precision, recall, F1-score, balanced accuracy, and average precision.
8. Visualize the confusion matrix and feature importances.
9. Generate a maintenance recommendation based on the model output.

## Repository contents

```text
Predictive_Maintenance_AI/
├── predictive_maintenance_final.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Dataset

The original dataset contains 10,000 hourly observations and these columns:

| Column | Description |
|---|---|
| `Timestamp` | Observation timestamp |
| `Sensor_Temp_C` | Sensor temperature in Celsius |
| `Sensor_Vibration_mm` | Sensor vibration measurement |
| `Sensor_Voltage_V` | Sensor voltage measurement |
| `Catastrophic_Failure` | Failure indicator (0 = no failure, 1 = failure) |

**Dataset handling:** The notebook checks for `iot_sensor_telemetry.csv` in the same directory. If it is not present, it uses the compressed fallback copy embedded in the notebook. Therefore, the notebook can run as a single-file deliverable. If you are permitted to share the source CSV, place it beside the notebook; otherwise, do not upload restricted or confidential data.

## Installation

Python 3.9 or later is recommended.

```bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**
```powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**
```bat
.venv\Scripts\activate
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install -r requirements.txt
```

## Run the project

1. Open the repository folder in VS Code or JupyterLab.
2. Start Jupyter:

```bash
jupyter notebook
```

3. Open `predictive_maintenance_final.ipynb`.
4. Select the Python kernel from your environment.
5. Choose **Run All** and execute cells from top to bottom.

## Key methodology

### 12-hour rolling features

The project calculates the rolling mean of temperature and rolling standard deviation of vibration using the current hour and previous 11 hours. Rows without a complete rolling window are removed.

### 24-hour prediction target

`Catastrophic_Failure.shift(-24)` aligns the failure label 24 rows into the future with the current sensor readings. The final 24 rows are removed because their future labels are unavailable. The original failure column is excluded from the model inputs to prevent target leakage.

### Time-based validation

The dataset is split chronologically (first 80% for model development, final 20% for final testing). The notebook also selects a classification threshold using a later validation segment inside the training period, leaving the final test period untouched until evaluation.

### Model and explainability

A Random Forest classifier uses balanced class weights to account for rare failure events. Feature importance is plotted as a horizontal bar chart. Feature importance indicates model reliance, not causation.

## Evaluation

The notebook reports:
- Failure-class precision
- Failure-class recall
- Failure-class F1-score
- Balanced accuracy
- Average precision (PR AUC)
- Confusion matrix
- Random Forest feature importance

Run the notebook to generate the actual metrics for your execution. Do not copy metrics from a different run without verifying them.

## Limitations and responsible use

This is an academic prototype, not a production safety system. The dataset has very few recorded failure events, so performance estimates may be unstable. Before real-world use, collect more labelled failures, validate on additional future periods, review alerts with maintenance engineers, and monitor model drift. A model alert should trigger inspection, not an automatic machine shutdown.

## Learning resources

- [Scikit-learn: Random Forest Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
- [Pandas: Rolling window calculations](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rolling.html)
- [Pandas: shift](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.shift.html)
- [Matplotlib documentation](https://matplotlib.org/stable/)

## Project links

- **GitHub repository:** Add your repository URL here after creating it.
- **Project notebook:** `predictive_maintenance_final.ipynb`
- **Your project walkthrough video:** Add your own recorded video URL here if your instructor requires a demonstration of this exact notebook.
- **Related learning tutorial:** [Predictive Maintenance: Using Machine Learning to Anticipate Industrial Equipment Failure](https://www.youtube.com/watch?v=nmUKtsCUgnY) — an external example using a different dataset and workflow; it is not a recording of this assignment.

## Author

**Shashikumar Raj Urs**

Data Science Capstone Project
