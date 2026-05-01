# Empirical-Project-Data-Science-26
## Music Vs Macroeconomic Factors 
Data Science project (BEE2041)

## Motiviation
The motivation for this project stemmed from my insterest in music. Having been a teen during COVID 19 I felt a clear shift in the tempo and mood of music popularised during that time and wanted to research if there was an actual economic correlation with this. 

## Project Overview
This project investigates whether the tempo (BPM) and emotional mood (valence) of the most popular songs in the USA correlates with macroeconomic conditions, particularly unemployment and GDP growth. I focus on 1975-2025 to capture major economic events including the 2008 Global Financial Crisis and COVID 19.


**Blog post:** Can be found in the Github file

## Repository Structure 
```
|- README.md
|- blog.ipynb
|
|- data
|  |- raw
|  |  |- billboard_scraped.csv
|  |  |- KaggleSpotifyAudioFeatures.csv
|  |  |- audio_features2.csv
|  |  |- economic_fred.csv
|  |  
|  |- processed
|  |  |- songs_merges.csv
|  |  |- annual_merged.csv
|
|- notebooks
|  |- Billboard 100 1975-2025.ipynb
|  |- kaggle matching (valence and bpm).ipynb
|  |- FRED macroeconomic factors 1975-2025.ipynb
|  |- Merging BPM and Valence to Macro Factors.ipynb
```

## How to Replicate This Repository 

### Setting up API credentials
**FRED API** (for notebook economic_fred only)
- Go to https://fred.stlouisfed.org/docs/api/api_key.html
- Create a free account and request an API key

Create a file called .env in the terminal with:
FRED_API_KEY=your_fred_api_key_here

### Download the Kaggle dataset
- Go to https://www.kaggle.com/datasets/maharshipandya/spotify-tracks-dataset
- Download 'dataset.csv'
- Save it as '~/Desktop/kaggle_spotify.csv'

### Run the notebooks in order:
1. Billboard 100 1975-2025
2. kaggle matching (valence and bpm).ipynb
3. FRED macroeconomic factors 1975-2025.ipynb
4. Merging BPM and Valence to Macro Factors.ipynb

**Notebooks 1 and 2 involve webscraping and will take 30-60 minutes to run fully** If you want to skip scraping, the pre-scraped CSV files are uploaded onto the github repository under 'data/raw/'. You can skip straight to 'Merging BPM and Valence to Macro Factors.ipynb' to regenrate the merged dataset. 

## Notebook Descriptions

### 'Billboard 100 1975-2025.ipynb' 
This scrapes the billboard year end hot 100 charts from wikipedia between 1975-2025 using 'requests' and 'pandas'.

### 'kaggle matching (valence and bpm).ipynb' 
Matches each billboard song to the kaggle spotify audio features dataset to retreive BPM (which is stored as 'tempo') and valence using a two stage fuzzy matching approach. The first stage finds kaggle titles that closely match the Billboard title using 'token_set_ratio' from thefuzz library, handling where one string is asubset of the other. The second stage selects the entry whose artist best macthes the billboard one.
This is a replacement of the original single combined artist+title key approach which only had around a 5% match rate.

### 'FRED macroeconomic factors 1975-2025.ipynb'
pulls two economic time series from the FRED AP, GDPC1, real GDP quarterly converted to annual growth rate. And UNRATE, civilian unemployment rate monthly also converted to an annual average. A recession dummy is added too, 1 = recession year and 0 = non-recession year.

### 'Merging BPM and Valence to Macro Factors.ipynb'
Merges data into two clean sets:
- songs_merges.csv: one row person with BPM, valence and chart position
- annual_merged.csv: one row per year with position weighted average BPM and valence, GDP growth, unemployment rate, recession dummy and one-year lagged economic variables.

### 'blog.ipynb'
It is the main output and contains all analysis, visualisations and written narrative. Loading 'annual_merged.csv' directly from a Github raw url so it runs without any local files and sections include:
- BPM and Valence Trends Over 50 Years
- Close up on the 2008 crisis and COVID 19
- Scatter Plots With Correlation Coefficients
- OLS Regressions Models
- Summary Statistics Comparing Recession vs Non Recession years


## Requirements
requests
beautifulsoup4
pandas
numpy
matplotlib
seaborn
statsmodel
scipy
thefuzz
fredapi
python-dotenv
kagglehub
jupyter


## Data Sources 
- Billboard Year-End Hot 100 (1975-2025) -https://en.wikipedia.org/wiki/Category:Lists_of_Billboard_Year-End_Hot_100_singles - Web scraping 
- BPM + Valence - tomigelo/spotify-audio-features - Downloaded CSV - fuzzy matched
- GDP Growth Rate - FRED API - series GDPC1 - API (free key)
- Unemployment Rate - FRED API - series UNRATE - API (free key)


## Known Limitations 
- Valence and BPM coverage is limited: The Kaggle dataset is a 2018/2019 snapshot and songs from 1970s-1990s that were not onn Spotify at the time are not present. Our BPM and Valence coverage is approximately only 10% of the full song dataset initially scraped from wikipedia. A higher coverage rate would have given more statistical power and confidence in the results
- Fuzzy matching is imperfect: songs that have very common names or unusual artist name formatting may be incorrectly matched. A threshold of 90(title) and 70(artist) was chosen to balance recall precsion.
- Valence is an estimated value and does not directly measure the listeners sentiment to the song 
