# Data-Mining-Project

## Introduction

Within here you will see documentation of my analysis of UPI Apps Transaction Data in 2021 dataset. The main purpose of this project was to analyze this datatset, and acquire an understanding of the trends that can be identified from UPI Banks that have a years worth of data. Throughout this documentation, you will see that we will delve into what the dataset us about, whats within the datatset and the visualizations and information I was able to attain. What I am curious about this data set are the trends I can find from this data. I am curious to see what banks throughout the whole year of 2021 had the most change, specifically which ones prospered more and when this occurred.

# The Dataset

This datatset is the UPI Apps Tranasaction Data meaning this dataset contains the transaction data of UPI Banks.Unified Payments Interface (UPI) is an instant real-time payment system developed by National Payments Corporation of India (NPCI) facilitating inter-bank peer-to-peer (P2P) and person-to-merchant (P2M) transactions. The whole purpose of the dataset is to display the transaction data of these UPI Banks throughout the year 2021. This Datatset was obtained from Kaggle and can be found [here](https://www.kaggle.com/ramjasmaurya/upi-apps-transactions-in-2021)

## What I found from the dataset
![Collumns](https://user-images.githubusercontent.com/49813790/159499080-d2a60c99-b5b6-4cfe-8cd5-48d5e8c236b6.png)
When we access the datatset, we of course need to know how much information this dataset contains alongside the collumns. his dataset originally has 654 rows of data alongside 7 columns which are the following: 

**1. UPI_Banks : The UPI Banks Name**

**2. Volume__Mn__By_Costumers: The Volume of transactions**

**3. Value__Cr__by_Costumers: The Value of transactions**

**4. Volume__Mn_: The total volume of transactions**

**5. Value__Cr_: The total value of transactions**

**6. Month: The month which ranges from 1-12**

**7. Year: The year, which in this dataset is always 2021**

## Cleaning up the dataset

When I analyzed the datatset I noticied that the dataset contains UPI Banks that do not have a whole years worth of data, or even UPI Banks categorized as other alongside that the tenth month for WhatsApp has the UPIBank name as WhatsApp*. Since I want to be able to see the trends throughout the year of 2021 I solely want UPI banks that contain a years worth of data in the dataset.

To clean up the little issues I noticed from the dataset regarding WhatsApp, I did the following:
```
# I noticed that for October, WhatsApp was replaced with WhatsApps* which would make it counted as its own seperate UPI Bank 
data2["UPI_Banks"].replace({"WhatsApp*": "WhatsApp"}, inplace=True)
old_data["UPI_Banks"].replace({"WhatsApp*": "WhatsApp"}, inplace=True)
data2
```


To ensure that would happen: I did the following:
```
# I wanted to see which ones were 'incomplete'
few_data = data2.groupby('UPI_Banks').size()
groups = few_data[few_data < 12]
groups
```

This would then give me the following output:

![Output#1](https://user-images.githubusercontent.com/49813790/159501963-1f7e37e8-be33-4280-8f9a-d66342ae0d16.png)

This provides a list of the **incomplete** UPI Banks which do not have a years worth of data. Now that we know which ones do and do not, I would then do the following to my original dataset which would eliminate all of the incomplete banks from the dataset.
```
for name in groups.index:
    data2 = data2[data2.UPI_Banks != name]
data2
```

## Bucketing Information

As we can assess from the current dataset there is no category to detect if the numerical values for volume and value are high,low,medium or anything close to displaying how the numerical values are in compairosn of the others. To ensure we have some indication for the 4 collumns for value & volume, I did the following:

```
data2['volume_by_customers_level']=pd.qcut(data2.Volume__Mn__By_Costumers, q=6, labels=['very low volume', 'low volume', 'medium volume','high volume', 'very high volume','Extremely high volume'])
data2['value_by_customer_level']=pd.qcut(data2.Value__Cr__by_Costumers, q=6, labels=['very low value', 'low value', 'medium value','high value', 'very high value','Extremely high value'])
data2['volume_level']=pd.qcut(data2.Volume__Mn_, q=6, labels=['very low volume', 'low volume', 'medium volume','high volume', 'very high volume','Extremely high volume'])
data2['value_level']=pd.qcut(data2.Value__Cr_, q=6, labels=['very low value', 'low value', 'medium value','high value', 'very high value','Extremely high value'])
```

This is done to ensure that we have 6 possible values for the categories to ensure we can accurately describe the numerical values in the grand scheme of things. The dataset would then look like the following thanks to the alteration we just did.

![DisplayBucketing](https://user-images.githubusercontent.com/49813790/159507475-5ccc518d-30ec-4827-bb34-1dbbb836061f.png)





