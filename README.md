<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white" />
  <img src="https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" />
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" />
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" />
</p>

# 🏦 Credit Risk Modeling — Lauki Finance

> **A machine‑learning‑powered web application that predicts loan default probability, generates a credit score (300–900), and assigns a risk rating — all in real time.**

---

## 📑 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Model Details](#model-details)
- [Credit Score & Rating Scale](#credit-score--rating-scale)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

Traditional credit assessment is slow, subjective, and error-prone. **Lauki Finance's Credit Risk Model** replaces manual underwriting with a trained logistic regression pipeline that:

1. Accepts applicant and loan information through an interactive web form.
2. Scales and encodes the inputs exactly as the model was trained.
3. Outputs the **probability of default**, a **credit score**, and a human-readable **risk rating**.

The application is built with **Streamlit**, making it instantly deployable on Streamlit Community Cloud or any Python-capable server.

---

## Key Features

| Feature | Description |
|---|---|
| 🎯 **Real-Time Prediction** | Get default probability, credit score, and risk rating on a button click |
| 📊 **12 Input Parameters** | Covers demographics, financials, loan details, and credit history |
| ⚙️ **Auto-Computed Ratios** | Loan-to-income ratio is calculated automatically from user inputs |
| 🧠 **Pre-Trained Pipeline** | Serialized model + scaler + feature list via `joblib` — no retraining needed |
| 🌐 **One-Click Deploy** | Ready for Streamlit Community Cloud out of the box |

---

## How It Works

```
┌──────────────┐     ┌──────────────────┐     ┌───────────────────┐     ┌──────────────┐
│  User Input  │ ──▸ │  Feature Eng. &  │ ──▸ │  Logistic Regr.   │ ──▸ │   Output:    │
│  (Streamlit) │     │  MinMax Scaling   │     │  Prediction       │     │  Score/Rating│
└──────────────┘     └──────────────────┘     └───────────────────┘     └──────────────┘
```

1. **User Input** — The Streamlit form collects age, income, loan amount, tenure, delinquency stats, credit utilization, residence type, loan purpose, and loan type.
2. **Feature Engineering** — Loan-to-income ratio is computed; categorical features are one-hot encoded; numeric features are scaled using a pre-fitted `MinMaxScaler`.
3. **Prediction** — The model's coefficients are applied to the processed input to produce a default probability via the logistic (sigmoid) function.
4. **Credit Score Mapping** — The non-default probability is linearly mapped to a 300–900 credit score range, and a categorical rating (Poor → Excellent) is assigned.

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend / UI** | [Streamlit](https://streamlit.io/) |
| **ML Model** | Logistic Regression (`scikit-learn`) |
| **Data Processing** | Pandas, NumPy |
| **Serialization** | Joblib |
| **Language** | Python 3.8+ |

---

## Project Structure

```
ML-project-Credit-Risk-model/
│
├── main.py                  # Streamlit app — UI and user interaction
├── prediction_helper.py     # Model loading, feature prep, prediction logic
├── requirements.txt         # Python dependencies
├── .gitignore               # Standard Python gitignore
│
└── artifacts/
    └── model_data.joblib    # Serialized model bundle (model + scaler + features)
```

| File | Purpose |
|---|---|
| `main.py` | Defines the Streamlit interface: input widgets, layout, and result display |
| `prediction_helper.py` | Loads the trained model, prepares input features, computes credit score & rating |
| `artifacts/model_data.joblib` | Contains the trained `LogisticRegression` model, fitted `MinMaxScaler`, feature list, and column-scaling config |
| `requirements.txt` | Lists all runtime dependencies (`streamlit`, `joblib`, `numpy`, `pandas`, `scikit-learn`, `xgboost`) |

---

## Getting Started

### Prerequisites

- **Python 3.8+** installed
- **pip** (or any Python package manager)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Yerrithathachoppa/ML-project-Credit-Risk-model.git
cd ML-project-Credit-Risk-model

# 2. (Optional) Create a virtual environment
python -m venv venv
source venv/bin/activate      # Linux / macOS
venv\Scripts\activate         # Windows

# 3. Install dependencies
pip install -r requirements.txt
```

### Run Locally

```bash
streamlit run main.py
```

The app will open in your default browser at **http://localhost:8501**.

---

## Usage

1. **Fill in the form** — Enter applicant details across the four rows of input fields:
   - **Row 1:** Age, Income, Loan Amount
   - **Row 2:** Loan-to-Income Ratio *(auto-calculated)*, Loan Tenure, Avg DPD
   - **Row 3:** Delinquency Ratio, Credit Utilization Ratio, Open Loan Accounts
   - **Row 4:** Residence Type, Loan Purpose, Loan Type

2. **Click "Calculate Risk"** — The model processes your inputs and displays:
   - **Default Probability** — Likelihood the applicant will default (e.g., `12.34%`)
   - **Credit Score** — Numeric score on a 300–900 scale
   - **Rating** — Categorical risk label (Poor / Average / Good / Excellent)

---

## Model Details

| Attribute | Value |
|---|---|
| **Algorithm** | Logistic Regression |
| **Scaling** | MinMaxScaler (applied to numeric columns) |
| **Encoding** | One-hot encoding for categorical features (Residence Type, Loan Purpose, Loan Type) |
| **Serialization** | `joblib` — bundles model, scaler, feature names, and columns-to-scale list |

### Input Features

| # | Feature | Type | Description |
|---|---|---|---|
| 1 | `age` | Numeric | Applicant's age (18–100) |
| 2 | `income` | Numeric | Annual income |
| 3 | `loan_amount` | Numeric | Requested loan amount |
| 4 | `loan_tenure_months` | Numeric | Loan duration in months |
| 5 | `avg_dpd_per_delinquency` | Numeric | Average days past due per delinquency event |
| 6 | `delinquency_ratio` | Numeric | Ratio of delinquent payments (0–100%) |
| 7 | `credit_utilization_ratio` | Numeric | Credit utilization percentage (0–100%) |
| 8 | `number_of_open_accounts` | Numeric | Number of active loan accounts (1–4) |
| 9 | `residence_type` | Categorical | Owned / Rented / Mortgage |
| 10 | `loan_purpose` | Categorical | Education / Home / Auto / Personal |
| 11 | `loan_type` | Categorical | Secured / Unsecured |
| 12 | `loan_to_income` | Derived | Computed as `loan_amount / income` |

---

## Credit Score & Rating Scale

| Score Range | Rating | Interpretation |
|---|---|---|
| 300 – 499 | 🔴 **Poor** | High default risk — loan likely to be declined |
| 500 – 649 | 🟠 **Average** | Moderate risk — may require additional collateral |
| 650 – 749 | 🟢 **Good** | Low risk — favorable terms likely |
| 750 – 900 | 🟢 **Excellent** | Minimal risk — best rates and terms |

---

## Contributing

Contributions are welcome! To get started:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/your-feature`)
3. **Commit** your changes (`git commit -m "Add your feature"`)
4. **Push** to the branch (`git push origin feature/your-feature`)
5. **Open** a Pull Request

---

## License

This project is open source and available for educational and personal use.

---

<p align="center">
  <b>Built with ❤️ by <a href="https://github.com/Yerrithathachoppa">Yerrithathachoppa</a></b>
</p>