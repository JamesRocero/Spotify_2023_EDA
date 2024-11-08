# Spotify_2023_EDA
### 1. Importing Necessary Libraries
``` Python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import calendar
```
### 2. Loading Data Set
- This code reads a CSV file named spotify-2023.csv into a pandas DataFrame called df_spotify.
``` Python
df_spotify = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
df_spotify
```
- After loading the data above, the df_spotify DataFrame is displayed, which will show the contents of the file.

