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

## 📥 Quick Data Access from Python

You can download any of the processed outputs directly using the following custom class:

```python
import os
import requests

class LaborDataDownloader:
    """
    Download Colombian labor market estimates from GitHub, supporting multiple formats,
    frequencies, and geographic levels.
    """

    def __init__(self, save_path=".", verbose=True):
        # Store local output path
        self.save_path = save_path

        # Store verbosity setting
        self.verbose = verbose

        # Define valid options
        self.valid_levels = {"departmental", "national"}
        self.valid_freqs = {"monthly", "annual"}
        self.valid_formats = {"csv": "csv", "xlsx": "excel", "parquet": "parquet"}

    def download(self, level="departmental", freq="monthly", file_format="csv"):
        """
        Download and save labor data file based on specified parameters.

        Parameters:
        - level: "departmental" or "national"
        - freq: "monthly" or "annual"
        - file_format: "csv", "xlsx", or "parquet"

        Returns:
        - Full local path to the downloaded file
        """
        # Validate level
        if level not in self.valid_levels:
            raise ValueError(f"Invalid level '{level}'. Must be one of: {self.valid_levels}")

        # Validate frequency
        if freq not in self.valid_freqs:
            raise ValueError(f"Invalid frequency '{freq}'. Must be one of: {self.valid_freqs}")

        # Validate file format
        if file_format not in self.valid_formats:
            raise ValueError(f"Invalid format '{file_format}'. Must be one of: {set(self.valid_formats.keys())}")

        # Build folder and file path
        folder = self.valid_formats[file_format]
        file_name = f"{level}_{freq}.{file_format}"
        url = (
            f"https://raw.githubusercontent.com/jaimevera1107/"
            f"dept-labor-estimates-colombia/main/outputs/{folder}/{file_name}"
        )
        destination = os.path.join(self.save_path, file_name)

        # Verbose log: starting download
        if self.verbose:
            print(f"Downloading: {url}")

        # Attempt request
        try:
            response = requests.get(url)
            response.raise_for_status()
        except requests.RequestException as e:
            raise RuntimeError(f"Failed to download file: {e}")

        # Save file locally
        try:
            with open(destination, "wb") as f:
                f.write(response.content)
        except Exception as e:
            raise RuntimeError(f"Failed to save file locally: {e}")

        # Verbose log: saved file
        if self.verbose:
            print(f"Saved to: {destination}")

        return destination


if __name__ == "__main__":
    # Example usage
    downloader = LaborDataDownloader(verbose=True)

    # Download departmental monthly data in CSV
    downloader.download(level="departmental", freq="monthly", file_format="csv")

    # Download national annual data in Excel
    downloader.download(level="national", freq="annual", file_format="xlsx")

```


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
