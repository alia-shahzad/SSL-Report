# HW06: An Analysis of the CPD's Heat List
Alia Shahzad

### What is this?

This repository contains publically available data from the Strategic Subject List (SSL), street name the "Heat List." It's an algorithm used by the Chicago Police Department to assign a risk score of an individual becoming a victim or perpetrator of violence (that is, party to violence) and intervene before this potential violence occurs. The SSL assigned a risk score to every Chicagoan with an arrest history prior to September of 2019, when its use was formally retired; the data has been de-identified for the privacy of its subjects.

This project aims to make sense of the data, and particularly the claim by the CPD that the SSL algorithm did not use race as a factor when assigning risk scores. 

### What can I find here?

The [raw SSL data](Strategic_Subject_List.csv) in CSV format].

The [clean SSL data](ssl.csv) in CSV format.

An Rmd with my [*preliminary* analysis of the SSL data](ssl.Rmd). This has some extra code, as well as what went on in my head while I was choosing data visualizations for the graphs I chose. You probably don't need to read this if you only care about looking at the data visualizations and reading polished analysis.

An Rmd with my [*final and complete* analysis of the SSL data](ssl_report.Rmd). This has all the important data visualizations and polished analysis but is missing some of the code I used to get there.

An [md](ssl.md) with my preliminary analysis of the SSL data.

An [md](ssl_report.md)  with my final analysis of the SSL data.

### What other packages do I need to execute these files?

You'll need RSocrata and forcats to execute these files.

Check out my [preliminary analysis](ssl.Rmd) of the SSL data to see how I downloaded these packages.

### Where did this data come from?

The data in this repo was loaded in an R-friendly csv format created by the good folks of [RSocrata](https://github.com/Chicago/RSocrata/blob/master/README.md), a group of data scientists employed by the City of Chicago.

SSL data is also available in other formats on the the [City of Chicago's Data portal](https://data.cityofchicago.org/Public-Safety/Strategic-Subject-List/4aki-r3np).

Thanks to the journalists from the Sun-Times and the Invisible Institute, as well as the state Attorney General's Office, for pressuring the CPD to make this data publically available. 
