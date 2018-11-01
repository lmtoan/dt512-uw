# Bias in Data

Name: Toan Luong

Date: October 31, 2018

As part of the [Assignment 2: Bias in data](https://wiki.communitydata.cc/Human_Centered_Data_Science_(Fall_2018)/Assignments#A2:_Bias_in_data). The template was sourced from https://github.com/Ironholds/data-512-a2.

## Goal
The goal of this project is to examine bias in Wikipedia articles related to politicians in a number of countries. The [ORES (Objective Revision Evaluation Service) Machine Learning API](https://www.mediawiki.org/wiki/ORES) is utilized to determine the quality of each article, where applicable.

2 primary metrics are explored:
1. The coverage of Wikipedia articles about politicians for each country as a proportion of population
2. The proportion of high-quality articles about politicians for each country

## Data Sources

There are 2 primary data sources. They are located in `orig/` folder.

1. [Wikipedia Dataset](orig/page_data.csv)

Citation: (Keyes, Oliver (2017): Politicians by Country from the English-language Wikipedia. figshare. Fileset.)[https://figshare.com/articles/Untitled_Item/5513449]. Code and data are licensed under the CC-BY-SA 4.0 license. 

For each politician of each country, the Wikipedia article `revision_id` is provided in the dataset.

| Column  | Description                                         |
|---------|-----------------------------------------------------|
| page    | Wikipedia article tittle related to the politicians |
| country | Country                                             |
| rev_id  | Revision ID                                         |

2. [Population Dataset](orig/WPDS_2018_data.csv)

Sourced from the [2018 World Population Data Sheet](https://www.prb.org/2018-world-population-data-sheet-with-focus-on-changing-age-structures/), the dataset contains the population (in millions) for 207 countries in the world. The data is licensed by Population Reference Bureau (PRB).

| Column                         | Description                                       |
|--------------------------------|---------------------------------------------------|
| Geography                      | Country                                           |
| Population mid-2018 (millions) | Population of that country in mid-2018 (millions) |


## Reflection

#### Final DataFrame (`final_data.csv`)

| Column            | Description                                        |
|-------------------|----------------------------------------------------|
| revision_id       | Revision ID (unique identifier)                    |
| article_name      | Article Name                                       |
| country           | Country                                            |
| population        | Population in million                              |
| article_quality   | Article quality from ORES                          |
| probability_B     | Probability that the article is a B-class          |
| probability_C     | Probability that the article is a C-class          |
| probability_FA    | Probability that the article is a Featured Article |
| probability_GA    | Probability that the article is a Good Article     |
| probability_Start | Probability that the article is a Start-class      |
| probability_Start | Probability that the article is a Stub-class       |

For the ORES ratings, the following information are quoted from https://en.wikipedia.org/wiki/Wikipedia:Content_assessment#Grades. Ratings are from Good to Bad.

| FA    | "The article has attained featured article status by passing an in-depth examination by impartial reviewers"                             |
|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| A     | "The article is well organized and essentially complete, having been examined by impartial reviewers from a WikiProject or elsewhere."   |
| GA    | "The article has attained good article status having been examined by one or more impartial reviewers from WP:Good article nominations." |
| B     | "The article is mostly complete and without major problems, but requires some further work"                                              |
| C     | "The article is substantial, but is still missing important content or contains much irrelevant material."                               |
| Start | "An article that is developing, but which is quite incomplete. It might or might not cite adequate reliable sources."                    |
| Stub  | "A very basic description of the topic. However, all very-bad-quality articles will fall into this category."                            |

#### Metric 1: proportion of politician articles-per-population for each country

From `final_data.csv`, the top 10 and bottom 10 countries with regards to that metric are shown in the following table.

For this metric, I expected a large coverage bias toward developed countries (US, UK, Europe) because of citizens' active political involvements. The data is sourced exclusively for English-language Wikipedia articles, thus the result will be biased toward English-speaking countries and not accurate for other countries.

The result came to my suprise and reflected several biases of the Wikipedia dataset. For sparsely-populated countries like Tuvalu and Naura, the proportion of number of Wikipedia articles over a small-range population can significantly skew those of large-range population. Most Western countries did not make the top 10 list. It seems like South-East Asia countries has the most number of articles dedicated to politicians.


*Top 10*

| country                        | population | num_article | articles_per_population_perc |
|--------------------------------|------------|-------------|------------------------------|
| tuvalu                         | 0.01       | 55          | 0.550000                     |
| nauru                          | 0.01       | 53          | 0.530000                     |
| san marino                     | 0.03       | 82          | 0.273333                     |
| monaco                         | 0.04       | 40          | 0.100000                     |
| liechtenstein                  | 0.04       | 29          | 0.072500                     |
| tonga                          | 0.10       | 63          | 0.063000                     |
| marshall islands               | 0.06       | 37          | 0.061667                     |
| iceland                        | 0.40       | 206         | 0.051500                     |
| andorra                        | 0.08       | 34          | 0.042500                     |
| federated states of micronesia | 0.10       | 38          | 0.038000                     |

*Bottom 10*

| country      | population | num_article | articles_per_population_perc |
|--------------|------------|-------------|------------------------------|
| india        | 1371.3     | 986         | 0.000072                     |
| indonesia    | 265.2      | 214         | 0.000081                     |
| china        | 1393.8     | 1135        | 0.000081                     |
| uzbekistan   | 32.9       | 29          | 0.000088                     |
| ethiopia     | 107.5      | 105         | 0.000098                     |
| zambia       | 17.7       | 25          | 0.000141                     |
| korea, north | 25.6       | 39          | 0.000152                     |
| thailand     | 66.2       | 112         | 0.000169                     |
| bangladesh   | 166.4      | 323         | 0.000194                     |
| mozambique   | 30.5       | 60          | 0.000197                     |

#### Metric 2: proportion of high-quality articles about politicians for each country

From `final_data.csv`, the top 10 and bottom 10 countries with regards to that metric are shown in the following table.

For this metric, the biases expectations toward English-speaking and developed countries as discussed in metric 1 are also present here. In addition, metric 2 involves a Machine Learning service to rate article based on past Wikipedia history. The edit quality ratings are subjective to Wikiproject reviewers. As mentioned in ORES documentation, curation effort is labor-intensive and not updated regularly. Issues such as spam, vandalism, and attack articles might not be fully accounted.

The top 10 result has North Korea and Saudi Arabia at the top. I am very curious of how those "high quality" Wikipedia articles are managed among contributors both internal and external. Bottom 10 countries, as expected, are developing countries and not English native. Such factors contribute to the low number of high quality articles.

*Top 10*

| country                  | num_high_quality_article | num_article | high_quality_perc |
|--------------------------|--------------------------|-------------|-------------------|
| korea, north             | 7                        | 39          | 17.948718         |
| saudi arabia             | 16                       | 119         | 13.445378         |
| central african republic | 8                        | 68          | 11.764706         |
| romania                  | 40                       | 348         | 11.494253         |
| mauritania               | 5                        | 52          | 9.615385          |
| bhutan                   | 3                        | 33          | 9.090909          |
| tuvalu                   | 5                        | 55          | 9.090909          |
| dominica                 | 1                        | 12          | 8.333333          |
| united states            | 82                       | 1092        | 7.509158          |
| benin                    | 7                        | 94          | 7.446809          |

*Bottom 10*

| country      | num_high_quality_article | num_article | high_quality_perc |
|--------------|--------------------------|-------------|-------------------|
| tanzania     | 1                        | 408         | 0.245098          |
| peru         | 1                        | 354         | 0.282486          |
| lithuania    | 1                        | 248         | 0.403226          |
| nigeria      | 3                        | 682         | 0.439883          |
| morocco      | 1                        | 208         | 0.480769          |
| fiji         | 1                        | 199         | 0.502513          |
| bolivia      | 1                        | 187         | 0.534759          |
| brazil       | 3                        | 551         | 0.544465          |
| luxembourg   | 1                        | 180         | 0.555556          |
| sierra leone | 1                        | 166         | 0.602410          |


## Files


## Resources
This analysis was prepared using Python 3.5 running in a Jupyter Notebook environment.  
Documentation for Python can be found here: https://docs.python.org/3.5/  
Documentation for Jupyter Notebook can be found here: http://jupyter-notebook.readthedocs.io/en/latest/  
Packages
- Pandas
- Numpy
- [ORES (Objective Revision Evaluation Service) Machine Learning API](https://www.mediawiki.org/wiki/ORES)


## License

This assignment code is released under [MIT License](LICENSE). Data sources and copyrights are acknowledged in previous sections.
