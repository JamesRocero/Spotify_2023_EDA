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

##### After loading the data above, the df_spotify DataFrame is displayed, which will show the contents of the file.
  
### 3. Overview of the Data Set
``` Python
df_spotify.head()
```
![image](https://github.com/user-attachments/assets/92020afd-d4a7-42c0-9d2f-45fb2fdc0f77)
``` Python
df_spotify.tail()
```
![image](https://github.com/user-attachments/assets/084f45f5-b33d-466c-9350-56a28a3337d6)

#### Checking the Data
- Show the basic information of the data
```Python
df_spotify.info()
```
![image](https://github.com/user-attachments/assets/388df24a-46cc-40db-aca8-a0c9f9f2c189)

#### Missing Values
``` Python
missing_values = df_spotify.isnull().sum()

print("Characteristics in the dataset with missing values:")
print(missing_values[missing_values > 0])
```
![image](https://github.com/user-attachments/assets/4f6bb11e-26eb-40b1-b416-d0c638b6d4b9)

#### For Identical Values
``` Python
Identical_tracks = pd.DataFrame(df_spotify[df_spotify.duplicated(['track_name','artist(s)_name'])])

print("The identcal tracks that has similar values are: ")
print(Identical_tracks)
```
![image](https://github.com/user-attachments/assets/6314e4b3-9701-40a6-a694-3f60a43add3b)

##### The code's output above, indicates that some of the tracks are identical to one another. The songs "Take My Breath," "About Damn Time," "SNAP," and "SPIT ON MY FACE!" are such.

##### Unnecessary tracks must be eliminated, especially those with improper outputs.

#### "SPIT IN MY FACE!"
``` Python
df_spotify[df_spotify['track_name'] == 'SPIT IN MY FACE!']
```
![image](https://github.com/user-attachments/assets/91b17c27-aa79-4222-b195-28b242fbfe0d)

##### Additional investigation has revealed that the right bpm and key for "SPIT IN MY FACE!" is 166. Its key is also C#, not G#.
##### With this, we need to remove the one that is incorrect.

``` Python
df_spotify = df_spotify.drop(345)
df_spotify[df_spotify['track_name'] == 'SPIT IN MY FACE!']
```
![image](https://github.com/user-attachments/assets/7a93d3c1-c4b8-41ff-b7d1-cc6fa4aaf39b)

#### "Take My Breath"



