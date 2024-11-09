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
``` Python
df_spotify[df_spotify['track_name'] == 'Take My Breath']
```
![image](https://github.com/user-attachments/assets/c27afd47-b37b-49a6-8a0f-0093963e73f7)

##### The correct key for "Take My Breath" is G#, not A#, according to further examination.
##### With this, we must eliminate the inaccurate one.

``` Python
df_spotify = df_spotify.drop(512)
df_spotify[df_spotify['track_name'] == 'Take My Breath']
```
![image](https://github.com/user-attachments/assets/a8a7da50-6a0a-4b9d-b6de-4a6a1049e50d)

#### "About Damn Time"
``` Python
df_spotify[df_spotify['track_name'] == 'About Damn Time']
```
![image](https://github.com/user-attachments/assets/abeadf3d-4bfc-4fb7-b072-26b240f296e7)

##### 'About Damn Time' was released on April 14, 2022, in response to further investigations.
##### The one with the correct release date should be retained.

``` Python
df_spotify = df_spotify.drop(372)
df_spotify[df_spotify['track_name'] == 'About Damn Time']
```
![image](https://github.com/user-attachments/assets/a12b441e-b191-4a53-a712-cf6beb924057)

#### "SNAP"
``` Python
df_spotify[df_spotify['track_name'] == 'SNAP']
```
![image](https://github.com/user-attachments/assets/e50c9d73-d565-41eb-886d-858e3e2c72da)

##### We should just maintain the most recent one (most streams) since the information is same.
``` Python
df_spotify = df_spotify.drop(873)
df_spotify[df_spotify['track_name'] == 'SNAP']
```
![image](https://github.com/user-attachments/assets/fba991e3-74e6-41f5-9d72-72da6523b6ee)

#### Data Cleaning
- For streams
``` Python
df_spotify['streams'] = df_spotify['streams'].apply(pd.to_numeric, errors='coerce')
df_spotify['streams']
```
![image](https://github.com/user-attachments/assets/9677e352-c20c-49d5-b945-46e95fcab4b6)

##### This process above, ensures that all values in the streams column are of numeric type, allowing for proper analysis or calculations, such as aggregations or sorting.
##### The value in row 574 is changed to nan, making it a missing value. We must store the updated value in that row after determining the streams' values.

``` Python
df_spotify.iloc[574, df_spotify.columns.get_loc('streams')] = 227373748
df_spotify.iloc[574, df_spotify.columns.get_loc('streams')]
```
![image](https://github.com/user-attachments/assets/af64bdf2-48c5-4c81-ac80-b17760b15b17)

##### This code above, updates the number of streams for a specific track (row 574) in the streams column and then checks the updated value.

- Check if the missing values were properly replaced/fixed.
``` Python
df_spotify.iloc[574]
```

![image](https://github.com/user-attachments/assets/1456899e-97fd-478e-9878-ceb43760817d)

### Data Descriptive Statistics
#### Total streams of the most streamed spotify song
``` Python
"The 2023 Spotify song that has received the most streams overall is " + str(df_spotify['streams'].sum())
```

![image](https://github.com/user-attachments/assets/bda9f782-ccbb-41df-8b7a-c478f043eaf1)

#### Mean of the streams column
``` Python
"In 2023, the average number of streams for the most popular songs on Spotify is {}".format(df_spotify['streams'].mean())
```

![image](https://github.com/user-attachments/assets/a9234411-e3c6-4b8c-bf3c-7579168e51e5)

#### Median of the streams column
``` Python
"The 2023 median number of plays for the most popular songs on Spotify is {}".format(df_spotify['streams'].median())
```

![image](https://github.com/user-attachments/assets/6c0cda7b-0dcb-4e90-9c64-158080148ce9)

#### Standard deviation of the streams column
``` Python
std_dev_streams = df_spotify['streams'].std()
"The 2023 Spotify song with the most streams, as measured by the standard deviation of total streams: {}".format(std_dev_streams)
```

![image](https://github.com/user-attachments/assets/d3776a50-ade9-4874-bbc5-d74078e633bc)

#### Obtains the title's top ten by using.head(10) and the amount of streams.
``` Python
df_spotify.loc[:, ['track_name', 'streams']].nlargest(10, 'streams')
```

![image](https://github.com/user-attachments/assets/4a1e5e96-eaaa-4a56-83b5-983821d829e8)

- Create a Histogram Plot
``` Python
# Plot the histogram
ax = sns.histplot(data=df_spotify, x='streams', color='green', element="bars")
ax.set_title('Distribution of Total Streams')

# Set grid style
sns.set_style("whitegrid", {'grid.linestyle': '--'})

# Label x and y axes
ax.set_xlabel('Streams (Billions)')
ax.set_ylabel('Track Count')
```

![image](https://github.com/user-attachments/assets/79112410-31a1-4001-908d-2d6d0c73c7e1)

#### Distribution of released_year and artist_count
- This code creates a distribution plot of Spotify songs' release years, customizing the plot's appearance.
``` Python
# Plot the distribution with updated characteristics
ryd = sns.displot(
    data=df_spotify,
    x='released_year',
    color='red',
    discrete=True,
    aspect=2.5,
    height=5
)
ryd.set(title="Distribution of Most Streamed Spotify Songs of 2023 by Release Year")

# Access the underlying axes and modify grid and axes properties
ax = ryd.axes[0, 0]

# Modify the grid: Make it thicker and change the style
ax.grid(True, axis="both", color="black", linestyle='-', linewidth=1.2, alpha=0.3)

# Change the axes' properties
ax.set_xlabel('Release Year of Tracks', fontsize=12, color='darkgreen')
ax.set_ylabel('Track Count', fontsize=12, color='darkgreen')

# Change axis limits if needed
ax.set_xlim(2015, 2023)  # Example: setting custom x-axis limits
ax.set_ylim(0, 100)      # Example: setting custom y-axis limits
```

![image](https://github.com/user-attachments/assets/0cb4606a-c575-4ee1-a367-6286e22a3146)

- The code creates a visually appealing distribution plot that shows the number of credited artists per track in the df_spotify dataset.
``` Python
# Set the figure size and style for the plot
plt.figure(figsize=(12, 6))

# Create a displot with updated characteristics for the 'artist_count' distribution
ac = sns.displot(
    data=df_spotify,
    x='artist_count',
    color='yellow',      # Use a new color for the bars
    discrete=True,
    aspect=2,                # Adjust aspect ratio
    height=5                 # Set plot height
)
ac.set(title="Distribution of Credited Artists per Track")

# Apply the white background style
sns.set_style("white")

# Access the axes and modify grid properties
ax = ac.axes[0, 0]  # Access the primary axis
ax.grid(True, axis="y", color="gray", linestyle="--", linewidth=0.8, alpha=0.6)

# Set axis labels with modified font sizes and colors
plt.xlabel('Number of Credited Artists', fontsize=12, color='darkblue')
plt.ylabel('Track Count', fontsize=12, color='darkblue')

# Show the plot
plt.show()
```

![image](https://github.com/user-attachments/assets/bbfd9d19-fe2d-4f09-ac85-cfbf09e773f4)

### Top Performers








