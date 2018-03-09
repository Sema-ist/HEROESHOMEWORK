

```python
import pandas as pd
import json

jsonpath = 'purchase_data.json'
#jsonpath = 'purchase_data2.json'

purchase_data = pd.read_json(jsonpath)

total_number_of_players = purchase_data['SN'].nunique()

x = pd.DataFrame({"Total Players":[total_number_of_players]})

x
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




```python
number_of_unique_items  = purchase_data['Item Name'].nunique()

average_purchase_price = '${:.2f}'.format(purchase_data['Price'].mean())

total_number_of_purchases = (purchase_data['Price'].count())

total_revenue = '${:.2f}'.format(purchase_data['Price'].sum())

y = pd.DataFrame({"Number of Unique Items":[number_of_unique_items], 
                  "Average price":[average_purchase_price],
                  "Number of Purchases": [total_number_of_purchases],
                 "Total Revenue":[total_revenue]})
y
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
      <th>Average price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics

#Percentage and Count of Male Players
#Percentage and Count of Female Players
#Percentage and Count of Other / Non-Disclosed

gender_count = purchase_data.drop_duplicates(['SN']).Gender.value_counts()

percentage_of_players = round(purchase_data.drop_duplicates(['SN']).Gender.value_counts(normalize=True)*100, 2)

gender_demographic = pd.DataFrame({"Total Count" : gender_count, "Percentage of Players" : percentage_of_players}, index=["Male", "Female", "Other / Non-Disclosed"])

gender_demographic
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python


purchase_data_gender = purchase_data.groupby("Gender")


#Purchase Count
purchase_count = purchase_data.Gender.value_counts()

#Average Purchase Price
avg_price_gender = purchase_data_gender["Price"].mean()

#Total Purchase Value
total_purchase_value = purchase_data_gender["Price"].sum()


#Normalized Totals
normalized_total = purchase_data.groupby('Gender').Price.sum()

normalized_avg = total_purchase_value/gender_count

normalized_avg


#Normalized Totals


purchasing_analysis = pd.DataFrame({"Purchase Count" : purchase_count,
                                    "Average Purchase Price" : avg_price_gender.map('${:.2f}'.format),
                                    "Total Purchase Value" : total_purchase_value.map('${:.2f}'.format),
                                    "Normalized Totals" : normalized_total.map('${:.2f}'.format)})


purchasing_analysis
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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals



# Create the bins in which Data will be held

bins = [0, 10, 15, 20, 25, 30, 35, 40, 1000]

# Create the names for the four bins
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

purchase_data["AgeSummary"] = pd.cut(purchase_data["Age"], bins, labels=group_names)


age_count = purchase_data.drop_duplicates(['SN']).AgeSummary.value_counts()

percentage_of_players = round(purchase_data.drop_duplicates(['SN']).AgeSummary.value_counts(normalize=True)*100, 2)


age_demographic = pd.DataFrame({"Total Count" : age_count, "Percentage of Players" : percentage_of_players}, group_names)

age_demographic
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
purchase_data_age = purchase_data.groupby("AgeSummary")

#Purchase Count
purchase_count = purchase_data.AgeSummary.value_counts()


#Average Purchase Price
avg_price_AgeSummary = purchase_data_age["Price"].mean()

#Total Purchase Value
total_purchase_value = purchase_data_age["Price"].sum()

#Normalized Totals
normalized_total = purchase_data.groupby('AgeSummary').Price.sum()

normalized_avg = total_purchase_value/purchase_data.AgeSummary.value_counts()

purchasing_analysis = pd.DataFrame({"Purchase Count" : purchase_count,
                                    "Average Purchase Price" : avg_price_AgeSummary,
                                    "Total Purchase Value" : total_purchase_value,
                                    "Normalized Totals" : normalized_total}, group_names)

purchasing_analysis
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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.019375</td>
      <td>96.62</td>
      <td>32</td>
      <td>96.62</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>2.873718</td>
      <td>224.15</td>
      <td>78</td>
      <td>224.15</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>2.873587</td>
      <td>528.74</td>
      <td>184</td>
      <td>528.74</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>2.959377</td>
      <td>902.61</td>
      <td>305</td>
      <td>902.61</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>2.892368</td>
      <td>219.82</td>
      <td>76</td>
      <td>219.82</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>3.073448</td>
      <td>178.26</td>
      <td>58</td>
      <td>178.26</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>2.897500</td>
      <td>127.49</td>
      <td>44</td>
      <td>127.49</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.880000</td>
      <td>8.64</td>
      <td>3</td>
      <td>8.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#SN
#Purchase Count
#Average Purchase Price
#Total Purchase Value



group_dataframe = purchase_data.groupby("SN")

group_dataframe

total_purchase_value = group_dataframe["Price"].sum()

average_purchase_price = group_dataframe["Price"].mean()

purchase_count = group_dataframe["Price"].count()


new_frame = pd.DataFrame({"Total Purchase Value": total_purchase_value, 
                          "Average Purchase Price": average_purchase_price, 
                          "Purchase Count": purchase_count})


new_frame["Average Purchase Price"] = new_frame["Average Purchase Price"].map("${:,.2f}".format)
new_frame["Total Purchase Value"] = new_frame["Total Purchase Value"].map("${:,.2f}".format)
new_frame = new_frame.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

new_frame_sorted = new_frame.sort_values("Total Purchase Value", ascending=False)

new_frame_sorted.head(5)
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
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items

# Identify the 5 most popular items by purchase count, then list (in a table):
# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value


most_popular_items = purchase_data.loc[:,["Item ID", "Item Name", "Price"]]

total_item_purchase = most_popular_items.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")

average_item_purchase = most_popular_items.groupby(["Item ID", "Item Name"]).mean()["Price"]

item_count = most_popular_items.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")


most_popular_items_pd = pd.DataFrame({"Total Purchase Value": total_item_purchase, "Item Price": average_item_purchase, "Purchase Count": item_count})

most_popular_items_pd["Item Price"] = most_popular_items_pd["Item Price"].map("${:,.2f}".format)

most_popular_items_pd ["Purchase Count"] = most_popular_items_pd["Purchase Count"].map("{:,}".format)

most_popular_items_pd["Total Purchase Value"] = most_popular_items_pd["Total Purchase Value"].map("${:,.2f}".format)

most_popular_items_pd = most_popular_items_pd.loc[:,["Purchase Count", "Item Price", "Total Purchase Value"]]


most_popular_items_pd.sort_values("Purchase Count", ascending=False).head(5)







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
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>106</th>
      <th>Crying Steel Sickle</th>
      <td>8</td>
      <td>$2.29</td>
      <td>$18.32</td>
    </tr>
  </tbody>
</table>
</div>




```python
most_popular_items_pd.sort_values("Total Purchase Value", ascending=False).head(5)
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
      <th>170</th>
      <th>Shadowsteel</th>
      <td>5</td>
      <td>$1.98</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>21</th>
      <th>Souleater</th>
      <td>3</td>
      <td>$3.27</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>37</th>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>5</td>
      <td>$1.93</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>127</th>
      <th>Heartseeker, Reaver of Souls</th>
      <td>3</td>
      <td>$3.21</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>120</th>
      <th>Agatha</th>
      <td>5</td>
      <td>$1.91</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>


