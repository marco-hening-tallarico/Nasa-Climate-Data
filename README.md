# Nasa-Climate-Data

Here's a bunch of climate data from the NASA POWER API.

## Table of contents
- [About](#about)
- [Data description](#data-description)
- [Files included](#files-included)
- [Quick start](#quick-start)
  - [Using the CSVs (Python / pandas)](#using-the-csvs-python--pandas)
  - [Fetching new data from NASA POWER API](#fetching-new-data-from-nasa-power-api)
- [Typical data fields](#typical-data-fields)
- [Use cases](#use-cases)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

## About
This repository contains climate and meteorological data exported from the NASA POWER (Prediction Of Worldwide Energy Resource) API. It is intended for analysis, visualization, modeling, and educational purposes.

## Data description
Data are time series (daily/hourly depending on exports) for geographic locations and ranges of dates requested from the NASA POWER API. The dataset includes commonly used climate variables such as temperature, precipitation, solar radiation and wind speed.

Source: NASA POWER API — https://power.larc.nasa.gov

## Files included
- CSV files containing climate time-series (e.g., one file per location or per date-range).
- README.md (this file)

(If you have a different structure — e.g., an organized `data/` folder or JSON/Parquet files — update this section to list the actual filenames and their purpose.)

## Quick start

Prerequisites:
- Python 3.8+
- pandas
- requests (if you want to fetch more data programmatically)

Install dependencies:
```bash
pip install pandas requests
```

### Using the CSVs (Python / pandas)
Example: load a CSV and show summary statistics.
```python
import pandas as pd

df = pd.read_csv("data/example_location_2020-01-01_to_2020-12-31.csv", parse_dates=["DATE"])
print(df.head())
print(df.describe())
```

### Fetching new data from NASA POWER API
You can request data directly from the NASA POWER REST API. Example curl:
```bash
curl "https://power.larc.nasa.gov/api/temporal/daily/point?latitude=38.0&longitude=-77.0&start=20200101&end=20201231&parameters=T2M,PRECTOT,WS2M,ALLSKY_SFC_SW_DWN&community=RE" -o output.json
```

A minimal Python example:
```python
import requests
import pandas as pd

url = (
    "https://power.larc.nasa.gov/api/temporal/daily/point"
    "?latitude=38.0&longitude=-77.0"
    "&start=20200101&end=20201231"
    "&parameters=T2M,PRECTOT,WS2M,ALLSKY_SFC_SW_DWN"
    "&community=RE&format=JSON"
)

r = requests.get(url)
r.raise_for_status()
data = r.json()
# parse the time series into a DataFrame (structure depends on API response)
# Example conversion for daily T2M:
t2m = data["properties"]["parameter"]["T2M"]
df = pd.DataFrame.from_dict(t2m, orient="index", columns=["T2M"])
df.index = pd.to_datetime(df.index)
print(df.head())
```

Refer to the NASA POWER API docs for parameter names, temporal resolutions, and available communities:
https://power.larc.nasa.gov/docs/

## Typical data fields
Depending on the export parameters, typical fields include:
- DATE — date (YYYYMMDD) or timestamp
- T2M — air temperature at 2 m (°C)
- T2M_MAX / T2M_MIN — daily max/min temperature (°C)
- PRECTOT — precipitation total (mm)
- WS2M — wind speed at 2 m (m/s)
- ALLSKY_SFC_SW_DWN — surface shortwave radiation (W/m²)
- RH2M — relative humidity at 2 m (%)
- Other derived or requested parameters available from NASA POWER

Adjust parsing and units according to the parameters you requested.

## Use cases
- Climate trend analysis and visualization
- Solar/renewable energy resource assessment
- Agro-meteorological modeling
- Education and demonstrations

## Contributing
Contributions are welcome. Suggestions:
- Add a script to fetch and standardize API exports into a canonical CSV layout
- Add metadata files describing each CSV (location, bounding box, variables, time range)
- Add tests or example notebooks demonstrating analysis workflows

Please open issues or pull requests to propose changes.

## License
This repository does not include an explicit license file. If you want others to reuse the repository, add a license (e.g., MIT) in a LICENSE file. Data retrieved from NASA POWER is subject to NASA terms — see the NASA POWER API documentation and terms for usage restrictions and attribution requirements.

## Acknowledgements
- NASA POWER (Prediction Of Worldwide Energy Resource) — https://power.larc.nasa.gov

## Contact
If you have questions or need help with this repository, open an issue or contact the repository owner.
