# A Data Analytics Approach to Understanding the Link Between Music and Country Happiness

*Link to the notebook:* [Python Notebook](music-happiness.ipynb)

### Abstract
By leveraging Python-based data analytics, this study endeavors to decipher the interplay between a country's happiness levels and their musical preferences. The study spans October 2023 to January 2024. It compares audio features such as 'danceability', 'duration' or 'positiveness' between countries with lower and higher Happiness Score.

### Datasets used
- **Music features:** The data for the audio features is a result of API calling the Top-50 playlist of every country available. The playlists are updated every week, and the dataset (Top-50-Oct-Jan.csv) covers from 2023W45 to 2024W05 (13 weeks). The code created for the Spotify API calling [here](https://github.com/Miquelc0ll/Spotify-World-Top50-Features/blob/main/README.md).
You can find all countries available in the following list (using the [ISO 3166-1 Alpha-3 code](https://www.iso.org/iso-3166-country-codes.html): 'DEU', 'SAU', 'ARG', 'AUS', 'AUT', 'BLR', 'BOL', 'BRA', 'BGR', 'BEL', 'CAN', 'CHL', 'COL', 'KOR', 'CRI', 'DNK', 'ARE', 'ECU', 'EGY', 'SLV', 'SVK' , 'ESP', 'USA', 'EST', 'PHL', 'FIN', 'FRA', 'GRC', 'GTM', 'HND', 'HKG', 'HUN', 'IND', 'IDN', 'IRL', 'ISL', 'ISR', 'ITA', 'JPN', 'KAZ', 'CZE', 'DOM', 'LVA', 'LTU','NLD', 'LUX', 'MYS', 'MAR', 'MEX', 'NIC', 'NGA', 'NOR', 'NZL', 'PAK', 'PAN', 'PRY', 'PER', 'POL', 'PRT', 'GBR', 'ROU', 'SGP', 'ZAF', 'SWE', 'CHE', 'THA', 'TWN', 'TUR', 'UKR', 'URY', 'VEN', 'VNM'.
  
- **Happiness:** The dataset used to study the Happiness of each country is the [Happiness Report 2023](https://worldhappiness.report/data/) (Happiness.csv). This yearly report assigns a Happiness Score for each country based on the following aspects: 'Logged GDP per capita',	'Social support',	'Healthy life expectancy',	'Freedom to make life choices',	'Generosity', 'Perceptions of corruption'.


#### Other datasets and documentations:
- [(countries_codes_and_coordinates.csv)](https://gist.github.com/tadast/8827699): This dataset from Guthub was helpflul to convert Country names to ISO 3166-1 Alpha-3.
- [Matplotlib documentation](https://matplotlib.org/stable/index.html)
- [NetworkX Graph Documentation](https://networkx.org/documentation/stable/reference/)
- [Pandas documentation](https://pandas.pydata.org/docs/user_guide/index.html)
