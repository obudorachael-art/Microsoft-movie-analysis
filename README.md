BUSINESS UNDERSTANDING

BUSINESS PROBLEM

Creating a new movie studio

Task:

Exploring what types of films are currently doing the best at the box office.
Objective(s)

To generate a guiding analysis to Microsoft concerning trending films.

This analysis will help the manager of the new movie studio at the Microsoft to make decision on which types of films to produce.

DATA UNDERSTANDING

Datasets that involve movie basics, movie ratings and movie gross have been used to provide basic information required to make decisions.

DATA ANALYSIS

The data collected is analised to show the most trending movie genres and their ratings

VISUALIZATION

Data cleaning and merging/joining


[ ]
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

[ ]
df = pd.read_csv('/content/imdb.title.basics.csv.gz')
df.head()


[ ]
df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 146144 entries, 0 to 146143
Data columns (total 6 columns):
 #   Column           Non-Null Count   Dtype  
---  ------           --------------   -----  
 0   tconst           146144 non-null  object 
 1   primary_title    146143 non-null  object 
 2   original_title   146122 non-null  object 
 3   start_year       146144 non-null  int64  
 4   runtime_minutes  114405 non-null  float64
 5   genres           140736 non-null  object 
dtypes: float64(1), int64(1), object(4)
memory usage: 6.7+ MB

[ ]
# checking for duplicates
df.duplicated().sum()
np.int64(0)

[ ]
# checking for missing data
df.isnull().sum()


[ ]
# dropping the missing columns

df.dropna(subset=['primary_title', 'original_title', 'runtime_minutes', 'genres'], inplace=True)
print(df)
           tconst                                      primary_title  \
0       tt0063540                                          Sunghursh   
1       tt0066787                    One Day Before the Rainy Season   
2       tt0069049                         The Other Side of the Wind   
4       tt0100275                           The Wandering Soap Opera   
5       tt0111414                                        A Thin Life   
...           ...                                                ...   
146134  tt9916160                                         Drømmeland   
146135  tt9916170                                      The Rehearsal   
146136  tt9916186  Illenau - die Geschichte einer ehemaligen Heil...   
146137  tt9916190                                          Safeguard   
146139  tt9916538                                Kuambil Lagi Hatiku   

                                           original_title  start_year  \
0                                               Sunghursh        2013   
1                                         Ashad Ka Ek Din        2019   
2                              The Other Side of the Wind        2018   
4                                   La Telenovela Errante        2017   
5                                             A Thin Life        2018   
...                                                   ...         ...   
146134                                         Drømmeland        2019   
146135                                           O Ensaio        2019   
146136  Illenau - die Geschichte einer ehemaligen Heil...        2017   
146137                                          Safeguard        2019   
146139                                Kuambil Lagi Hatiku        2019   

        runtime_minutes                genres  
0                 175.0    Action,Crime,Drama  
1                 114.0       Biography,Drama  
2                 122.0                 Drama  
4                  80.0  Comedy,Drama,Fantasy  
5                  75.0                Comedy  
...                 ...                   ...  
146134             72.0           Documentary  
146135             51.0                 Drama  
146136             84.0           Documentary  
146137             90.0        Drama,Thriller  
146139            123.0                 Drama  

[112232 rows x 6 columns]

[ ]
df1 = pd.read_csv('/content/imdb.title.ratings.csv.gz')
df1.head()


[ ]
df1.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 73856 entries, 0 to 73855
Data columns (total 3 columns):
 #   Column         Non-Null Count  Dtype  
---  ------         --------------  -----  
 0   tconst         73856 non-null  object 
 1   averagerating  73856 non-null  float64
 2   numvotes       73856 non-null  int64  
dtypes: float64(1), int64(1), object(1)
memory usage: 1.7+ MB

[ ]
# checking for duplicates
df1.duplicated().sum()
np.int64(0)

[ ]
# checking for missing data
df1.isnull().sum()


[ ]
df2 = pd.read_csv('/content/bom.movie_gross.csv.gz')
df2.head()


[ ]
df2.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3387 entries, 0 to 3386
Data columns (total 5 columns):
 #   Column          Non-Null Count  Dtype  
---  ------          --------------  -----  
 0   title           3387 non-null   object 
 1   studio          3382 non-null   object 
 2   domestic_gross  3359 non-null   float64
 3   foreign_gross   2037 non-null   object 
 4   year            3387 non-null   int64  
dtypes: float64(1), int64(1), object(3)
memory usage: 132.4+ KB

[ ]
# checking for duplicates
df2.duplicated().sum()
np.int64(0)

[ ]
# checking for missing data
df2.isnull().sum()


[ ]
# dropping the missing columns
df2.dropna(subset=['studio', 'domestic_gross', 'foreign_gross'], inplace=True)
print(df2)
                                                  title        studio  \
0                                           Toy Story 3            BV   
1                            Alice in Wonderland (2010)            BV   
2           Harry Potter and the Deathly Hallows Part 1            WB   
3                                             Inception            WB   
4                                   Shrek Forever After          P/DW   
...                                                 ...           ...   
3275                                    I Still See You           LGF   
3286                              The Catcher Was a Spy           IFC   
3309                                         Time Freak    Grindstone   
3342  Reign of Judges: Title of Liberty - Concept Short  Darin Southa   
3353            Antonio Lopez 1970: Sex Fashion & Disco            FM   

      domestic_gross foreign_gross  year  
0        415000000.0     652000000  2010  
1        334200000.0     691300000  2010  
2        296000000.0     664300000  2010  
3        292600000.0     535700000  2010  
4        238700000.0     513900000  2010  
...              ...           ...   ...  
3275          1400.0       1500000  2018  
3286        725000.0        229000  2018  
3309         10000.0        256000  2018  
3342         93200.0          5200  2018  
3353         43200.0         30000  2018  

[2007 rows x 5 columns]

[ ]
# merging the datasets
df3 = pd.merge(df, df1, on='tconst')
df3.head()


[ ]
df3.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 65720 entries, 0 to 65719
Data columns (total 8 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   tconst           65720 non-null  object 
 1   primary_title    65720 non-null  object 
 2   original_title   65720 non-null  object 
 3   start_year       65720 non-null  int64  
 4   runtime_minutes  65720 non-null  float64
 5   genres           65720 non-null  object 
 6   averagerating    65720 non-null  float64
 7   numvotes         65720 non-null  int64  
dtypes: float64(2), int64(2), object(4)
memory usage: 4.0+ MB

[ ]
df3.describe()


[ ]
# plotting histogram
plt.hist(df3['averagerating'], bins=10, color='blue', edgecolor='black')
plt.xlabel('Average Rating')
plt.ylabel('Frequency')
plt.title('Histogram of Average Ratings')
plt.show()


[ ]
# plotting a scatter diagram
plt.scatter(df3['averagerating'], df3['numvotes'], color='green')
plt.xlabel('Average Rating')
plt.ylabel('Number of Votes')
plt.title('Scatter Plot of Average Ratings vs. Number of Votes')
plt.show()


[ ]
# sort by most rated genre in descnding order and take top 10

most_rated_genres = df3['genres'].value_counts().head(10)
print(most_rated_genres)
genres
Drama          28394
Documentary    16423
Comedy         15514
Thriller        7583
Horror          6917
Action          6297
Romance         5976
Crime           4338
Biography       3693
Adventure       3621
Name: count, dtype: int64

[ ]
# plotting a bar chart of most rated genres

plt.bar(most_rated_genres.index, most_rated_genres.values)
plt.xlabel('Genre')
plt.ylabel('Number of Movies')
plt.title('Most Rated Genres')
plt.xticks(rotation=45)
plt.show()

