
# Pandas: Heroes Of Pymoli - Analysis Report
 - Observed Trend 1: Mourning Blade is the most profitable item for file 2, Retribution Axe is the most profitable item for file 1
 - Observed Trend 2: most people only buy 1 item and male is the main buyer for both file 1 & 2
 - Observed Trend 3: age group(20-24) consumed the most items for file 1 & 2


```python
import pandas as pd
```


```python
# Read the JSON file

fpath = input("Please choice file (1) or file (2)?")

if (fpath == "1"):
    JS = pd.read_json("purchase_data.json")
elif (fpath == "2"):
    JS = pd.read_json("purchase_data2.json")
else:
    print("no such file")
JS.head()

ee =JS.to_excel("test"+fpath+".xlsx","Sheet1")
```

    Please choice file (1) or file (2)?1
    

#### Player Count


```python
# Total Number of Players
Player_Count = pd.DataFrame([{"Total Players":JS["SN"].nunique()}])
Player_Count
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



#### Purchasing Analysis (Total)


```python
#Number of Unique Items
UItem = JS["Item ID"].nunique()
#Average Purchase Price
AvePur = JS["Price"].mean() 
#Total Number of Purchases
NPur = JS["Price"].count()
#Total Revenue
TRev = JS["Price"].sum()

PurAnsys = pd.DataFrame([{"Number of Unique Items":UItem, "Average Price":"$"+str(round(AvePur,2)), "Number of Purchases":NPur, "Total Revenue":"$"+str(round(TRev,2))}])

PurAnsys = PurAnsys[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
PurAnsys
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>



#### Gender Demographics


```python
TotalC = JS["Gender"].count()
MaleC =JS["Gender"].value_counts()
MaleP =round(MaleC / TotalC*100,2)

# Creating a new DataFrame using both duration and count
GenderDemo = pd.DataFrame({"Percentage of players":MaleP,"Total Count":MaleC})
GenderDemo

#Alternative Method
    #TotalC = JS["Gender"].count()
    #MaleC =pd.DataFrame(JS["Gender"].value_counts())
    #MaleP =pd.DataFrame(round(MaleC / TotalC*100,2))
    #MaleC = MaleC.reset_index().rename(columns={'index':'Gender','Gender':'Total Count'})
    #MaleP = MaleP.reset_index().rename(columns={'index':'Gender','Gender':'Percentage of Players'})
    #GenderDemo = pd.merge(MaleP,MaleC,how='left',on='Gender')
    #GenderDemo

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



#### Purchasing Analysis (Gender)
 ###### The below each broken by gender


```python
JSMax = JS['Price'].max()
JSMin = JS['Price'].min()
JSMean = JS['Price'].mean()
JSStd = JS['Price'].std()
JSN = (JS['Price'] - JSMin)/(JSMax-JSMin)
JS['JSNor']=JSN
JS.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>JSNor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>0.596939</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>0.329082</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>0.364796</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>0.084184</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>0.061224</td>
    </tr>
  </tbody>
</table>
</div>




```python
JSGroup = JS.groupby(['Gender'])
#Purchase Count
PCbyG = JSGroup['Price'].count()
#Average Purchase Price
APbyG = round(JSGroup['Price'].mean(),2)
#Total Purchase Value
TPbyG = JSGroup['Price'].sum()
#Normalized Totals
TPbyGN = JSGroup['JSNor'].sum()
          
PAbyG = pd.DataFrame({"Purchase Count":PCbyG,"Average Purchase Price":APbyG,"Total Purchase Value":TPbyG,"Normalized Totals":TPbyGN})
PAbyG = PAbyG[["Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"]]
PAbyG
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.82</td>
      <td>382.91</td>
      <td>61.946429</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.95</td>
      <td>1867.68</td>
      <td>310.125000</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.25</td>
      <td>35.74</td>
      <td>6.227041</td>
    </tr>
  </tbody>
</table>
</div>



#### Age Demographics
 ###### The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 


```python
# Create the bins in which Data will be held
JS["Age"]=pd.to_numeric(JS['Age'])
bins = [0,9,14,19,24,29,34,39,40]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
JS["Age Group"] = pd.cut(JS["Age"],bins,labels=group_names)
JSgrAge = JS.groupby(['Age Group'])

TCbyA = JS["Age Group"].count()
VCbyA =JS["Age Group"].value_counts()
PCbyA =round(VCbyA / TCbyA*100,2)

# Creating a new DataFrame using both duration and count
AgeDemo = pd.DataFrame({"Percentage of players":PCbyA,"Total Count":VCbyA})
AgeDemo = AgeDemo.sort_index()
AgeDemo
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.60</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.50</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.12</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>43.24</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.09</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.24</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.41</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.80</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>




```python
JSgrAge = JS.groupby(['Age Group'])

#Purchase Count
PCbyA =JSgrAge['Price'].count()
#Average Purchase Price
APbyA =round(JSgrAge['Price'].mean(),2)
#Total Purchase Value
TPbyA =JSgrAge['Price'].sum()
#Normalized Totals
#TPbyAN = JSgrAge['Price'].sum(normalize=True)
MaxbyA = JSgrAge['Price'].max()
MinbyA = JSgrAge['Price'].min()
TPbyAN = round((TPbyA-APbyA) / (MaxbyA-MinbyA),2)

ADbyG = pd.DataFrame({"Purchase Count":PCbyA,"Average Purchase Price":APbyA,"Total Purchase Value":TPbyA,"Normalized Totals":TPbyAN})
ADbyG = ADbyG[["Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"]]
ADbyG
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>2.98</td>
      <td>83.46</td>
      <td>20.85</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>2.77</td>
      <td>96.95</td>
      <td>26.31</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>2.91</td>
      <td>386.42</td>
      <td>97.83</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>2.91</td>
      <td>978.77</td>
      <td>248.94</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>2.96</td>
      <td>370.33</td>
      <td>93.72</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>3.08</td>
      <td>197.25</td>
      <td>49.53</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>2.84</td>
      <td>119.40</td>
      <td>32.47</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>14</td>
      <td>3.22</td>
      <td>45.11</td>
      <td>14.85</td>
    </tr>
  </tbody>
</table>
</div>



#### Top Spenders
 ###### Identify the the top 5 spenders in the game by total purchase value, then list (in a table):


```python
#SN
GroupbySN = JS.groupby(['SN'])

#Total Purchase Value by SN
TPbyS =GroupbySN['Price'].sum()
#TPbyS = TPbyA.sort_values('Price', ascending=False)
#Purchase Count
PCbyS =GroupbySN['Price'].count()
#Average Purchase Price
APbyS =GroupbySN['Price'].mean()

TopS = pd.DataFrame({"Purchase Count":PCbyS,"Average Purchase Price":round(APbyS,2),"Total Purchase Value":TPbyS})
TopS = TopS[["Purchase Count","Average Purchase Price","Total Purchase Value"]]
TopS = TopS.sort_values('Total Purchase Value',ascending=False)
TopS.head(5)


```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



#### Most Popular Items
###### Identify the 5 most popular items by purchase count, then list (in a table):


```python
#Item ID
GroupbyID = JS.groupby(['Item ID','Item Name'])

#Total Purchase Value by ITem
TPbyI =GroupbyID['Price'].sum()
#TPbyS = TPbyA.sort_values('Price', ascending=False)
#Purchase Count
PCbyI =GroupbyID['Price'].count()
#Average Purchase Price
APbyI =GroupbyID['Price'].mean()
#Item Price
IPbyI =GroupbyID['Price']

TopI = pd.DataFrame({"Purchase Count":PCbyI,"Item Price":APbyI,"Total Purchase Value":TPbyI})
TopI = TopI[["Purchase Count","Item Price","Total Purchase Value"]]
TopI = TopI.sort_values('Purchase Count',ascending=False)
TopI.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



#### Most Profitable Items
###### Identify the 5 most profitable items by total purchase value, then list (in a table):


```python
#Item ID
GroupbyID = JS.groupby(['Item ID','Item Name'])

#Total Purchase Value by ITem
TPbyI =GroupbyID['Price'].sum()
#TPbyS = TPbyA.sort_values('Price', ascending=False)
#Purchase Count
PCbyI =GroupbyID['Price'].count()
#Average Purchase Price
APbyI =GroupbyID['Price'].mean()
#Item Price
IPbyI =GroupbyID['Price']

TopI = pd.DataFrame({"Purchase Count":PCbyI,"Item Price":APbyI,"Total Purchase Value":TPbyI})
TopI = TopI[["Purchase Count","Item Price","Total Purchase Value"]]
TopI = TopI.sort_values('Total Purchase Value',ascending=False)
TopI.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


