# Healthcare Provider Fraud Detection

## Project Overview
This project is a data science pipeline designed to detect fraudulent healthcare claims. Instead of relying on a machine learning model right away, this project builds a transparent, multi-tiered statistical engine using Pandas and Python to isolate anomalous behavior at the provider, procedure, and individual claim levels.

## The Data
The dataset (`healthcare_fraud_detection.csv`) contains 10,000 synthetic healthcare claims with 20 features, including:
* **Provider & Patient Demographics:** `Provider_ID`, `Patient_Age`, `Patient_Gender`, `Patient_State`
* **Medical Context:** `Diagnosis_Code`, `Procedure_Code`, `Chronic_Condition_Flag`
* **Financials:** `Claim_Amount`, `Approved_Amount`, `Insurance_Type`
* **Ground Truth:** An `Is_Fraud` binary indicator used for final evaluation.

## Methodology: The 3-Tiered Detection Engine
To avoid the "smoothing effect" where normal claims hide fraudulent ones, the analysis was broken into three progressively tighter tests using Z-Score statistical thresholds:

1. **Test 1: Global Provider Benchmarking** 
   * Evaluated each provider's overall average claim amount against the global average. 
2. **Test 2: Systematic Procedure Markup Detection** 
   * Grouped claims by `Procedure_Code` to find providers systematically overcharging for specific services compared to the global average for that exact service.
3. **Test 3: Isolated Claim Anomaly Detection** 
   * Evaluated every individual claim against its specific procedure's global mean, aiming to catch massive one-off scams (Z-scores > 3.0).

## Results & Evaluation
The tiered system was evaluated against the dataset's ground truth (`Is_Fraud`). Moving from a generalized test (Test 1) to a more precise (Test 3) yielded massive improvements:

* **Test 1 (Overall Averages):** Failed to isolate fraud due to data smoothing.
* **Test 2 (Systematic Fraud):** Identified only 0.5% patterns of markup abuse.
* **Test 3 (Isolated Anomalies):** Caught the highest percentage of fraudulent claims (only 10.9%), though had many false positive.

## Tech Stack
* **Language:** Python 3.12
* **Data Manipulation:** Pandas
* **Environment:** Jupyter Notebook

## How to Run
1. Clone this repository.
2. Ensure you have pandas and scikit-learn installed (`pip install pandas scikit-learn`).
3. Place the `healthcare_fraud_detection.csv` file in the `data/raw/` directory.
4. Run the Jupyter Notebook cell-by-cell.

## Future
1. More accurate tests
2. Visual representation of findings