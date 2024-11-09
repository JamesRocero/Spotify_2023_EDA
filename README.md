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
![image](https://github.com/user-attachments/assets/02fed677-2962-43e3-8d22-5d8eedcf6807)

###### After loading the data above, the df_spotify DataFrame is displayed, which will show the contents of the file.
  
### 3. Overview of the Data Set
``` Python
df_spotify.head()
```
![image](https://github.com/user-attachments/assets/92020afd-d4a7-42c0-9d2f-45fb2fdc0f77)
``` Python
df_spotify.tail()
```
![image](https://github.com/user-attachments/assets/084f45f5-b33d-466c-9350-56a28a3337d6)

