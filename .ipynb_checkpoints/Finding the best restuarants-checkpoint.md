
##  Exploring the ratings of restaurants in India

Zomato is a restaurant reviews aggregator site which provides a platform for restaurant goers to review various restaurants they have visited. This data has been obtained from Kaggle datasets. 




```python
import numpy
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

## Exploring the Data


```python
data = pd.read_csv('zomato.csv', encoding = "ISO-8859-1")
```


```python
data.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Restaurant ID</th>
      <th>Country Code</th>
      <th>Longitude</th>
      <th>Latitude</th>
      <th>Average Cost for two</th>
      <th>Price range</th>
      <th>Aggregate rating</th>
      <th>Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>9.551000e+03</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
      <td>9551.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>9.051128e+06</td>
      <td>18.365616</td>
      <td>64.126574</td>
      <td>25.854381</td>
      <td>1199.210763</td>
      <td>1.804837</td>
      <td>2.666370</td>
      <td>156.909748</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.791521e+06</td>
      <td>56.750546</td>
      <td>41.467058</td>
      <td>11.007935</td>
      <td>16121.183073</td>
      <td>0.905609</td>
      <td>1.516378</td>
      <td>430.169145</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.300000e+01</td>
      <td>1.000000</td>
      <td>-157.948486</td>
      <td>-41.330428</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>3.019625e+05</td>
      <td>1.000000</td>
      <td>77.081343</td>
      <td>28.478713</td>
      <td>250.000000</td>
      <td>1.000000</td>
      <td>2.500000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>6.004089e+06</td>
      <td>1.000000</td>
      <td>77.191964</td>
      <td>28.570469</td>
      <td>400.000000</td>
      <td>2.000000</td>
      <td>3.200000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.835229e+07</td>
      <td>1.000000</td>
      <td>77.282006</td>
      <td>28.642758</td>
      <td>700.000000</td>
      <td>2.000000</td>
      <td>3.700000</td>
      <td>131.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.850065e+07</td>
      <td>216.000000</td>
      <td>174.832089</td>
      <td>55.976980</td>
      <td>800000.000000</td>
      <td>4.000000</td>
      <td>4.900000</td>
      <td>10934.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Restaurant ID</th>
      <th>Restaurant Name</th>
      <th>Country Code</th>
      <th>City</th>
      <th>Address</th>
      <th>Locality</th>
      <th>Locality Verbose</th>
      <th>Longitude</th>
      <th>Latitude</th>
      <th>Cuisines</th>
      <th>...</th>
      <th>Currency</th>
      <th>Has Table booking</th>
      <th>Has Online delivery</th>
      <th>Is delivering now</th>
      <th>Switch to order menu</th>
      <th>Price range</th>
      <th>Aggregate rating</th>
      <th>Rating color</th>
      <th>Rating text</th>
      <th>Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6317637</td>
      <td>Le Petit Souffle</td>
      <td>162</td>
      <td>Makati City</td>
      <td>Third Floor, Century City Mall, Kalayaan Avenu...</td>
      <td>Century City Mall, Poblacion, Makati City</td>
      <td>Century City Mall, Poblacion, Makati City, Mak...</td>
      <td>121.027535</td>
      <td>14.565443</td>
      <td>French, Japanese, Desserts</td>
      <td>...</td>
      <td>Botswana Pula(P)</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>4.8</td>
      <td>Dark Green</td>
      <td>Excellent</td>
      <td>314</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6304287</td>
      <td>Izakaya Kikufuji</td>
      <td>162</td>
      <td>Makati City</td>
      <td>Little Tokyo, 2277 Chino Roces Avenue, Legaspi...</td>
      <td>Little Tokyo, Legaspi Village, Makati City</td>
      <td>Little Tokyo, Legaspi Village, Makati City, Ma...</td>
      <td>121.014101</td>
      <td>14.553708</td>
      <td>Japanese</td>
      <td>...</td>
      <td>Botswana Pula(P)</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>4.5</td>
      <td>Dark Green</td>
      <td>Excellent</td>
      <td>591</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6300002</td>
      <td>Heat - Edsa Shangri-La</td>
      <td>162</td>
      <td>Mandaluyong City</td>
      <td>Edsa Shangri-La, 1 Garden Way, Ortigas, Mandal...</td>
      <td>Edsa Shangri-La, Ortigas, Mandaluyong City</td>
      <td>Edsa Shangri-La, Ortigas, Mandaluyong City, Ma...</td>
      <td>121.056831</td>
      <td>14.581404</td>
      <td>Seafood, Asian, Filipino, Indian</td>
      <td>...</td>
      <td>Botswana Pula(P)</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>4</td>
      <td>4.4</td>
      <td>Green</td>
      <td>Very Good</td>
      <td>270</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6318506</td>
      <td>Ooma</td>
      <td>162</td>
      <td>Mandaluyong City</td>
      <td>Third Floor, Mega Fashion Hall, SM Megamall, O...</td>
      <td>SM Megamall, Ortigas, Mandaluyong City</td>
      <td>SM Megamall, Ortigas, Mandaluyong City, Mandal...</td>
      <td>121.056475</td>
      <td>14.585318</td>
      <td>Japanese, Sushi</td>
      <td>...</td>
      <td>Botswana Pula(P)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>4</td>
      <td>4.9</td>
      <td>Dark Green</td>
      <td>Excellent</td>
      <td>365</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6314302</td>
      <td>Sambo Kojin</td>
      <td>162</td>
      <td>Mandaluyong City</td>
      <td>Third Floor, Mega Atrium, SM Megamall, Ortigas...</td>
      <td>SM Megamall, Ortigas, Mandaluyong City</td>
      <td>SM Megamall, Ortigas, Mandaluyong City, Mandal...</td>
      <td>121.057508</td>
      <td>14.584450</td>
      <td>Japanese, Korean</td>
      <td>...</td>
      <td>Botswana Pula(P)</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>4</td>
      <td>4.8</td>
      <td>Dark Green</td>
      <td>Excellent</td>
      <td>229</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
country_code = pd.read_excel('Country-Code.xlsx')
```


```python
country_code.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Code</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>India</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>Australia</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30</td>
      <td>Brazil</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>Canada</td>
    </tr>
    <tr>
      <th>4</th>
      <td>94</td>
      <td>Indonesia</td>
    </tr>
  </tbody>
</table>
</div>



### Let's have a look at the restaurants in India

We need to get the country code of India first. Although we see that directly here that the country code is 1 for India, it would be a good exercise to try to extract it from the data at hand.


```python
country_code[country_code['Country'] == 'India']['Country Code']
```




    0    1
    Name: Country Code, dtype: int64



Thus the country code is 1, it is possible to find the country code of any other country using the above command.


```python
data_india = data[data['Country Code'] == 1]
```


```python
data_india.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Restaurant ID</th>
      <th>Restaurant Name</th>
      <th>Country Code</th>
      <th>City</th>
      <th>Address</th>
      <th>Locality</th>
      <th>Locality Verbose</th>
      <th>Longitude</th>
      <th>Latitude</th>
      <th>Cuisines</th>
      <th>...</th>
      <th>Currency</th>
      <th>Has Table booking</th>
      <th>Has Online delivery</th>
      <th>Is delivering now</th>
      <th>Switch to order menu</th>
      <th>Price range</th>
      <th>Aggregate rating</th>
      <th>Rating color</th>
      <th>Rating text</th>
      <th>Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>624</th>
      <td>3400025</td>
      <td>Jahanpanah</td>
      <td>1</td>
      <td>Agra</td>
      <td>E 23, Shopping Arcade, Sadar Bazaar, Agra Cant...</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>78.011544</td>
      <td>27.161661</td>
      <td>North Indian, Mughlai</td>
      <td>...</td>
      <td>Indian Rupees(Rs.)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>3.9</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>140</td>
    </tr>
    <tr>
      <th>625</th>
      <td>3400341</td>
      <td>Rangrezz Restaurant</td>
      <td>1</td>
      <td>Agra</td>
      <td>E-20, Shopping Arcade, Sadar Bazaar, Agra Cant...</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>North Indian, Mughlai</td>
      <td>...</td>
      <td>Indian Rupees(Rs.)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>3.5</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>71</td>
    </tr>
    <tr>
      <th>626</th>
      <td>3400005</td>
      <td>Time2Eat - Mama Chicken</td>
      <td>1</td>
      <td>Agra</td>
      <td>Main Market, Sadar Bazaar, Agra Cantt, Agra</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>78.011608</td>
      <td>27.160832</td>
      <td>North Indian</td>
      <td>...</td>
      <td>Indian Rupees(Rs.)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>3.6</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>94</td>
    </tr>
    <tr>
      <th>627</th>
      <td>3400021</td>
      <td>Chokho Jeeman Marwari Jain Bhojanalya</td>
      <td>1</td>
      <td>Agra</td>
      <td>1/48, Delhi Gate, Station Road, Raja Mandi, Ci...</td>
      <td>Civil Lines</td>
      <td>Civil Lines, Agra</td>
      <td>77.998092</td>
      <td>27.195928</td>
      <td>Rajasthani</td>
      <td>...</td>
      <td>Indian Rupees(Rs.)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>4.0</td>
      <td>Green</td>
      <td>Very Good</td>
      <td>87</td>
    </tr>
    <tr>
      <th>628</th>
      <td>3400017</td>
      <td>Pinch Of Spice</td>
      <td>1</td>
      <td>Agra</td>
      <td>23/453, Opposite Sanjay Cinema, Wazipura Road,...</td>
      <td>Civil Lines</td>
      <td>Civil Lines, Agra</td>
      <td>78.007553</td>
      <td>27.201725</td>
      <td>North Indian, Chinese, Mughlai</td>
      <td>...</td>
      <td>Indian Rupees(Rs.)</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>4.2</td>
      <td>Green</td>
      <td>Very Good</td>
      <td>177</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



#### Finding the list of cuisines available in Indian restaurants


```python
data_india['Cuisines'].unique()
```




    array(['North Indian, Mughlai', 'North Indian', 'Rajasthani', ...,
           'Continental, Seafood, Chinese, North Indian, Biryani',
           'Burger, Pizza, Biryani',
           'American, North Indian, Thai, Continental'], dtype=object)



#### Total no of cuisines 


```python
data_india['Cuisines'].nunique()
```




    1392



#### Total no. of restaurants in India


```python
data_india['Restaurant ID'].nunique()
```




    8652




```python
data_india.columns
```




    Index(['Restaurant ID', 'Restaurant Name', 'Country Code', 'City', 'Address',
           'Locality', 'Locality Verbose', 'Longitude', 'Latitude', 'Cuisines',
           'Average Cost for two', 'Currency', 'Has Table booking',
           'Has Online delivery', 'Is delivering now', 'Switch to order menu',
           'Price range', 'Aggregate rating', 'Rating color', 'Rating text',
           'Votes'],
          dtype='object')



Most of the restaurants covered by Zomata ratings and reviews seems to be in India in this particular data set. 

### What are we looking for?

As we go along with the analysis we will try to find out what factors influences the ratings of the restaurants. The ability to book a table for example might influence how a potential customer leaves a rating. Another important factor would be the authenticity of the cuisine being served. As a general rule of thumb one might say that a multi-cuisine restaurant might be less authentic than one that serves only one type of cuisine. I will try to cover such aspects in this analysis:


```python
sns.set(font_scale=1.2, rc={'figure.figsize':(8,5)})
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)
```


```python
sns.barplot(x="Price range", y='Aggregate rating', hue='Has Table booking', data=data_india)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c1f8def0>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_23_1.png)


It seems that for restaurants that are more affordable (has a lower price range) have higher ratings if they provide table booking services.For pricier restaurants table booking option does not seem to affect the ratings much.


```python
sns.barplot(x="Price range", y='Aggregate rating', hue='Has Online delivery', data=data_india)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c1f8d780>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_25_1.png)


Again for restaurants that are in the lower price range (1 and 2) seems to have better average rating if they provide online delivery services. This is not so signficant for the restaurants falling in the pricier category.


```python
sns.barplot(x="Price range", y='Aggregate rating', hue='Is delivering now', data=data_india)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c1fa7160>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_27_1.png)



```python
sns.barplot(x="Price range", y='Aggregate rating', data=data_india)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c359ac50>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_28_1.png)


The 'Switch to order menu' service is not provided by any of the restaurants that have been reviewed in Zomato. But this interestingly shows the general trend of ratings provided by the users. The lower price restaurants in general have worser ratings compared to the pricier ones. There is essentially little difference in the ratings of price categories 1 and 2.

### Cuisines provided by the restaurants
The above restaurants are a mix of single cusine and multi-cuisine restaurant. The rating of restaurants will be affected by the authenticity of the cuisine they are serving. I will first make a column of cusines that is served by the restaurants.


```python
def cusines_count(text):
    x = text.split(',')
    return len(x)
```


```python
x = data_india['Cuisines'].apply(cusines_count)
```


```python
x = x.rename("No. of cuisines")
```


```python
x.name
```




    'No. of cuisines'




```python
data_modified = pd.concat([data_india, x], axis= 1)
```


```python
#Mean of all ratings
sns.distplot(data_modified['Aggregate rating'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c3507a90>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_36_1.png)


There are a lot of unrated restaurants in this dataset which would not contribute to understanding the dynamics of restaurant goers in India. We will remove all this data points to have a more representative sample. It might be possible to predict the ratings of those restaurants based on the characteristics of the restaurants that have already been rated.

Let us now remove the data points that do not have any votes or ratings


```python
data_rated_restaurants = data_modified[(data_modified['Aggregate rating']>0)&(data_modified['Votes']>0)]
```


```python
data_rated_restaurants.shape
```




    (6513, 22)



Let us have another look now at the distribution of aggregrate ratings among all restaurants.


```python
sns.distplot(data_rated_restaurants['Aggregate rating'], color='k')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c651ab70>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_42_1.png)



```python
sns.barplot(x ='No. of cuisines', y = 'Aggregate rating', data= data_rated_restaurants, palette="Blues_d" )
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d40ec5d2e8>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_43_1.png)


This is very interesting situation. It seems that contrary to my own expectations, it seems that multi-cusine restaurants in India seems to have a little better aggregrate rating. It would be interesting to see the distribution divided between the four price ranges


```python
sns.barplot(x ="Price range", y = 'Aggregate rating', hue= 'No. of cuisines', data= data_rated_restaurants, palette="Blues_d" )
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d4151b26d8>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_45_1.png)


The trend of the aggregate rating being better for multicuisine restaurant seems to be valid for all the price range of restaurants. Thus in India it is likely that if you have a multi-cuisine restaurant it would probably get better ratings. Additionally, the highest price category restaurants provide no more than 7 cuisines.



```python
sns.barplot(x ='No. of cuisines', y = 'Aggregate rating', hue="Price range" , data= data_rated_restaurants, palette="Blues_d" )
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d41587a358>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_47_1.png)



```python
data_rated_restaurants.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Restaurant ID</th>
      <th>Restaurant Name</th>
      <th>Country Code</th>
      <th>City</th>
      <th>Address</th>
      <th>Locality</th>
      <th>Locality Verbose</th>
      <th>Longitude</th>
      <th>Latitude</th>
      <th>Cuisines</th>
      <th>...</th>
      <th>Has Table booking</th>
      <th>Has Online delivery</th>
      <th>Is delivering now</th>
      <th>Switch to order menu</th>
      <th>Price range</th>
      <th>Aggregate rating</th>
      <th>Rating color</th>
      <th>Rating text</th>
      <th>Votes</th>
      <th>No. of cuisines</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>624</th>
      <td>3400025</td>
      <td>Jahanpanah</td>
      <td>1</td>
      <td>Agra</td>
      <td>E 23, Shopping Arcade, Sadar Bazaar, Agra Cant...</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>78.011544</td>
      <td>27.161661</td>
      <td>North Indian, Mughlai</td>
      <td>...</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>3.9</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>140</td>
      <td>2</td>
    </tr>
    <tr>
      <th>625</th>
      <td>3400341</td>
      <td>Rangrezz Restaurant</td>
      <td>1</td>
      <td>Agra</td>
      <td>E-20, Shopping Arcade, Sadar Bazaar, Agra Cant...</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>North Indian, Mughlai</td>
      <td>...</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>3.5</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>71</td>
      <td>2</td>
    </tr>
    <tr>
      <th>626</th>
      <td>3400005</td>
      <td>Time2Eat - Mama Chicken</td>
      <td>1</td>
      <td>Agra</td>
      <td>Main Market, Sadar Bazaar, Agra Cantt, Agra</td>
      <td>Agra Cantt</td>
      <td>Agra Cantt, Agra</td>
      <td>78.011608</td>
      <td>27.160832</td>
      <td>North Indian</td>
      <td>...</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>3.6</td>
      <td>Yellow</td>
      <td>Good</td>
      <td>94</td>
      <td>1</td>
    </tr>
    <tr>
      <th>627</th>
      <td>3400021</td>
      <td>Chokho Jeeman Marwari Jain Bhojanalya</td>
      <td>1</td>
      <td>Agra</td>
      <td>1/48, Delhi Gate, Station Road, Raja Mandi, Ci...</td>
      <td>Civil Lines</td>
      <td>Civil Lines, Agra</td>
      <td>77.998092</td>
      <td>27.195928</td>
      <td>Rajasthani</td>
      <td>...</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>4.0</td>
      <td>Green</td>
      <td>Very Good</td>
      <td>87</td>
      <td>1</td>
    </tr>
    <tr>
      <th>628</th>
      <td>3400017</td>
      <td>Pinch Of Spice</td>
      <td>1</td>
      <td>Agra</td>
      <td>23/453, Opposite Sanjay Cinema, Wazipura Road,...</td>
      <td>Civil Lines</td>
      <td>Civil Lines, Agra</td>
      <td>78.007553</td>
      <td>27.201725</td>
      <td>North Indian, Chinese, Mughlai</td>
      <td>...</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>3</td>
      <td>4.2</td>
      <td>Green</td>
      <td>Very Good</td>
      <td>177</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



Another important parameter to know the is number of footfalls of each restaurants. However what we have here is the number of reviewers which is only proportional to the total number of footfalls over a given duration. Since the duration during which the reviews were obtained are not given, we will assume this to be the same for all restaurants as of now to have a general idea about the number of people who visit the restaurant. The number of footfalls per restaurant should also depend on the population of the city in which the restaurant is located, the location itself, cuisine etc. Curiously enough a better aggregate rating should also contribute to the number of footfalls. 


```python
sns.barplot(x ="Price range", y = 'Votes', data= data_rated_restaurants, palette="Blues_d" )
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d4153e8278>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_50_1.png)


The more pricier restaurants seems to have larger number of reviews which implies the pricier restaurants seems to have a better footfall. It might be also possible that people who visit these restaurants are more internet savvy and likely to leave a review. Let us look at the relation between average rating and number of votes:


```python
sns.set(font_scale=1.5, rc={'figure.figsize':(12,6)})
sns.barplot('Aggregate rating', 'Votes', data= data_rated_restaurants, palette="Blues_d")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1d4153ffcf8>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_52_1.png)


The above plot shows that the average votes received are disproportionately higher in the case of restaurants with higher ratings. Also the variability of footfalls is also large for the highly rated restaurants. This might be because there might be new restaurants which has better aggregate rating. However since the data does not mention the period during which the restaurants were obtained this is just a speculation. The location of the restaurant, how easily it is accessible by public transport, the population density in the region where it is located, whether it is closer to the center of the city etc. will also similarly play a role in determining the number of footfalls. 


```python
sns.set(font_scale=1.5, rc={'figure.figsize':(12,6)})
sns.barplot('No. of cuisines', 'Votes', data= data_rated_restaurants, palette="Blues_d")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c2c607b8>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_54_1.png)


The average votes received are also higher for restaurants that are multi-cuisine. It would be also be interesting to see the voting amoung different number of cuisines provided.


```python
sns.barplot('No. of cuisines', 'Votes', hue= 'Price range', data= data_rated_restaurants, palette=sns.color_palette("Blues_d"))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f84c0e57390>




![png](Finding%20the%20best%20restuarants_files/Finding%20the%20best%20restuarants_56_1.png)


### Cities 
It would also be interesting to know the cities in India where these restaurants are located. 


```python
print(data_modified['City'].unique())
print(data_modified['City'].nunique())
```

    ['Agra' 'Ahmedabad' 'Allahabad' 'Amritsar' 'Aurangabad' 'Bangalore'
     'Bhopal' 'Bhubaneshwar' 'Chandigarh' 'Chennai' 'Coimbatore' 'Dehradun'
     'Faridabad' 'Ghaziabad' 'Goa' 'Gurgaon' 'Guwahati' 'Hyderabad' 'Indore'
     'Jaipur' 'Kanpur' 'Kochi' 'Kolkata' 'Lucknow' 'Ludhiana' 'Mangalore'
     'Mohali' 'Mumbai' 'Mysore' 'Nagpur' 'Nashik' 'New Delhi' 'Noida'
     'Panchkula' 'Patna' 'Puducherry' 'Pune' 'Ranchi' 'Secunderabad' 'Surat'
     'Vadodara' 'Varanasi' 'Vizag']
    43
    

It would be interesting to see which in which people use Zomato to rate the restaurants. The total number of votes in each city could also be a measure of the popularity of Zomato in that city (or state in which the city is located). 

### Most Popular cuisines

In the above restaurants we have both single cuisine and multi-cuisine restaurants. We first need to make the list of all available cuisines across all restaurants. I will try to generate the list of cuisines that are provided by all the restaurants above.


```python
cuisine = []
list_of_cuisines = data_modified.loc[:,9]
len(list_of_cuisines)
```




    8652




```python
for x in list_of_cuisines:
    y = x.split(', ')
    for i in y:
        if not(i in cuisine):
            cuisine.append(i)
        else:
            pass
        
```


```python
print(cuisine, 'total no. of cuisines {}'.format(len(cuisine)))
```

    ['North Indian', 'Mughlai', 'Rajasthani', 'Chinese', 'European', 'Gujarati', 'Continental', 'South Indian', 'Desserts', 'Cafe', 'Italian', 'Mexican', 'Pizza', 'Fast Food', 'Mediterranean', 'Thai', 'Ice Cream', 'Beverages', 'Asian', 'Street Food', 'Sandwich', 'Burger', 'Healthy Food', 'American', 'Armenian', 'Salad', 'Bakery', 'Mithai', 'Biryani', 'Juices', 'Maharashtrian', 'Hyderabadi', 'Modern Indian', 'Finger Food', 'Tex-Mex', 'Arabian', 'Charcoal Grill', 'Steak', 'Seafood', 'Tea', 'Japanese', 'Malaysian', 'Burmese', 'Chettinad', 'Spanish', 'Greek', 'Indian', 'Parsi', 'Tibetan', 'Raw Meats', 'French', 'Goan', 'German', 'Kerala', 'Lebanese', 'Belgian', 'Kashmiri', 'Sushi', 'South American', 'Persian', 'Bengali', 'Portuguese', 'African', 'Iranian', 'Vietnamese', 'Lucknowi', 'Korean', 'Awadhi', 'Nepalese', 'Drinks Only', 'Pakistani', 'North Eastern', 'Oriya', 'Bihari', 'Afghani', 'Middle Eastern', 'Indonesian', 'Assamese', 'Andhra', 'Mangalorean', 'British', 'Malwani', 'Cuisine Varies', 'Turkish', 'Moroccan', 'Naga', 'Deli', 'Sri Lankan', 'BBQ', 'Cajun'] total no. of cuisines 90
    

Above you can see the list of all cuisines that are served by the restaurants that have been rated by Zomato. There are in total 90 types of cuisines available in total. It would be really interesting to see which cuisines are the most popular among restaurant goers according to Zomato. This is a little challenging as seen from the analysis before as the most popular restaurants are most likely a multi-cuisine restaurant. It is most likely that a combination of particular cuisines seems to be the key to a successful rating. It is also possible that the popular multi-cuisine restaurants have one or two common cuisines. Cuisines served can definitely provide a clue to understanding the popularity of a restaurant.


```python
list_ratings = data_modified.loc[:,'Aggregate rating']
len(list_ratings)
```




    8652




```python
ratings_per_cusine = []
for cusi in cuisine:
    count = 0
    total_rating = 0
    for cuisines, ratings in zip(list_of_cuisines, list_ratings):
        if cusi in cuisines.split(', '):
            count+=1
            total_rating+=ratings
        else:
            pass
    ratings_per_cusine.append(total_rating/count)
            
```


```python
len(ratings_per_cusine)
```




    90




```python
ratings_cuisines = pd.DataFrame({'Cuisines':cuisine, 'Average Rating':ratings_per_cusine}, )
```


```python
ratings_cuisines.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Rating</th>
      <th>Cuisines</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.505170</td>
      <td>North Indian</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.608073</td>
      <td>Mughlai</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.135000</td>
      <td>Rajasthani</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.598065</td>
      <td>Chinese</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.846218</td>
      <td>European</td>
    </tr>
  </tbody>
</table>
</div>




```python
import cufflinks as cf
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
init_notebook_mode(connected= True)
cf.go_offline()
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>


    IOPub data rate exceeded.
    The notebook server will temporarily stop sending output
    to the client in order to avoid crashing it.
    To change this limit, set the config variable
    `--NotebookApp.iopub_data_rate_limit`.
    


```python
import plotly.graph_objs as go
data = [go.Bar(
            x=ratings_cuisines['Cuisines'],
            y=ratings_cuisines['Average Rating']
    )]

iplot(data, filename='basic-bar')
```


<div id="5ea789fb-9b23-4fad-a9c7-49d2ddadca0e" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("5ea789fb-9b23-4fad-a9c7-49d2ddadca0e", [{"type": "bar", "x": ["North Indian", "Mughlai", "Rajasthani", "Chinese", "European", "Gujarati", "Continental", "South Indian", "Desserts", "Cafe", "Italian", "Mexican", "Pizza", "Fast Food", "Mediterranean", "Thai", "Ice Cream", "Beverages", "Asian", "Street Food", "Sandwich", "Burger", "Healthy Food", "American", "Armenian", "Salad", "Bakery", "Mithai", "Biryani", "Juices", "Maharashtrian", "Hyderabadi", "Modern Indian", "Finger Food", "Tex-Mex", "Arabian", "Charcoal Grill", "Steak", "Seafood", "Tea", "Japanese", "Malaysian", "Burmese", "Chettinad", "Spanish", "Greek", "Indian", "Parsi", "Tibetan", "Raw Meats", "French", "Goan", "German", "Kerala", "Lebanese", "Belgian", "Kashmiri", "Sushi", "South American", "Persian", "Bengali", "Portuguese", "African", "Iranian", "Vietnamese", "Lucknowi", "Korean", "Awadhi", "Nepalese", "Drinks Only", "Pakistani", "North Eastern", "Oriya", "Bihari", "Afghani", "Middle Eastern", "Indonesian", "Assamese", "Andhra", "Mangalorean", "British", "Malwani", "Cuisine Varies", "Turkish", "Moroccan", "Naga", "Deli", "Sri Lankan", "BBQ", "Cajun"], "y": [2.5051697921946277, 2.608072653884967, 3.1350000000000002, 2.598065476190476, 3.8462184873949568, 3.354545454545455, 3.507872928176794, 2.4589540412044393, 2.8587939698492466, 3.2320574162679407, 3.5005865102639304, 3.64923076923077, 2.69329073482428, 2.5497198166072343, 3.9311111111111123, 3.6004878048780493, 2.5124999999999997, 2.64139534883721, 3.7451612903225806, 2.3288808664259912, 3.8, 3.2666666666666675, 3.042465753424658, 3.4119999999999986, 1.3, 3.1489130434782613, 2.396137931034482, 2.0736842105263142, 2.401714285714287, 2.8076923076923084, 3.5700000000000003, 3.4959999999999996, 4.28125, 3.2385321100917452, 3.8687499999999995, 3.11, 4.175000000000001, 4.05, 3.6358024691358026, 2.711363636363636, 3.6109756097560957, 3.744444444444444, 4.05, 3.6727272727272724, 4.028571428571428, 3.9200000000000004, 2.1428571428571432, 4.1, 2.3022727272727277, 2.1526315789473682, 3.558333333333333, 3.952631578947368, 4.35, 3.4545454545454546, 3.3953846153846152, 3.9, 2.9349999999999996, 3.847619047619048, 3.5, 4.6, 3.006896551724139, 3.216666666666667, 3.1, 4.066666666666666, 3.828571428571428, 2.384615384615385, 3.45625, 1.5727272727272728, 1.6555555555555557, 1.75, 1.9, 1.7999999999999998, 3.25, 3.65, 1.4181818181818182, 3.8692307692307697, 4.0, 2.4, 3.87, 3.725, 3.9, 3.5, 0.0, 3.1874999999999996, 1.6200000000000003, 3.475, 3.8, 4.0, 3.6, 3.6]}], {}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>

