# NYC-restaurants-building-inspections-and-complaints-analysis
[DatawarehousingProject_REPORT (1).pdf](https://github.com/kieumy179/NYC-restaurants-building-inspections-and-complaints-analysis/files/8064081/DatawarehousingProject_REPORT.1.pdf)
CIS <9440>
Final Project
<New York City restaurant inspections and complaints>
<Yun Wang
Songchen Nan
Kieu My Giang
Sunny Liu>
<Dec 14th 2020>
1
Introduction
Our project focuses on analyzing the New York City restaurants and buildings in terms of their
violation inspections and complaints that they received. We want to help the users have an
overview and gain insights about the restaurants and buildings in NYC with different types of
dashboards. Depending on the user’s interests, they have a choice in which dashboards are
suitable for them. For example, if the users want to know which building in NYC has the most
complaints and violation inspection, which community is in, they can use the dashboard 2.
We used two datasets: DOHMH New York City Restaurant Inspection Results and DOB
Complaints Received in the NYCOpendata. The datasets contain every sustained or not yet
adjudicated violation citation from every full or special program inspection conducted up to three
years prior to the most recent inspection for restaurants and college cafeterias in an active
status on the RECORD DATE, combined with universe of complaints received by Department of
Buildings (DOB).
Here are some examples of the KPIs that we want to monitor:
1. Which cuisine has the most complaints and violations and in which borough?
2. What is the violation/complaints category that each restaurant or building receives?
3. What is the average, max and min score of each building from its inspection result?
4. Which community board that each building belongs to?
5. Which building has the highest number of complaints and violation inspections?
6. Which category of complaints do the restaurants get?
7. Which season/months that the buildings and restaurants receive most violation
inspections and complaints?
Grain of two fact tables:
As we know, the grain of a fact table represents the most atomic level by which the facts may be
defined. Therefore, our grain is as below:
Table: fact_complaints_record1:
The complaint number by building, by specific inspection date, by status and by disposition date
Table: fact_violation_record1:
The violation code by each restaurant (CAMIS) and by grade date
2
Dimensional Model Diagram
This is a constellation schema because it has multiple fact tables. Here are two fact tables:
fact_violation_record, fact_complaints_record. Also, it has a common dimension table: table
dim_date.
3
Each table in the diagram is described below:
<dim_restaurant>
FieldName Description
CAMIS (PK) Unique identifier for the restaurant
BIN (FK1) Number assigned by City Planning to a
specific building
DBA Restaurant name (DBA = Doing Business As)
BORO Borough of restaurant location
cuisine_description Restaurant cuisine type
building Building number for restaurant location
street Street name for restaurant location
zipcode Zipcode for restaurant location
phone Phone number of the restaurant
community_board The number include 3-digit identifier: borough
code (first position) and community board
(last 2)
council_district The number of the council district
<dim_violation>
FieldName Description
violation_code (PK) Violation code associated with the restaurant
inspection
violation_description Violation description associated with the
restaurant inspection
critical_flag Indicator of critical violation (critical, not
critical, not applicable). Critical violations are
those most likely to contribute to foodborne
illness
score Total score for a particular inspection. Scores
are updated based on adjudication results
grade Grade associated with the inspection. Grades
4
given during a reopening inspection are
derived from the previous re-inspection.
• N= Not Yet Graded
• A = Grade A
• B = Grade B
• C = Grade C
• Z = Grade Pending
• P=Grade Pending issued on re-opening
following an initial inspection that resulted in a
closure
grade_date Date when grade was issued to the
restaurant
record_date Date record was added to dataset
inspection_type A combination of the inspection program and
the type of inspection performed
<dim_building>
FieldName Description
BIN (PK) Number assigned by City Planning to a
specific building
community_board The number include 3-digit identifier: borough
code (first position) and community board
(last 2)
special_district It is recorded when the complaint made in
special district
unit The unit of the building
<dim_complaints>
FieldName Description
complaint_number Complaint number starting with borough
code:
1 = Manhattan, 2= Bronx, 3 = Brooklyn, 4 =
Queens, 5 = Staten Island
status Status of complaint
5
date_entered Date complaint was entered
complaint_category The complaint category code
disposition_date Date complaint was dispositioned
<dim_date>
FieldName Description
date_key (PK) The unique identifier for each date
day A period of twenty-four hours as a unit of time
month One-twelfth of a year
quarter One-fourth of a year
year A cycle in the Gregorian calendar of 365 or
366 days
<dim_disposition>
FieldName Description
disposition_code (PK) Disposition code of complaint (E.g.: A1:
building violation served; L1-Partial stop work
order)
disposition_date Date complaint was dispositioned
inspection_date Inspection date of complaint
<fact_violation_record1>
FieldName Description
uniqueId (PK) Unique number to identify each record
violation_code (FK1) Violation code associated with the restaurant
inspection
date_key (FK2) The unique identifier for each date
6
CAMIS (FK3) Unique identifier for the restaurant
violation_description Violation description associated with the
restaurant inspection
grade_date Date when grade was issued to the
restaurant
DBA Restaurant name
<fact_complaints_record1>
FieldName Description
uniqueId (PK) Unique number to identify each record
complaint_number (FK1) Complaint number starting with borough
code:
1 = Manhattan, 2= Bronx, 3 = Brooklyn, 4 =
Queens, 5 = Staten Island
date_key (FK2) The unique identifier for each date
BIN (FK3) Number assigned by City Planning to a
specific building
status Status of complaint
disposition_date Date complaint was dispositioned
inspection_date Inspection date of complaint
7
ETL Processes
A description and screen pictures of the ETL processes.
Step1 : Transformation of all the dimension tables
1. dim_building_transformation:
2. dim_restaurant_transformation:
8
3. dim_complaints_transformation:
4. dim_disposition_transformation:
9
5. dim_date_transformation:
10
6. Dim_violation_transformation:
11
Step 2 : Transformation of all fact tables
1. fact_violation_transformation:
12
2. fact_complaints_transformation:
13
ETL Statistics
For each table, populate the number of rows that was processed
<Table Stats>
Table Name SCD Type (if applicable) Number of Rows Loaded
dim_building Type 1 719650
dim_complaints Type 1 1048576
dim_disposition Type 1 335343
dim_restaurant Type 1 28597
dim_violation Type 1 157904
dim_date Type 1 2501
fact_complaints_record1 Type 2 333326
fact_violation_record1 Type 2 23143
14
Analytics
Dashboard 1: Cuisine Complaint and Violation Analysis
This dashboard mainly shows the analysis of the number of complaints and violations for each
cuisine. We used the first worksheet as the filter for the 2nd and 3rd to show the detailed
information. Therefore, we can monitor violations and complaints by cuisine, in each borough
and for each type of violation.
For instance, in this dashboard, Chinese cuisine has the second highest number of complaints
(303779 complaints) and the third highest number of violations (1937 violations). In addition,
when we click the Chinese cuisine in the horizontal bar chart, we can see that the tree map also
displays the borough Queens, where the Chinese cuisine has the most complaints and
violations compared to others. Moreover, the bubble chart shows that the Chinese cuisine has
15
the most complaints with the code 06A which means the vesting inspection, according to the
complaint category description documents by NYC Buildings.
Dashboard 2: Building Complaints and Violation Analysis
The second dashboard demonstrates the analysis based on the number of complaints and
violations by the score by buildings (BIN). From this dashboard, we can easily track the KPIs to
see which building receives the most complaints and violation inspections. We can also observe
which community board that each building belongs to. Each community board includes many
different committees working on the planning and construction issues of the buildings. Similar to
the dashboard 1, this interactive dashboard helps us to monitor each individual building, which
is very helpful for the users. For example, the building with BIN 1014130 received 11 complaints
and 10 violation inspections with an average score of 10.67. This building belongs to the
community board number 407.
16
Dashboard 3:
The third dashboard illustrates the analysis of the number of complaints and violations by year.
From these charts we can monitor the trends of complaints and violations by borough over the
years. For the first chart, we could see that all boroughs have a lower violation amount in 2015
but have a large increase in 2016 and the trend is maintained until 2019 and got a slight
decrease in year 2020. The second chart shows that, over the years, Manhattan, Brooklyn and
Queens borough own most of the complaints, and for 2017 the complaint numbers in each
borough reached their peak in the recent 6-year period except Bronx. Finally, for the third chart,
it shows the trend of the average score by year. According to the Health Department, a
restaurant’s score depends on how well it follows City and State food safety requirements.
Restaurants with a score between 0 and 13 points earn a Grade A, those with 14 to 27 points
receive a Grade B and those with 28 or more a Grade C. Thus, the average score of the
restaurants that have complaints were all in Grade B category over the years.
17
Conclusion
The most difficult part of this project is the ETL process conducted in Pentaho. It took us
approximately 40-50 hours to load all the dataset. There were many errors that we could not
solve quickly and with limited open source solutions it was hard for us to find the answers. We
consulted the professor as well. We also had to revise the schema several times to make all the
tables connect and run smoothly. Whenever we made a mistake, we had to run everything
again, so it took a lot of time.
The easiest part in our project was the last part, when we used Tableau to visualize the data,
build the charts and dashboards. This part was easy because we finished all the complicated
previous steps so in this part, we just needed to play around with the data, choose the
appropriate charts to show our KPIs.
In addition to the knowledge and technical skills, the best thing we learned from this project is
patience since we had to do a lot of trial and error, especially for the model diagram and ETL
process part. We had to spend most of our time debugging and discovering which part we have
problems with. If we had to do it all over again, we would have read the instructions very
carefully before running the whole process. The reason is that it took much time to finish each
step, particularly loading the dataset and transforming tables. Once we did it wrong, we had to
do it again several times which took us many days to complete. In the future, we may want to try
another platform to do this project such as AWS to learn new things and check if it can help us
to save time and see if we can avoid some bugs and errors as we did in Pentaho.
18
Appendix
Data source:
DOHMH New York City Restaurant Inspection Results:
https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43n
n-pn8j
DOB Complaints Received:
https://data.cityofnewyork.us/Housing-Development/DOB-Complaints-Received/eabe-havvhttps:
//data.cityofnewyork.us/Business/License-Applications/ptev-4hud
Complaint Category Description:
https://www1.nyc.gov/assets/buildings/pdf/complaint_category.pdf
The Group 3’s blog;
https://blogs.baruch.cuny.edu/dwgroup3/
19
