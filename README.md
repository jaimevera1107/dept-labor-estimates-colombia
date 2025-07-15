# 🇨🇴 Colombian Labor Market Reconstruction (1993–2025)

This repository implements a statistical framework to reconstruct monthly and annual labor indicators for all 33 departments of Colombia over the 1993–2025 period. The methodology combines demographic projections, economic structure, survey-based labor data, and behavioral proxies into a unified, supervised inference pipeline.

## 📊 Overview

The reconstruction process integrates multiple public data sources and advanced statistical techniques, including:

- 📈 **Population projections** from DANE (1993–2050)
- 🏙️ **Urban labor statistics** from the GEIH (household survey, 2007–2024)
- 🧮 **Temporal disaggregation methods** (Chow-Lin, Denton, Litterman)
- 🧠 **Supervised learning models** (XGBoost regressors)
- 🧾 **Sectoral GDP structure** at the departmental level
- 🔍 **Behavioral indices** from Google Trends (labor-related queries)
- 🔄 **Retropolarization procedures** to ensure internal demographic and accounting consistency

The result is a fully standardized, demographically grounded panel of departmental labor indicators suitable for policy design, territorial analysis, and academic research.

## 📁 Repository Structure

```text
├── clm_estimation.ipynb            # Main notebook with the estimation pipeline
├── data/                           # Raw and processed input data (not versioned)
│   ├── Google/
│   │   └── Tendencias de Google.xlsx
│   ├── ILOSTAT/
│   │   └── Estadísticas ILOSTAT.xlsx
│   ├── Mercado Laboral/
│   ├── Población/
│   │   ├── Proyecciones 1993–2004.xlsx
│   │   ├── Proyecciones 2005–2019.xlsx
│   │   └── Proyecciones 2020–2050.xlsx
│   ├── Precios/
│   └── Producción/
├── modelos_xgb/                    # Trained XGBoost models (.joblib)
│   ├── xgb_pet_dep.joblib
│   ├── xgb_pea_dep.joblib
│   └── ...
├── outputs/                        # Final output datasets in multiple formats
│   ├── csv/
│   │   ├── departmental_monthly.csv
│   │   └── national_annual.csv
│   ├── excel/
│   ├── parquet/
│   └── ...


📚 Documentation
The full methodological description and results are available in the working paper:

Vera-Jaramillo, J. A. (2025).
A Statistical Framework for Reconstruction of Labor Patterns: Colombian Departments 1993–2025.
arXiv:2503.22054

🗂️ Data Sources
DANE – Population projections, GEIH microdata, sectoral GDP

ILOSTAT – Historical labor aggregates

Google Trends – Digital behavioral proxies