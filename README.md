# A Data Analytics Approach to Understanding the Link Between Music and Country Happiness

*Link to the notebook:* [Python Notebook](music-happiness.ipynb) (Download or open in Github.dev to see the full notebook).

## Introduction

### Abstract
By leveraging Python-based data analytics, this study endeavors to **decipher the interplay between a country's happiness levels and their musical preferences**. The study spans October 2023 to January 2024. It compares audio features such as 'danceability', 'duration' or 'positiveness' between countries with lower and higher Happiness Score.

### Datasets used
- **Music features:** The data for the audio features is a result of API calling the Top-50 playlist of every country available. The playlists are updated every week, and the dataset (Top-50-Oct-Jan.csv) covers from 2023W45 to 2024W05 (13 weeks). The code created for the Spotify API calling [here](https://github.com/Miquelc0ll/Spotify-World-Top50-Features/blob/main/README.md).
You can find all countries available in the following list (using the [ISO 3166-1 Alpha-3 code](https://www.iso.org/iso-3166-country-codes.html): 'DEU', 'SAU', 'ARG', 'AUS', 'AUT', 'BLR', 'BOL', 'BRA', 'BGR', 'BEL', 'CAN', 'CHL', 'COL', 'KOR', 'CRI', 'DNK', 'ARE', 'ECU', 'EGY', 'SLV', 'SVK' , 'ESP', 'USA', 'EST', 'PHL', 'FIN', 'FRA', 'GRC', 'GTM', 'HND', 'HKG', 'HUN', 'IND', 'IDN', 'IRL', 'ISL', 'ISR', 'ITA', 'JPN', 'KAZ', 'CZE', 'DOM', 'LVA', 'LTU','NLD', 'LUX', 'MYS', 'MAR', 'MEX', 'NIC', 'NGA', 'NOR', 'NZL', 'PAK', 'PAN', 'PRY', 'PER', 'POL', 'PRT', 'GBR', 'ROU', 'SGP', 'ZAF', 'SWE', 'CHE', 'THA', 'TWN', 'TUR', 'UKR', 'URY', 'VEN', 'VNM'.
  
- **Happiness:** The dataset used to study the Happiness of each country is the [Happiness Report 2023](https://worldhappiness.report/data/) (Happiness.csv). This yearly report assigns a Happiness Score for each country based on the following aspects: 'Logged GDP per capita',	'Social support',	'Healthy life expectancy',	'Freedom to make life choices',	'Generosity', 'Perceptions of corruption'.


#### Other datasets and documentations:
- [(countries_codes_and_coordinates.csv)](https://gist.github.com/tadast/8827699): This dataset from the Githubuser *tadast* was helpflul to convert Country names to ISO 3166-1 Alpha-3.
- [Matplotlib documentation](https://matplotlib.org/stable/index.html)
- [NetworkX Graph Documentation](https://networkx.org/documentation/stable/reference/)
- [Pandas documentation](https://pandas.pydata.org/docs/user_guide/index.html)




## Preprocessing
### Preparing the data
To start with the project, I had to prepare the data. This is the first step into a good analysis. As mentioned before, I had 2 main datasets: *df_aufe* (for audio features and songs) and *df_hpy* (the Happiness Report). Both dataframe had to be joined into a bigger one, called *df_proj*, but before merging them I had to prepare the available data.

The *df_hpy* dataset had a column named 'Country name', which was not in ISO 3166-1 Alpha-3 format. Therefore, I used the countries_codes_and_coordinates.csv as *df_coun* to match each country in *df_hpy* to his own code. Then I had to do some manual revision to look if there was any error and solve it. For example, the country codes all had a space before, which could cause me some problems when joining. Also, there were some countries in different nomenclature in both datasets (f.e. 'North Macedonia' vs. 'NorthMacedonia') which I had to map manually in a dictionary to change to country code.


### Creating the final dataset
After all those first steps, I chose the columns I wanted in the final dataset from each of the two dfs. From *df_aufe*: 'Country', 'Track Name', 'Artist Name', 'Popularity', 'Danceability', 'Acousticness', 'duration', 'Energy', 'Key', 'Liveness', 'Loudness', 'Mode', 'Speechiness', 'Tempo', 'TSignature', 'Positiveness'. From *df_hpy*: 'Country name', 'Ladder score', 'Logged GDP per capita', 'Healthy life expectancy'. The final dataset *df_proj* was built with pd.merge() on left. 

I changed some column names to make them easier to call and understand (f.e. 'Logged GDP per capita' : 'GDP', 'Healthy life expectancy' : 'Lifexp') and assigned the dtype() to each one. There were audio feature columns that were numeric values but categorical columns, so I had to be careful with that. With that, the part of creating the dataset was finished.

### Removing outliers
Once I had the ready-to-use dataset, I had to make sure that my data points were relevant and useful, so I checked if there was any outlier. The first step I took onto finding outliers was to plot a box for each numerical column and describe the df to observe if there was anything strange.

![boxplot-Lifexp](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/5c90ab79-8930-4039-94d1-bd09dbfa98d4)

As we see in the boxplots, there were some outlier values in each column. This values would have biased our information because of its weight in calculating the mean or other statistics. For this reason, these were replaced by the mean of the country in that column. It was only applied to music features, because the columns from the Happiness Report were already reviewed and official.

#### Outlier finding method: IQR
To determine which values are outliers we are using the 1.5 * IQR method. This method says that a data point is an outlier if it is more than 1.5 ⋅ IQR ‍ above the q3 or below the q1. For example: Let's say we have an outlier in the positiveness column from a song of the Spanish chart. It will be replaced by the positiveness mean of Spanish songs.




## Analysis and data visualisations
To start the analysis part it is important to keep in mind the purpose of the project: decipher the interplay between a country's happiness levels and their musical preferences. To start warming-up with the final *df_proj*, I plotted the **top-10 countries for every numerical value**. I did it with the purpose of having an easy way to check if I get some surprisingly high value for any country in any column.

![rankings1](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/8184150a-197a-4e7e-9c66-f0717c35f605)

### Relations between audio features and happiness
To answer the question of the project, the next organic step was to create scatterplots to relate every audio feature to the happiness score of each country. With these graphs, we could start to see which kind of music do listen for every value of happiness, and see if there's any relation between the values.

![scatters](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/fcc81c04-8561-4cd2-9b76-8adfa7e3ac8f)

As can be seen in the above figure, I could not find any clear relation between any feature and the happiness value. The most uniform scatterplot from the figure is the Happiness / Tempo, followed by the Happiness / Mode and the popularity. 

However, it is quite difficult to state this just by looking at the graph. I created a correlation matrix (pd.df.corr()) and plotted it to find correlations in a mathematical way. All columns and rows are good for big-picture understanding, but the one column/row that we want is the one involving Happiness.

*Crazy value: The correlation between Danceability and GDP is -0.52. Rich countries listen to way less danceable music.*

![cov](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/15de428b-f36c-440c-be0f-998b5b23f7e2)

In this correlation plot we start to see some valuable information. From the 13 audio features we have, **just 4 audio features have a positive correlation with Happiness**: Tempo (0.31), Mode (0.27), Popularity (0.12) and Liveness (0.02). This means that happier countries tend to listen to music with more beats per minute (BPM), in major scale and more mainstream. The average Tempo in happy countries is 122.77. 120 is considered by most producers as the perfect BPM to craft a hit and songs like Dynamite (BTS), Blurred Lines (Robin Thicke ft. T.I. and Pharrell) or Bad Romance (Lady Gaga) were produced with 120 BPM. Meanwhile, in countries with lower happiness, the average mean is 112.05; songs like Eye of the Tiger (Survivor) or Another One Bites the Dust (Queen) were built under a 110 BPM structure. Average popularity in happier countries is 83.99, and in lower happpiness countries it is 72.65. Popularity is a value authomatically displayed by spotify to qualify the importance of a song based on reproductions, so we can state that songs in happier countries are more in the mainstream.

Happiness also does have **negative correlations with 9 audio features**: T-Signature (-0.38), Speechiness (-0.3), Danceability (-0.27), Duration (-0.22), Key (-0.22), Energy (-0.11), Positiveness (-0.11), Acousticness (-0.07), Loudness (-0.04). Songs in happier countries are mostly composed in 3/4 or 4/4 time signature, and in countries with lower happiness the T Signature ranges from 4/4 to 7/4. For example, the song Money from Pinkfloyd has a 7/4 Time Signature. Speechiness determines songs in happier countries have less spoken words. Following with the next negative correlation, the average danceability in happier countries is 0.63 (Baddadan - Chase & Status, Bou, Flowdan, IRAH, Trigga, Takura) vs. the 0.71 (TQG - Karol G ft. Shakira) in countries with lower happiness. The average song duration in happier countries is 3:15 (Quevedo: Bzrp Music Sessions, Vol. 52 - 3:18), compared to the 3:26 ((Plastic Hearts, Miley Cyrus - 3:26) from the lower happiness countries. The negative correlation of happiness with Energy and positiveness is quite surprising, as we may think of more positive songs listened in happier countries. The correlation with acousticness and loudness is so small we can ignore them.

### How do countries relate with themselves?
Once we know the correlation values between audio features and the happiness score, it's time to find groups of countries to see how they relate between them. To achieve this, we plotted a graph using the NetworkX python library. By definition, graphs are networks consisting of nodes connected by edges or arcs. In this case, countries will be the source and tracks will be the target, so both countries and songs will be nodes. The edges will relate countries based on the songs they share. 
*Example: If Norway's Top-50 playlist has 'Song A' and 'Song B' in it and they also are in Italy's Top-50, both Norway and Italy will have a connection and will be closer on the map.*

All the green small nodes are the songs, and the coloured nodes are the countries labelled with the ISO 3166-1 Alpha-3 code. The color of the country is based on the Happiness Score, so we can see if happiness plays a big role when sharing music between countries.
*Note: Turkey is not on the map because it is not related with any country, and the black countries do not have Happiness data.*

![Happiness Score](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/0d629a56-966c-4529-9736-bcc999498ba4)

#### Finding communities in the graph
The graph is quite clear visually, but may not be optimal to find conclusions. There are some countries close to their neighbours and others close to countries with same language / religion. To find the real communities of the graph I used the *greedy_modularity_communities* function from NetworkX. I got 20 different communities, but 12 of them were lonely groups with just one country, so I ignored them and focused on communities with at least 2 countries. Finally, the communities from the graph were the following:
- 1: 'DEU', 'SAU', 'AUT', 'AUS', 'BEL', 'CAN', 'ARE', 'SVK', 'USA', 'EST', 'PHL', 'IDN', 'IRL', 'LVA', 'LTU', 'LUX', 'MYS', 'NZL', 'GBR', 'CHE', 'SGP'
- 2: 'ARG', 'CHL', 'COL', 'CRI', 'ECU', 'SLV', 'ESP', 'GTM', 'HND', 'DOM', 'MEX', 'NIC', 'PAN', 'PRY', 'PER', 'URY', 'VEN', 'BOL'
- 3: 'KOR', 'HKG', 'JPN', 'THA', 'TWN', 'VNM'
- 4: 'BLR', 'KAZ', 'UKR'
- 5: 'NOR', 'SWE'
- 6: 'NGA', 'ZAF'
- 7: 'BRA', 'PRT'
- 8: 'IND', 'PAK'

Let's see them on the map (plotted with geopandas library and geopandas dataset 'naturalearth_lowres'):

![world-map-for-communities](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/4cf55d90-d5cd-482d-9c63-659956590f93)

When I saw the communities I thought they were quite logical: grouped by location, but mainly by language. The english-speaking countries form the first and larger community, including North-America, Center Europe and Oceania. The second largest group is the spanish-speaking, covering Spain and Center + South America except Brazil. The third largest community is the asian one, covering Japan, Thailand, Taiwan, South Korea, Hong Kong and Vietnam. The fourth is the last with more than 2 countries: Belarus, Kazakhstan and Ukraine; because there is only left Norway - Sweden, Nigeria - South Africa, Brazil - Portugal and India - Pakistan.

I plotted the mean happiness score of every community to find the happiest and the least happier communities:

![mean-happiness-community](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/3cea49c4-9979-4e9e-8bb7-e6bddfdbe9b1)

Sorting the communities by the mean happiness score we get the following ranking:
- 5 (7.35): 'NOR', 'SWE'
- 1 (6.63): 'DEU', 'SAU', 'AUT', 'AUS', 'BEL', 'CAN', 'ARE', 'SVK', 'USA', 'EST', 'PHL', 'IDN', 'IRL', 'LVA', 'LTU', 'LUX', 'MYS', 'NZL', 'GBR', 'CHE', 'SGP'
- 7 (6.04): 'BRA', 'PRT'
- 2 (6.00): 'ARG', 'CHL', 'COL', 'CRI', 'ECU', 'SLV', 'ESP', 'GTM', 'HND', 'DOM', 'MEX', 'NIC', 'PAN', 'PRY', 'PER', 'URY', 'VEN', 'BOL'
- 3 (5.80): 'KOR', 'HKG', 'JPN', 'THA', 'TWN', 'VNM'
- 4 (5.60): 'BLR', 'KAZ', 'UKR'
- 6 (5.12): 'NGA', 'ZAF'
- 8 (4.29): 'IND', 'PAK'

#### Comparing the 2 highest and lowest commmunities based on Happiness Score
To end the project, I decided to get the 2 communities with higher Happiness value and the 2 communities with lower Happiness value and compare them. These communities were 1 and 5 (High Happiness) and 6 and 8 (Low Happiness). In the following bar plot, the blue bars correspond to the Higher Happiness Score, and the Red bars to the lower. All values are scaled to fit correctly in the plot.

![comparison-highest-lowest-happiness-communities](https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/20b45a6c-5d9a-40e4-8144-fdfdf0696745)




## Conclusions
There are some remarkable differences between the 2 groups. The values found in the plot confirm what was stated before with the correlation matrix. The higher happiness community track boast a higher popularity score, while the lower happiness group excels in danceability by 8 points. Acousticness shows no apparent discrepancy. However, the lower happiness tracks tend to be slightly longer. Energy levels remain consistent across both groups. Notably, the higher happiness community tunes exhibit greater liveness, contrasting with the lower happiness group's louder compositions. Additionally, the lower happiness leans towards more speechiness. Tempo favors the higher happiness community by 5 points, while the lower happiness carries a 5-point advantage in positiveness. These nuances delineate distinct musical experiences between the two categories.

The relations and groups between countries do not depend on happiness, they depend on the language spoken and proximity. The happiness differences among communities are a consequence of these groups.




## Appendix
### Did you know that? The ranking
The original *Top-50-Oct-Jan.csv* file has data for 43,720 different songs. I wanted to add an appendix for the top songs in each feature.

#### The song with the highest danceability
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/c1b6d707-7018-4752-9657-5b9ebe75a05d" alt="Image Description" width="150">

Нефертити - Ицык Цыпер (Nefertiti - Itsyk Tsyper)

This song's danceability is 0.988. It was published the 13/10/2023 and has been in the Top-50 in weeks 2024W03 (Belarus and Ukraine) and 2024W04 (Belarus, Kazakhstan, Ukraine).

#### The song with the highest acousticness
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/c8722803-3610-4575-88f8-21652cfde925" alt="Captura" width="150px">

Alive - Krolly Music

This song's acousticness is 0.996. It was published the 15/06/2023 and was in the Bulgarian's Top-50 the 46th week of 2023.

#### The longest song
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/0d741000-c5b8-497e-8063-98d755ccc246" alt="ab67616d00001e02113bb5ff902822d6fc3db0e7" width="150px">

Poesia Acústica 15 - Pineapple StormTv, Salve Malak, Mc Poze do Rodo, Luiz Lins, MC Hariel, Azzy, JayA Luuck, Oruam, Slipmami, MC Cabelinho

This song's duration is 11:20. It was published the 24/12/2023 and has been in Portugal's Top-50 since then (2024 weeks 1, 2, 3 and 4).

#### The most energetic song
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/90a2bd7b-b42c-406e-a454-63a11bb5a9a2" alt="image" width="150px">

ياحلوه يا مظلومه      ابو سراج  (Mazlouh - Abu Siraj)

This song's energy is 0.992. It was published the 17/05/2010 and has been in Saudi Arabia's Top-50 in weeks 45, 47, 48, 51 and 52 in 2023; and weeks 2, 3 and 4 in 2024.

#### The song with the most speechiness
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/261bbc61-0ab9-4a94-8a26-23c29d1c2d2f" alt="artworks-QDajL9iEdBW6-0-t500x500" width="150px">

CODE BARRE - Lacrim

This song's speechiness is 0.811. It was uploaded the 8/11/2023 and has been in the French Top-50 in weeks 46 and 47 in 2023.

#### The song with the highest tempo
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/233f4c87-a68f-4c7b-badc-dd7b70ccf277" alt="ab67616d0000b273a0369b863f523411e35d817b" width="150px">

Rhythm & Blues - Ayra Starr

This song's tempo is 218. It was published the 12/09/2023 and was in the Nigeria's Top-50 in weeks 45 and 46 in 2023.

#### The most positive song
<img src="https://github.com/Miquelc0ll/music-happiness-project/assets/84017268/f1545d22-62ef-47f2-ade3-3c68cb5916f7" alt="26905adfd940a98412552c37a6ab57c4fda17d09" width="150px">

Enfermera - Los Hermanos Flores

This song's positiveness is 0.983. It was published the 9/5/1995 and entered the El Salvador's Top-50 the first week of 2024.
