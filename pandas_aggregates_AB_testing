import codecademylib
import pandas as pd

ad_clicks = pd.read_csv('ad_clicks.csv')
print(ad_clicks.head(10))
#inspects the first 10 rows of the DataFrame

## USING PANDAS TO EXPLORE AD DATA. USING AGGREGATES AND PIVOTS TO COME TO CONCLUSIONS ABOUT AD PERFORMANCE ##

##ANALYZING AD SOURCES##

print(ad_clicks.groupby('utm_source').user_id.count().reset_index())
#shows the number of ad views for each platform

ad_clicks['is_click'] = ~ad_clicks.ad_click_timestamp.isnull()
#adds a new column which is True for instances where an ad was clicked on after being displayed 

clicks_by_source = ad_clicks.groupby(['utm_source', 'is_click']).user_id.count().reset_index()
#counts the number of clicks and non clicks for each ad source

clicks_pivot = clicks_by_source.pivot(
  columns = 'is_click',
  index = 'utm_source',
  values = 'user_id'
).reset_index()
#pivots the data for better reading

clicks_pivot['percent_clicked'] = clicks_pivot[True] / (clicks_pivot[True] + clicks_pivot[False])
print(clicks_pivot)
#adds a column to show the percent of people who clicked on ads from each utm source


## A/B TESTING ##

print(ad_clicks.groupby('experimental_group').user_id.count().reset_index())
#shows the number of users that were shown each each ad

num_of_clicks_pivot = ad_clicks.groupby(['experimental_group', 'is_click']).user_id.count()\
.reset_index()\
.pivot(
  columns = 'is_click',
  index = 'experimental_group',
  values = 'user_id'
).reset_index()
#counts the number of clicked ads for each group and pivots the data for better reading

num_of_clicks_pivot['percentage_clicked'] = clicks_pivot[True] / (clicks_pivot[True] + clicks_pivot[False])
print(num_of_clicks_pivot)
#adds a column to show the percentage of each group that clicked on an ad

a_clicks = ad_clicks[
  ad_clicks.experimental_group == 'A']
b_clicks = ad_clicks[
  ad_clicks.experimental_group == 'B']
#creates DataFrames which contain only the results for group A and group B, respectively

a_clicks_pivot = a_clicks.groupby(['is_click', 'day']).user_id.count()\
.reset_index()\
.pivot(
  columns = 'is_click',
  index = 'day',
  values = 'user_id'
).reset_index()
#calculates number of A users that clicked on an ad and pivots the data for better reading

a_clicks_pivot['percent_clicked'] = a_clicks_pivot[True] / (a_clicks_pivot[True] + a_clicks_pivot[False])
print(a_clicks_pivot)
#adds a column to show the percentage that clicked on an ad

b_clicks_pivot = b_clicks.groupby(['is_click', 'day']).user_id.count()\
.reset_index()\
.pivot(
  columns = 'is_click',
  index = 'day',
  values = 'user_id'
).reset_index()
#calculates number of B users that clicked on an ad and pivots the data for better reading

b_clicks_pivot['percent_clicked'] = b_clicks_pivot[True] / (b_clicks_pivot[True] + b_clicks_pivot[False])
print(b_clicks_pivot)
#adds a column to show the percentage that clicked on an ad

# On inspection A appears to be the better ad, since it had a higher total click rate, and outperformed ad B on every day of the week except for Tuesday

