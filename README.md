# Real-Estate-Project
This is the project to scrap the data of Housing properties in Hyderabad from [Makaan.com](https://www.makaan.com/hyderabad-residential-property/buy-property-in-hyderabad-city?_=1624617705367&page=1)
The Project was done by [Sahil Sreedharan](https://www.linkedin.com/in/sahil-sreedharan/)
## Contents of repository
1. Program for web scrapping
2. Collected data in .csv

## Objectives
To Scrap 50 pages of Data and Analyse the Data find out insights and outcomes which drive to understand the best option to purchase.

## Programming Language
Python 3

## Modules
1. requests
2. bs4
3. pandas
4. time
5. IPython.display

## Scrapping the data regarding :
1. Owner Info
2. BHK Info
3. Price Info
4. Size Info
5. Status
7. Price per sq.ft

# Coding
## Step-1
### Importing modules
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
from IPython.display import clear_output
```
## Step-2
### Scrapping data
```python
base_url = 'https://www.makaan.com/hyderabad-residential-property/buy-property-in-hyderabad-city?page='

ow = [] #owner info
bhk = [] #bhk info
p = [] #price info
s = [] #size info
loc = [] #location info
st = [] #status info
pps = [] #price per sqft info

for i in range(1,51):
  time.sleep(3)
  print(f'going to scrap data from {i} page')
  clear_output(wait=True)
  url = base_url+str(i)
  req = requests.get(url)
  soup = BeautifulSoup(req.text,'html')

  #owner info
  ow_info = soup.findAll('span',attrs={'class':'seller-type'})
  for j in ow_info:
    ow.append(j.text)
  
  #bhk info
  bhk_info = soup.findAll('div',attrs={'class':'title-line'})
  for j in bhk_info:
    bhk.append(j.strong.span.text)

  #price info
  p_info = soup.findAll('div',attrs={'data-type':'price-link'})
  for  j in p_info:
    p.append(j.text)
  
  #size info
  s_info = soup.findAll("td",attrs={'class':'size'})
  for j in s_info:
    s.append(j.text)
  
  #location info
  loc_info = soup.findAll('span',attrs={'itemprop':'addressLocality'})
  for j in loc_info:
    loc.append(j.text)
  
    #status info
  st_info = soup.findAll('td',attrs={'class':'val'})
  for i in st_info:
    st.append(i.text)

  #price per sqft info
  pps_info = soup.findAll('td',attrs={'class':'lbl rate'})
  for j in pps_info:
    pps.append(j.text)
```
## Step-3
### Convertng to Structured Formmat
```python
data = pd.DataFrame({'Owner_inf' : ow,
              'Bhk_info' : bhk,
              'Price' : p,
              'Locality' : loc,
              'Area_sqft' : s,
              'Status' : st,
              'Price_Per_sqft' : pps})
data
```
## Step-4
### Storing data into .csv file
```python
data.to_csv('web_scrapped.csv',index=False) #index = false to exclude index column
```
