# Climate-Analysis

## Overview 
This project represents a series of data queries and their results, written in Python 3 using Pandas and SQLAlchemy in order to selectively filter and subsequently analyze particular data from within a .sqlite database of climatic measurements.    

### Purpose
The purpose of the analyses performed in this project was asses the feasibility of a 12-month retail business cycle based on historical weather patterns recorded in the area, specifically investigating the months of June and December.  By querying and summarizing the data trends for these months when temperatures are presumed to be most extreme, conclusions can be reached regarding the question of whether it would be advisable for a business reliant heavily on outdoor foot traffic to be kept open during all seasons.    

## Results
To produce the aforementioned results, it was necessary to first establish a connection to the SQLite databse containing the relevant data.  This was accomplished by first using the "create_engine()" function to prepare the database for access, then reflecting the database and its tables into the Python enviroment using the "automap_base()" and "prepare()" functions, respectively.  With the SQLite data made accessible, the "Base.classes" function was used to define two relevant key variables ("Measurement" and "Station") so that they could be called directly for analysis.

```
engine = create_engine("sqlite:///hawaii.sqlite")

# reflect an existing database into a new model
Base = automap_base()
# reflect the tables
Base.prepare(engine, reflect=True)

# Save references to each table
Measurement = Base.classes.measurement
Station = Base.classes.station
```

The results of these analyses and their interpretations are as follow:

* Temperatures for June

After compiling all temperature readings gathered during June into a dataframe, these data were statistically summarized with the following results:

![JUNE_SUMMARY](https://github.com/AC-Melamed/Climate-Analysis/blob/main/June_summary.png)


From these statistics, it appears that the typical climatic conditions in this region during June are neither comparatively extreme nor intrinsically diverse.  That is to say, the small standard deviation combined with the relatively mild upper and lower quartiles indicates that, even during the middle of the summer, this location experiences comfortable temperatures with few exceptions.   

* Temperatures for December

After compiling all temperature readings gathered during December into a dataframe, these data were statistically summarized with the following results:

![DEC_SUMMARY](https://github.com/AC-Melamed/Climate-Analysis/blob/main/Dec_summary.png)


The values in these statistics are evidently similar to those produced for June, despite being gathered during the middle of the winter as opposed to the middle of the summer.  Even the absolute minimum value for December remains well within tolerable outdoor conditions.  This all suggests that winter in this location remains amenable to customer foot traffic.

* Overall Temperature Trends

With nearly equivalent standard deviations and reasonably similar sample sizes between the two dataframes, the mean temperature for December is only 4 degrees colder than that of June -- a difference just slightly more than the average of the two months' standard deviations.  From this comparison it can be reasoned that there is, overall, very little difference in temperature between seasons at this location.      

## Summary
The analyses performed as part of this project clearly indicate that the prospective 12-month business plan being investigated would not be compromised by inclement temperatures during either the winter nor the summer.  While there are certainly other climatic factors which might complicate this conclusion, such as cloud coverage or rainfall, these lie outside of the data provided and are therefor not pertinent to the analysis at hand.

To supplement these conclusions, two possible additional queries are proposed below:

### Additional Query 1
The most obviously salient additional clamtic variable already accessible in the available data is the precipitation measurement for each month.  By quering the database to return this data, a highly relevant second vector for analysis could be opened.  To do so, only minor modifications would need to be made to the existing code for quering the temperature data for each month:

```
prcpJune = session.query(Measurement.date, Measurement.prcp).\
    filter(extract("month", Measurement.date) == 6).all()
prcpJune
```

### Additional Query 2
A second, less obvious and more tangential query could be performed to investigate the actual weather stations which originally generated the data being analyzed.  There is a possibility in this case, as in any empirical study conducted across multiple laboratories, that one or more of the stations was producing faulty observations during the months in question.  Although the statistical analysis already performed does not suggest the presence of significant outliers, other evidence of faulty observations might reveal themselves if the data was reindexed according to weather station -- implausibly repetitious values, for example.  Like the previous suggested query, this would require only a minor alteration to existing code:

```
stationJune = session.query(Measurement.date, Measurement.station, Measurement.tobs).\
    filter(extract("month", Measurement.date) == 6).all()
stationJune
```
