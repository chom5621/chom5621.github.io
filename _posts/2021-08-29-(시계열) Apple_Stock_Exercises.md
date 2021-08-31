---
layout: single
categories: ['ESAA']
tags: ['python','time series','pandas','numpy','matplotlib']
last_modified_at : 2021-04-08
---

# Apple Stock

### Introduction:

We are going to use Apple's stock price.


### Step 1. Import the necessary libraries


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### Step 2. Import the dataset from this [address](https://raw.githubusercontent.com/guipsamora/pandas_exercises/master/09_Time_Series/Apple_Stock/appl_1980_2014.csv)


```python
pd.read_csv("https://raw.githubusercontent.com/guipsamora/pandas_exercises/master/09_Time_Series/Apple_Stock/appl_1980_2014.csv")
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
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-07-08</td>
      <td>96.27</td>
      <td>96.80</td>
      <td>93.92</td>
      <td>95.35</td>
      <td>65130000</td>
      <td>95.35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-07-07</td>
      <td>94.14</td>
      <td>95.99</td>
      <td>94.10</td>
      <td>95.97</td>
      <td>56305400</td>
      <td>95.97</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-07-03</td>
      <td>93.67</td>
      <td>94.10</td>
      <td>93.20</td>
      <td>94.03</td>
      <td>22891800</td>
      <td>94.03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2014-07-02</td>
      <td>93.87</td>
      <td>94.06</td>
      <td>93.09</td>
      <td>93.48</td>
      <td>28420900</td>
      <td>93.48</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-07-01</td>
      <td>93.52</td>
      <td>94.07</td>
      <td>93.13</td>
      <td>93.52</td>
      <td>38170200</td>
      <td>93.52</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8460</th>
      <td>1980-12-18</td>
      <td>26.63</td>
      <td>26.75</td>
      <td>26.63</td>
      <td>26.63</td>
      <td>18362400</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>8461</th>
      <td>1980-12-17</td>
      <td>25.87</td>
      <td>26.00</td>
      <td>25.87</td>
      <td>25.87</td>
      <td>21610400</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>8462</th>
      <td>1980-12-16</td>
      <td>25.37</td>
      <td>25.37</td>
      <td>25.25</td>
      <td>25.25</td>
      <td>26432000</td>
      <td>0.39</td>
    </tr>
    <tr>
      <th>8463</th>
      <td>1980-12-15</td>
      <td>27.38</td>
      <td>27.38</td>
      <td>27.25</td>
      <td>27.25</td>
      <td>43971200</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>8464</th>
      <td>1980-12-12</td>
      <td>28.75</td>
      <td>28.87</td>
      <td>28.75</td>
      <td>28.75</td>
      <td>117258400</td>
      <td>0.45</td>
    </tr>
  </tbody>
</table>
<p>8465 rows × 7 columns</p>
</div>



### Step 3. Assign it to a variable apple


```python
apple = pd.read_csv("https://raw.githubusercontent.com/guipsamora/pandas_exercises/master/09_Time_Series/Apple_Stock/appl_1980_2014.csv")
```

### Step 4.  Check out the type of the columns


```python
apple.dtypes
```




    Date          object
    Open         float64
    High         float64
    Low          float64
    Close        float64
    Volume         int64
    Adj Close    float64
    dtype: object



### Step 5. Transform the Date column as a datetime type


```python
apple.Date = pd.to_datetime(apple.Date)
```


```python
apple.dtypes
```




    Date         datetime64[ns]
    Open                float64
    High                float64
    Low                 float64
    Close               float64
    Volume                int64
    Adj Close           float64
    dtype: object



### Step 6.  Set the date as the index


```python
apple = apple.set_index("Date")
apple
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
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2014-07-08</th>
      <td>96.27</td>
      <td>96.80</td>
      <td>93.92</td>
      <td>95.35</td>
      <td>65130000</td>
      <td>95.35</td>
    </tr>
    <tr>
      <th>2014-07-07</th>
      <td>94.14</td>
      <td>95.99</td>
      <td>94.10</td>
      <td>95.97</td>
      <td>56305400</td>
      <td>95.97</td>
    </tr>
    <tr>
      <th>2014-07-03</th>
      <td>93.67</td>
      <td>94.10</td>
      <td>93.20</td>
      <td>94.03</td>
      <td>22891800</td>
      <td>94.03</td>
    </tr>
    <tr>
      <th>2014-07-02</th>
      <td>93.87</td>
      <td>94.06</td>
      <td>93.09</td>
      <td>93.48</td>
      <td>28420900</td>
      <td>93.48</td>
    </tr>
    <tr>
      <th>2014-07-01</th>
      <td>93.52</td>
      <td>94.07</td>
      <td>93.13</td>
      <td>93.52</td>
      <td>38170200</td>
      <td>93.52</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1980-12-18</th>
      <td>26.63</td>
      <td>26.75</td>
      <td>26.63</td>
      <td>26.63</td>
      <td>18362400</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>1980-12-17</th>
      <td>25.87</td>
      <td>26.00</td>
      <td>25.87</td>
      <td>25.87</td>
      <td>21610400</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>1980-12-16</th>
      <td>25.37</td>
      <td>25.37</td>
      <td>25.25</td>
      <td>25.25</td>
      <td>26432000</td>
      <td>0.39</td>
    </tr>
    <tr>
      <th>1980-12-15</th>
      <td>27.38</td>
      <td>27.38</td>
      <td>27.25</td>
      <td>27.25</td>
      <td>43971200</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>1980-12-12</th>
      <td>28.75</td>
      <td>28.87</td>
      <td>28.75</td>
      <td>28.75</td>
      <td>117258400</td>
      <td>0.45</td>
    </tr>
  </tbody>
</table>
<p>8465 rows × 6 columns</p>
</div>



### Step 7.  Is there any duplicate dates?


```python
dup = apple[apple.index.duplicated]
if len(dup) != 0:
    print("Yes, there are duplicate dates.")
else:
    print("No, there are no duplicate dates.")
```

    No, there are no duplicate dates.
    

### Step 8.  Ops...it seems the index is from the most recent date. Make the first entry the oldest date.


```python
apple = apple.sort_values(by="Date", ascending=True)
apple
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
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980-12-12</th>
      <td>28.75</td>
      <td>28.87</td>
      <td>28.75</td>
      <td>28.75</td>
      <td>117258400</td>
      <td>0.45</td>
    </tr>
    <tr>
      <th>1980-12-15</th>
      <td>27.38</td>
      <td>27.38</td>
      <td>27.25</td>
      <td>27.25</td>
      <td>43971200</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>1980-12-16</th>
      <td>25.37</td>
      <td>25.37</td>
      <td>25.25</td>
      <td>25.25</td>
      <td>26432000</td>
      <td>0.39</td>
    </tr>
    <tr>
      <th>1980-12-17</th>
      <td>25.87</td>
      <td>26.00</td>
      <td>25.87</td>
      <td>25.87</td>
      <td>21610400</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>1980-12-18</th>
      <td>26.63</td>
      <td>26.75</td>
      <td>26.63</td>
      <td>26.63</td>
      <td>18362400</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2014-07-01</th>
      <td>93.52</td>
      <td>94.07</td>
      <td>93.13</td>
      <td>93.52</td>
      <td>38170200</td>
      <td>93.52</td>
    </tr>
    <tr>
      <th>2014-07-02</th>
      <td>93.87</td>
      <td>94.06</td>
      <td>93.09</td>
      <td>93.48</td>
      <td>28420900</td>
      <td>93.48</td>
    </tr>
    <tr>
      <th>2014-07-03</th>
      <td>93.67</td>
      <td>94.10</td>
      <td>93.20</td>
      <td>94.03</td>
      <td>22891800</td>
      <td>94.03</td>
    </tr>
    <tr>
      <th>2014-07-07</th>
      <td>94.14</td>
      <td>95.99</td>
      <td>94.10</td>
      <td>95.97</td>
      <td>56305400</td>
      <td>95.97</td>
    </tr>
    <tr>
      <th>2014-07-08</th>
      <td>96.27</td>
      <td>96.80</td>
      <td>93.92</td>
      <td>95.35</td>
      <td>65130000</td>
      <td>95.35</td>
    </tr>
  </tbody>
</table>
<p>8465 rows × 6 columns</p>
</div>



### Step 9. Get the last business day of each month


```python
apple_month = apple.resample("BM").mean()
apple_month
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
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980-12-31</th>
      <td>30.481538</td>
      <td>30.567692</td>
      <td>30.443077</td>
      <td>30.443077</td>
      <td>2.586252e+07</td>
      <td>0.473077</td>
    </tr>
    <tr>
      <th>1981-01-30</th>
      <td>31.754762</td>
      <td>31.826667</td>
      <td>31.654762</td>
      <td>31.654762</td>
      <td>7.249867e+06</td>
      <td>0.493810</td>
    </tr>
    <tr>
      <th>1981-02-27</th>
      <td>26.480000</td>
      <td>26.572105</td>
      <td>26.407895</td>
      <td>26.407895</td>
      <td>4.231832e+06</td>
      <td>0.411053</td>
    </tr>
    <tr>
      <th>1981-03-31</th>
      <td>24.937727</td>
      <td>25.016818</td>
      <td>24.836364</td>
      <td>24.836364</td>
      <td>7.962691e+06</td>
      <td>0.387727</td>
    </tr>
    <tr>
      <th>1981-04-30</th>
      <td>27.286667</td>
      <td>27.368095</td>
      <td>27.227143</td>
      <td>27.227143</td>
      <td>6.392000e+06</td>
      <td>0.423333</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2014-03-31</th>
      <td>533.593333</td>
      <td>536.453810</td>
      <td>530.070952</td>
      <td>533.214286</td>
      <td>5.954403e+07</td>
      <td>75.750000</td>
    </tr>
    <tr>
      <th>2014-04-30</th>
      <td>540.081905</td>
      <td>544.349048</td>
      <td>536.262381</td>
      <td>541.074286</td>
      <td>7.660787e+07</td>
      <td>76.867143</td>
    </tr>
    <tr>
      <th>2014-05-30</th>
      <td>601.301905</td>
      <td>606.372857</td>
      <td>598.332857</td>
      <td>603.195714</td>
      <td>6.828177e+07</td>
      <td>86.058571</td>
    </tr>
    <tr>
      <th>2014-06-30</th>
      <td>222.360000</td>
      <td>224.084286</td>
      <td>220.735714</td>
      <td>222.658095</td>
      <td>5.745506e+07</td>
      <td>91.885714</td>
    </tr>
    <tr>
      <th>2014-07-31</th>
      <td>94.294000</td>
      <td>95.004000</td>
      <td>93.488000</td>
      <td>94.470000</td>
      <td>4.218366e+07</td>
      <td>94.470000</td>
    </tr>
  </tbody>
</table>
<p>404 rows × 6 columns</p>
</div>



### Step 10.  What is the difference in days between the first day and the oldest


```python
diff = apple.index.max() - apple.index.min() 
diff.days
```




    12261



### Step 11.  How many months in the data we have?


```python
apple_month = apple.resample("BM").mean()
len(apple_month.index)
```




    404



### Step 12. Plot the 'Adj Close' value. Set the size of the figure to 13.5 x 9 inches


```python
data = apple['Adj Close'].plot()
fig = data.get_figure()
fig.set_size_inches(13.5, 9)
```


![png](/assets/images/output_25_0.png)


### BONUS: Create your own question and answer it.

### Plot the total 'volume' of Apple Stock being traded each day. Set the size of the figure to 13.5 x 9 inches


```python
data2 = apple['Volume'].plot()
fig = data2.get_figure()
fig.set_size_inches(13.5, 9)
```


![png](/assets/images/output_28_0.png)


### Plot the price-earnings ratio each day. Set the size of the figure to 13.5 x 9 inches


```python
apple['Price_lag_1'] = apple['Adj Close'].shift(-1)
apple['Price_diff_1'] = apple['Price_lag_1'] - apple['Adj Close']
apple['Daily_return'] = apple['Price_diff_1'] / apple['Adj Close']
apple['Updown'] = [1 if apple['Daily_return'].loc[Date] > 0 else 0 for Date in apple.index]

apple_chart = apple[['Adj Close','Price_lag_1','Price_diff_1','Daily_return','Updown']]
apple_chart.head()
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
      <th>Adj Close</th>
      <th>Price_lag_1</th>
      <th>Price_diff_1</th>
      <th>Daily_return</th>
      <th>Updown</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980-12-12</th>
      <td>0.45</td>
      <td>0.42</td>
      <td>-0.03</td>
      <td>-0.066667</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1980-12-15</th>
      <td>0.42</td>
      <td>0.39</td>
      <td>-0.03</td>
      <td>-0.071429</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1980-12-16</th>
      <td>0.39</td>
      <td>0.40</td>
      <td>0.01</td>
      <td>0.025641</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1980-12-17</th>
      <td>0.40</td>
      <td>0.41</td>
      <td>0.01</td>
      <td>0.025000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1980-12-18</th>
      <td>0.41</td>
      <td>0.44</td>
      <td>0.03</td>
      <td>0.073171</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
apple['Updown'].value_counts() #주가 상승일(1), 하락일(0)의 수
```




    0    4744
    1    3721
    Name: Updown, dtype: int64




```python
plt.figure(figsize=(13.5, 9))
plt.plot(apple.index, apple['Daily_return'])
plt.axhline(y=0, color='red', ls='--')
plt.show() #일일 주가 수익률 그래프
```


![png](/assets/images/output_32_0.png)



```python

```
