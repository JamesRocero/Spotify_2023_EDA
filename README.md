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

track_name	artist(s)_name	artist_count	released_year	released_month	released_day	in_spotify_playlists	in_spotify_charts	streams	in_apple_playlists	...	bpm	key	mode	danceability_%	valence_%	energy_%	acousticness_%	instrumentalness_%	liveness_%	speechiness_%
0	Seven (feat. Latto) (Explicit Ver.)	Latto, Jung Kook	2	2023	7	14	553	147	141381703	43	...	125	B	Major	80	89	83	31	0	8	4
1	LALA	Myke Towers	1	2023	3	23	1474	48	133716286	48	...	92	C#	Major	71	61	74	7	0	10	4
2	vampire	Olivia Rodrigo	1	2023	6	30	1397	113	140003974	94	...	138	F	Major	51	32	53	17	0	31	6
3	Cruel Summer	Taylor Swift	1	2019	8	23	7858	100	800840817	116	...	170	A	Major	55	58	72	11	0	11	15
4	WHERE SHE GOES	Bad Bunny	1	2023	5	18	3133	50	303236322	84	...	144	A	Minor	65	23	80	14	63	11	6
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
948	My Mind & Me	Selena Gomez	1	2022	11	3	953	0	91473363	61	...	144	A	Major	60	24	39	57	0	8	3
949	Bigger Than The Whole Sky	Taylor Swift	1	2022	10	21	1180	0	121871870	4	...	166	F#	Major	42	7	24	83	1	12	6
950	A Veces (feat. Feid)	Feid, Paulo Londra	2	2022	11	3	573	0	73513683	2	...	92	C#	Major	80	81	67	4	0	8	6
951	En La De Ella	Feid, Sech, Jhayco	3	2022	10	20	1320	0	133895612	29	...	97	C#	Major	82	67	77	8	0	12	5
952	Alone	Burna Boy	1	2022	11	4	782	2	96007391	27	...	90	E	Minor	61	32	67	15	0	11	5
```
- After loading the data above, the df_spotify DataFrame is displayed, which will show the contents of the file.

