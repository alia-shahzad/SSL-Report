# HW06: An Analysis of the CPD's Heat List
Alia Shahzad

### What is this?

This repository contains publically available data from the Strategic Subject List (SSL), street name the "Heat List," which was retired in 2019. During its time, the Chicago Police Department (CPD) used an algorithm to assess the risk of a given individual becoming party to violence. The CPD used SSL data to anticipate and intervene in high risk individuals' lives before potential violence occured. This data set contains risk scores for every Chicagoan with an arrest history in the time period of 2012-2016; the data has been de-identified for the privacy of its subjects.

This project aims to make sense of this data, and particularly assess the claim by the CPD that the SSL algorithm did not use race as a factor when assigning risk scores. To that end, it also uses 2012-2016 census tract data to calculate the percentage of black men in the city of Chicago who were assigned SSL scores. 

### What can I find here?

The [raw SSL data](Strategic_Subject_List.csv) in CSV format].

The [clean SSL data](ssl.csv) in CSV format.

An Rmd with my [*preliminary* analysis of the SSL data](ssl.Rmd). This has some extra code, as well as what went on in my head while I was choosing data visualizations for the graphs I chose. You probably don't need to read this if you only care about looking at the data visualizations and reading polished analysis.

An Rmd with my [*final and complete* analysis of the SSL data](ssl_report.Rmd). This has all the important data visualizations and polished analysis but is missing some of the code I used to get there.

An [md](ssl.md) with my preliminary analysis of the SSL data.

An [md](ssl_report.md)  with my final analysis of the SSL data.

### What other packages do I need to execute these files?

You'll need RSocrata and forcats to execute these files.

Check out my [preliminary analysis](ssl.Rmd) of the SSL data to see an example of how I downloaded these packages.

### Where did this data come from?

The data in this repo was loaded in an R-friendly csv format created by the good folks of [RSocrata](https://github.com/Chicago/RSocrata/blob/master/README.md), a group of data scientists employed by the City of Chicago.

SSL data is also available in other formats on the the [City of Chicago's Data portal](https://data.cityofchicago.org/Public-Safety/Strategic-Subject-List/4aki-r3np).

Thanks to the journalists from the Sun-Times and the Invisible Institute, the ACLU, as well as the State Attorney General's Office for pressuring the CPD to make this data publically available. 
