# Import-csv-to-MongoDB
following repository can be used to know how to import csv (World bank data) in Mongodb &amp; apply aggregation function to extract specific records of data
## About Dataset:
World Bank Dataset of India consists of various Indicators that deals with all kind of transaction happening in India like- Population Growth, Exports and Imports, GDP growth, Education enrollment, Tax Revenue, Interest Payments, Government Expenditure, etc. This dataset is distributed from 1960 to 2020 with all Indicator descriptions and Indicator Codes.
### Part 1:
Import Data in MongoDB environment, display full data and perform Descriptive Statistics of Data using MongoDB Query Language.
#### Step 1- Created a Collection in Database1 for the Dataset.
#### Step 2- Imported dataset to Collection.
![image](https://user-images.githubusercontent.com/81969659/156785810-93fff83d-f598-4a29-a1ad-6c3be3780f0f.png)
![image](https://user-images.githubusercontent.com/81969659/156785836-2006471f-83b5-421e-a62d-b4cd650839cf.png)
#### Step 3- Descriptive Statistics of Data
Code:
Db.World_Bank.stats()
Output:
![image](https://user-images.githubusercontent.com/81969659/156785881-5656e13c-2ac5-4772-bae4-accc93d99695.png)
#### Step 4- Unique Query
Question: Calculate Total Revenue for last 20 years and the overall Average for those years for Indicators “GC.TAX.TOTL.CN”, “GC.REV.GOTR.CN”, “GC.REV.XGRT.CN” and “IQ.CPA.REVN.XQ”.


db.Data5.aggregate([
{$project: {'Country Name':1,'Country Code':1,'Indicator Code': 1,'Indicator Name':1,
TotalRevenue:{ $sum: ["$Year.2000","$Year.2001","$Year.2002","$Year.2003","$Year.2004",
"$Year.2005","$Year.2006","$Year.2007","$Year.2008","$Year.2009","$Year.2010",
"$Year.2011","$Year.2012","$Year.2013","$Year.2014","$Year.2015","$Year.2016",
"$Year.2017","$Year.2018","$Year.2019","$Year.2020"]},
Avgerage:{ $avg: ["$Year.2000","$Year.2001","$Year.2002","$Year.2003","$Year.2004",
"$Year.2005","$Year.2006","$Year.2007","$Year.2008","$Year.2009","$Year.2010",
"$Year.2011","$Year.2012","$Year.2013","$Year.2014","$Year.2015","$Year.2016",
"$Year.2017","$Year.2018","$Year.2019","$Year.2020"]}, _id: 0}}, {$match:{$or:[{'Indicator Code': "GC.TAX.TOTL.CN"},
{'Indicator Code': "GC.REV.GOTR.CN"},{'Indicator Code':"GC.REV.XGRT.CN"},{'Indicator Code':"IQ.CPA.REVN.XQ"}]}}
])

output:
![image](https://user-images.githubusercontent.com/81969659/156786126-1156094b-0b56-4d03-883e-117c9603f207.png)
![image](https://user-images.githubusercontent.com/81969659/156786166-a756c39b-9220-40c8-90ed-0c9500c9e67a.png)
### Inference:
The above result shows the Total Revenue for above 4 Revenue Indicators and what has been the overall average of Revenue by each Indicator in last 20 years in India.

### Question: Extract the Total Population for last 10 years and update the field for Year 2021.
db.World_Bank.aggregate([
    {$match: {
        'Indicator Name': "Population, total",},},
    { $group: {
        _id: "$Indicator Code",
        data: {
          $push: "$$ROOT",},},},
    {
      $project:{_id:1,
        "data.Indicator Name":1,
        "data.Country Name": 1,
        "data.2010": 1,
        "data.2011": 1,
        "data.2012": 1,
        "data.2013": 1,
        "data.2014": 1,
        "data.2015": 1,
        "data.2016": 1,
        "data.2016": 1,
        "data.2017": 1,
        "data.2018": 1,
        "data.2019": 1,
        "data.2020": 1
      },},{ $addFields: { "data.2021": "1405674400",},},]).pretty()
![image](https://user-images.githubusercontent.com/81969659/156786428-550d55f1-e7a4-4a8f-b3fe-104c8abc1b1b.png)
#### Output:
![image](https://user-images.githubusercontent.com/81969659/156786509-22fc3f08-1535-42b6-850f-01419d1aff75.png)

Inference:
The above result shows the Total Population of last 10 years in India and how we can update another field of year 2021 with its Population count.
