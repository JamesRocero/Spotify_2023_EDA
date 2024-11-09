# Spotify_2023_EDA
## Table of Contents:
- [1. Importing Necessary Libraries](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#1-importing-necessary-libraries)
- [2. Loading Data Set](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#2-loading-data-set)
- [3. Overview of the Data Set](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#3-overview-of-the-data-set)
- [4. Data Descriptive Statistics](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#4-data-descriptive-statistics)
- [5. Top Performers](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#5-top-performers)
- [6. Temporal Trends](https://github.com/JamesRocero/Spotify_2023_EDA/blob/main/README.md#6-temporal-trends)
- [7. Genre and Music Characteristic](
- [8. Platform Popularity](
- [9. Advanced Analysis](

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

### 4. Data Descriptive Statistics

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

### 5. Top Performers

##### The top 5 most streamed tracks (highest number of streams)
- This code sorts the df_spotify dataset by the 'streams' column in descending order and then selects the 'track_name', 'artist(s)_name', and 'streams' columns. It displays the first 5 rows of the sorted data, showing the top tracks with the highest number of streams.
``` Python
# Sort the data and display the first 5 rows directly
df_spotify.sort_values(by='streams', ascending=False).loc[:, ['track_name', 'artist(s)_name', 'streams']].head()
```

![image](https://github.com/user-attachments/assets/b322bb90-5845-498f-8821-61c1a6e25163)

- This code splits the artist(s)_name column into a list of individual artist names by using a lambda function in combination with the apply() method. Instead of directly using str.split(), it applies a custom split operation on each value in the column, separating the names by commas. The result is a new column where each entry is a list of artist names.

``` Python
# Split the 'artist(s)_name' into a list without using 'str.split' directly
df_spotify['artist(s)_name'] = df_spotify['artist(s)_name'].apply(lambda x: x.split(','))
df_spotify['artist(s)_name']
```

![image](https://github.com/user-attachments/assets/6d5e974e-6106-42ed-888f-741826f53d38)

``` Python
# Explode the 'artist(s)_name' column and reset the index for a cleaner result
df_spotify = df_spotify.dropna(subset=['artist(s)_name']).reset_index(drop=True)
df_spotify['artist(s)_name'] = df_spotify['artist(s)_name'].apply(lambda x: x.strip() if isinstance(x, str) else x)
df_spotify = df_spotify.explode('artist(s)_name')
df_spotify
```

![image](https://github.com/user-attachments/assets/0eab49c2-5d8e-4c99-8b41-3ab3b452e85f)

##### This code above processes the df_spotify DataFrame by removing rows with missing artist names, cleaning up spaces, and then exploding the artist(s)_name column so each artist has its own row. The index is reset afterward for a cleaner structure.

- Rewrite the artist(s)_name attribute to simply artist_name.
``` Python
# Drop the 'artist_count' column and rename 'artist(s)_name' to 'artist_name'
df_spotify.drop(columns='artist_count', inplace=True)
df_spotify['artist_name'] = df_spotify.pop('artist(s)_name')
```

#### The top 5 most frequent artists based on the number of tracks in the data set
- Create a bar plot
``` Python
# Get the top 5 most frequent artists
top_artists = df_spotify.groupby('artist_name').size().nlargest(5)

# Create a bar plot for the top 5 artists
plt.figure(figsize=(10, 6))
sns.barplot(x=top_artists.index, y=top_artists.values, hue=top_artists.index, palette='viridis', legend=False)

# Add labels and title
plt.title('Top 5 Most Frequent Artists', fontsize=16)
plt.xlabel('Artist Name', fontsize=12)
plt.ylabel('Number of Tracks', fontsize=12)

# Rotate x-axis labels for better readability
plt.xticks(rotation=45, ha='right')

# Display the plot
print (top_artists)
plt.tight_layout()
plt.show()
```

![image](https://github.com/user-attachments/assets/d6a21b43-312d-4e47-afa7-4fb667a675d9)

### 6. Temporal Trends

#### Number of tracks released per year
``` Python
# Count the number of tracks released each year
year_counts = df_spotify['released_year'].value_counts().sort_index()

# Create a barplot instead of displot
sns.barplot(
    x=year_counts.index,
    y=year_counts.values,
    color='lightblue'
)

# Customize the plot labels and title
plt.xlabel('Release Year')
plt.ylabel('Number of Tracks Released')
plt.title('Number of Tracks Released Per Year')

# Rotate x-axis labels to prevent overlap and show every other year
plt.xticks(
    ticks=range(0, len(year_counts), 2),  # Show every other year
    labels=year_counts.index[::2],       # Display only every second year
    rotation=45                           # Rotate for better readability
)

# Display the plot
plt.show()
```

![image](https://github.com/user-attachments/assets/964777e4-14cf-402e-8a67-9d6a2e1f8a05)

##### The greatest amount of tracks were released between 2020 and 2022; this could have been caused by the impact of the pandemic.

#### Number of tracks released per month
``` Python
# Count the number of tracks released each month
month_counts = df_spotify['released_month'].value_counts().sort_index()

# Create a barplot instead of displot
sns.barplot(
    x=month_counts.index,
    y=month_counts.values,
    color='lightblue'
)

# Customize the plot labels and title
plt.xlabel('Months')
plt.ylabel('Number of Tracks Released')
plt.title('Number of Tracks Released Per Month')

# Set the x-axis labels to the month names
plt.xticks(
    ticks=range(12),
    labels=[calendar.month_name[i+1] for i in range(12)],
    rotation=45  # Rotate for better readability
)

# Adjust x-axis to cover all months
plt.xlim(-0.5, 11.5)

# Display the plot
plt.show()
```

![image](https://github.com/user-attachments/assets/6d28f9c6-7a4c-4cce-a8a6-00f68969dc9c)

##### January, May, and June saw the highest number of music released per month.
##### We may observe that the quantity of tracks released in a given year or month does not follow any trends.

### 7. Genre and Music Characteristic

#### Correlation between streams and musical attributes

``` Python
# Define musical attributes to focus on
musical_attr = ['streams', 'bpm', 'danceability_%', 'valence_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']

# Extract the data for these attributes into a new DataFrame
df_musical_attr = df_spotify[musical_attr]

# Compute correlation matrix for the musical attributes
corr_music_attr = df_musical_attr.corr()

# Use a clustermap to visualize the correlations with hierarchical clustering
sns.clustermap(corr_music_attr, annot=True, cmap='coolwarm', vmin=-1, vmax=1, figsize=(10, 8))

# Set the title for the cluster map
plt.suptitle("Cluster Map of Streams & Different Music Attributes", y=1.05)

# Show the plot
plt.show()
```

![image](https://github.com/user-attachments/assets/f074f6aa-a2bc-454d-972c-4adbc76bc53b)

##### Using cluster map, we can see the correlation between the streams and musical attributes

### 8. Platform Popularity

``` Python
# Selecting platform playlist columns
platform_playlists = ['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']

# Step 1: Ensure all values are numeric, convert if possible, and drop rows with any invalid values.
for column in platform_playlists:
    df_spotify[column] = pd.to_numeric(df_spotify[column], errors='coerce')

# Remove any rows with NaN values in the selected columns
df_spotify_cleaned = df_spotify.dropna(subset=platform_playlists)

# Step 2: Calculate the sum of each platform's playlist occurrences
playlist_sums = df_spotify_cleaned[platform_playlists].sum()

# Step 3: Create a DataFrame for plotting
playlist_counts = pd.DataFrame({
    'Platform': platform_playlists,
    'Total Occurrences': playlist_sums.values
})

# Step 4: Plotting
plt.figure(figsize=(8, 5))

# Use 'hue' for platform instead of 'palette' directly to fix the warning
sns.barplot(data=playlist_counts, x='Platform', y='Total Occurrences', hue='Platform', palette='Blues')

# Add labels and title
plt.title("Playlist Occurrences by Platform", fontsize=14)
plt.xlabel("Platform Playlist", fontsize=12)
plt.ylabel("Total Occurrences", fontsize=12)

# Annotate the bars with the total occurrences
for index, value in enumerate(playlist_counts['Total Occurrences']):
    plt.text(index, value + 0.05 * value, f'{int(value)}', ha='center', va='bottom', fontsize=10)

# Show the plot
plt.tight_layout()
plt.show()
```

![image](https://github.com/user-attachments/assets/33f30584-f188-4c51-b475-0b9ac7497a25)

##### We can see that the spotif platform seems to favor the most popular tracks.
##### It has 4,336,536 million total playlist occurences compared to the other playlist that only have 137,599 in dezzer playlists, and 73,420 on apple playlists

### 9. Advanced Analysis

#### Patterns between songs in the same key or mode (Major vs. Minor)
``` Python
# Count the number of tracks for each key and exclude 'Missing'
key_summary = df_spotify.groupby('key').size().reset_index(name='number_of_tracks')
key_summary = key_summary[key_summary['key'] != 'Missing']

# Sort by key alphabetically
key_summary = key_summary.sort_values('key')

key_summary
```
![image](https://github.com/user-attachments/assets/9627f063-665e-4ba3-b888-f55fc2f346a3)

- Create a bar plot based on the key summary
``` Python
# Count the number of tracks for each key and exclude 'Missing'
key_summary = df_spotify.groupby('key').size().reset_index(name='number_of_tracks')
key_summary = key_summary[key_summary['key'] != 'Missing']

# Plot with hue set to 'x' to avoid FutureWarning
plt.figure(figsize=(10, 6))
sns.barplot(x='key', y='number_of_tracks', data=key_summary, hue='key', palette='viridis', legend=False)

# Add values on top of each bar
for p in plt.gca().patches:
    plt.text(p.get_x() + p.get_width() / 2., p.get_height() + 0.1, str(int(p.get_height())), 
             ha='center', fontsize=12)

plt.title('Number of Tracks by Key')
plt.xlabel('Key')
plt.ylabel('Number of Tracks')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

![image](https://github.com/user-attachments/assets/9839f11d-d863-406a-a9d5-d9fc4daed4c0)

##### As we can see, the most frequently utilized tracks are in the keys C#, G, B, and both F and G#

#### Average streams for each mode
``` Python
# Calculate the average streams for each mode (Major vs Minor)
mode_streams = df_spotify.groupby('mode')['streams'].mean()

# Create a bar plot for the average streams of Major vs Minor tracks
plt.figure(figsize=(8, 6))
sns.barplot(x=mode_streams.index, y=mode_streams.values, hue=mode_streams.index, palette='pastel', legend=False)

# Add title and labels with exact values
plt.title('Average Streams for Major vs Minor Tracks', fontsize=16)
plt.xlabel('Mode', fontsize=12)
plt.ylabel('Average Streams', fontsize=12)

# Display the exact average stream values on the plot
for i, v in enumerate(mode_streams.values):
    plt.text(i, v + 1000000, f'{v:.0f}', ha='center', fontsize=12)

# Show the plot
plt.tight_layout()
plt.show()
```

![image](https://github.com/user-attachments/assets/47c3e1d7-c4e2-4ce4-bfc8-47d2841d97ed)

##### We can see above that Major tracks are greater than Minor tracks

#### Most frequent artists in different platform charts
- Make a bar plot
``` Python
# List of platform-related columns
platform_columns = ['in_spotify_charts', 'in_apple_charts', 'in_deezer_charts', 'in_shazam_charts']

# Ensure numeric columns for platforms and calculate total occurrences for each artist
df_spotify[platform_columns] = df_spotify[platform_columns].apply(pd.to_numeric, errors='coerce')
artist_chart_data = df_spotify.groupby('artist_name')[platform_columns].sum()
artist_chart_data['total_occurrences'] = artist_chart_data.sum(axis=1)

# Sort and select top artists based on total occurrences
top_artists_chart_data = artist_chart_data.sort_values('total_occurrences', ascending=False).head()

# Plotting
plt.figure(figsize=(10, 6))
sns.barplot(y='artist_name', x='total_occurrences', data=top_artists_chart_data, palette='viridis', hue='artist_name')

# Add labels and values
plt.title('Top Artists by Total Chart Occurrences', fontsize=16)
plt.xlabel('Total Chart Occurrences', fontsize=12)
plt.ylabel('Artist Name', fontsize=12)

# Display values on the bars
for i, value in enumerate(top_artists_chart_data['total_occurrences']):
    plt.text(value + 0.1, i, f'{value:.0f}', va='center', fontsize=12)

plt.tight_layout()
plt.show()
```
![image](https://github.com/user-attachments/assets/f9deadd8-3012-4957-a108-77d925320e72)

##### It is shown in the graph the artists that consistently appear in more playlists or charts.
##### The plot shown here is the Total Chart Occurrences from all the playlist or charts.
