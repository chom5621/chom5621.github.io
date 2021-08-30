---
layout: single
categories: ['K-DATA']
tags: ['python','plotly','dash']
last_modified_at : 2021-07-09
---

```python
import pandas as pd
from pandas import Series, DataFrame
```

## 1. Covid-19 데이터 가져오기
- https://github.com/owid/covid-19-data/tree/master/public/data : 매일 업데이트된 파일을 제공함


```python
covid = pd.read_excel('data/owid-covid-data.xlsx')
```

## 2. 데이터 탐색 및 전처리

##### 데이터 크기, 컬럼들의 개수와 타입 등 확인


```python
covid.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 70851 entries, 0 to 70850
    Data columns (total 59 columns):
     #   Column                                 Non-Null Count  Dtype  
    ---  ------                                 --------------  -----  
     0   iso_code                               70851 non-null  object 
     1   continent                              67325 non-null  object 
     2   location                               70851 non-null  object 
     3   date                                   70851 non-null  object 
     4   total_cases                            69938 non-null  float64
     5   new_cases                              69936 non-null  float64
     6   new_cases_smoothed                     68935 non-null  float64
     7   total_deaths                           60914 non-null  float64
     8   new_deaths                             61072 non-null  float64
     9   new_deaths_smoothed                    68935 non-null  float64
     10  total_cases_per_million                69555 non-null  float64
     11  new_cases_per_million                  69553 non-null  float64
     12  new_cases_smoothed_per_million         68557 non-null  float64
     13  total_deaths_per_million               60544 non-null  float64
     14  new_deaths_per_million                 60702 non-null  float64
     15  new_deaths_smoothed_per_million        68557 non-null  float64
     16  reproduction_rate                      56738 non-null  float64
     17  icu_patients                           7473 non-null   float64
     18  icu_patients_per_million               7473 non-null   float64
     19  hosp_patients                          8882 non-null   float64
     20  hosp_patients_per_million              8882 non-null   float64
     21  weekly_icu_admissions                  702 non-null    float64
     22  weekly_icu_admissions_per_million      702 non-null    float64
     23  weekly_hosp_admissions                 1155 non-null   float64
     24  weekly_hosp_admissions_per_million     1155 non-null   float64
     25  new_tests                              32012 non-null  float64
     26  total_tests                            31878 non-null  float64
     27  total_tests_per_thousand               31878 non-null  float64
     28  new_tests_per_thousand                 32012 non-null  float64
     29  new_tests_smoothed                     36506 non-null  float64
     30  new_tests_smoothed_per_thousand        36506 non-null  float64
     31  positive_rate                          35076 non-null  float64
     32  tests_per_case                         34557 non-null  float64
     33  tests_units                            37791 non-null  object 
     34  total_vaccinations                     2585 non-null   float64
     35  people_vaccinated                      2175 non-null   float64
     36  people_fully_vaccinated                1400 non-null   float64
     37  new_vaccinations                       2192 non-null   float64
     38  new_vaccinations_smoothed              3742 non-null   float64
     39  total_vaccinations_per_hundred         2585 non-null   float64
     40  people_vaccinated_per_hundred          2175 non-null   float64
     41  people_fully_vaccinated_per_hundred    1400 non-null   float64
     42  new_vaccinations_smoothed_per_million  3742 non-null   float64
     43  stringency_index                       61003 non-null  float64
     44  population                             70459 non-null  float64
     45  population_density                     66235 non-null  float64
     46  median_age                             64491 non-null  float64
     47  aged_65_older                          63747 non-null  float64
     48  aged_70_older                          64127 non-null  float64
     49  gdp_per_capita                         64654 non-null  float64
     50  extreme_poverty                        44174 non-null  float64
     51  cardiovasc_death_rate                  65261 non-null  float64
     52  diabetes_prevalence                    66068 non-null  float64
     53  female_smokers                         51381 non-null  float64
     54  male_smokers                           50676 non-null  float64
     55  handwashing_facilities                 32737 non-null  float64
     56  hospital_beds_per_thousand             59816 non-null  float64
     57  life_expectancy                        67333 non-null  float64
     58  human_development_index                65240 non-null  float64
    dtypes: float64(54), object(5)
    memory usage: 31.9+ MB
    

##### date 컬럼을 datetime으로 변경하기


```python
covid['date'] =  pd.to_datetime(covid['date'], format='%Y-%m-%d')
```


```python
covid.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 70851 entries, 0 to 70850
    Data columns (total 59 columns):
     #   Column                                 Non-Null Count  Dtype         
    ---  ------                                 --------------  -----         
     0   iso_code                               70851 non-null  object        
     1   continent                              67325 non-null  object        
     2   location                               70851 non-null  object        
     3   date                                   70851 non-null  datetime64[ns]
     4   total_cases                            69938 non-null  float64       
     5   new_cases                              69936 non-null  float64       
     6   new_cases_smoothed                     68935 non-null  float64       
     7   total_deaths                           60914 non-null  float64       
     8   new_deaths                             61072 non-null  float64       
     9   new_deaths_smoothed                    68935 non-null  float64       
     10  total_cases_per_million                69555 non-null  float64       
     11  new_cases_per_million                  69553 non-null  float64       
     12  new_cases_smoothed_per_million         68557 non-null  float64       
     13  total_deaths_per_million               60544 non-null  float64       
     14  new_deaths_per_million                 60702 non-null  float64       
     15  new_deaths_smoothed_per_million        68557 non-null  float64       
     16  reproduction_rate                      56738 non-null  float64       
     17  icu_patients                           7473 non-null   float64       
     18  icu_patients_per_million               7473 non-null   float64       
     19  hosp_patients                          8882 non-null   float64       
     20  hosp_patients_per_million              8882 non-null   float64       
     21  weekly_icu_admissions                  702 non-null    float64       
     22  weekly_icu_admissions_per_million      702 non-null    float64       
     23  weekly_hosp_admissions                 1155 non-null   float64       
     24  weekly_hosp_admissions_per_million     1155 non-null   float64       
     25  new_tests                              32012 non-null  float64       
     26  total_tests                            31878 non-null  float64       
     27  total_tests_per_thousand               31878 non-null  float64       
     28  new_tests_per_thousand                 32012 non-null  float64       
     29  new_tests_smoothed                     36506 non-null  float64       
     30  new_tests_smoothed_per_thousand        36506 non-null  float64       
     31  positive_rate                          35076 non-null  float64       
     32  tests_per_case                         34557 non-null  float64       
     33  tests_units                            37791 non-null  object        
     34  total_vaccinations                     2585 non-null   float64       
     35  people_vaccinated                      2175 non-null   float64       
     36  people_fully_vaccinated                1400 non-null   float64       
     37  new_vaccinations                       2192 non-null   float64       
     38  new_vaccinations_smoothed              3742 non-null   float64       
     39  total_vaccinations_per_hundred         2585 non-null   float64       
     40  people_vaccinated_per_hundred          2175 non-null   float64       
     41  people_fully_vaccinated_per_hundred    1400 non-null   float64       
     42  new_vaccinations_smoothed_per_million  3742 non-null   float64       
     43  stringency_index                       61003 non-null  float64       
     44  population                             70459 non-null  float64       
     45  population_density                     66235 non-null  float64       
     46  median_age                             64491 non-null  float64       
     47  aged_65_older                          63747 non-null  float64       
     48  aged_70_older                          64127 non-null  float64       
     49  gdp_per_capita                         64654 non-null  float64       
     50  extreme_poverty                        44174 non-null  float64       
     51  cardiovasc_death_rate                  65261 non-null  float64       
     52  diabetes_prevalence                    66068 non-null  float64       
     53  female_smokers                         51381 non-null  float64       
     54  male_smokers                           50676 non-null  float64       
     55  handwashing_facilities                 32737 non-null  float64       
     56  hospital_beds_per_thousand             59816 non-null  float64       
     57  life_expectancy                        67333 non-null  float64       
     58  human_development_index                65240 non-null  float64       
    dtypes: datetime64[ns](1), float64(54), object(4)
    memory usage: 31.9+ MB
    

##### 년도(year), 달(month), 일(day), 주차(WeekNumber, %U)과 요일(weekDay, %a) 컬럼 추가하기


```python
covid['year'] = covid['date'].apply(lambda x: x.strftime('%Y'))
covid['month'] = covid['date'].apply(lambda x: x.strftime('%m'))
covid['day'] = covid['date'].apply(lambda x: x.strftime('%d'))
covid['weekNumber'] = covid['date'].apply(lambda x: x.strftime('%U'))
covid['weekDay'] = covid['date'].apply(lambda x: x.strftime('%a'))
```


```python
covid.head()
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>date</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2020-02-24</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>37.746</td>
      <td>0.5</td>
      <td>64.83</td>
      <td>0.511</td>
      <td>2020</td>
      <td>02</td>
      <td>24</td>
      <td>08</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2020-02-25</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>37.746</td>
      <td>0.5</td>
      <td>64.83</td>
      <td>0.511</td>
      <td>2020</td>
      <td>02</td>
      <td>25</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AFG</td>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2020-02-26</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>37.746</td>
      <td>0.5</td>
      <td>64.83</td>
      <td>0.511</td>
      <td>2020</td>
      <td>02</td>
      <td>26</td>
      <td>08</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AFG</td>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2020-02-27</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>37.746</td>
      <td>0.5</td>
      <td>64.83</td>
      <td>0.511</td>
      <td>2020</td>
      <td>02</td>
      <td>27</td>
      <td>08</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AFG</td>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2020-02-28</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>37.746</td>
      <td>0.5</td>
      <td>64.83</td>
      <td>0.511</td>
      <td>2020</td>
      <td>02</td>
      <td>28</td>
      <td>08</td>
      <td>Fri</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 64 columns</p>
</div>



##### date를 row index로 설정하기


```python
covid2 = covid.set_index(['date']).sort_index()
```


```python
type(covid2.index)
```




    pandas.core.indexes.datetimes.DatetimeIndex



##### 데이터 분석에 활용할 데이터의 범위 정하기 (나라, 기간)

1) 수집되고 있는 나라의 개수 확인


```python
covid2.location.unique().size
```




    214




```python
covid2[covid2.location == 'Asia']
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>total_cases_per_million</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2020-01-22</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>556.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.120</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020</td>
      <td>01</td>
      <td>22</td>
      <td>03</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-01-23</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>654.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>18.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0.141</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020</td>
      <td>01</td>
      <td>23</td>
      <td>03</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-01-24</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>937.0</td>
      <td>283.0</td>
      <td>NaN</td>
      <td>26.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.202</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020</td>
      <td>01</td>
      <td>24</td>
      <td>03</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>1428.0</td>
      <td>491.0</td>
      <td>NaN</td>
      <td>42.0</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>0.308</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020</td>
      <td>01</td>
      <td>25</td>
      <td>03</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>2105.0</td>
      <td>677.0</td>
      <td>NaN</td>
      <td>56.0</td>
      <td>14.0</td>
      <td>NaN</td>
      <td>0.454</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020</td>
      <td>01</td>
      <td>26</td>
      <td>04</td>
      <td>Sun</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-02-19</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>24385712.0</td>
      <td>67228.0</td>
      <td>64754.857</td>
      <td>390453.0</td>
      <td>894.0</td>
      <td>838.429</td>
      <td>5255.714</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021</td>
      <td>02</td>
      <td>19</td>
      <td>07</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-02-20</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>24448415.0</td>
      <td>62703.0</td>
      <td>64704.571</td>
      <td>391362.0</td>
      <td>909.0</td>
      <td>845.857</td>
      <td>5269.228</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021</td>
      <td>02</td>
      <td>20</td>
      <td>07</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-02-21</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>24515896.0</td>
      <td>67481.0</td>
      <td>65962.571</td>
      <td>392079.0</td>
      <td>717.0</td>
      <td>842.143</td>
      <td>5283.772</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021</td>
      <td>02</td>
      <td>21</td>
      <td>08</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-02-22</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>24583145.0</td>
      <td>67249.0</td>
      <td>67088.429</td>
      <td>392833.0</td>
      <td>754.0</td>
      <td>849.714</td>
      <td>5298.266</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021</td>
      <td>02</td>
      <td>22</td>
      <td>08</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>OWID_ASI</td>
      <td>NaN</td>
      <td>Asia</td>
      <td>24656846.0</td>
      <td>73701.0</td>
      <td>67795.857</td>
      <td>393774.0</td>
      <td>941.0</td>
      <td>852.286</td>
      <td>5314.150</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
  </tbody>
</table>
<p>399 rows × 63 columns</p>
</div>




```python
# 더 정확히 알아보기
# 204개의 나라에서 데이터 수집 중
covid2[covid2.continent.notnull()].location.unique().size
```




    204



2) 데이터 수집 기간 확인


```python
print(covid2.index.min(), covid2.index.max())
```

    2020-01-01 00:00:00 2021-02-23 00:00:00
    

3) 날짜별 데이터 수집 개수 확인


```python
# 결과에 표현되는 최대 row 갯수
pd.set_option('display.max_rows', 500)

# 결과에 표현되는 최대 columns 갯수
#pd.set_option('display.max_columns', 100)
```


```python
covid2.index.value_counts().sort_index()
```




    2020-01-01      2
    2020-01-02      2
    2020-01-03      2
    2020-01-04      3
    2020-01-05      3
    2020-01-06      3
    2020-01-07      3
    2020-01-08      3
    2020-01-09      3
    2020-01-10      3
    2020-01-11      3
    2020-01-12      3
    2020-01-13      3
    2020-01-14      3
    2020-01-15      3
    2020-01-16      4
    2020-01-17      4
    2020-01-18      4
    2020-01-19      4
    2020-01-20      4
    2020-01-21      5
    2020-01-22     11
    2020-01-23     15
    2020-01-24     17
    2020-01-25     20
    2020-01-26     22
    2020-01-27     25
    2020-01-28     25
    2020-01-29     27
    2020-01-30     30
    2020-01-31     34
    2020-02-01     36
    2020-02-02     42
    2020-02-03     38
    2020-02-04     39
    2020-02-05     39
    2020-02-06     40
    2020-02-07     43
    2020-02-08     43
    2020-02-09     49
    2020-02-10     43
    2020-02-11     43
    2020-02-12     43
    2020-02-13     44
    2020-02-14     45
    2020-02-15     45
    2020-02-16     50
    2020-02-17     46
    2020-02-18     46
    2020-02-19     47
    2020-02-20     48
    2020-02-21     49
    2020-02-22     50
    2020-02-23     58
    2020-02-24     58
    2020-02-25     63
    2020-02-26     69
    2020-02-27     72
    2020-02-28     76
    2020-02-29     82
    2020-03-01     87
    2020-03-02     90
    2020-03-03     93
    2020-03-04     99
    2020-03-05    101
    2020-03-06    109
    2020-03-07    110
    2020-03-08    113
    2020-03-09    116
    2020-03-10    118
    2020-03-11    124
    2020-03-12    127
    2020-03-13    132
    2020-03-14    145
    2020-03-15    149
    2020-03-16    154
    2020-03-17    157
    2020-03-18    161
    2020-03-19    164
    2020-03-20    171
    2020-03-21    173
    2020-03-22    178
    2020-03-23    179
    2020-03-24    180
    2020-03-25    183
    2020-03-26    183
    2020-03-27    184
    2020-03-28    184
    2020-03-29    185
    2020-03-30    186
    2020-03-31    189
    2020-04-01    188
    2020-04-02    188
    2020-04-03    188
    2020-04-04    188
    2020-04-05    189
    2020-04-06    190
    2020-04-07    190
    2020-04-08    190
    2020-04-09    190
    2020-04-10    191
    2020-04-11    191
    2020-04-12    191
    2020-04-13    191
    2020-04-14    191
    2020-04-15    191
    2020-04-16    191
    2020-04-17    191
    2020-04-18    191
    2020-04-19    191
    2020-04-20    191
    2020-04-21    191
    2020-04-22    191
    2020-04-23    191
    2020-04-24    191
    2020-04-25    191
    2020-04-26    191
    2020-04-27    191
    2020-04-28    191
    2020-04-29    191
    2020-04-30    194
    2020-05-01    193
    2020-05-02    193
    2020-05-03    193
    2020-05-04    193
    2020-05-05    193
    2020-05-06    193
    2020-05-07    193
    2020-05-08    193
    2020-05-09    193
    2020-05-10    193
    2020-05-11    193
    2020-05-12    193
    2020-05-13    194
    2020-05-14    194
    2020-05-15    194
    2020-05-16    194
    2020-05-17    194
    2020-05-18    194
    2020-05-19    194
    2020-05-20    194
    2020-05-21    194
    2020-05-22    194
    2020-05-23    194
    2020-05-24    194
    2020-05-25    194
    2020-05-26    194
    2020-05-27    194
    2020-05-28    194
    2020-05-29    194
    2020-05-30    194
    2020-05-31    195
    2020-06-01    194
    2020-06-02    194
    2020-06-03    194
    2020-06-04    194
    2020-06-05    194
    2020-06-06    194
    2020-06-07    194
    2020-06-08    194
    2020-06-09    194
    2020-06-10    194
    2020-06-11    194
    2020-06-12    194
    2020-06-13    194
    2020-06-14    194
    2020-06-15    194
    2020-06-16    194
    2020-06-17    194
    2020-06-18    194
    2020-06-19    194
    2020-06-20    194
    2020-06-21    194
    2020-06-22    194
    2020-06-23    194
    2020-06-24    194
    2020-06-25    194
    2020-06-26    194
    2020-06-27    194
    2020-06-28    194
    2020-06-29    194
    2020-06-30    195
    2020-07-01    194
    2020-07-02    194
    2020-07-03    194
    2020-07-04    194
    2020-07-05    194
    2020-07-06    194
    2020-07-07    194
    2020-07-08    194
    2020-07-09    194
    2020-07-10    194
    2020-07-11    194
    2020-07-12    194
    2020-07-13    194
    2020-07-14    194
    2020-07-15    194
    2020-07-16    194
    2020-07-17    194
    2020-07-18    194
    2020-07-19    194
    2020-07-20    194
    2020-07-21    194
    2020-07-22    194
    2020-07-23    194
    2020-07-24    194
    2020-07-25    194
    2020-07-26    194
    2020-07-27    194
    2020-07-28    194
    2020-07-29    194
    2020-07-30    194
    2020-07-31    195
    2020-08-01    194
    2020-08-02    194
    2020-08-03    194
    2020-08-04    194
    2020-08-05    194
    2020-08-06    194
    2020-08-07    194
    2020-08-08    194
    2020-08-09    194
    2020-08-10    194
    2020-08-11    194
    2020-08-12    194
    2020-08-13    194
    2020-08-14    194
    2020-08-15    194
    2020-08-16    194
    2020-08-17    194
    2020-08-18    194
    2020-08-19    194
    2020-08-20    194
    2020-08-21    194
    2020-08-22    194
    2020-08-23    194
    2020-08-24    194
    2020-08-25    194
    2020-08-26    194
    2020-08-27    194
    2020-08-28    194
    2020-08-29    194
    2020-08-30    194
    2020-08-31    195
    2020-09-01    195
    2020-09-02    195
    2020-09-03    195
    2020-09-04    195
    2020-09-05    195
    2020-09-06    195
    2020-09-07    195
    2020-09-08    195
    2020-09-09    195
    2020-09-10    195
    2020-09-11    195
    2020-09-12    195
    2020-09-13    195
    2020-09-14    195
    2020-09-15    195
    2020-09-16    195
    2020-09-17    195
    2020-09-18    195
    2020-09-19    195
    2020-09-20    195
    2020-09-21    195
    2020-09-22    195
    2020-09-23    195
    2020-09-24    195
    2020-09-25    195
    2020-09-26    195
    2020-09-27    195
    2020-09-28    195
    2020-09-29    195
    2020-09-30    195
    2020-10-01    195
    2020-10-02    195
    2020-10-03    195
    2020-10-04    195
    2020-10-05    195
    2020-10-06    195
    2020-10-07    195
    2020-10-08    195
    2020-10-09    195
    2020-10-10    195
    2020-10-11    195
    2020-10-12    196
    2020-10-13    196
    2020-10-14    196
    2020-10-15    196
    2020-10-16    196
    2020-10-17    196
    2020-10-18    196
    2020-10-19    196
    2020-10-20    196
    2020-10-21    196
    2020-10-22    196
    2020-10-23    196
    2020-10-24    196
    2020-10-25    196
    2020-10-26    196
    2020-10-27    196
    2020-10-28    197
    2020-10-29    197
    2020-10-30    197
    2020-10-31    197
    2020-11-01    196
    2020-11-02    196
    2020-11-03    196
    2020-11-04    196
    2020-11-05    196
    2020-11-06    196
    2020-11-07    196
    2020-11-08    196
    2020-11-09    196
    2020-11-10    197
    2020-11-11    197
    2020-11-12    197
    2020-11-13    197
    2020-11-14    197
    2020-11-15    197
    2020-11-16    197
    2020-11-17    197
    2020-11-18    198
    2020-11-19    198
    2020-11-20    198
    2020-11-21    198
    2020-11-22    198
    2020-11-23    198
    2020-11-24    198
    2020-11-25    198
    2020-11-26    198
    2020-11-27    198
    2020-11-28    198
    2020-11-29    198
    2020-11-30    199
    2020-12-01    198
    2020-12-02    198
    2020-12-03    198
    2020-12-04    198
    2020-12-05    198
    2020-12-06    198
    2020-12-07    198
    2020-12-08    198
    2020-12-09    198
    2020-12-10    198
    2020-12-11    198
    2020-12-12    198
    2020-12-13    198
    2020-12-14    198
    2020-12-15    198
    2020-12-16    198
    2020-12-17    198
    2020-12-18    198
    2020-12-19    198
    2020-12-20    198
    2020-12-21    198
    2020-12-22    198
    2020-12-23    198
    2020-12-24    198
    2020-12-25    198
    2020-12-26    199
    2020-12-27    199
    2020-12-28    199
    2020-12-29    199
    2020-12-30    199
    2020-12-31    200
    2021-01-01    199
    2021-01-02    199
    2021-01-03    199
    2021-01-04    199
    2021-01-05    199
    2021-01-06    199
    2021-01-07    199
    2021-01-08    199
    2021-01-09    199
    2021-01-10    202
    2021-01-11    202
    2021-01-12    202
    2021-01-13    202
    2021-01-14    203
    2021-01-15    203
    2021-01-16    203
    2021-01-17    203
    2021-01-18    203
    2021-01-19    203
    2021-01-20    203
    2021-01-21    205
    2021-01-22    205
    2021-01-23    204
    2021-01-24    206
    2021-01-25    206
    2021-01-26    206
    2021-01-27    207
    2021-01-28    206
    2021-01-29    207
    2021-01-30    207
    2021-01-31    208
    2021-02-01    208
    2021-02-02    208
    2021-02-03    208
    2021-02-04    208
    2021-02-05    208
    2021-02-06    208
    2021-02-07    209
    2021-02-08    210
    2021-02-09    209
    2021-02-10    209
    2021-02-11    209
    2021-02-12    209
    2021-02-13    209
    2021-02-14    209
    2021-02-15    206
    2021-02-16    204
    2021-02-17    204
    2021-02-18    204
    2021-02-19    203
    2021-02-20    203
    2021-02-21    203
    2021-02-22    203
    2021-02-23    202
    Name: date, dtype: int64



4) 코로나 바이러스가 가장 많이 걸린 상위 100개 나라만 선택


```python
covid2[covid2.location == 'South Korea'] # total_cases가 누적 값인 것을 확인
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>total_cases_per_million</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2020-01-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>21</td>
      <td>03</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-01-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.020</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>22</td>
      <td>03</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-01-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.020</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>23</td>
      <td>03</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-01-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.039</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>24</td>
      <td>03</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.039</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>25</td>
      <td>03</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.059</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>26</td>
      <td>04</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-01-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.078</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>27</td>
      <td>04</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-01-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.078</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>28</td>
      <td>04</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-01-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.078</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>29</td>
      <td>04</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-01-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.078</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>30</td>
      <td>04</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-01-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11.0</td>
      <td>7.0</td>
      <td>1.286</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.215</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>01</td>
      <td>31</td>
      <td>04</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-02-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>1.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.234</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>01</td>
      <td>04</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-02-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15.0</td>
      <td>3.0</td>
      <td>1.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.293</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>02</td>
      <td>05</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-02-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>1.571</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.293</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>03</td>
      <td>05</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-02-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.312</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>04</td>
      <td>05</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-02-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>19.0</td>
      <td>3.0</td>
      <td>2.143</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.371</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>05</td>
      <td>05</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-02-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23.0</td>
      <td>4.0</td>
      <td>2.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.449</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>06</td>
      <td>05</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-02-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24.0</td>
      <td>1.0</td>
      <td>1.857</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.468</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>07</td>
      <td>05</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-02-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24.0</td>
      <td>0.0</td>
      <td>1.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.468</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>08</td>
      <td>05</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-02-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25.0</td>
      <td>1.0</td>
      <td>1.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.488</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>09</td>
      <td>06</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-02-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.527</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>10</td>
      <td>06</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-02-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.546</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>11</td>
      <td>06</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-02-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>1.286</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.546</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>12</td>
      <td>06</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-02-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.546</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>13</td>
      <td>06</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-02-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.546</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>14</td>
      <td>06</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-02-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.546</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>15</td>
      <td>06</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-02-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>29.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.566</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>16</td>
      <td>07</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-02-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>30.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.585</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>17</td>
      <td>07</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-02-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>31.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.605</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>18</td>
      <td>07</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-02-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000</td>
      <td>0.605</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>19</td>
      <td>07</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-02-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>104.0</td>
      <td>73.0</td>
      <td>10.857</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>2.029</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>20</td>
      <td>07</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-02-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>204.0</td>
      <td>100.0</td>
      <td>25.143</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>3.979</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>21</td>
      <td>07</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-02-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>433.0</td>
      <td>229.0</td>
      <td>57.857</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>8.446</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>22</td>
      <td>07</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-02-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>602.0</td>
      <td>169.0</td>
      <td>81.857</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>0.857</td>
      <td>11.742</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-02-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>833.0</td>
      <td>231.0</td>
      <td>114.714</td>
      <td>8.0</td>
      <td>2.0</td>
      <td>1.143</td>
      <td>16.248</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>24</td>
      <td>08</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-02-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>977.0</td>
      <td>144.0</td>
      <td>135.143</td>
      <td>10.0</td>
      <td>2.0</td>
      <td>1.429</td>
      <td>19.056</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>25</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-02-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>1261.0</td>
      <td>284.0</td>
      <td>175.714</td>
      <td>12.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>24.596</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>26</td>
      <td>08</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-02-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>1766.0</td>
      <td>505.0</td>
      <td>237.429</td>
      <td>13.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>34.446</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>27</td>
      <td>08</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-02-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>2337.0</td>
      <td>571.0</td>
      <td>304.714</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>1.571</td>
      <td>45.583</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>28</td>
      <td>08</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-02-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>3150.0</td>
      <td>813.0</td>
      <td>388.143</td>
      <td>16.0</td>
      <td>3.0</td>
      <td>2.000</td>
      <td>61.440</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>02</td>
      <td>29</td>
      <td>08</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-03-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>3736.0</td>
      <td>586.0</td>
      <td>447.714</td>
      <td>17.0</td>
      <td>1.0</td>
      <td>1.571</td>
      <td>72.870</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>01</td>
      <td>09</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-03-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>4335.0</td>
      <td>599.0</td>
      <td>500.286</td>
      <td>28.0</td>
      <td>11.0</td>
      <td>2.857</td>
      <td>84.554</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>02</td>
      <td>09</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-03-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>5186.0</td>
      <td>851.0</td>
      <td>601.286</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>2.571</td>
      <td>101.152</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>03</td>
      <td>09</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-03-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>5621.0</td>
      <td>435.0</td>
      <td>622.857</td>
      <td>35.0</td>
      <td>7.0</td>
      <td>3.286</td>
      <td>109.637</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>04</td>
      <td>09</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-03-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>6088.0</td>
      <td>467.0</td>
      <td>617.429</td>
      <td>35.0</td>
      <td>0.0</td>
      <td>3.143</td>
      <td>118.746</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>05</td>
      <td>09</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-03-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>6593.0</td>
      <td>505.0</td>
      <td>608.000</td>
      <td>42.0</td>
      <td>7.0</td>
      <td>4.143</td>
      <td>128.596</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>06</td>
      <td>09</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7041.0</td>
      <td>448.0</td>
      <td>555.857</td>
      <td>44.0</td>
      <td>2.0</td>
      <td>4.000</td>
      <td>137.334</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>07</td>
      <td>09</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-03-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7314.0</td>
      <td>273.0</td>
      <td>511.143</td>
      <td>50.0</td>
      <td>6.0</td>
      <td>4.714</td>
      <td>142.659</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>08</td>
      <td>10</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-03-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7478.0</td>
      <td>164.0</td>
      <td>449.000</td>
      <td>53.0</td>
      <td>3.0</td>
      <td>3.571</td>
      <td>145.858</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>09</td>
      <td>10</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-03-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7513.0</td>
      <td>35.0</td>
      <td>332.429</td>
      <td>54.0</td>
      <td>1.0</td>
      <td>3.714</td>
      <td>146.540</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>10</td>
      <td>10</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-03-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7755.0</td>
      <td>242.0</td>
      <td>304.857</td>
      <td>60.0</td>
      <td>6.0</td>
      <td>3.571</td>
      <td>151.260</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>11</td>
      <td>10</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-03-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7869.0</td>
      <td>114.0</td>
      <td>254.429</td>
      <td>66.0</td>
      <td>6.0</td>
      <td>4.429</td>
      <td>153.484</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>12</td>
      <td>10</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-03-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>7979.0</td>
      <td>110.0</td>
      <td>198.000</td>
      <td>66.0</td>
      <td>0.0</td>
      <td>3.429</td>
      <td>155.630</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>13</td>
      <td>10</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8086.0</td>
      <td>107.0</td>
      <td>149.286</td>
      <td>72.0</td>
      <td>6.0</td>
      <td>4.000</td>
      <td>157.717</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>14</td>
      <td>10</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-03-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8162.0</td>
      <td>76.0</td>
      <td>121.143</td>
      <td>75.0</td>
      <td>3.0</td>
      <td>3.571</td>
      <td>159.199</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>15</td>
      <td>11</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-03-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8236.0</td>
      <td>74.0</td>
      <td>108.286</td>
      <td>75.0</td>
      <td>0.0</td>
      <td>3.143</td>
      <td>160.642</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>16</td>
      <td>11</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-03-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8320.0</td>
      <td>84.0</td>
      <td>115.286</td>
      <td>81.0</td>
      <td>6.0</td>
      <td>3.857</td>
      <td>162.281</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>17</td>
      <td>11</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-03-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8413.0</td>
      <td>93.0</td>
      <td>94.000</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>3.429</td>
      <td>164.095</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>18</td>
      <td>11</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-03-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8565.0</td>
      <td>152.0</td>
      <td>99.429</td>
      <td>91.0</td>
      <td>7.0</td>
      <td>3.571</td>
      <td>167.059</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>19</td>
      <td>11</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-03-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8652.0</td>
      <td>87.0</td>
      <td>96.143</td>
      <td>94.0</td>
      <td>3.0</td>
      <td>4.000</td>
      <td>168.756</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>20</td>
      <td>11</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8799.0</td>
      <td>147.0</td>
      <td>101.857</td>
      <td>102.0</td>
      <td>8.0</td>
      <td>4.286</td>
      <td>171.624</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>21</td>
      <td>11</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-03-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8961.0</td>
      <td>162.0</td>
      <td>114.143</td>
      <td>111.0</td>
      <td>9.0</td>
      <td>5.143</td>
      <td>174.783</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>22</td>
      <td>12</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-03-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>8961.0</td>
      <td>0.0</td>
      <td>103.571</td>
      <td>111.0</td>
      <td>0.0</td>
      <td>5.143</td>
      <td>174.783</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>23</td>
      <td>12</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-03-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9037.0</td>
      <td>76.0</td>
      <td>102.429</td>
      <td>120.0</td>
      <td>9.0</td>
      <td>5.571</td>
      <td>176.266</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>24</td>
      <td>12</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-03-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9137.0</td>
      <td>100.0</td>
      <td>103.429</td>
      <td>126.0</td>
      <td>6.0</td>
      <td>6.000</td>
      <td>178.216</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>25</td>
      <td>12</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-03-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9241.0</td>
      <td>104.0</td>
      <td>96.571</td>
      <td>131.0</td>
      <td>5.0</td>
      <td>5.714</td>
      <td>180.245</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>26</td>
      <td>12</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9332.0</td>
      <td>91.0</td>
      <td>97.143</td>
      <td>139.0</td>
      <td>8.0</td>
      <td>6.429</td>
      <td>182.020</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9478.0</td>
      <td>146.0</td>
      <td>97.000</td>
      <td>144.0</td>
      <td>5.0</td>
      <td>6.000</td>
      <td>184.867</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>28</td>
      <td>12</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-03-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9583.0</td>
      <td>105.0</td>
      <td>88.857</td>
      <td>152.0</td>
      <td>8.0</td>
      <td>5.857</td>
      <td>186.915</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>29</td>
      <td>13</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-03-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9661.0</td>
      <td>78.0</td>
      <td>100.000</td>
      <td>158.0</td>
      <td>6.0</td>
      <td>6.714</td>
      <td>188.437</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>30</td>
      <td>13</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-03-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9786.0</td>
      <td>125.0</td>
      <td>107.000</td>
      <td>162.0</td>
      <td>4.0</td>
      <td>6.000</td>
      <td>190.875</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>03</td>
      <td>31</td>
      <td>13</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-04-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9887.0</td>
      <td>101.0</td>
      <td>107.143</td>
      <td>165.0</td>
      <td>3.0</td>
      <td>5.571</td>
      <td>192.845</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>01</td>
      <td>13</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-04-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>9976.0</td>
      <td>89.0</td>
      <td>105.000</td>
      <td>169.0</td>
      <td>4.0</td>
      <td>5.429</td>
      <td>194.581</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>02</td>
      <td>13</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-04-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10062.0</td>
      <td>86.0</td>
      <td>104.286</td>
      <td>174.0</td>
      <td>5.0</td>
      <td>5.000</td>
      <td>196.258</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>03</td>
      <td>13</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-04-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10156.0</td>
      <td>94.0</td>
      <td>96.857</td>
      <td>177.0</td>
      <td>3.0</td>
      <td>4.714</td>
      <td>198.092</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>04</td>
      <td>13</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-04-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10237.0</td>
      <td>81.0</td>
      <td>93.429</td>
      <td>183.0</td>
      <td>6.0</td>
      <td>4.429</td>
      <td>199.672</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>05</td>
      <td>14</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-04-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10284.0</td>
      <td>47.0</td>
      <td>89.000</td>
      <td>186.0</td>
      <td>3.0</td>
      <td>4.000</td>
      <td>200.588</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>06</td>
      <td>14</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-04-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10331.0</td>
      <td>47.0</td>
      <td>77.857</td>
      <td>192.0</td>
      <td>6.0</td>
      <td>4.286</td>
      <td>201.505</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>07</td>
      <td>14</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-04-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10384.0</td>
      <td>53.0</td>
      <td>71.000</td>
      <td>200.0</td>
      <td>8.0</td>
      <td>5.000</td>
      <td>202.539</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>08</td>
      <td>14</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-04-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10423.0</td>
      <td>39.0</td>
      <td>63.857</td>
      <td>204.0</td>
      <td>4.0</td>
      <td>5.000</td>
      <td>203.300</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>09</td>
      <td>14</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-04-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10450.0</td>
      <td>27.0</td>
      <td>55.429</td>
      <td>208.0</td>
      <td>4.0</td>
      <td>4.857</td>
      <td>203.826</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>10</td>
      <td>14</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-04-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10480.0</td>
      <td>30.0</td>
      <td>46.286</td>
      <td>211.0</td>
      <td>3.0</td>
      <td>4.857</td>
      <td>204.411</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>11</td>
      <td>14</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-04-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10512.0</td>
      <td>32.0</td>
      <td>39.286</td>
      <td>214.0</td>
      <td>3.0</td>
      <td>4.429</td>
      <td>205.035</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>12</td>
      <td>15</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-04-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10537.0</td>
      <td>25.0</td>
      <td>36.143</td>
      <td>217.0</td>
      <td>3.0</td>
      <td>4.429</td>
      <td>205.523</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>13</td>
      <td>15</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-04-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10564.0</td>
      <td>27.0</td>
      <td>33.286</td>
      <td>222.0</td>
      <td>5.0</td>
      <td>4.286</td>
      <td>206.050</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>14</td>
      <td>15</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-04-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10591.0</td>
      <td>27.0</td>
      <td>29.571</td>
      <td>225.0</td>
      <td>3.0</td>
      <td>3.571</td>
      <td>206.576</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>15</td>
      <td>15</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-04-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10613.0</td>
      <td>22.0</td>
      <td>27.143</td>
      <td>229.0</td>
      <td>4.0</td>
      <td>3.571</td>
      <td>207.005</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>16</td>
      <td>15</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-04-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10635.0</td>
      <td>22.0</td>
      <td>26.429</td>
      <td>230.0</td>
      <td>1.0</td>
      <td>3.143</td>
      <td>207.435</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>17</td>
      <td>15</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-04-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10653.0</td>
      <td>18.0</td>
      <td>24.714</td>
      <td>232.0</td>
      <td>2.0</td>
      <td>3.000</td>
      <td>207.786</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>18</td>
      <td>15</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-04-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10661.0</td>
      <td>8.0</td>
      <td>21.286</td>
      <td>234.0</td>
      <td>2.0</td>
      <td>2.857</td>
      <td>207.942</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>19</td>
      <td>16</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-04-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10674.0</td>
      <td>13.0</td>
      <td>19.571</td>
      <td>236.0</td>
      <td>2.0</td>
      <td>2.714</td>
      <td>208.195</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>20</td>
      <td>16</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-04-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10683.0</td>
      <td>9.0</td>
      <td>17.000</td>
      <td>237.0</td>
      <td>1.0</td>
      <td>2.143</td>
      <td>208.371</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>21</td>
      <td>16</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-04-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10694.0</td>
      <td>11.0</td>
      <td>14.714</td>
      <td>238.0</td>
      <td>1.0</td>
      <td>1.857</td>
      <td>208.585</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>22</td>
      <td>16</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-04-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10708.0</td>
      <td>14.0</td>
      <td>13.571</td>
      <td>240.0</td>
      <td>2.0</td>
      <td>1.571</td>
      <td>208.858</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>23</td>
      <td>16</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-04-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10718.0</td>
      <td>10.0</td>
      <td>11.857</td>
      <td>240.0</td>
      <td>0.0</td>
      <td>1.429</td>
      <td>209.053</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>24</td>
      <td>16</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-04-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10728.0</td>
      <td>10.0</td>
      <td>10.714</td>
      <td>242.0</td>
      <td>2.0</td>
      <td>1.429</td>
      <td>209.249</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>25</td>
      <td>16</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-04-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10738.0</td>
      <td>10.0</td>
      <td>11.000</td>
      <td>243.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>209.444</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>26</td>
      <td>17</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-04-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10752.0</td>
      <td>14.0</td>
      <td>11.143</td>
      <td>244.0</td>
      <td>1.0</td>
      <td>1.143</td>
      <td>209.717</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>27</td>
      <td>17</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-04-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10761.0</td>
      <td>9.0</td>
      <td>11.143</td>
      <td>246.0</td>
      <td>2.0</td>
      <td>1.286</td>
      <td>209.892</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>28</td>
      <td>17</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-04-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10765.0</td>
      <td>4.0</td>
      <td>10.143</td>
      <td>247.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>209.970</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>29</td>
      <td>17</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-04-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10774.0</td>
      <td>9.0</td>
      <td>9.429</td>
      <td>248.0</td>
      <td>1.0</td>
      <td>1.143</td>
      <td>210.146</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>04</td>
      <td>30</td>
      <td>17</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-05-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10780.0</td>
      <td>6.0</td>
      <td>8.857</td>
      <td>250.0</td>
      <td>2.0</td>
      <td>1.429</td>
      <td>210.263</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>01</td>
      <td>17</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-05-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10793.0</td>
      <td>13.0</td>
      <td>9.286</td>
      <td>250.0</td>
      <td>0.0</td>
      <td>1.143</td>
      <td>210.516</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>02</td>
      <td>17</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-05-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10801.0</td>
      <td>8.0</td>
      <td>9.000</td>
      <td>252.0</td>
      <td>2.0</td>
      <td>1.286</td>
      <td>210.672</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>03</td>
      <td>18</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-05-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10804.0</td>
      <td>3.0</td>
      <td>7.429</td>
      <td>254.0</td>
      <td>2.0</td>
      <td>1.429</td>
      <td>210.731</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>04</td>
      <td>18</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-05-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10806.0</td>
      <td>2.0</td>
      <td>6.429</td>
      <td>255.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>210.770</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>05</td>
      <td>18</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-05-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10810.0</td>
      <td>4.0</td>
      <td>6.429</td>
      <td>256.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>210.848</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>06</td>
      <td>18</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-05-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10822.0</td>
      <td>12.0</td>
      <td>6.857</td>
      <td>256.0</td>
      <td>0.0</td>
      <td>1.143</td>
      <td>211.082</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>07</td>
      <td>18</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-05-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10840.0</td>
      <td>18.0</td>
      <td>8.571</td>
      <td>256.0</td>
      <td>0.0</td>
      <td>0.857</td>
      <td>211.433</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>08</td>
      <td>18</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-05-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10874.0</td>
      <td>34.0</td>
      <td>11.571</td>
      <td>256.0</td>
      <td>0.0</td>
      <td>0.857</td>
      <td>212.096</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>09</td>
      <td>18</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-05-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10909.0</td>
      <td>35.0</td>
      <td>15.429</td>
      <td>256.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>212.779</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>10</td>
      <td>19</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-05-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10936.0</td>
      <td>27.0</td>
      <td>18.857</td>
      <td>258.0</td>
      <td>2.0</td>
      <td>0.571</td>
      <td>213.306</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>11</td>
      <td>19</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-05-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10962.0</td>
      <td>26.0</td>
      <td>22.286</td>
      <td>259.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>213.813</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>12</td>
      <td>19</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-05-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>10991.0</td>
      <td>29.0</td>
      <td>25.857</td>
      <td>260.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>214.378</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>13</td>
      <td>19</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-05-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11018.0</td>
      <td>27.0</td>
      <td>28.000</td>
      <td>260.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>214.905</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>14</td>
      <td>19</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-05-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11037.0</td>
      <td>19.0</td>
      <td>28.143</td>
      <td>262.0</td>
      <td>2.0</td>
      <td>0.857</td>
      <td>215.276</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>15</td>
      <td>19</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-05-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11050.0</td>
      <td>13.0</td>
      <td>25.143</td>
      <td>262.0</td>
      <td>0.0</td>
      <td>0.857</td>
      <td>215.529</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>16</td>
      <td>19</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-05-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11065.0</td>
      <td>15.0</td>
      <td>22.286</td>
      <td>263.0</td>
      <td>1.0</td>
      <td>1.000</td>
      <td>215.822</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>17</td>
      <td>20</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-05-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11078.0</td>
      <td>13.0</td>
      <td>20.286</td>
      <td>263.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>216.075</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>18</td>
      <td>20</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-05-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11110.0</td>
      <td>32.0</td>
      <td>21.143</td>
      <td>263.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>216.699</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>19</td>
      <td>20</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-05-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11122.0</td>
      <td>12.0</td>
      <td>18.714</td>
      <td>264.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>216.933</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>20</td>
      <td>20</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-05-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11142.0</td>
      <td>20.0</td>
      <td>17.714</td>
      <td>264.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>217.324</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>21</td>
      <td>20</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-05-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11165.0</td>
      <td>23.0</td>
      <td>18.286</td>
      <td>266.0</td>
      <td>2.0</td>
      <td>0.571</td>
      <td>217.772</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>22</td>
      <td>20</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-05-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11190.0</td>
      <td>25.0</td>
      <td>20.000</td>
      <td>266.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>218.260</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>23</td>
      <td>20</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-05-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11206.0</td>
      <td>16.0</td>
      <td>20.143</td>
      <td>267.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>218.572</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>24</td>
      <td>21</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-05-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11225.0</td>
      <td>19.0</td>
      <td>21.000</td>
      <td>269.0</td>
      <td>2.0</td>
      <td>0.857</td>
      <td>218.942</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>25</td>
      <td>21</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-05-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11265.0</td>
      <td>40.0</td>
      <td>22.143</td>
      <td>269.0</td>
      <td>0.0</td>
      <td>0.857</td>
      <td>219.723</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>26</td>
      <td>21</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-05-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11344.0</td>
      <td>79.0</td>
      <td>31.714</td>
      <td>269.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>221.264</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>27</td>
      <td>21</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-05-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11402.0</td>
      <td>58.0</td>
      <td>37.143</td>
      <td>269.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>222.395</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>28</td>
      <td>21</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-05-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11441.0</td>
      <td>39.0</td>
      <td>39.429</td>
      <td>269.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>223.155</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>29</td>
      <td>21</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-05-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11468.0</td>
      <td>27.0</td>
      <td>39.714</td>
      <td>270.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>223.682</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>30</td>
      <td>21</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-05-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11503.0</td>
      <td>35.0</td>
      <td>42.429</td>
      <td>271.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>224.365</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>05</td>
      <td>31</td>
      <td>22</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-06-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11541.0</td>
      <td>38.0</td>
      <td>45.143</td>
      <td>272.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>225.106</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>01</td>
      <td>22</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-06-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11590.0</td>
      <td>49.0</td>
      <td>46.429</td>
      <td>273.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>226.062</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>02</td>
      <td>22</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-06-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11629.0</td>
      <td>39.0</td>
      <td>40.714</td>
      <td>273.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>226.822</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>03</td>
      <td>22</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-06-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11668.0</td>
      <td>39.0</td>
      <td>38.000</td>
      <td>273.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>227.583</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>04</td>
      <td>22</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-06-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11719.0</td>
      <td>51.0</td>
      <td>39.714</td>
      <td>273.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>228.578</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>05</td>
      <td>22</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-06-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11776.0</td>
      <td>57.0</td>
      <td>44.000</td>
      <td>273.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>229.690</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>06</td>
      <td>22</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-06-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11814.0</td>
      <td>38.0</td>
      <td>44.429</td>
      <td>273.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>230.431</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>07</td>
      <td>23</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-06-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11852.0</td>
      <td>38.0</td>
      <td>44.429</td>
      <td>274.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>231.172</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>08</td>
      <td>23</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-06-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11902.0</td>
      <td>50.0</td>
      <td>44.571</td>
      <td>276.0</td>
      <td>2.0</td>
      <td>0.429</td>
      <td>232.147</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>09</td>
      <td>23</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-06-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>11947.0</td>
      <td>45.0</td>
      <td>45.429</td>
      <td>276.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>233.025</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>10</td>
      <td>23</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-06-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12003.0</td>
      <td>56.0</td>
      <td>47.857</td>
      <td>277.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>234.117</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>11</td>
      <td>23</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-06-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12051.0</td>
      <td>48.0</td>
      <td>47.429</td>
      <td>277.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>235.053</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>12</td>
      <td>23</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-06-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12085.0</td>
      <td>34.0</td>
      <td>44.143</td>
      <td>277.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>235.717</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>13</td>
      <td>23</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-06-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12121.0</td>
      <td>36.0</td>
      <td>43.857</td>
      <td>277.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>236.419</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>14</td>
      <td>24</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-06-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12155.0</td>
      <td>34.0</td>
      <td>43.286</td>
      <td>278.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>237.082</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>15</td>
      <td>24</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-06-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12198.0</td>
      <td>43.0</td>
      <td>42.286</td>
      <td>279.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>237.921</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>16</td>
      <td>24</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-06-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12257.0</td>
      <td>59.0</td>
      <td>44.286</td>
      <td>280.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>239.071</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>17</td>
      <td>24</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-06-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12306.0</td>
      <td>49.0</td>
      <td>43.286</td>
      <td>280.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>240.027</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>18</td>
      <td>24</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-06-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12373.0</td>
      <td>67.0</td>
      <td>46.000</td>
      <td>280.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>241.334</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>19</td>
      <td>24</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-06-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12421.0</td>
      <td>48.0</td>
      <td>48.000</td>
      <td>280.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>242.270</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>20</td>
      <td>24</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-06-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12438.0</td>
      <td>17.0</td>
      <td>45.286</td>
      <td>280.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>242.602</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>21</td>
      <td>25</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-06-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12484.0</td>
      <td>46.0</td>
      <td>47.000</td>
      <td>281.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>243.499</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>22</td>
      <td>25</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-06-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12535.0</td>
      <td>51.0</td>
      <td>48.143</td>
      <td>281.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>244.494</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>23</td>
      <td>25</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-06-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12563.0</td>
      <td>28.0</td>
      <td>43.714</td>
      <td>282.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>245.040</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>24</td>
      <td>25</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-06-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12602.0</td>
      <td>39.0</td>
      <td>42.286</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>245.801</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>25</td>
      <td>25</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-06-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12653.0</td>
      <td>51.0</td>
      <td>40.000</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>246.795</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>26</td>
      <td>25</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-06-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12715.0</td>
      <td>62.0</td>
      <td>42.000</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>248.005</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>27</td>
      <td>25</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-06-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12757.0</td>
      <td>42.0</td>
      <td>45.571</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>248.824</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>28</td>
      <td>26</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-06-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12800.0</td>
      <td>43.0</td>
      <td>45.143</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>249.663</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>29</td>
      <td>26</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-06-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12850.0</td>
      <td>50.0</td>
      <td>45.000</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>250.638</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>06</td>
      <td>30</td>
      <td>26</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-07-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12904.0</td>
      <td>54.0</td>
      <td>48.714</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>251.691</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>01</td>
      <td>26</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-07-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>12967.0</td>
      <td>63.0</td>
      <td>52.143</td>
      <td>282.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>252.920</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>02</td>
      <td>26</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-07-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13030.0</td>
      <td>63.0</td>
      <td>53.857</td>
      <td>283.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>254.149</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>03</td>
      <td>26</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-07-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13091.0</td>
      <td>61.0</td>
      <td>53.714</td>
      <td>283.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>255.339</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>04</td>
      <td>26</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-07-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13137.0</td>
      <td>46.0</td>
      <td>54.286</td>
      <td>284.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>256.236</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>05</td>
      <td>27</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-07-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13181.0</td>
      <td>44.0</td>
      <td>54.429</td>
      <td>285.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>257.094</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>06</td>
      <td>27</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-07-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13244.0</td>
      <td>63.0</td>
      <td>56.286</td>
      <td>285.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>258.323</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>07</td>
      <td>27</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-07-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13293.0</td>
      <td>49.0</td>
      <td>55.571</td>
      <td>287.0</td>
      <td>2.0</td>
      <td>0.714</td>
      <td>259.279</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>08</td>
      <td>27</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-07-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13338.0</td>
      <td>45.0</td>
      <td>53.000</td>
      <td>288.0</td>
      <td>1.0</td>
      <td>0.857</td>
      <td>260.156</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>09</td>
      <td>27</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-07-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13373.0</td>
      <td>35.0</td>
      <td>49.000</td>
      <td>288.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>260.839</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>10</td>
      <td>27</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-07-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13417.0</td>
      <td>44.0</td>
      <td>46.571</td>
      <td>289.0</td>
      <td>1.0</td>
      <td>0.857</td>
      <td>261.697</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>11</td>
      <td>27</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-07-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13479.0</td>
      <td>62.0</td>
      <td>48.857</td>
      <td>289.0</td>
      <td>0.0</td>
      <td>0.714</td>
      <td>262.906</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>12</td>
      <td>28</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-07-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13512.0</td>
      <td>33.0</td>
      <td>47.286</td>
      <td>289.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>263.550</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>13</td>
      <td>28</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-07-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13551.0</td>
      <td>39.0</td>
      <td>43.857</td>
      <td>289.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>264.311</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>14</td>
      <td>28</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-07-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13612.0</td>
      <td>61.0</td>
      <td>45.571</td>
      <td>291.0</td>
      <td>2.0</td>
      <td>0.571</td>
      <td>265.501</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>15</td>
      <td>28</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-07-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13672.0</td>
      <td>60.0</td>
      <td>47.714</td>
      <td>293.0</td>
      <td>2.0</td>
      <td>0.714</td>
      <td>266.671</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>16</td>
      <td>28</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-07-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13711.0</td>
      <td>39.0</td>
      <td>48.286</td>
      <td>294.0</td>
      <td>1.0</td>
      <td>0.857</td>
      <td>267.432</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>17</td>
      <td>28</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-07-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13745.0</td>
      <td>34.0</td>
      <td>46.857</td>
      <td>295.0</td>
      <td>1.0</td>
      <td>0.857</td>
      <td>268.095</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>18</td>
      <td>28</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-07-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13771.0</td>
      <td>26.0</td>
      <td>41.714</td>
      <td>296.0</td>
      <td>1.0</td>
      <td>1.000</td>
      <td>268.602</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>19</td>
      <td>29</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-07-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13816.0</td>
      <td>45.0</td>
      <td>43.429</td>
      <td>296.0</td>
      <td>0.0</td>
      <td>1.000</td>
      <td>269.480</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>20</td>
      <td>29</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-07-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13879.0</td>
      <td>63.0</td>
      <td>46.857</td>
      <td>297.0</td>
      <td>1.0</td>
      <td>1.143</td>
      <td>270.708</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>21</td>
      <td>29</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-07-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13938.0</td>
      <td>59.0</td>
      <td>46.571</td>
      <td>297.0</td>
      <td>0.0</td>
      <td>0.857</td>
      <td>271.859</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>22</td>
      <td>29</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-07-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>13979.0</td>
      <td>41.0</td>
      <td>43.857</td>
      <td>298.0</td>
      <td>1.0</td>
      <td>0.714</td>
      <td>272.659</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>23</td>
      <td>29</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-07-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14092.0</td>
      <td>113.0</td>
      <td>54.429</td>
      <td>298.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>274.863</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>24</td>
      <td>29</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-07-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14150.0</td>
      <td>58.0</td>
      <td>57.857</td>
      <td>298.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>275.994</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>25</td>
      <td>29</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-07-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14175.0</td>
      <td>25.0</td>
      <td>57.714</td>
      <td>299.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>276.482</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>26</td>
      <td>30</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-07-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14203.0</td>
      <td>28.0</td>
      <td>55.286</td>
      <td>300.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>277.028</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>27</td>
      <td>30</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-07-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14251.0</td>
      <td>48.0</td>
      <td>53.143</td>
      <td>300.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>277.964</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>28</td>
      <td>30</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-07-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14269.0</td>
      <td>18.0</td>
      <td>47.286</td>
      <td>300.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>278.315</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>29</td>
      <td>30</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-07-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14305.0</td>
      <td>36.0</td>
      <td>46.571</td>
      <td>301.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>279.018</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>30</td>
      <td>30</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-07-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14336.0</td>
      <td>31.0</td>
      <td>34.857</td>
      <td>301.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>279.622</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>07</td>
      <td>31</td>
      <td>30</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-08-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14366.0</td>
      <td>30.0</td>
      <td>30.857</td>
      <td>301.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>280.207</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>01</td>
      <td>30</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-08-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14389.0</td>
      <td>23.0</td>
      <td>30.571</td>
      <td>301.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>280.656</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>02</td>
      <td>31</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-08-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14423.0</td>
      <td>34.0</td>
      <td>31.429</td>
      <td>301.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>281.319</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>03</td>
      <td>31</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-08-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14456.0</td>
      <td>33.0</td>
      <td>29.286</td>
      <td>302.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>281.963</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>04</td>
      <td>31</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-08-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14499.0</td>
      <td>43.0</td>
      <td>32.857</td>
      <td>302.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>282.801</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>05</td>
      <td>31</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-08-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14519.0</td>
      <td>20.0</td>
      <td>30.571</td>
      <td>303.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>283.192</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>06</td>
      <td>31</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-08-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14562.0</td>
      <td>43.0</td>
      <td>32.286</td>
      <td>304.0</td>
      <td>1.0</td>
      <td>0.429</td>
      <td>284.030</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>07</td>
      <td>31</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-08-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14598.0</td>
      <td>36.0</td>
      <td>33.143</td>
      <td>305.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>284.732</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>08</td>
      <td>31</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-08-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14626.0</td>
      <td>28.0</td>
      <td>33.857</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>285.279</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>09</td>
      <td>32</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-08-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14660.0</td>
      <td>34.0</td>
      <td>33.857</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>285.942</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>10</td>
      <td>32</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-08-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14714.0</td>
      <td>54.0</td>
      <td>36.857</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>286.995</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>11</td>
      <td>32</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-08-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14770.0</td>
      <td>56.0</td>
      <td>38.714</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.429</td>
      <td>288.087</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>12</td>
      <td>32</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-08-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>14873.0</td>
      <td>103.0</td>
      <td>50.571</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.286</td>
      <td>290.096</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>13</td>
      <td>32</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-08-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15039.0</td>
      <td>166.0</td>
      <td>68.143</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>293.334</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>14</td>
      <td>32</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-08-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15318.0</td>
      <td>279.0</td>
      <td>102.857</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>298.776</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>15</td>
      <td>32</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-08-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15515.0</td>
      <td>197.0</td>
      <td>127.000</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>302.618</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>16</td>
      <td>33</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-08-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>15761.0</td>
      <td>246.0</td>
      <td>157.286</td>
      <td>306.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>307.417</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>17</td>
      <td>33</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-08-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>16058.0</td>
      <td>297.0</td>
      <td>192.000</td>
      <td>306.0</td>
      <td>0.0</td>
      <td>0.143</td>
      <td>313.210</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>18</td>
      <td>33</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-08-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>16346.0</td>
      <td>288.0</td>
      <td>225.143</td>
      <td>307.0</td>
      <td>1.0</td>
      <td>0.286</td>
      <td>318.827</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>19</td>
      <td>33</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-08-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>16670.0</td>
      <td>324.0</td>
      <td>256.714</td>
      <td>309.0</td>
      <td>2.0</td>
      <td>0.571</td>
      <td>325.147</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>20</td>
      <td>33</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-08-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>17002.0</td>
      <td>332.0</td>
      <td>280.429</td>
      <td>309.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>331.622</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>21</td>
      <td>33</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-08-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>17399.0</td>
      <td>397.0</td>
      <td>297.286</td>
      <td>309.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>339.366</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>22</td>
      <td>33</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-08-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>17665.0</td>
      <td>266.0</td>
      <td>307.143</td>
      <td>309.0</td>
      <td>0.0</td>
      <td>0.571</td>
      <td>344.554</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>23</td>
      <td>34</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-08-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>17945.0</td>
      <td>280.0</td>
      <td>312.000</td>
      <td>310.0</td>
      <td>1.0</td>
      <td>0.571</td>
      <td>350.015</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>24</td>
      <td>34</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-08-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>18265.0</td>
      <td>320.0</td>
      <td>315.286</td>
      <td>312.0</td>
      <td>2.0</td>
      <td>0.857</td>
      <td>356.257</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>25</td>
      <td>34</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-08-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>18706.0</td>
      <td>441.0</td>
      <td>337.143</td>
      <td>313.0</td>
      <td>1.0</td>
      <td>0.857</td>
      <td>364.859</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>26</td>
      <td>34</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-08-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>19077.0</td>
      <td>371.0</td>
      <td>343.857</td>
      <td>316.0</td>
      <td>3.0</td>
      <td>1.000</td>
      <td>372.095</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>27</td>
      <td>34</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-08-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>19400.0</td>
      <td>323.0</td>
      <td>342.571</td>
      <td>321.0</td>
      <td>5.0</td>
      <td>1.714</td>
      <td>378.395</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>28</td>
      <td>34</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-08-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>19699.0</td>
      <td>299.0</td>
      <td>328.571</td>
      <td>323.0</td>
      <td>2.0</td>
      <td>2.000</td>
      <td>384.227</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>29</td>
      <td>34</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-08-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>19947.0</td>
      <td>248.0</td>
      <td>326.000</td>
      <td>324.0</td>
      <td>1.0</td>
      <td>2.143</td>
      <td>389.064</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>30</td>
      <td>35</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-08-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>20182.0</td>
      <td>235.0</td>
      <td>319.571</td>
      <td>324.0</td>
      <td>0.0</td>
      <td>2.000</td>
      <td>393.648</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>08</td>
      <td>31</td>
      <td>35</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-09-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>20449.0</td>
      <td>267.0</td>
      <td>312.000</td>
      <td>326.0</td>
      <td>2.0</td>
      <td>2.000</td>
      <td>398.856</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>01</td>
      <td>35</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-09-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>20644.0</td>
      <td>195.0</td>
      <td>276.857</td>
      <td>329.0</td>
      <td>3.0</td>
      <td>2.286</td>
      <td>402.659</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>02</td>
      <td>35</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-09-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>20842.0</td>
      <td>198.0</td>
      <td>252.143</td>
      <td>331.0</td>
      <td>2.0</td>
      <td>2.143</td>
      <td>406.521</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>03</td>
      <td>35</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-09-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21010.0</td>
      <td>168.0</td>
      <td>230.000</td>
      <td>333.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>409.798</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>04</td>
      <td>35</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-09-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21177.0</td>
      <td>167.0</td>
      <td>211.143</td>
      <td>334.0</td>
      <td>1.0</td>
      <td>1.571</td>
      <td>413.055</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>05</td>
      <td>35</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-09-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21296.0</td>
      <td>119.0</td>
      <td>192.714</td>
      <td>336.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>415.376</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>06</td>
      <td>36</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-09-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21432.0</td>
      <td>136.0</td>
      <td>178.571</td>
      <td>341.0</td>
      <td>5.0</td>
      <td>2.429</td>
      <td>418.029</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>07</td>
      <td>36</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-09-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21588.0</td>
      <td>156.0</td>
      <td>162.714</td>
      <td>344.0</td>
      <td>3.0</td>
      <td>2.571</td>
      <td>421.072</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>08</td>
      <td>36</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-09-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21743.0</td>
      <td>155.0</td>
      <td>157.000</td>
      <td>346.0</td>
      <td>2.0</td>
      <td>2.429</td>
      <td>424.095</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>09</td>
      <td>36</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-09-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>21919.0</td>
      <td>176.0</td>
      <td>153.857</td>
      <td>350.0</td>
      <td>4.0</td>
      <td>2.714</td>
      <td>427.528</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>10</td>
      <td>36</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-09-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22055.0</td>
      <td>136.0</td>
      <td>149.286</td>
      <td>355.0</td>
      <td>5.0</td>
      <td>3.143</td>
      <td>430.180</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>11</td>
      <td>36</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-09-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22176.0</td>
      <td>121.0</td>
      <td>142.714</td>
      <td>358.0</td>
      <td>3.0</td>
      <td>3.429</td>
      <td>432.541</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>12</td>
      <td>36</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-09-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22285.0</td>
      <td>109.0</td>
      <td>141.286</td>
      <td>363.0</td>
      <td>5.0</td>
      <td>3.857</td>
      <td>434.667</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>13</td>
      <td>37</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-09-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22391.0</td>
      <td>106.0</td>
      <td>137.000</td>
      <td>367.0</td>
      <td>4.0</td>
      <td>3.714</td>
      <td>436.734</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>14</td>
      <td>37</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-09-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22504.0</td>
      <td>113.0</td>
      <td>130.857</td>
      <td>367.0</td>
      <td>0.0</td>
      <td>3.286</td>
      <td>438.938</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>15</td>
      <td>37</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-09-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22657.0</td>
      <td>153.0</td>
      <td>130.571</td>
      <td>372.0</td>
      <td>5.0</td>
      <td>3.714</td>
      <td>441.922</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>16</td>
      <td>37</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-09-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22783.0</td>
      <td>126.0</td>
      <td>123.429</td>
      <td>377.0</td>
      <td>5.0</td>
      <td>3.857</td>
      <td>444.380</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>17</td>
      <td>37</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-09-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22893.0</td>
      <td>110.0</td>
      <td>119.714</td>
      <td>378.0</td>
      <td>1.0</td>
      <td>3.286</td>
      <td>446.526</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>18</td>
      <td>37</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-09-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>22975.0</td>
      <td>82.0</td>
      <td>114.143</td>
      <td>383.0</td>
      <td>5.0</td>
      <td>3.571</td>
      <td>448.125</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>19</td>
      <td>37</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-09-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23045.0</td>
      <td>70.0</td>
      <td>108.571</td>
      <td>385.0</td>
      <td>2.0</td>
      <td>3.143</td>
      <td>449.490</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>20</td>
      <td>38</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-09-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23106.0</td>
      <td>61.0</td>
      <td>102.143</td>
      <td>388.0</td>
      <td>3.0</td>
      <td>3.000</td>
      <td>450.680</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>21</td>
      <td>38</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-09-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23216.0</td>
      <td>110.0</td>
      <td>101.714</td>
      <td>388.0</td>
      <td>0.0</td>
      <td>3.000</td>
      <td>452.826</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>22</td>
      <td>38</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-09-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23341.0</td>
      <td>125.0</td>
      <td>97.714</td>
      <td>393.0</td>
      <td>5.0</td>
      <td>3.000</td>
      <td>455.264</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>23</td>
      <td>38</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-09-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23455.0</td>
      <td>114.0</td>
      <td>96.000</td>
      <td>395.0</td>
      <td>2.0</td>
      <td>2.571</td>
      <td>457.487</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>24</td>
      <td>38</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-09-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23516.0</td>
      <td>61.0</td>
      <td>89.000</td>
      <td>399.0</td>
      <td>4.0</td>
      <td>3.000</td>
      <td>458.677</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>25</td>
      <td>38</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-09-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23611.0</td>
      <td>95.0</td>
      <td>90.857</td>
      <td>401.0</td>
      <td>2.0</td>
      <td>2.571</td>
      <td>460.530</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>26</td>
      <td>38</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-09-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23661.0</td>
      <td>50.0</td>
      <td>88.000</td>
      <td>406.0</td>
      <td>5.0</td>
      <td>3.000</td>
      <td>461.505</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>27</td>
      <td>39</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-09-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23699.0</td>
      <td>38.0</td>
      <td>84.714</td>
      <td>407.0</td>
      <td>1.0</td>
      <td>2.714</td>
      <td>462.246</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>28</td>
      <td>39</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-09-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23812.0</td>
      <td>113.0</td>
      <td>85.143</td>
      <td>413.0</td>
      <td>6.0</td>
      <td>3.571</td>
      <td>464.451</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>29</td>
      <td>39</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-09-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23889.0</td>
      <td>77.0</td>
      <td>78.286</td>
      <td>415.0</td>
      <td>2.0</td>
      <td>3.143</td>
      <td>465.952</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>09</td>
      <td>30</td>
      <td>39</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-10-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>23952.0</td>
      <td>63.0</td>
      <td>71.000</td>
      <td>416.0</td>
      <td>1.0</td>
      <td>3.000</td>
      <td>467.181</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>01</td>
      <td>39</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-10-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24027.0</td>
      <td>75.0</td>
      <td>73.000</td>
      <td>420.0</td>
      <td>4.0</td>
      <td>3.000</td>
      <td>468.644</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>02</td>
      <td>39</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-10-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24091.0</td>
      <td>64.0</td>
      <td>68.571</td>
      <td>421.0</td>
      <td>1.0</td>
      <td>2.857</td>
      <td>469.892</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>03</td>
      <td>39</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-10-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24164.0</td>
      <td>73.0</td>
      <td>71.857</td>
      <td>422.0</td>
      <td>1.0</td>
      <td>2.286</td>
      <td>471.316</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>04</td>
      <td>40</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-10-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24239.0</td>
      <td>75.0</td>
      <td>77.143</td>
      <td>422.0</td>
      <td>0.0</td>
      <td>2.143</td>
      <td>472.779</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>05</td>
      <td>40</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-10-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24353.0</td>
      <td>114.0</td>
      <td>77.286</td>
      <td>425.0</td>
      <td>3.0</td>
      <td>1.714</td>
      <td>475.003</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>06</td>
      <td>40</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-10-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24422.0</td>
      <td>69.0</td>
      <td>76.143</td>
      <td>427.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>476.349</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>07</td>
      <td>40</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-10-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24476.0</td>
      <td>54.0</td>
      <td>74.857</td>
      <td>428.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>477.402</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>08</td>
      <td>40</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-10-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24548.0</td>
      <td>72.0</td>
      <td>74.429</td>
      <td>430.0</td>
      <td>2.0</td>
      <td>1.429</td>
      <td>478.806</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>09</td>
      <td>40</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-10-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24606.0</td>
      <td>58.0</td>
      <td>73.571</td>
      <td>432.0</td>
      <td>2.0</td>
      <td>1.571</td>
      <td>479.937</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>10</td>
      <td>40</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-10-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24703.0</td>
      <td>97.0</td>
      <td>77.000</td>
      <td>433.0</td>
      <td>1.0</td>
      <td>1.571</td>
      <td>481.829</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>11</td>
      <td>41</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-10-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24805.0</td>
      <td>102.0</td>
      <td>80.857</td>
      <td>434.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>483.819</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>12</td>
      <td>41</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-10-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24889.0</td>
      <td>84.0</td>
      <td>76.571</td>
      <td>438.0</td>
      <td>4.0</td>
      <td>1.857</td>
      <td>485.457</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>13</td>
      <td>41</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-10-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>24988.0</td>
      <td>99.0</td>
      <td>80.857</td>
      <td>439.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>487.388</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>14</td>
      <td>41</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-10-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25035.0</td>
      <td>47.0</td>
      <td>79.857</td>
      <td>441.0</td>
      <td>2.0</td>
      <td>1.857</td>
      <td>488.305</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>15</td>
      <td>41</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-10-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25108.0</td>
      <td>73.0</td>
      <td>80.000</td>
      <td>443.0</td>
      <td>2.0</td>
      <td>1.857</td>
      <td>489.729</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>16</td>
      <td>41</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-10-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25199.0</td>
      <td>91.0</td>
      <td>84.714</td>
      <td>444.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>491.504</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>17</td>
      <td>41</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-10-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25275.0</td>
      <td>76.0</td>
      <td>81.714</td>
      <td>444.0</td>
      <td>0.0</td>
      <td>1.571</td>
      <td>492.986</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>18</td>
      <td>42</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-10-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25333.0</td>
      <td>58.0</td>
      <td>75.429</td>
      <td>447.0</td>
      <td>3.0</td>
      <td>1.857</td>
      <td>494.117</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>19</td>
      <td>42</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-10-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25424.0</td>
      <td>91.0</td>
      <td>76.429</td>
      <td>450.0</td>
      <td>3.0</td>
      <td>1.714</td>
      <td>495.892</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>20</td>
      <td>42</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-10-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25543.0</td>
      <td>119.0</td>
      <td>79.286</td>
      <td>453.0</td>
      <td>3.0</td>
      <td>2.000</td>
      <td>498.214</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>21</td>
      <td>42</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-10-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25698.0</td>
      <td>155.0</td>
      <td>94.714</td>
      <td>455.0</td>
      <td>2.0</td>
      <td>2.000</td>
      <td>501.237</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>22</td>
      <td>42</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-10-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25775.0</td>
      <td>77.0</td>
      <td>95.286</td>
      <td>457.0</td>
      <td>2.0</td>
      <td>2.000</td>
      <td>502.739</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>23</td>
      <td>42</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-10-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25836.0</td>
      <td>61.0</td>
      <td>91.000</td>
      <td>457.0</td>
      <td>0.0</td>
      <td>1.857</td>
      <td>503.928</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>24</td>
      <td>42</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-10-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>25955.0</td>
      <td>119.0</td>
      <td>97.143</td>
      <td>457.0</td>
      <td>0.0</td>
      <td>1.857</td>
      <td>506.250</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>25</td>
      <td>43</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-10-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26043.0</td>
      <td>88.0</td>
      <td>101.429</td>
      <td>460.0</td>
      <td>3.0</td>
      <td>1.857</td>
      <td>507.966</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>26</td>
      <td>43</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-10-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26146.0</td>
      <td>103.0</td>
      <td>103.143</td>
      <td>461.0</td>
      <td>1.0</td>
      <td>1.571</td>
      <td>509.975</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>27</td>
      <td>43</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-10-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26271.0</td>
      <td>125.0</td>
      <td>104.000</td>
      <td>462.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>512.413</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>28</td>
      <td>43</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-10-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26385.0</td>
      <td>114.0</td>
      <td>98.143</td>
      <td>463.0</td>
      <td>1.0</td>
      <td>1.143</td>
      <td>514.637</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>29</td>
      <td>43</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-10-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26511.0</td>
      <td>126.0</td>
      <td>105.143</td>
      <td>464.0</td>
      <td>1.0</td>
      <td>1.000</td>
      <td>517.094</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>30</td>
      <td>43</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-10-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26635.0</td>
      <td>124.0</td>
      <td>114.143</td>
      <td>466.0</td>
      <td>2.0</td>
      <td>1.286</td>
      <td>519.513</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>10</td>
      <td>31</td>
      <td>43</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-11-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26732.0</td>
      <td>97.0</td>
      <td>111.000</td>
      <td>468.0</td>
      <td>2.0</td>
      <td>1.571</td>
      <td>521.405</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>01</td>
      <td>44</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-11-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26807.0</td>
      <td>75.0</td>
      <td>109.143</td>
      <td>472.0</td>
      <td>4.0</td>
      <td>1.714</td>
      <td>522.868</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>02</td>
      <td>44</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-11-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>26925.0</td>
      <td>118.0</td>
      <td>111.286</td>
      <td>474.0</td>
      <td>2.0</td>
      <td>1.857</td>
      <td>525.169</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>03</td>
      <td>44</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-11-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27050.0</td>
      <td>125.0</td>
      <td>111.286</td>
      <td>475.0</td>
      <td>1.0</td>
      <td>1.857</td>
      <td>527.607</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>04</td>
      <td>44</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-11-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27195.0</td>
      <td>145.0</td>
      <td>115.714</td>
      <td>476.0</td>
      <td>1.0</td>
      <td>1.857</td>
      <td>530.436</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>05</td>
      <td>44</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-11-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27284.0</td>
      <td>89.0</td>
      <td>110.429</td>
      <td>477.0</td>
      <td>1.0</td>
      <td>1.857</td>
      <td>532.172</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>06</td>
      <td>44</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-11-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27427.0</td>
      <td>143.0</td>
      <td>113.143</td>
      <td>478.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>534.961</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>07</td>
      <td>44</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-11-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27553.0</td>
      <td>126.0</td>
      <td>117.286</td>
      <td>480.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>537.418</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>08</td>
      <td>45</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-11-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27653.0</td>
      <td>100.0</td>
      <td>120.857</td>
      <td>485.0</td>
      <td>5.0</td>
      <td>1.857</td>
      <td>539.369</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>09</td>
      <td>45</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-11-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27799.0</td>
      <td>146.0</td>
      <td>124.857</td>
      <td>487.0</td>
      <td>2.0</td>
      <td>1.857</td>
      <td>542.217</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>10</td>
      <td>45</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-11-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>27942.0</td>
      <td>143.0</td>
      <td>127.429</td>
      <td>487.0</td>
      <td>0.0</td>
      <td>1.714</td>
      <td>545.006</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>11</td>
      <td>45</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-11-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28133.0</td>
      <td>191.0</td>
      <td>134.000</td>
      <td>488.0</td>
      <td>1.0</td>
      <td>1.714</td>
      <td>548.731</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>12</td>
      <td>45</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-11-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28338.0</td>
      <td>205.0</td>
      <td>150.571</td>
      <td>492.0</td>
      <td>4.0</td>
      <td>2.143</td>
      <td>552.730</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>13</td>
      <td>45</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-11-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28546.0</td>
      <td>208.0</td>
      <td>159.857</td>
      <td>493.0</td>
      <td>1.0</td>
      <td>2.143</td>
      <td>556.787</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>14</td>
      <td>45</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-11-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28769.0</td>
      <td>223.0</td>
      <td>173.714</td>
      <td>494.0</td>
      <td>1.0</td>
      <td>2.000</td>
      <td>561.136</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>15</td>
      <td>46</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-11-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>28998.0</td>
      <td>229.0</td>
      <td>192.143</td>
      <td>494.0</td>
      <td>0.0</td>
      <td>1.286</td>
      <td>565.603</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>16</td>
      <td>46</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-11-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>29311.0</td>
      <td>313.0</td>
      <td>216.000</td>
      <td>496.0</td>
      <td>2.0</td>
      <td>1.286</td>
      <td>571.708</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>17</td>
      <td>46</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-11-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>29654.0</td>
      <td>343.0</td>
      <td>244.571</td>
      <td>498.0</td>
      <td>2.0</td>
      <td>1.571</td>
      <td>578.398</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>18</td>
      <td>46</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-11-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>30017.0</td>
      <td>363.0</td>
      <td>269.143</td>
      <td>501.0</td>
      <td>3.0</td>
      <td>1.857</td>
      <td>585.478</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>19</td>
      <td>46</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-11-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>30403.0</td>
      <td>386.0</td>
      <td>295.000</td>
      <td>503.0</td>
      <td>2.0</td>
      <td>1.571</td>
      <td>593.007</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>20</td>
      <td>46</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-11-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>30733.0</td>
      <td>330.0</td>
      <td>312.429</td>
      <td>505.0</td>
      <td>2.0</td>
      <td>1.714</td>
      <td>599.444</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>21</td>
      <td>46</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-11-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>31004.0</td>
      <td>271.0</td>
      <td>319.286</td>
      <td>509.0</td>
      <td>4.0</td>
      <td>2.143</td>
      <td>604.730</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>22</td>
      <td>47</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-11-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>31353.0</td>
      <td>349.0</td>
      <td>336.429</td>
      <td>510.0</td>
      <td>1.0</td>
      <td>2.286</td>
      <td>611.537</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>23</td>
      <td>47</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-11-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>31735.0</td>
      <td>382.0</td>
      <td>346.286</td>
      <td>513.0</td>
      <td>3.0</td>
      <td>2.429</td>
      <td>618.988</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>24</td>
      <td>47</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-11-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>32318.0</td>
      <td>583.0</td>
      <td>380.571</td>
      <td>515.0</td>
      <td>2.0</td>
      <td>2.429</td>
      <td>630.359</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>25</td>
      <td>47</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-11-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>32887.0</td>
      <td>569.0</td>
      <td>410.000</td>
      <td>516.0</td>
      <td>1.0</td>
      <td>2.143</td>
      <td>641.457</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>26</td>
      <td>47</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-11-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>33375.0</td>
      <td>488.0</td>
      <td>424.571</td>
      <td>522.0</td>
      <td>6.0</td>
      <td>2.714</td>
      <td>650.976</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>27</td>
      <td>47</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-11-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>33824.0</td>
      <td>449.0</td>
      <td>441.571</td>
      <td>523.0</td>
      <td>1.0</td>
      <td>2.571</td>
      <td>659.734</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>28</td>
      <td>47</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-11-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>34201.0</td>
      <td>377.0</td>
      <td>456.714</td>
      <td>526.0</td>
      <td>3.0</td>
      <td>2.429</td>
      <td>667.087</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>29</td>
      <td>48</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-11-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>34652.0</td>
      <td>451.0</td>
      <td>471.286</td>
      <td>526.0</td>
      <td>0.0</td>
      <td>2.286</td>
      <td>675.884</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>11</td>
      <td>30</td>
      <td>48</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-12-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>35163.0</td>
      <td>511.0</td>
      <td>489.714</td>
      <td>526.0</td>
      <td>0.0</td>
      <td>1.857</td>
      <td>685.851</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>01</td>
      <td>48</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-12-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>35703.0</td>
      <td>540.0</td>
      <td>483.571</td>
      <td>529.0</td>
      <td>3.0</td>
      <td>2.000</td>
      <td>696.383</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>02</td>
      <td>48</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-12-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>36332.0</td>
      <td>629.0</td>
      <td>492.143</td>
      <td>536.0</td>
      <td>7.0</td>
      <td>2.857</td>
      <td>708.652</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>03</td>
      <td>48</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-12-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>36915.0</td>
      <td>583.0</td>
      <td>505.714</td>
      <td>540.0</td>
      <td>4.0</td>
      <td>2.571</td>
      <td>720.023</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>04</td>
      <td>48</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-12-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>37546.0</td>
      <td>631.0</td>
      <td>531.714</td>
      <td>545.0</td>
      <td>5.0</td>
      <td>3.143</td>
      <td>732.331</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>05</td>
      <td>48</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-12-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>38161.0</td>
      <td>615.0</td>
      <td>565.714</td>
      <td>549.0</td>
      <td>4.0</td>
      <td>3.286</td>
      <td>744.326</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>06</td>
      <td>49</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-12-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>38755.0</td>
      <td>594.0</td>
      <td>586.143</td>
      <td>552.0</td>
      <td>3.0</td>
      <td>3.714</td>
      <td>755.912</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>07</td>
      <td>49</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-12-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>39432.0</td>
      <td>677.0</td>
      <td>609.857</td>
      <td>556.0</td>
      <td>4.0</td>
      <td>4.286</td>
      <td>769.117</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>08</td>
      <td>49</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-12-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>40098.0</td>
      <td>666.0</td>
      <td>627.857</td>
      <td>564.0</td>
      <td>8.0</td>
      <td>5.000</td>
      <td>782.107</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>09</td>
      <td>49</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-12-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>40786.0</td>
      <td>688.0</td>
      <td>636.286</td>
      <td>572.0</td>
      <td>8.0</td>
      <td>5.143</td>
      <td>795.527</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>10</td>
      <td>49</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-12-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>41736.0</td>
      <td>950.0</td>
      <td>688.714</td>
      <td>578.0</td>
      <td>6.0</td>
      <td>5.429</td>
      <td>814.056</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>11</td>
      <td>49</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-12-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>42766.0</td>
      <td>1030.0</td>
      <td>745.714</td>
      <td>580.0</td>
      <td>2.0</td>
      <td>5.000</td>
      <td>834.146</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>12</td>
      <td>49</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-12-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>43484.0</td>
      <td>718.0</td>
      <td>760.429</td>
      <td>587.0</td>
      <td>7.0</td>
      <td>5.429</td>
      <td>848.151</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>13</td>
      <td>50</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-12-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>44364.0</td>
      <td>880.0</td>
      <td>801.286</td>
      <td>600.0</td>
      <td>13.0</td>
      <td>6.857</td>
      <td>865.315</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>14</td>
      <td>50</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-12-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>45442.0</td>
      <td>1078.0</td>
      <td>858.571</td>
      <td>612.0</td>
      <td>12.0</td>
      <td>8.000</td>
      <td>886.341</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>15</td>
      <td>50</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-12-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>46453.0</td>
      <td>1011.0</td>
      <td>907.857</td>
      <td>634.0</td>
      <td>22.0</td>
      <td>10.000</td>
      <td>906.061</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>16</td>
      <td>50</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-12-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>47515.0</td>
      <td>1062.0</td>
      <td>961.286</td>
      <td>645.0</td>
      <td>11.0</td>
      <td>10.429</td>
      <td>926.775</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>17</td>
      <td>50</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-12-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>48570.0</td>
      <td>1055.0</td>
      <td>976.286</td>
      <td>659.0</td>
      <td>14.0</td>
      <td>11.571</td>
      <td>947.353</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>18</td>
      <td>50</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-12-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>49665.0</td>
      <td>1095.0</td>
      <td>985.571</td>
      <td>674.0</td>
      <td>15.0</td>
      <td>13.429</td>
      <td>968.711</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>19</td>
      <td>50</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-12-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>50591.0</td>
      <td>926.0</td>
      <td>1015.286</td>
      <td>698.0</td>
      <td>24.0</td>
      <td>15.857</td>
      <td>986.772</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>20</td>
      <td>51</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-12-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>51460.0</td>
      <td>869.0</td>
      <td>1013.714</td>
      <td>722.0</td>
      <td>24.0</td>
      <td>17.429</td>
      <td>1003.722</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>21</td>
      <td>51</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-12-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>52550.0</td>
      <td>1090.0</td>
      <td>1015.429</td>
      <td>739.0</td>
      <td>17.0</td>
      <td>18.143</td>
      <td>1024.982</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>22</td>
      <td>51</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-12-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>53533.0</td>
      <td>983.0</td>
      <td>1011.429</td>
      <td>756.0</td>
      <td>17.0</td>
      <td>17.429</td>
      <td>1044.156</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>23</td>
      <td>51</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-12-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>54770.0</td>
      <td>1237.0</td>
      <td>1036.429</td>
      <td>773.0</td>
      <td>17.0</td>
      <td>18.286</td>
      <td>1068.283</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>24</td>
      <td>51</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2020-12-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>55902.0</td>
      <td>1132.0</td>
      <td>1047.429</td>
      <td>793.0</td>
      <td>20.0</td>
      <td>19.143</td>
      <td>1090.363</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>25</td>
      <td>51</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-12-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>56872.0</td>
      <td>970.0</td>
      <td>1029.571</td>
      <td>808.0</td>
      <td>15.0</td>
      <td>19.143</td>
      <td>1109.282</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>26</td>
      <td>51</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2020-12-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>57680.0</td>
      <td>808.0</td>
      <td>1012.714</td>
      <td>819.0</td>
      <td>11.0</td>
      <td>17.286</td>
      <td>1125.042</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>27</td>
      <td>52</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2020-12-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>58725.0</td>
      <td>1045.0</td>
      <td>1037.857</td>
      <td>859.0</td>
      <td>40.0</td>
      <td>19.571</td>
      <td>1145.425</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>28</td>
      <td>52</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2020-12-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>59773.0</td>
      <td>1048.0</td>
      <td>1031.857</td>
      <td>879.0</td>
      <td>20.0</td>
      <td>20.000</td>
      <td>1165.866</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>29</td>
      <td>52</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2020-12-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>60740.0</td>
      <td>967.0</td>
      <td>1029.571</td>
      <td>900.0</td>
      <td>21.0</td>
      <td>20.571</td>
      <td>1184.727</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>30</td>
      <td>52</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2020-12-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>61769.0</td>
      <td>1029.0</td>
      <td>999.857</td>
      <td>917.0</td>
      <td>17.0</td>
      <td>20.571</td>
      <td>1204.798</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2020</td>
      <td>12</td>
      <td>31</td>
      <td>52</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-01-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>62593.0</td>
      <td>824.0</td>
      <td>955.857</td>
      <td>942.0</td>
      <td>25.0</td>
      <td>21.286</td>
      <td>1220.870</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>01</td>
      <td>00</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>63244.0</td>
      <td>651.0</td>
      <td>910.286</td>
      <td>962.0</td>
      <td>20.0</td>
      <td>22.000</td>
      <td>1233.568</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>02</td>
      <td>00</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>64264.0</td>
      <td>1020.0</td>
      <td>940.571</td>
      <td>981.0</td>
      <td>19.0</td>
      <td>23.143</td>
      <td>1253.463</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>03</td>
      <td>01</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>64979.0</td>
      <td>715.0</td>
      <td>893.429</td>
      <td>1007.0</td>
      <td>26.0</td>
      <td>21.143</td>
      <td>1267.409</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>04</td>
      <td>01</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>65818.0</td>
      <td>839.0</td>
      <td>863.571</td>
      <td>1027.0</td>
      <td>20.0</td>
      <td>21.143</td>
      <td>1283.773</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>05</td>
      <td>01</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>66686.0</td>
      <td>868.0</td>
      <td>849.429</td>
      <td>1046.0</td>
      <td>19.0</td>
      <td>20.857</td>
      <td>1300.703</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>06</td>
      <td>01</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-01-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>67358.0</td>
      <td>672.0</td>
      <td>798.429</td>
      <td>1081.0</td>
      <td>35.0</td>
      <td>23.429</td>
      <td>1313.811</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>07</td>
      <td>01</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-01-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>67999.0</td>
      <td>641.0</td>
      <td>772.286</td>
      <td>1100.0</td>
      <td>19.0</td>
      <td>22.571</td>
      <td>1326.313</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>08</td>
      <td>01</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-01-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>68664.0</td>
      <td>665.0</td>
      <td>774.286</td>
      <td>1125.0</td>
      <td>25.0</td>
      <td>23.286</td>
      <td>1339.284</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>09</td>
      <td>01</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-01-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>69114.0</td>
      <td>450.0</td>
      <td>692.857</td>
      <td>1140.0</td>
      <td>15.0</td>
      <td>22.714</td>
      <td>1348.061</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>10</td>
      <td>02</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-01-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>69651.0</td>
      <td>537.0</td>
      <td>667.429</td>
      <td>1165.0</td>
      <td>25.0</td>
      <td>22.571</td>
      <td>1358.535</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>11</td>
      <td>02</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-01-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>70212.0</td>
      <td>561.0</td>
      <td>627.714</td>
      <td>1185.0</td>
      <td>20.0</td>
      <td>22.571</td>
      <td>1369.478</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>12</td>
      <td>02</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-01-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>70728.0</td>
      <td>516.0</td>
      <td>577.429</td>
      <td>1195.0</td>
      <td>10.0</td>
      <td>21.286</td>
      <td>1379.542</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>13</td>
      <td>02</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-01-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>71241.0</td>
      <td>513.0</td>
      <td>554.714</td>
      <td>1217.0</td>
      <td>22.0</td>
      <td>19.429</td>
      <td>1389.548</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>14</td>
      <td>02</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-01-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>71820.0</td>
      <td>579.0</td>
      <td>545.857</td>
      <td>1236.0</td>
      <td>19.0</td>
      <td>19.429</td>
      <td>1400.842</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>15</td>
      <td>02</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-01-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>72340.0</td>
      <td>520.0</td>
      <td>525.143</td>
      <td>1249.0</td>
      <td>13.0</td>
      <td>17.714</td>
      <td>1410.984</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>16</td>
      <td>02</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-01-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>72729.0</td>
      <td>389.0</td>
      <td>516.429</td>
      <td>1264.0</td>
      <td>15.0</td>
      <td>17.714</td>
      <td>1418.571</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>17</td>
      <td>03</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-01-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>73115.0</td>
      <td>386.0</td>
      <td>494.857</td>
      <td>1283.0</td>
      <td>19.0</td>
      <td>16.857</td>
      <td>1426.100</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>18</td>
      <td>03</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-01-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>73518.0</td>
      <td>403.0</td>
      <td>472.286</td>
      <td>1300.0</td>
      <td>17.0</td>
      <td>16.429</td>
      <td>1433.961</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>19</td>
      <td>03</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-01-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>73918.0</td>
      <td>400.0</td>
      <td>455.714</td>
      <td>1316.0</td>
      <td>16.0</td>
      <td>17.286</td>
      <td>1441.763</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>20</td>
      <td>03</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>74262.0</td>
      <td>344.0</td>
      <td>431.571</td>
      <td>1328.0</td>
      <td>12.0</td>
      <td>15.857</td>
      <td>1448.472</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>21</td>
      <td>03</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-01-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>74692.0</td>
      <td>430.0</td>
      <td>410.286</td>
      <td>1337.0</td>
      <td>9.0</td>
      <td>14.429</td>
      <td>1456.860</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>22</td>
      <td>03</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-01-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>75084.0</td>
      <td>392.0</td>
      <td>392.000</td>
      <td>1349.0</td>
      <td>12.0</td>
      <td>14.286</td>
      <td>1464.505</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>23</td>
      <td>03</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-01-24</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>75521.0</td>
      <td>437.0</td>
      <td>398.857</td>
      <td>1360.0</td>
      <td>11.0</td>
      <td>13.714</td>
      <td>1473.029</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>24</td>
      <td>04</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-01-25</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>75875.0</td>
      <td>354.0</td>
      <td>394.286</td>
      <td>1371.0</td>
      <td>11.0</td>
      <td>12.571</td>
      <td>1479.934</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>25</td>
      <td>04</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-01-26</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>76429.0</td>
      <td>554.0</td>
      <td>415.857</td>
      <td>1378.0</td>
      <td>7.0</td>
      <td>11.143</td>
      <td>1490.740</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>26</td>
      <td>04</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-01-27</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>76926.0</td>
      <td>497.0</td>
      <td>429.714</td>
      <td>1386.0</td>
      <td>8.0</td>
      <td>10.000</td>
      <td>1500.434</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>27</td>
      <td>04</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-01-28</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>77395.0</td>
      <td>469.0</td>
      <td>447.571</td>
      <td>1399.0</td>
      <td>13.0</td>
      <td>10.143</td>
      <td>1509.581</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>28</td>
      <td>04</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-01-29</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>77850.0</td>
      <td>455.0</td>
      <td>451.143</td>
      <td>1414.0</td>
      <td>15.0</td>
      <td>11.000</td>
      <td>1518.456</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>29</td>
      <td>04</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-01-30</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>78205.0</td>
      <td>355.0</td>
      <td>445.857</td>
      <td>1420.0</td>
      <td>6.0</td>
      <td>10.143</td>
      <td>1525.380</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>30</td>
      <td>04</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-01-31</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>78508.0</td>
      <td>303.0</td>
      <td>426.714</td>
      <td>1425.0</td>
      <td>5.0</td>
      <td>9.286</td>
      <td>1531.290</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>01</td>
      <td>31</td>
      <td>05</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-02-01</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>78844.0</td>
      <td>336.0</td>
      <td>424.143</td>
      <td>1435.0</td>
      <td>10.0</td>
      <td>9.143</td>
      <td>1537.844</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>01</td>
      <td>05</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-02-02</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>79311.0</td>
      <td>467.0</td>
      <td>411.714</td>
      <td>1441.0</td>
      <td>6.0</td>
      <td>9.000</td>
      <td>1546.953</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>02</td>
      <td>05</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-03</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>79762.0</td>
      <td>451.0</td>
      <td>405.143</td>
      <td>1448.0</td>
      <td>7.0</td>
      <td>8.857</td>
      <td>1555.749</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>03</td>
      <td>05</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-02-04</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>80131.0</td>
      <td>369.0</td>
      <td>390.857</td>
      <td>1459.0</td>
      <td>11.0</td>
      <td>8.571</td>
      <td>1562.947</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>04</td>
      <td>05</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-02-05</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>80524.0</td>
      <td>393.0</td>
      <td>382.000</td>
      <td>1464.0</td>
      <td>5.0</td>
      <td>7.143</td>
      <td>1570.612</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>05</td>
      <td>05</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-02-06</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>80896.0</td>
      <td>372.0</td>
      <td>384.429</td>
      <td>1471.0</td>
      <td>7.0</td>
      <td>7.286</td>
      <td>1577.868</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>06</td>
      <td>05</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-02-07</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>81185.0</td>
      <td>289.0</td>
      <td>382.429</td>
      <td>1474.0</td>
      <td>3.0</td>
      <td>7.000</td>
      <td>1583.505</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>07</td>
      <td>06</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-02-08</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>81487.0</td>
      <td>302.0</td>
      <td>377.571</td>
      <td>1482.0</td>
      <td>8.0</td>
      <td>6.714</td>
      <td>1589.395</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>08</td>
      <td>06</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-02-09</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>81930.0</td>
      <td>443.0</td>
      <td>374.143</td>
      <td>1486.0</td>
      <td>4.0</td>
      <td>6.429</td>
      <td>1598.036</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>09</td>
      <td>06</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-10</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>82434.0</td>
      <td>504.0</td>
      <td>381.714</td>
      <td>1496.0</td>
      <td>10.0</td>
      <td>6.857</td>
      <td>1607.866</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>10</td>
      <td>06</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-02-11</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>82837.0</td>
      <td>403.0</td>
      <td>386.571</td>
      <td>1507.0</td>
      <td>11.0</td>
      <td>6.857</td>
      <td>1615.727</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>11</td>
      <td>06</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-02-12</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>83199.0</td>
      <td>362.0</td>
      <td>382.143</td>
      <td>1514.0</td>
      <td>7.0</td>
      <td>7.143</td>
      <td>1622.788</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>12</td>
      <td>06</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-02-13</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>83525.0</td>
      <td>326.0</td>
      <td>375.571</td>
      <td>1522.0</td>
      <td>8.0</td>
      <td>7.286</td>
      <td>1629.146</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>13</td>
      <td>06</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-02-14</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>83869.0</td>
      <td>344.0</td>
      <td>383.429</td>
      <td>1527.0</td>
      <td>5.0</td>
      <td>7.571</td>
      <td>1635.856</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>14</td>
      <td>07</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-02-15</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>84325.0</td>
      <td>456.0</td>
      <td>405.429</td>
      <td>1534.0</td>
      <td>7.0</td>
      <td>7.429</td>
      <td>1644.750</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>15</td>
      <td>07</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-02-16</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>84946.0</td>
      <td>621.0</td>
      <td>430.857</td>
      <td>1538.0</td>
      <td>4.0</td>
      <td>7.429</td>
      <td>1656.863</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>16</td>
      <td>07</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-17</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>85567.0</td>
      <td>621.0</td>
      <td>447.571</td>
      <td>1544.0</td>
      <td>6.0</td>
      <td>6.857</td>
      <td>1668.975</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>17</td>
      <td>07</td>
      <td>Wed</td>
    </tr>
    <tr>
      <th>2021-02-18</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>86128.0</td>
      <td>561.0</td>
      <td>470.143</td>
      <td>1550.0</td>
      <td>6.0</td>
      <td>6.143</td>
      <td>1679.918</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>18</td>
      <td>07</td>
      <td>Thu</td>
    </tr>
    <tr>
      <th>2021-02-19</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>86574.0</td>
      <td>446.0</td>
      <td>482.143</td>
      <td>1553.0</td>
      <td>3.0</td>
      <td>5.571</td>
      <td>1688.617</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>19</td>
      <td>07</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2021-02-20</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>86992.0</td>
      <td>418.0</td>
      <td>495.286</td>
      <td>1557.0</td>
      <td>4.0</td>
      <td>5.000</td>
      <td>1696.770</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>20</td>
      <td>07</td>
      <td>Sat</td>
    </tr>
    <tr>
      <th>2021-02-21</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>87324.0</td>
      <td>332.0</td>
      <td>493.571</td>
      <td>1562.0</td>
      <td>5.0</td>
      <td>5.000</td>
      <td>1703.245</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>21</td>
      <td>08</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>2021-02-22</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>87681.0</td>
      <td>357.0</td>
      <td>479.429</td>
      <td>1573.0</td>
      <td>11.0</td>
      <td>5.571</td>
      <td>1710.209</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>22</td>
      <td>08</td>
      <td>Mon</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>KOR</td>
      <td>Asia</td>
      <td>South Korea</td>
      <td>88120.0</td>
      <td>439.0</td>
      <td>453.429</td>
      <td>1576.0</td>
      <td>3.0</td>
      <td>5.429</td>
      <td>1718.771</td>
      <td>...</td>
      <td>40.9</td>
      <td>NaN</td>
      <td>12.27</td>
      <td>83.03</td>
      <td>0.916</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
  </tbody>
</table>
<p>400 rows × 63 columns</p>
</div>




```python
# 1. covid2.index.max() 가장 최근 날짜 구하기
covid2.index.max()
```




    Timestamp('2021-02-23 00:00:00')




```python
# 2. 가장 최신 날짜의 데이터만 선택
recent = covid2[covid2.index == covid2.index.max()]
```


```python
# 3. 값이 가장 큰 100개의 나라(location) 선택
top100 = recent.nlargest(100, 'total_cases').location.values
top100
```




    array(['World', 'Europe', 'North America', 'United States', 'Asia',
           'European Union', 'South America', 'India', 'Brazil',
           'United Kingdom', 'Russia', 'Africa', 'France', 'Spain', 'Italy',
           'Turkey', 'Germany', 'Colombia', 'Argentina', 'Mexico', 'Poland',
           'Iran', 'South Africa', 'Ukraine', 'Indonesia', 'Peru', 'Czechia',
           'Netherlands', 'Canada', 'Chile', 'Portugal', 'Romania', 'Israel',
           'Belgium', 'Iraq', 'Sweden', 'Pakistan', 'Philippines',
           'Switzerland', 'Bangladesh', 'Morocco', 'Austria', 'Serbia',
           'Japan', 'Hungary', 'Saudi Arabia', 'United Arab Emirates',
           'Jordan', 'Lebanon', 'Panama', 'Slovakia', 'Malaysia', 'Belarus',
           'Ecuador', 'Nepal', 'Georgia', 'Kazakhstan', 'Bolivia', 'Bulgaria',
           'Croatia', 'Dominican Republic', 'Azerbaijan', 'Tunisia',
           'Ireland', 'Denmark', 'Costa Rica', 'Lithuania', 'Kuwait',
           'Slovenia', 'Greece', 'Egypt', 'Moldova', 'Palestine', 'Guatemala',
           'Armenia', 'Honduras', 'Qatar', 'Ethiopia', 'Paraguay', 'Nigeria',
           'Myanmar', 'Oman', 'Venezuela', 'Libya', 'Bosnia and Herzegovina',
           'Bahrain', 'Algeria', 'Kenya', 'Albania', 'China',
           'North Macedonia', 'South Korea', 'Kyrgyzstan', 'Latvia',
           'Sri Lanka', 'Ghana', 'Uzbekistan', 'Zambia', 'Montenegro',
           'Norway'], dtype=object)




```python
# 4. covid2에서 top100 나라 데이터만 선택하여, covid2에 다시 저장하기
covid2 = covid2[covid2.location.isin(top100)]
```

5) 분석에 활용할 데이터의 수집 기간 정하기


```python
covid2.index.value_counts().sort_index()
# 2020-03-27~2021-02-23까지는 빠짐 없이 수집됨을 확인하였으므로, 해당 기간의 데이터만 활용하기로 함
```




    2020-01-01      2
    2020-01-02      2
    2020-01-03      2
    2020-01-04      2
    2020-01-05      2
    2020-01-06      2
    2020-01-07      2
    2020-01-08      2
    2020-01-09      2
    2020-01-10      2
    2020-01-11      2
    2020-01-12      2
    2020-01-13      2
    2020-01-14      2
    2020-01-15      2
    2020-01-16      2
    2020-01-17      2
    2020-01-18      2
    2020-01-19      2
    2020-01-20      2
    2020-01-21      3
    2020-01-22      9
    2020-01-23     11
    2020-01-24     13
    2020-01-25     15
    2020-01-26     16
    2020-01-27     18
    2020-01-28     18
    2020-01-29     19
    2020-01-30     21
    2020-01-31     24
    2020-02-01     26
    2020-02-02     32
    2020-02-03     28
    2020-02-04     29
    2020-02-05     29
    2020-02-06     29
    2020-02-07     31
    2020-02-08     31
    2020-02-09     36
    2020-02-10     31
    2020-02-11     31
    2020-02-12     31
    2020-02-13     32
    2020-02-14     33
    2020-02-15     33
    2020-02-16     38
    2020-02-17     34
    2020-02-18     34
    2020-02-19     35
    2020-02-20     36
    2020-02-21     37
    2020-02-22     38
    2020-02-23     46
    2020-02-24     44
    2020-02-25     49
    2020-02-26     55
    2020-02-27     56
    2020-02-28     58
    2020-02-29     62
    2020-03-01     68
    2020-03-02     70
    2020-03-03     73
    2020-03-04     77
    2020-03-05     79
    2020-03-06     84
    2020-03-07     85
    2020-03-08     87
    2020-03-09     88
    2020-03-10     88
    2020-03-11     91
    2020-03-12     91
    2020-03-13     93
    2020-03-14     95
    2020-03-15     96
    2020-03-16     96
    2020-03-17     97
    2020-03-18     99
    2020-03-19     99
    2020-03-20     99
    2020-03-21     99
    2020-03-22     99
    2020-03-23     99
    2020-03-24     99
    2020-03-25     99
    2020-03-26     99
    2020-03-27    100
    2020-03-28    100
    2020-03-29    100
    2020-03-30    100
    2020-03-31    100
    2020-04-01    100
    2020-04-02    100
    2020-04-03    100
    2020-04-04    100
    2020-04-05    100
    2020-04-06    100
    2020-04-07    100
    2020-04-08    100
    2020-04-09    100
    2020-04-10    100
    2020-04-11    100
    2020-04-12    100
    2020-04-13    100
    2020-04-14    100
    2020-04-15    100
    2020-04-16    100
    2020-04-17    100
    2020-04-18    100
    2020-04-19    100
    2020-04-20    100
    2020-04-21    100
    2020-04-22    100
    2020-04-23    100
    2020-04-24    100
    2020-04-25    100
    2020-04-26    100
    2020-04-27    100
    2020-04-28    100
    2020-04-29    100
    2020-04-30    100
    2020-05-01    100
    2020-05-02    100
    2020-05-03    100
    2020-05-04    100
    2020-05-05    100
    2020-05-06    100
    2020-05-07    100
    2020-05-08    100
    2020-05-09    100
    2020-05-10    100
    2020-05-11    100
    2020-05-12    100
    2020-05-13    100
    2020-05-14    100
    2020-05-15    100
    2020-05-16    100
    2020-05-17    100
    2020-05-18    100
    2020-05-19    100
    2020-05-20    100
    2020-05-21    100
    2020-05-22    100
    2020-05-23    100
    2020-05-24    100
    2020-05-25    100
    2020-05-26    100
    2020-05-27    100
    2020-05-28    100
    2020-05-29    100
    2020-05-30    100
    2020-05-31    100
    2020-06-01    100
    2020-06-02    100
    2020-06-03    100
    2020-06-04    100
    2020-06-05    100
    2020-06-06    100
    2020-06-07    100
    2020-06-08    100
    2020-06-09    100
    2020-06-10    100
    2020-06-11    100
    2020-06-12    100
    2020-06-13    100
    2020-06-14    100
    2020-06-15    100
    2020-06-16    100
    2020-06-17    100
    2020-06-18    100
    2020-06-19    100
    2020-06-20    100
    2020-06-21    100
    2020-06-22    100
    2020-06-23    100
    2020-06-24    100
    2020-06-25    100
    2020-06-26    100
    2020-06-27    100
    2020-06-28    100
    2020-06-29    100
    2020-06-30    100
    2020-07-01    100
    2020-07-02    100
    2020-07-03    100
    2020-07-04    100
    2020-07-05    100
    2020-07-06    100
    2020-07-07    100
    2020-07-08    100
    2020-07-09    100
    2020-07-10    100
    2020-07-11    100
    2020-07-12    100
    2020-07-13    100
    2020-07-14    100
    2020-07-15    100
    2020-07-16    100
    2020-07-17    100
    2020-07-18    100
    2020-07-19    100
    2020-07-20    100
    2020-07-21    100
    2020-07-22    100
    2020-07-23    100
    2020-07-24    100
    2020-07-25    100
    2020-07-26    100
    2020-07-27    100
    2020-07-28    100
    2020-07-29    100
    2020-07-30    100
    2020-07-31    100
    2020-08-01    100
    2020-08-02    100
    2020-08-03    100
    2020-08-04    100
    2020-08-05    100
    2020-08-06    100
    2020-08-07    100
    2020-08-08    100
    2020-08-09    100
    2020-08-10    100
    2020-08-11    100
    2020-08-12    100
    2020-08-13    100
    2020-08-14    100
    2020-08-15    100
    2020-08-16    100
    2020-08-17    100
    2020-08-18    100
    2020-08-19    100
    2020-08-20    100
    2020-08-21    100
    2020-08-22    100
    2020-08-23    100
    2020-08-24    100
    2020-08-25    100
    2020-08-26    100
    2020-08-27    100
    2020-08-28    100
    2020-08-29    100
    2020-08-30    100
    2020-08-31    100
    2020-09-01    100
    2020-09-02    100
    2020-09-03    100
    2020-09-04    100
    2020-09-05    100
    2020-09-06    100
    2020-09-07    100
    2020-09-08    100
    2020-09-09    100
    2020-09-10    100
    2020-09-11    100
    2020-09-12    100
    2020-09-13    100
    2020-09-14    100
    2020-09-15    100
    2020-09-16    100
    2020-09-17    100
    2020-09-18    100
    2020-09-19    100
    2020-09-20    100
    2020-09-21    100
    2020-09-22    100
    2020-09-23    100
    2020-09-24    100
    2020-09-25    100
    2020-09-26    100
    2020-09-27    100
    2020-09-28    100
    2020-09-29    100
    2020-09-30    100
    2020-10-01    100
    2020-10-02    100
    2020-10-03    100
    2020-10-04    100
    2020-10-05    100
    2020-10-06    100
    2020-10-07    100
    2020-10-08    100
    2020-10-09    100
    2020-10-10    100
    2020-10-11    100
    2020-10-12    100
    2020-10-13    100
    2020-10-14    100
    2020-10-15    100
    2020-10-16    100
    2020-10-17    100
    2020-10-18    100
    2020-10-19    100
    2020-10-20    100
    2020-10-21    100
    2020-10-22    100
    2020-10-23    100
    2020-10-24    100
    2020-10-25    100
    2020-10-26    100
    2020-10-27    100
    2020-10-28    100
    2020-10-29    100
    2020-10-30    100
    2020-10-31    100
    2020-11-01    100
    2020-11-02    100
    2020-11-03    100
    2020-11-04    100
    2020-11-05    100
    2020-11-06    100
    2020-11-07    100
    2020-11-08    100
    2020-11-09    100
    2020-11-10    100
    2020-11-11    100
    2020-11-12    100
    2020-11-13    100
    2020-11-14    100
    2020-11-15    100
    2020-11-16    100
    2020-11-17    100
    2020-11-18    100
    2020-11-19    100
    2020-11-20    100
    2020-11-21    100
    2020-11-22    100
    2020-11-23    100
    2020-11-24    100
    2020-11-25    100
    2020-11-26    100
    2020-11-27    100
    2020-11-28    100
    2020-11-29    100
    2020-11-30    100
    2020-12-01    100
    2020-12-02    100
    2020-12-03    100
    2020-12-04    100
    2020-12-05    100
    2020-12-06    100
    2020-12-07    100
    2020-12-08    100
    2020-12-09    100
    2020-12-10    100
    2020-12-11    100
    2020-12-12    100
    2020-12-13    100
    2020-12-14    100
    2020-12-15    100
    2020-12-16    100
    2020-12-17    100
    2020-12-18    100
    2020-12-19    100
    2020-12-20    100
    2020-12-21    100
    2020-12-22    100
    2020-12-23    100
    2020-12-24    100
    2020-12-25    100
    2020-12-26    100
    2020-12-27    100
    2020-12-28    100
    2020-12-29    100
    2020-12-30    100
    2020-12-31    100
    2021-01-01    100
    2021-01-02    100
    2021-01-03    100
    2021-01-04    100
    2021-01-05    100
    2021-01-06    100
    2021-01-07    100
    2021-01-08    100
    2021-01-09    100
    2021-01-10    100
    2021-01-11    100
    2021-01-12    100
    2021-01-13    100
    2021-01-14    100
    2021-01-15    100
    2021-01-16    100
    2021-01-17    100
    2021-01-18    100
    2021-01-19    100
    2021-01-20    100
    2021-01-21    100
    2021-01-22    100
    2021-01-23    100
    2021-01-24    100
    2021-01-25    100
    2021-01-26    100
    2021-01-27    100
    2021-01-28    100
    2021-01-29    100
    2021-01-30    100
    2021-01-31    100
    2021-02-01    100
    2021-02-02    100
    2021-02-03    100
    2021-02-04    100
    2021-02-05    100
    2021-02-06    100
    2021-02-07    100
    2021-02-08    100
    2021-02-09    100
    2021-02-10    100
    2021-02-11    100
    2021-02-12    100
    2021-02-13    100
    2021-02-14    100
    2021-02-15    100
    2021-02-16    100
    2021-02-17    100
    2021-02-18    100
    2021-02-19    100
    2021-02-20    100
    2021-02-21    100
    2021-02-22    100
    2021-02-23    100
    Name: date, dtype: int64




```python
data = covid2['2020-03-27':'2021-02-23']
```


```python
data # 최종 데이터
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>total_cases_per_million</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2020-03-27</th>
      <td>PAK</td>
      <td>Asia</td>
      <td>Pakistan</td>
      <td>1495.0</td>
      <td>122.0</td>
      <td>109.286</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>6.768</td>
      <td>...</td>
      <td>36.700</td>
      <td>59.607</td>
      <td>0.600</td>
      <td>67.27</td>
      <td>0.557</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>VEN</td>
      <td>South America</td>
      <td>Venezuela</td>
      <td>107.0</td>
      <td>0.0</td>
      <td>9.286</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>3.763</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.800</td>
      <td>72.06</td>
      <td>0.711</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>HUN</td>
      <td>Europe</td>
      <td>Hungary</td>
      <td>300.0</td>
      <td>39.0</td>
      <td>30.714</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>1.000</td>
      <td>31.055</td>
      <td>...</td>
      <td>34.800</td>
      <td>NaN</td>
      <td>7.020</td>
      <td>76.88</td>
      <td>0.854</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>MDA</td>
      <td>Europe</td>
      <td>Moldova</td>
      <td>199.0</td>
      <td>22.0</td>
      <td>19.000</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>49.331</td>
      <td>...</td>
      <td>44.600</td>
      <td>86.979</td>
      <td>5.800</td>
      <td>71.90</td>
      <td>0.750</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>NOR</td>
      <td>Europe</td>
      <td>Norway</td>
      <td>3755.0</td>
      <td>386.0</td>
      <td>263.000</td>
      <td>19.0</td>
      <td>5.0</td>
      <td>1.714</td>
      <td>692.646</td>
      <td>...</td>
      <td>20.700</td>
      <td>NaN</td>
      <td>3.600</td>
      <td>82.40</td>
      <td>0.957</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>GBR</td>
      <td>Europe</td>
      <td>United Kingdom</td>
      <td>4146760.0</td>
      <td>8523.0</td>
      <td>10917.714</td>
      <td>121536.0</td>
      <td>548.0</td>
      <td>445.000</td>
      <td>61084.167</td>
      <td>...</td>
      <td>24.700</td>
      <td>NaN</td>
      <td>2.540</td>
      <td>81.32</td>
      <td>0.932</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>NPL</td>
      <td>Asia</td>
      <td>Nepal</td>
      <td>273666.0</td>
      <td>110.0</td>
      <td>103.000</td>
      <td>2065.0</td>
      <td>4.0</td>
      <td>1.429</td>
      <td>9392.450</td>
      <td>...</td>
      <td>37.800</td>
      <td>47.782</td>
      <td>0.300</td>
      <td>70.78</td>
      <td>0.602</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>RUS</td>
      <td>Europe</td>
      <td>Russia</td>
      <td>4142126.0</td>
      <td>11679.0</td>
      <td>12655.857</td>
      <td>82666.0</td>
      <td>411.0</td>
      <td>429.571</td>
      <td>28383.467</td>
      <td>...</td>
      <td>58.300</td>
      <td>NaN</td>
      <td>8.050</td>
      <td>72.58</td>
      <td>0.824</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>OWID_WRL</td>
      <td>NaN</td>
      <td>World</td>
      <td>112109753.0</td>
      <td>387864.0</td>
      <td>371186.429</td>
      <td>2485434.0</td>
      <td>11256.0</td>
      <td>9467.571</td>
      <td>14382.636</td>
      <td>...</td>
      <td>34.635</td>
      <td>60.130</td>
      <td>2.705</td>
      <td>72.58</td>
      <td>0.737</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>CHL</td>
      <td>South America</td>
      <td>Chile</td>
      <td>805317.0</td>
      <td>2308.0</td>
      <td>3325.429</td>
      <td>20151.0</td>
      <td>25.0</td>
      <td>72.429</td>
      <td>42127.443</td>
      <td>...</td>
      <td>41.500</td>
      <td>NaN</td>
      <td>2.110</td>
      <td>80.18</td>
      <td>0.851</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
  </tbody>
</table>
<p>33400 rows × 63 columns</p>
</div>



## 3. 데이터 분석

#### [실습 #1] 누적 확진자수가 가장 많은 국가 10개 찾아보기


```python
data[(data.index == data.index.max()) & (data.continent.notnull())].nlargest(10, 'total_cases')
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>total_cases_per_million</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2021-02-23</th>
      <td>USA</td>
      <td>North America</td>
      <td>United States</td>
      <td>28261595.0</td>
      <td>71436.0</td>
      <td>71562.143</td>
      <td>502660.0</td>
      <td>2350.0</td>
      <td>2030.714</td>
      <td>85381.779</td>
      <td>...</td>
      <td>24.6</td>
      <td>NaN</td>
      <td>2.77</td>
      <td>78.86</td>
      <td>0.926</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>IND</td>
      <td>Asia</td>
      <td>India</td>
      <td>11030176.0</td>
      <td>13742.0</td>
      <td>13265.143</td>
      <td>156567.0</td>
      <td>104.0</td>
      <td>93.429</td>
      <td>7992.856</td>
      <td>...</td>
      <td>20.6</td>
      <td>59.55</td>
      <td>0.53</td>
      <td>69.66</td>
      <td>0.645</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>BRA</td>
      <td>South America</td>
      <td>Brazil</td>
      <td>10257875.0</td>
      <td>62715.0</td>
      <td>47984.857</td>
      <td>248529.0</td>
      <td>1386.0</td>
      <td>1084.143</td>
      <td>48258.861</td>
      <td>...</td>
      <td>17.9</td>
      <td>NaN</td>
      <td>2.20</td>
      <td>75.88</td>
      <td>0.765</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>GBR</td>
      <td>Europe</td>
      <td>United Kingdom</td>
      <td>4146760.0</td>
      <td>8523.0</td>
      <td>10917.714</td>
      <td>121536.0</td>
      <td>548.0</td>
      <td>445.000</td>
      <td>61084.167</td>
      <td>...</td>
      <td>24.7</td>
      <td>NaN</td>
      <td>2.54</td>
      <td>81.32</td>
      <td>0.932</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>RUS</td>
      <td>Europe</td>
      <td>Russia</td>
      <td>4142126.0</td>
      <td>11679.0</td>
      <td>12655.857</td>
      <td>82666.0</td>
      <td>411.0</td>
      <td>429.571</td>
      <td>28383.467</td>
      <td>...</td>
      <td>58.3</td>
      <td>NaN</td>
      <td>8.05</td>
      <td>72.58</td>
      <td>0.824</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>FRA</td>
      <td>Europe</td>
      <td>France</td>
      <td>3689534.0</td>
      <td>20180.0</td>
      <td>20154.571</td>
      <td>85195.0</td>
      <td>431.0</td>
      <td>319.143</td>
      <td>56524.215</td>
      <td>...</td>
      <td>35.6</td>
      <td>NaN</td>
      <td>5.98</td>
      <td>82.66</td>
      <td>0.901</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>ESP</td>
      <td>Europe</td>
      <td>Spain</td>
      <td>3161432.0</td>
      <td>7461.0</td>
      <td>9298.429</td>
      <td>68079.0</td>
      <td>443.0</td>
      <td>300.000</td>
      <td>67617.296</td>
      <td>...</td>
      <td>31.4</td>
      <td>NaN</td>
      <td>2.97</td>
      <td>83.56</td>
      <td>0.904</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>ITA</td>
      <td>Europe</td>
      <td>Italy</td>
      <td>2832162.0</td>
      <td>13299.0</td>
      <td>13224.429</td>
      <td>96348.0</td>
      <td>356.0</td>
      <td>311.000</td>
      <td>46842.150</td>
      <td>...</td>
      <td>27.8</td>
      <td>NaN</td>
      <td>3.18</td>
      <td>83.51</td>
      <td>0.892</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>TUR</td>
      <td>Asia</td>
      <td>Turkey</td>
      <td>2655633.0</td>
      <td>9107.0</td>
      <td>7657.000</td>
      <td>28213.0</td>
      <td>75.0</td>
      <td>80.143</td>
      <td>31487.579</td>
      <td>...</td>
      <td>41.1</td>
      <td>NaN</td>
      <td>2.81</td>
      <td>77.69</td>
      <td>0.820</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>DEU</td>
      <td>Europe</td>
      <td>Germany</td>
      <td>2405263.0</td>
      <td>5764.0</td>
      <td>7499.571</td>
      <td>68785.0</td>
      <td>422.0</td>
      <td>422.286</td>
      <td>28707.923</td>
      <td>...</td>
      <td>33.1</td>
      <td>NaN</td>
      <td>8.00</td>
      <td>81.33</td>
      <td>0.947</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 63 columns</p>
</div>



#### [실습 #2] 2021-02 한 달 동안(21-02-01~21-02-23) 가장 많은 확진자가 발생한 국가 10개 찾아보기


```python
data
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
      <th>iso_code</th>
      <th>continent</th>
      <th>location</th>
      <th>total_cases</th>
      <th>new_cases</th>
      <th>new_cases_smoothed</th>
      <th>total_deaths</th>
      <th>new_deaths</th>
      <th>new_deaths_smoothed</th>
      <th>total_cases_per_million</th>
      <th>...</th>
      <th>male_smokers</th>
      <th>handwashing_facilities</th>
      <th>hospital_beds_per_thousand</th>
      <th>life_expectancy</th>
      <th>human_development_index</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekNumber</th>
      <th>weekDay</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2020-03-27</th>
      <td>PAK</td>
      <td>Asia</td>
      <td>Pakistan</td>
      <td>1495.0</td>
      <td>122.0</td>
      <td>109.286</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>1.286</td>
      <td>6.768</td>
      <td>...</td>
      <td>36.700</td>
      <td>59.607</td>
      <td>0.600</td>
      <td>67.27</td>
      <td>0.557</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>VEN</td>
      <td>South America</td>
      <td>Venezuela</td>
      <td>107.0</td>
      <td>0.0</td>
      <td>9.286</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>3.763</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.800</td>
      <td>72.06</td>
      <td>0.711</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>HUN</td>
      <td>Europe</td>
      <td>Hungary</td>
      <td>300.0</td>
      <td>39.0</td>
      <td>30.714</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>1.000</td>
      <td>31.055</td>
      <td>...</td>
      <td>34.800</td>
      <td>NaN</td>
      <td>7.020</td>
      <td>76.88</td>
      <td>0.854</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>MDA</td>
      <td>Europe</td>
      <td>Moldova</td>
      <td>199.0</td>
      <td>22.0</td>
      <td>19.000</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0.143</td>
      <td>49.331</td>
      <td>...</td>
      <td>44.600</td>
      <td>86.979</td>
      <td>5.800</td>
      <td>71.90</td>
      <td>0.750</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
    </tr>
    <tr>
      <th>2020-03-27</th>
      <td>NOR</td>
      <td>Europe</td>
      <td>Norway</td>
      <td>3755.0</td>
      <td>386.0</td>
      <td>263.000</td>
      <td>19.0</td>
      <td>5.0</td>
      <td>1.714</td>
      <td>692.646</td>
      <td>...</td>
      <td>20.700</td>
      <td>NaN</td>
      <td>3.600</td>
      <td>82.40</td>
      <td>0.957</td>
      <td>2020</td>
      <td>03</td>
      <td>27</td>
      <td>12</td>
      <td>Fri</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>GBR</td>
      <td>Europe</td>
      <td>United Kingdom</td>
      <td>4146760.0</td>
      <td>8523.0</td>
      <td>10917.714</td>
      <td>121536.0</td>
      <td>548.0</td>
      <td>445.000</td>
      <td>61084.167</td>
      <td>...</td>
      <td>24.700</td>
      <td>NaN</td>
      <td>2.540</td>
      <td>81.32</td>
      <td>0.932</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>NPL</td>
      <td>Asia</td>
      <td>Nepal</td>
      <td>273666.0</td>
      <td>110.0</td>
      <td>103.000</td>
      <td>2065.0</td>
      <td>4.0</td>
      <td>1.429</td>
      <td>9392.450</td>
      <td>...</td>
      <td>37.800</td>
      <td>47.782</td>
      <td>0.300</td>
      <td>70.78</td>
      <td>0.602</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>RUS</td>
      <td>Europe</td>
      <td>Russia</td>
      <td>4142126.0</td>
      <td>11679.0</td>
      <td>12655.857</td>
      <td>82666.0</td>
      <td>411.0</td>
      <td>429.571</td>
      <td>28383.467</td>
      <td>...</td>
      <td>58.300</td>
      <td>NaN</td>
      <td>8.050</td>
      <td>72.58</td>
      <td>0.824</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>OWID_WRL</td>
      <td>NaN</td>
      <td>World</td>
      <td>112109753.0</td>
      <td>387864.0</td>
      <td>371186.429</td>
      <td>2485434.0</td>
      <td>11256.0</td>
      <td>9467.571</td>
      <td>14382.636</td>
      <td>...</td>
      <td>34.635</td>
      <td>60.130</td>
      <td>2.705</td>
      <td>72.58</td>
      <td>0.737</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>CHL</td>
      <td>South America</td>
      <td>Chile</td>
      <td>805317.0</td>
      <td>2308.0</td>
      <td>3325.429</td>
      <td>20151.0</td>
      <td>25.0</td>
      <td>72.429</td>
      <td>42127.443</td>
      <td>...</td>
      <td>41.500</td>
      <td>NaN</td>
      <td>2.110</td>
      <td>80.18</td>
      <td>0.851</td>
      <td>2021</td>
      <td>02</td>
      <td>23</td>
      <td>08</td>
      <td>Tue</td>
    </tr>
  </tbody>
</table>
<p>33400 rows × 63 columns</p>
</div>




```python
ex2 = data[data.continent.notnull()]['2021-02']\
    .pivot_table(index = 'location', values = ['new_cases', 'new_deaths'], aggfunc = 'sum')\
    .nlargest(10, 'new_cases')
```

    C:\anaconda\envs\test3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: Indexing a DataFrame with a datetimelike index using a single string to slice the rows, like `frame[string]`, is deprecated and will be removed in a future version. Use `frame.loc[string]` instead.
      """Entry point for launching an IPython kernel.
    


```python
ex2
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
      <th>new_cases</th>
      <th>new_deaths</th>
    </tr>
    <tr>
      <th>location</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>United States</th>
      <td>2074560.0</td>
      <td>54560.0</td>
    </tr>
    <tr>
      <th>Brazil</th>
      <td>1053144.0</td>
      <td>24025.0</td>
    </tr>
    <tr>
      <th>France</th>
      <td>433614.0</td>
      <td>8994.0</td>
    </tr>
    <tr>
      <th>Spain</th>
      <td>418313.0</td>
      <td>9760.0</td>
    </tr>
    <tr>
      <th>Russia</th>
      <td>333778.0</td>
      <td>10637.0</td>
    </tr>
    <tr>
      <th>United Kingdom</th>
      <td>318573.0</td>
      <td>15169.0</td>
    </tr>
    <tr>
      <th>Italy</th>
      <td>279130.0</td>
      <td>7832.0</td>
    </tr>
    <tr>
      <th>India</th>
      <td>272566.0</td>
      <td>2175.0</td>
    </tr>
    <tr>
      <th>Indonesia</th>
      <td>220294.0</td>
      <td>5016.0</td>
    </tr>
    <tr>
      <th>Mexico</th>
      <td>188006.0</td>
      <td>23273.0</td>
    </tr>
  </tbody>
</table>
</div>



#### [실습 3] 실습 2에서 구한 나라들의 2020년 4월부터 2020년 12월까지의 월별 신규확진자수, 사망자수, 검사자수 구하기


```python
# 1) data에서 실습2에서 구한 나라들의 데이터만 선택
ex3 = data[data.location.isin(ex2.index)]
```


```python
# 2) 2020년 4월부터 2020년 12월 데이터만 선택
ex3 = ex3['2020-04':'2020-12']
```


```python
# 3 월별 신규확진자수, 사망자수, 검사자수 구하기
ex3 = ex3.pivot_table(index = 'month', columns = 'location', aggfunc = 'sum', values = ['new_cases', 'new_deaths', 'new_tests'])
```


```python
ex3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">new_cases</th>
      <th>...</th>
      <th colspan="10" halign="left">new_tests</th>
    </tr>
    <tr>
      <th>location</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
      <th>...</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>04</th>
      <td>81470.0</td>
      <td>116583.0</td>
      <td>33466.0</td>
      <td>8590.0</td>
      <td>99671.0</td>
      <td>18009.0</td>
      <td>104161.0</td>
      <td>117512.0</td>
      <td>139956.0</td>
      <td>888718.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>683294.0</td>
      <td>62214.0</td>
      <td>1472249.0</td>
      <td>77689.0</td>
      <td>3317307.0</td>
      <td>0.0</td>
      <td>718807.0</td>
      <td>5618576.0</td>
    </tr>
    <tr>
      <th>05</th>
      <td>427662.0</td>
      <td>22114.0</td>
      <td>155746.0</td>
      <td>16355.0</td>
      <td>27534.0</td>
      <td>71440.0</td>
      <td>299345.0</td>
      <td>26044.0</td>
      <td>78768.0</td>
      <td>717694.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>677559.0</td>
      <td>2906826.0</td>
      <td>136443.0</td>
      <td>1899522.0</td>
      <td>191581.0</td>
      <td>7199301.0</td>
      <td>0.0</td>
      <td>2520931.0</td>
      <td>11899129.0</td>
    </tr>
    <tr>
      <th>06</th>
      <td>887192.0</td>
      <td>13269.0</td>
      <td>394872.0</td>
      <td>29912.0</td>
      <td>7581.0</td>
      <td>135425.0</td>
      <td>241086.0</td>
      <td>9792.0</td>
      <td>27677.0</td>
      <td>843368.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>1087930.0</td>
      <td>4871627.0</td>
      <td>215368.0</td>
      <td>1511371.0</td>
      <td>306893.0</td>
      <td>8929059.0</td>
      <td>0.0</td>
      <td>2656857.0</td>
      <td>18014790.0</td>
    </tr>
    <tr>
      <th>07</th>
      <td>1260444.0</td>
      <td>22995.0</td>
      <td>1110507.0</td>
      <td>51991.0</td>
      <td>6959.0</td>
      <td>198548.0</td>
      <td>191532.0</td>
      <td>39251.0</td>
      <td>19577.0</td>
      <td>1924850.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>2051856.0</td>
      <td>10224316.0</td>
      <td>265632.0</td>
      <td>1423003.0</td>
      <td>407014.0</td>
      <td>8042857.0</td>
      <td>0.0</td>
      <td>3949332.0</td>
      <td>27454928.0</td>
    </tr>
    <tr>
      <th>08</th>
      <td>1245787.0</td>
      <td>93921.0</td>
      <td>1995178.0</td>
      <td>66420.0</td>
      <td>21677.0</td>
      <td>174923.0</td>
      <td>153941.0</td>
      <td>174336.0</td>
      <td>33290.0</td>
      <td>1458662.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>3452145.0</td>
      <td>23474944.0</td>
      <td>299905.0</td>
      <td>1831746.0</td>
      <td>367906.0</td>
      <td>7789748.0</td>
      <td>0.0</td>
      <td>5230736.0</td>
      <td>25348800.0</td>
    </tr>
    <tr>
      <th>09</th>
      <td>902663.0</td>
      <td>284733.0</td>
      <td>2621418.0</td>
      <td>112212.0</td>
      <td>45647.0</td>
      <td>143656.0</td>
      <td>178397.0</td>
      <td>306330.0</td>
      <td>117765.0</td>
      <td>1206239.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>5490691.0</td>
      <td>31888815.0</td>
      <td>633641.0</td>
      <td>2689063.0</td>
      <td>359178.0</td>
      <td>9520619.0</td>
      <td>0.0</td>
      <td>6879501.0</td>
      <td>26824516.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>724670.0</td>
      <td>808471.0</td>
      <td>1871498.0</td>
      <td>123080.0</td>
      <td>364569.0</td>
      <td>181746.0</td>
      <td>435468.0</td>
      <td>416490.0</td>
      <td>558947.0</td>
      <td>1926939.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>7445662.0</td>
      <td>34599335.0</td>
      <td>810521.0</td>
      <td>4450539.0</td>
      <td>411629.0</td>
      <td>13747959.0</td>
      <td>0.0</td>
      <td>8965952.0</td>
      <td>35280518.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>800273.0</td>
      <td>862510.0</td>
      <td>1278727.0</td>
      <td>128795.0</td>
      <td>922124.0</td>
      <td>188581.0</td>
      <td>669669.0</td>
      <td>462509.0</td>
      <td>618941.0</td>
      <td>4496449.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>6601086.0</td>
      <td>31583912.0</td>
      <td>907567.0</td>
      <td>6160638.0</td>
      <td>455048.0</td>
      <td>15726155.0</td>
      <td>0.0</td>
      <td>9332998.0</td>
      <td>46214321.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1340095.0</td>
      <td>400792.0</td>
      <td>803865.0</td>
      <td>204315.0</td>
      <td>505612.0</td>
      <td>312551.0</td>
      <td>851411.0</td>
      <td>280078.0</td>
      <td>862499.0</td>
      <td>6406683.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>9067261.0</td>
      <td>29731625.0</td>
      <td>644905.0</td>
      <td>4653508.0</td>
      <td>794686.0</td>
      <td>12575649.0</td>
      <td>0.0</td>
      <td>11407092.0</td>
      <td>52551708.0</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 30 columns</p>
</div>




```python
# 4) plotly로 그리기
pd.options.plotting.backend = "plotly"
fig1 = ex3.plot()
fig1.show()
# plotly.express는 아직 계층 색인은 지원하지 않음
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-35-57d5eee6cece> in <module>
          1 # 4) plotly로 그리기
          2 pd.options.plotting.backend = "plotly"
    ----> 3 fig1 = ex3.plot()
          4 fig1.show()
          5 # plotly.express는 아직 계층 색인은 지원하지 않음
    

    C:\anaconda\envs\test3\lib\site-packages\pandas\plotting\_core.py in __call__(self, *args, **kwargs)
        900         # when using another backend, get out of the way
        901         if plot_backend.__name__ != "pandas.plotting._matplotlib":
    --> 902             return plot_backend.plot(self._parent, x=x, y=y, kind=kind, **kwargs)
        903 
        904         if kind not in self._all_kinds:
    

    C:\anaconda\envs\test3\lib\site-packages\plotly\__init__.py in plot(data_frame, kind, **kwargs)
        100         return scatter(data_frame, **new_kwargs)
        101     if kind == "line":
    --> 102         return line(data_frame, **kwargs)
        103     if kind == "area":
        104         return area(data_frame, **kwargs)
    

    C:\anaconda\envs\test3\lib\site-packages\plotly\express\_chart_types.py in line(data_frame, x, y, line_group, color, line_dash, hover_name, hover_data, custom_data, text, facet_row, facet_col, facet_col_wrap, facet_row_spacing, facet_col_spacing, error_x, error_x_minus, error_y, error_y_minus, animation_frame, animation_group, category_orders, labels, orientation, color_discrete_sequence, color_discrete_map, line_dash_sequence, line_dash_map, log_x, log_y, range_x, range_y, line_shape, render_mode, title, template, width, height)
        250     a polyline mark in 2D space.
        251     """
    --> 252     return make_figure(args=locals(), constructor=go.Scatter)
        253 
        254 
    

    C:\anaconda\envs\test3\lib\site-packages\plotly\express\_core.py in make_figure(args, constructor, trace_patch, layout_patch)
       1877     apply_default_cascade(args)
       1878 
    -> 1879     args = build_dataframe(args, constructor)
       1880     if constructor in [go.Treemap, go.Sunburst, go.Icicle] and args["path"] is not None:
       1881         args = process_dataframe_hierarchy(args)
    

    C:\anaconda\envs\test3\lib\site-packages\plotly\express\_core.py in build_dataframe(args, constructor)
       1326             if isinstance(df_input.columns, pd.MultiIndex):
       1327                 raise TypeError(
    -> 1328                     "Data frame columns is a pandas MultiIndex. "
       1329                     "pandas MultiIndex is not supported by plotly express "
       1330                     "at the moment."
    

    TypeError: Data frame columns is a pandas MultiIndex. pandas MultiIndex is not supported by plotly express at the moment.



```python
# 4) plotly로 그리기
pd.options.plotting.backend = "plotly"
fig1 = ex3['new_cases'].plot(width = 700, height = 400)
fig1.show()
```


<div>                            <div id="9bb37355-0b3f-48b1-b32a-5bad0ce0033b" class="plotly-graph-div" style="height:400px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("9bb37355-0b3f-48b1-b32a-5bad0ce0033b")) {                    Plotly.newPlot(                        "9bb37355-0b3f-48b1-b32a-5bad0ce0033b",                        [{"hovertemplate":"location=Brazil<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Brazil","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"Brazil","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[81470.0,427662.0,887192.0,1260444.0,1245787.0,902663.0,724670.0,800273.0,1340095.0],"yaxis":"y"},{"hovertemplate":"location=France<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"France","line":{"color":"#EF553B","dash":"solid"},"mode":"lines","name":"France","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[116583.0,22114.0,13269.0,22995.0,93921.0,284733.0,808471.0,862510.0,400792.0],"yaxis":"y"},{"hovertemplate":"location=India<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"India","line":{"color":"#00cc96","dash":"solid"},"mode":"lines","name":"India","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[33466.0,155746.0,394872.0,1110507.0,1995178.0,2621418.0,1871498.0,1278727.0,803865.0],"yaxis":"y"},{"hovertemplate":"location=Indonesia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Indonesia","line":{"color":"#ab63fa","dash":"solid"},"mode":"lines","name":"Indonesia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[8590.0,16355.0,29912.0,51991.0,66420.0,112212.0,123080.0,128795.0,204315.0],"yaxis":"y"},{"hovertemplate":"location=Italy<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Italy","line":{"color":"#FFA15A","dash":"solid"},"mode":"lines","name":"Italy","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[99671.0,27534.0,7581.0,6959.0,21677.0,45647.0,364569.0,922124.0,505612.0],"yaxis":"y"},{"hovertemplate":"location=Mexico<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Mexico","line":{"color":"#19d3f3","dash":"solid"},"mode":"lines","name":"Mexico","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[18009.0,71440.0,135425.0,198548.0,174923.0,143656.0,181746.0,188581.0,312551.0],"yaxis":"y"},{"hovertemplate":"location=Russia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Russia","line":{"color":"#FF6692","dash":"solid"},"mode":"lines","name":"Russia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[104161.0,299345.0,241086.0,191532.0,153941.0,178397.0,435468.0,669669.0,851411.0],"yaxis":"y"},{"hovertemplate":"location=Spain<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Spain","line":{"color":"#B6E880","dash":"solid"},"mode":"lines","name":"Spain","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[117512.0,26044.0,9792.0,39251.0,174336.0,306330.0,416490.0,462509.0,280078.0],"yaxis":"y"},{"hovertemplate":"location=United Kingdom<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United Kingdom","line":{"color":"#FF97FF","dash":"solid"},"mode":"lines","name":"United Kingdom","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[139956.0,78768.0,27677.0,19577.0,33290.0,117765.0,558947.0,618941.0,862499.0],"yaxis":"y"},{"hovertemplate":"location=United States<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United States","line":{"color":"#FECB52","dash":"solid"},"mode":"lines","name":"United States","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[888718.0,717694.0,843368.0,1924850.0,1458662.0,1206239.0,1926939.0,4496449.0,6406683.0],"yaxis":"y"}],                        {"height":400,"legend":{"title":{"text":"location"},"tracegroupgap":0},"margin":{"t":60},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"width":700,"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"month"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"value"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('9bb37355-0b3f-48b1-b32a-5bad0ce0033b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig2 = ex3['new_deaths'].plot()
fig2.show()
```


<div>                            <div id="97a93be3-6584-41bb-aff9-36dd84cff4c9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("97a93be3-6584-41bb-aff9-36dd84cff4c9")) {                    Plotly.newPlot(                        "97a93be3-6584-41bb-aff9-36dd84cff4c9",                        [{"hovertemplate":"location=Brazil<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Brazil","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"Brazil","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[5805.0,23308.0,30280.0,32881.0,28906.0,22571.0,15932.0,13236.0,21829.0],"yaxis":"y"},{"hovertemplate":"location=France<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"France","line":{"color":"#EF553B","dash":"solid"},"mode":"lines","name":"France","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[20823.0,4456.0,1041.0,422.0,378.0,1332.0,4849.0,15992.0,11940.0],"yaxis":"y"},{"hovertemplate":"location=India<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"India","line":{"color":"#00cc96","dash":"solid"},"mode":"lines","name":"India","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[1119.0,4254.0,11992.0,19111.0,28777.0,33390.0,23433.0,15510.0,11117.0],"yaxis":"y"},{"hovertemplate":"location=Indonesia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Indonesia","line":{"color":"#ab63fa","dash":"solid"},"mode":"lines","name":"Indonesia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[656.0,821.0,1263.0,2255.0,2286.0,3323.0,3129.0,3076.0,5193.0],"yaxis":"y"},{"hovertemplate":"location=Italy<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Italy","line":{"color":"#FFA15A","dash":"solid"},"mode":"lines","name":"Italy","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[15539.0,5448.0,1352.0,374.0,342.0,411.0,2724.0,16958.0,18583.0],"yaxis":"y"},{"hovertemplate":"location=Mexico<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Mexico","line":{"color":"#19d3f3","dash":"solid"},"mode":"lines","name":"Mexico","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[1830.0,8071.0,17839.0,18919.0,17726.0,13232.0,14107.0,14187.0,19867.0],"yaxis":"y"},{"hovertemplate":"location=Russia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Russia","line":{"color":"#FF6692","dash":"solid"},"mode":"lines","name":"Russia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[1056.0,3620.0,4613.0,4633.0,3189.0,3502.0,7157.0,11704.0,16780.0],"yaxis":"y"},{"hovertemplate":"location=Spain<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Spain","line":{"color":"#B6E880","dash":"solid"},"mode":"lines","name":"Spain","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[16079.0,2584.0,1228.0,90.0,649.0,2697.0,4087.0,9191.0,5768.0],"yaxis":"y"},{"hovertemplate":"location=United Kingdom<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United Kingdom","line":{"color":"#FF97FF","dash":"solid"},"mode":"lines","name":"United Kingdom","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[24297.0,10773.0,2952.0,795.0,315.0,644.0,4412.0,11900.0,15077.0],"yaxis":"y"},{"hovertemplate":"location=United States<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United States","line":{"color":"#FECB52","dash":"solid"},"mode":"lines","name":"United States","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[61016.0,41735.0,19867.0,26593.0,29715.0,23448.0,24594.0,39368.0,81059.0],"yaxis":"y"}],                        {"legend":{"title":{"text":"location"},"tracegroupgap":0},"margin":{"t":60},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"month"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"value"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('97a93be3-6584-41bb-aff9-36dd84cff4c9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig3 = ex3['new_tests'].plot()
fig3.show()
```


<div>                            <div id="2f119e73-ca80-434d-a6b9-2ef87d8a0cb0" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2f119e73-ca80-434d-a6b9-2ef87d8a0cb0")) {                    Plotly.newPlot(                        "2f119e73-ca80-434d-a6b9-2ef87d8a0cb0",                        [{"hovertemplate":"location=Brazil<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Brazil","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"Brazil","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0],"yaxis":"y"},{"hovertemplate":"location=France<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"France","line":{"color":"#EF553B","dash":"solid"},"mode":"lines","name":"France","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[0.0,677559.0,1087930.0,2051856.0,3452145.0,5490691.0,7445662.0,6601086.0,9067261.0],"yaxis":"y"},{"hovertemplate":"location=India<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"India","line":{"color":"#00cc96","dash":"solid"},"mode":"lines","name":"India","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[683294.0,2906826.0,4871627.0,10224316.0,23474944.0,31888815.0,34599335.0,31583912.0,29731625.0],"yaxis":"y"},{"hovertemplate":"location=Indonesia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Indonesia","line":{"color":"#ab63fa","dash":"solid"},"mode":"lines","name":"Indonesia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[62214.0,136443.0,215368.0,265632.0,299905.0,633641.0,810521.0,907567.0,644905.0],"yaxis":"y"},{"hovertemplate":"location=Italy<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Italy","line":{"color":"#FFA15A","dash":"solid"},"mode":"lines","name":"Italy","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[1472249.0,1899522.0,1511371.0,1423003.0,1831746.0,2689063.0,4450539.0,6160638.0,4653508.0],"yaxis":"y"},{"hovertemplate":"location=Mexico<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Mexico","line":{"color":"#19d3f3","dash":"solid"},"mode":"lines","name":"Mexico","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[77689.0,191581.0,306893.0,407014.0,367906.0,359178.0,411629.0,455048.0,794686.0],"yaxis":"y"},{"hovertemplate":"location=Russia<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Russia","line":{"color":"#FF6692","dash":"solid"},"mode":"lines","name":"Russia","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[3317307.0,7199301.0,8929059.0,8042857.0,7789748.0,9520619.0,13747959.0,15726155.0,12575649.0],"yaxis":"y"},{"hovertemplate":"location=Spain<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Spain","line":{"color":"#B6E880","dash":"solid"},"mode":"lines","name":"Spain","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0],"yaxis":"y"},{"hovertemplate":"location=United Kingdom<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United Kingdom","line":{"color":"#FF97FF","dash":"solid"},"mode":"lines","name":"United Kingdom","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[718807.0,2520931.0,2656857.0,3949332.0,5230736.0,6879501.0,8965952.0,9332998.0,11407092.0],"yaxis":"y"},{"hovertemplate":"location=United States<br>month=%{x}<br>value=%{y}<extra></extra>","legendgroup":"United States","line":{"color":"#FECB52","dash":"solid"},"mode":"lines","name":"United States","orientation":"v","showlegend":true,"type":"scatter","x":["04","05","06","07","08","09","10","11","12"],"xaxis":"x","y":[5618576.0,11899129.0,18014790.0,27454928.0,25348800.0,26824516.0,35280518.0,46214321.0,52551708.0],"yaxis":"y"}],                        {"legend":{"title":{"text":"location"},"tracegroupgap":0},"margin":{"t":60},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"month"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"value"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2f119e73-ca80-434d-a6b9-2ef87d8a0cb0');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
ex3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">new_cases</th>
      <th>...</th>
      <th colspan="10" halign="left">new_tests</th>
    </tr>
    <tr>
      <th>location</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
      <th>...</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>04</th>
      <td>81470.0</td>
      <td>116583.0</td>
      <td>33466.0</td>
      <td>8590.0</td>
      <td>99671.0</td>
      <td>18009.0</td>
      <td>104161.0</td>
      <td>117512.0</td>
      <td>139956.0</td>
      <td>888718.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>683294.0</td>
      <td>62214.0</td>
      <td>1472249.0</td>
      <td>77689.0</td>
      <td>3317307.0</td>
      <td>0.0</td>
      <td>718807.0</td>
      <td>5618576.0</td>
    </tr>
    <tr>
      <th>05</th>
      <td>427662.0</td>
      <td>22114.0</td>
      <td>155746.0</td>
      <td>16355.0</td>
      <td>27534.0</td>
      <td>71440.0</td>
      <td>299345.0</td>
      <td>26044.0</td>
      <td>78768.0</td>
      <td>717694.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>677559.0</td>
      <td>2906826.0</td>
      <td>136443.0</td>
      <td>1899522.0</td>
      <td>191581.0</td>
      <td>7199301.0</td>
      <td>0.0</td>
      <td>2520931.0</td>
      <td>11899129.0</td>
    </tr>
    <tr>
      <th>06</th>
      <td>887192.0</td>
      <td>13269.0</td>
      <td>394872.0</td>
      <td>29912.0</td>
      <td>7581.0</td>
      <td>135425.0</td>
      <td>241086.0</td>
      <td>9792.0</td>
      <td>27677.0</td>
      <td>843368.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>1087930.0</td>
      <td>4871627.0</td>
      <td>215368.0</td>
      <td>1511371.0</td>
      <td>306893.0</td>
      <td>8929059.0</td>
      <td>0.0</td>
      <td>2656857.0</td>
      <td>18014790.0</td>
    </tr>
    <tr>
      <th>07</th>
      <td>1260444.0</td>
      <td>22995.0</td>
      <td>1110507.0</td>
      <td>51991.0</td>
      <td>6959.0</td>
      <td>198548.0</td>
      <td>191532.0</td>
      <td>39251.0</td>
      <td>19577.0</td>
      <td>1924850.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>2051856.0</td>
      <td>10224316.0</td>
      <td>265632.0</td>
      <td>1423003.0</td>
      <td>407014.0</td>
      <td>8042857.0</td>
      <td>0.0</td>
      <td>3949332.0</td>
      <td>27454928.0</td>
    </tr>
    <tr>
      <th>08</th>
      <td>1245787.0</td>
      <td>93921.0</td>
      <td>1995178.0</td>
      <td>66420.0</td>
      <td>21677.0</td>
      <td>174923.0</td>
      <td>153941.0</td>
      <td>174336.0</td>
      <td>33290.0</td>
      <td>1458662.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>3452145.0</td>
      <td>23474944.0</td>
      <td>299905.0</td>
      <td>1831746.0</td>
      <td>367906.0</td>
      <td>7789748.0</td>
      <td>0.0</td>
      <td>5230736.0</td>
      <td>25348800.0</td>
    </tr>
    <tr>
      <th>09</th>
      <td>902663.0</td>
      <td>284733.0</td>
      <td>2621418.0</td>
      <td>112212.0</td>
      <td>45647.0</td>
      <td>143656.0</td>
      <td>178397.0</td>
      <td>306330.0</td>
      <td>117765.0</td>
      <td>1206239.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>5490691.0</td>
      <td>31888815.0</td>
      <td>633641.0</td>
      <td>2689063.0</td>
      <td>359178.0</td>
      <td>9520619.0</td>
      <td>0.0</td>
      <td>6879501.0</td>
      <td>26824516.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>724670.0</td>
      <td>808471.0</td>
      <td>1871498.0</td>
      <td>123080.0</td>
      <td>364569.0</td>
      <td>181746.0</td>
      <td>435468.0</td>
      <td>416490.0</td>
      <td>558947.0</td>
      <td>1926939.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>7445662.0</td>
      <td>34599335.0</td>
      <td>810521.0</td>
      <td>4450539.0</td>
      <td>411629.0</td>
      <td>13747959.0</td>
      <td>0.0</td>
      <td>8965952.0</td>
      <td>35280518.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>800273.0</td>
      <td>862510.0</td>
      <td>1278727.0</td>
      <td>128795.0</td>
      <td>922124.0</td>
      <td>188581.0</td>
      <td>669669.0</td>
      <td>462509.0</td>
      <td>618941.0</td>
      <td>4496449.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>6601086.0</td>
      <td>31583912.0</td>
      <td>907567.0</td>
      <td>6160638.0</td>
      <td>455048.0</td>
      <td>15726155.0</td>
      <td>0.0</td>
      <td>9332998.0</td>
      <td>46214321.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1340095.0</td>
      <td>400792.0</td>
      <td>803865.0</td>
      <td>204315.0</td>
      <td>505612.0</td>
      <td>312551.0</td>
      <td>851411.0</td>
      <td>280078.0</td>
      <td>862499.0</td>
      <td>6406683.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>9067261.0</td>
      <td>29731625.0</td>
      <td>644905.0</td>
      <td>4653508.0</td>
      <td>794686.0</td>
      <td>12575649.0</td>
      <td>0.0</td>
      <td>11407092.0</td>
      <td>52551708.0</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 30 columns</p>
</div>




```python
ex3.to_excel('data/covid_top10.xlsx')
```


```python
# 이 부분은 하지 말자..## 
```


```python
ex3_1 = ex3.stack(0).reset_index()
```


```python
ex3_1.columns[2:12]
```




    Index(['Brazil', 'France', 'India', 'Indonesia', 'Italy', 'Mexico', 'Russia',
           'Spain', 'United Kingdom', 'United States'],
          dtype='object', name='location')




```python
# 하나의 figure 안에 각각의 subplot으로 그리기
ex3_1.plot(x='month',y = ex_3_1.columns[2:12],
                               facet_row = 'level_1')
```


```python
# 5) 그래프를 html로 export 하기
fig1.write_html('covid_top10_countries_new_cases.html')
fig2.write_html('covid_top10_countries_new_deaths.html')
fig3.write_html('covid_top10_countries_new_tests.html')
```


```python

```

## 4. 데이터 분석 결과로 웹 서버 구축하기 with Dash
 - https://plotly.com/python/getting-started/


```python
#pip install dash
```

    Collecting dash
      Downloading dash-1.20.0.tar.gz (77 kB)
    Collecting Flask>=1.0.4
      Using cached Flask-2.0.1-py3-none-any.whl (94 kB)
    Collecting flask-compress
      Downloading Flask_Compress-1.10.1-py3-none-any.whl (7.9 kB)
    Requirement already satisfied: plotly in c:\anaconda\envs\test3\lib\site-packages (from dash) (5.1.0)
    Collecting dash_renderer==1.9.1
      Downloading dash_renderer-1.9.1.tar.gz (1.0 MB)
    Collecting dash-core-components==1.16.0
      Downloading dash_core_components-1.16.0.tar.gz (3.5 MB)
    Note: you may need to restart the kernel to use updated packages.Collecting dash-html-components==1.1.3
      Downloading dash_html_components-1.1.3.tar.gz (82 kB)
    Collecting dash-table==4.11.3
      Downloading dash_table-4.11.3.tar.gz (1.8 MB)
    Requirement already satisfied: future in c:\anaconda\envs\test3\lib\site-packages (from dash) (0.18.2)
    Collecting itsdangerous>=2.0
      Using cached itsdangerous-2.0.1-py3-none-any.whl (18 kB)
    Collecting click>=7.1.2
      Using cached click-8.0.1-py3-none-any.whl (97 kB)
    Collecting Werkzeug>=2.0
      Using cached Werkzeug-2.0.1-py3-none-any.whl (288 kB)
    Collecting Jinja2>=3.0
      Using cached Jinja2-3.0.1-py3-none-any.whl (133 kB)
    Requirement already satisfied: colorama in c:\anaconda\envs\test3\lib\site-packages (from click>=7.1.2->Flask>=1.0.4->dash) (0.4.3)
    Requirement already satisfied: importlib-metadata in c:\anaconda\envs\test3\lib\site-packages (from click>=7.1.2->Flask>=1.0.4->dash) (1.7.0)
    Collecting MarkupSafe>=2.0
    
    

    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
        WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
        WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
        WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    

      Using cached MarkupSafe-2.0.1-cp37-cp37m-win_amd64.whl (14 kB)
    Collecting brotli
      Downloading Brotli-1.0.9-cp37-cp37m-win_amd64.whl (365 kB)
    Requirement already satisfied: zipp>=0.5 in c:\anaconda\envs\test3\lib\site-packages (from importlib-metadata->click>=7.1.2->Flask>=1.0.4->dash) (3.1.0)
    Requirement already satisfied: six in c:\anaconda\envs\test3\lib\site-packages (from plotly->dash) (1.15.0)
    Requirement already satisfied: tenacity>=6.2.0 in c:\anaconda\envs\test3\lib\site-packages (from plotly->dash) (8.0.0)
    Building wheels for collected packages: dash, dash-core-components, dash-html-components, dash-renderer, dash-table
      Building wheel for dash (setup.py): started
      Building wheel for dash (setup.py): finished with status 'done'
      Created wheel for dash: filename=dash-1.20.0-py3-none-any.whl size=85841 sha256=74e400d5bbb4387a4ac8be141b2994976215de5879ea84fb52d1780344930095
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\4f\c3\6a\a7cb9cedfdf93e0f0e8db0817ff2646d319afa9e4ca543ef9c
      Building wheel for dash-core-components (setup.py): started
      Building wheel for dash-core-components (setup.py): finished with status 'done'
      Created wheel for dash-core-components: filename=dash_core_components-1.16.0-py3-none-any.whl size=3540990 sha256=4b9dacac8322557f9e05ab04a3d8ea496d6abd99d5f9c6cb422f6139c71beae5
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\6a\50\7e\c440176e48ba46baa157b0f10365f6871acd133460b4e0abe4
      Building wheel for dash-html-components (setup.py): started
      Building wheel for dash-html-components (setup.py): finished with status 'done'
      Created wheel for dash-html-components: filename=dash_html_components-1.1.3-py3-none-any.whl size=319490 sha256=88e473e91ae68a574e98d4a0e4f0aa88889dce362221acd59127f91973e9c5cb
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\7d\75\be\b836bd1c1f92c4823fef293f82375ca448c1fc627ce50acff0
      Building wheel for dash-renderer (setup.py): started
      Building wheel for dash-renderer (setup.py): finished with status 'done'
      Created wheel for dash-renderer: filename=dash_renderer-1.9.1-py3-none-any.whl size=1014875 sha256=97a77e94e680808a60d581402f08cc080dcee362be572f9aeb28699cc31fcd6a
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\1a\2f\c0\783c571f9861e5c716b56638926b03fb80bc58991e5a54947d
      Building wheel for dash-table (setup.py): started
      Building wheel for dash-table (setup.py): finished with status 'done'
      Created wheel for dash-table: filename=dash_table-4.11.3-py3-none-any.whl size=1827622 sha256=23c9cd147f84778ea9d772bf1187c057edf6eaaaa56b4d99238bce87a65ed9f9
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\99\5e\cb\e1918e34d079454bad0504e73a9195d9da8cd46605560e6864
    Successfully built dash dash-core-components dash-html-components dash-renderer dash-table
    Installing collected packages: MarkupSafe, Werkzeug, Jinja2, itsdangerous, click, Flask, brotli, flask-compress, dash-table, dash-renderer, dash-html-components, dash-core-components, dash
      Attempting uninstall: MarkupSafe
        Found existing installation: MarkupSafe 1.1.1
        Uninstalling MarkupSafe-1.1.1:
          Successfully uninstalled MarkupSafe-1.1.1
      Attempting uninstall: Werkzeug
        Found existing installation: Werkzeug 1.0.1
        Uninstalling Werkzeug-1.0.1:
          Successfully uninstalled Werkzeug-1.0.1
      Attempting uninstall: Jinja2
        Found existing installation: Jinja2 2.11.2
        Uninstalling Jinja2-2.11.2:
          Successfully uninstalled Jinja2-2.11.2
    Successfully installed Flask-2.0.1 Jinja2-3.0.1 MarkupSafe-2.0.1 Werkzeug-2.0.1 brotli-1.0.9 click-8.0.1 dash-1.20.0 dash-core-components-1.16.0 dash-html-components-1.1.3 dash-renderer-1.9.1 dash-table-4.11.3 flask-compress-1.10.1 itsdangerous-2.0.1
    


```python
#pip install Werkzeug>=2.0 
```

    Note: you may need to restart the kernel to use updated packages.
    

    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    


```python
#pip install jupyter-dash
```

    Collecting jupyter-dashNote: you may need to restart the kernel to use updated packages.
    

    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
        WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    

    
      Downloading jupyter_dash-0.4.0-py3-none-any.whl (20 kB)
    Collecting ansi2html
      Downloading ansi2html-1.6.0-py3-none-any.whl (14 kB)
    Requirement already satisfied: ipython in c:\anaconda\envs\test3\lib\site-packages (from jupyter-dash) (7.18.1)
    Collecting retrying
      Downloading retrying-1.3.3.tar.gz (10 kB)
    Requirement already satisfied: ipykernel in c:\anaconda\envs\test3\lib\site-packages (from jupyter-dash) (5.3.4)
    Requirement already satisfied: flask in c:\anaconda\envs\test3\lib\site-packages (from jupyter-dash) (2.0.1)
    Requirement already satisfied: dash in c:\anaconda\envs\test3\lib\site-packages (from jupyter-dash) (1.20.0)
    Requirement already satisfied: requests in c:\anaconda\envs\test3\lib\site-packages (from jupyter-dash) (2.24.0)
    Requirement already satisfied: plotly in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (5.1.0)
    Requirement already satisfied: future in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (0.18.2)
    Requirement already satisfied: dash-renderer==1.9.1 in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (1.9.1)
    Requirement already satisfied: dash-core-components==1.16.0 in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (1.16.0)
    Requirement already satisfied: dash-table==4.11.3 in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (4.11.3)
    Requirement already satisfied: dash-html-components==1.1.3 in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (1.1.3)
    Requirement already satisfied: flask-compress in c:\anaconda\envs\test3\lib\site-packages (from dash->jupyter-dash) (1.10.1)
    Requirement already satisfied: itsdangerous>=2.0 in c:\anaconda\envs\test3\lib\site-packages (from flask->jupyter-dash) (2.0.1)
    Requirement already satisfied: click>=7.1.2 in c:\anaconda\envs\test3\lib\site-packages (from flask->jupyter-dash) (8.0.1)
    Collecting Werkzeug>=2.0
      Using cached Werkzeug-2.0.1-py3-none-any.whl (288 kB)
    Requirement already satisfied: Jinja2>=3.0 in c:\anaconda\envs\test3\lib\site-packages (from flask->jupyter-dash) (3.0.1)
    Requirement already satisfied: importlib-metadata in c:\anaconda\envs\test3\lib\site-packages (from click>=7.1.2->flask->jupyter-dash) (1.7.0)
    Requirement already satisfied: colorama in c:\anaconda\envs\test3\lib\site-packages (from click>=7.1.2->flask->jupyter-dash) (0.4.3)
    Requirement already satisfied: MarkupSafe>=2.0 in c:\anaconda\envs\test3\lib\site-packages (from Jinja2>=3.0->flask->jupyter-dash) (2.0.1)
    Requirement already satisfied: brotli in c:\anaconda\envs\test3\lib\site-packages (from flask-compress->dash->jupyter-dash) (1.0.9)
    Requirement already satisfied: zipp>=0.5 in c:\anaconda\envs\test3\lib\site-packages (from importlib-metadata->click>=7.1.2->flask->jupyter-dash) (3.1.0)
    Requirement already satisfied: tornado>=4.2 in c:\anaconda\envs\test3\lib\site-packages (from ipykernel->jupyter-dash) (6.0.4)
    Requirement already satisfied: jupyter-client in c:\anaconda\envs\test3\lib\site-packages (from ipykernel->jupyter-dash) (6.1.6)
    Requirement already satisfied: traitlets>=4.1.0 in c:\anaconda\envs\test3\lib\site-packages (from ipykernel->jupyter-dash) (4.3.3)
    Requirement already satisfied: pickleshare in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (0.7.5)
    Requirement already satisfied: prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0 in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (3.0.7)
    Requirement already satisfied: pygments in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (2.6.1)
    Requirement already satisfied: setuptools>=18.5 in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (57.1.0)
    Requirement already satisfied: backcall in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (0.2.0)
    Requirement already satisfied: decorator in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (4.4.2)
    Requirement already satisfied: jedi>=0.10 in c:\anaconda\envs\test3\lib\site-packages (from ipython->jupyter-dash) (0.17.1)
    Requirement already satisfied: parso<0.8.0,>=0.7.0 in c:\anaconda\envs\test3\lib\site-packages (from jedi>=0.10->ipython->jupyter-dash) (0.7.0)
    Requirement already satisfied: wcwidth in c:\anaconda\envs\test3\lib\site-packages (from prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0->ipython->jupyter-dash) (0.2.5)
    Requirement already satisfied: ipython-genutils in c:\anaconda\envs\test3\lib\site-packages (from traitlets>=4.1.0->ipykernel->jupyter-dash) (0.2.0)
    Requirement already satisfied: six in c:\anaconda\envs\test3\lib\site-packages (from traitlets>=4.1.0->ipykernel->jupyter-dash) (1.15.0)
    Requirement already satisfied: jupyter-core>=4.6.0 in c:\anaconda\envs\test3\lib\site-packages (from jupyter-client->ipykernel->jupyter-dash) (4.6.3)
    Requirement already satisfied: pyzmq>=13 in c:\anaconda\envs\test3\lib\site-packages (from jupyter-client->ipykernel->jupyter-dash) (19.0.1)
    Requirement already satisfied: python-dateutil>=2.1 in c:\anaconda\envs\test3\lib\site-packages (from jupyter-client->ipykernel->jupyter-dash) (2.8.1)
    Requirement already satisfied: pywin32>=1.0 in c:\anaconda\envs\test3\lib\site-packages (from jupyter-core>=4.6.0->jupyter-client->ipykernel->jupyter-dash) (227)
    Requirement already satisfied: tenacity>=6.2.0 in c:\anaconda\envs\test3\lib\site-packages (from plotly->dash->jupyter-dash) (8.0.0)
    Requirement already satisfied: idna<3,>=2.5 in c:\anaconda\envs\test3\lib\site-packages (from requests->jupyter-dash) (2.10)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in c:\anaconda\envs\test3\lib\site-packages (from requests->jupyter-dash) (1.25.10)
    Requirement already satisfied: chardet<4,>=3.0.2 in c:\anaconda\envs\test3\lib\site-packages (from requests->jupyter-dash) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in c:\anaconda\envs\test3\lib\site-packages (from requests->jupyter-dash) (2020.6.20)
    Building wheels for collected packages: retrying
      Building wheel for retrying (setup.py): started
      Building wheel for retrying (setup.py): finished with status 'done'
      Created wheel for retrying: filename=retrying-1.3.3-py3-none-any.whl size=11448 sha256=3216bde678751880384a6a01a27a9c99b5d44278ec1b54d85b65a0b55ddaa1c1
      Stored in directory: c:\users\chom5\appdata\local\pip\cache\wheels\f9\8d\8d\f6af3f7f9eea3553bc2fe6d53e4b287dad18b06a861ac56ddf
    Successfully built retrying
    Installing collected packages: Werkzeug, retrying, ansi2html, jupyter-dash
      Attempting uninstall: Werkzeug
        Found existing installation: Werkzeug 0.16.0
        Uninstalling Werkzeug-0.16.0:
          Successfully uninstalled Werkzeug-0.16.0
    Successfully installed Werkzeug-2.0.1 ansi2html-1.6.0 jupyter-dash-0.4.0 retrying-1.3.3
    


```python
#pip install Flask
```

    Requirement already satisfied: Flask in c:\anaconda\envs\test3\lib\site-packages (0.12.2)
    Requirement already satisfied: Jinja2>=2.4 in c:\anaconda\envs\test3\lib\site-packages (from Flask) (3.0.1)
    Requirement already satisfied: itsdangerous>=0.21 in c:\anaconda\envs\test3\lib\site-packages (from Flask) (2.0.1)
    Requirement already satisfied: Werkzeug>=0.7 in c:\anaconda\envs\test3\lib\site-packages (from Flask) (2.0.1)
    Requirement already satisfied: click>=2.0 in c:\anaconda\envs\test3\lib\site-packages (from Flask) (8.0.1)
    Requirement already satisfied: colorama in c:\anaconda\envs\test3\lib\site-packages (from click>=2.0->Flask) (0.4.3)
    Requirement already satisfied: importlib-metadata in c:\anaconda\envs\test3\lib\site-packages (from click>=2.0->Flask) (1.7.0)
    Requirement already satisfied: MarkupSafe>=2.0 in c:\anaconda\envs\test3\lib\site-packages (from Jinja2>=2.4->Flask) (2.0.1)
    Requirement already satisfied: zipp>=0.5 in c:\anaconda\envs\test3\lib\site-packages (from importlib-metadata->click>=2.0->Flask) (3.1.0)
    Note: you may need to restart the kernel to use updated packages.
    

    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    WARNING: Ignoring invalid distribution -umpy (c:\anaconda\envs\test3\lib\site-packages)
    


```python
# https://plotly.com/python/getting-started/
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
import plotly.graph_objects as go

app = dash.Dash(__name__)

app.layout = html.Div([
    html.P("Color:"),
    dcc.Dropdown(
        id="dropdown",
        options=[
            {'label': x, 'value': x}
            for x in ['Gold', 'MediumTurquoise', 'LightGreen']
        ],
        value='Gold',
        clearable=False,
    ),
    dcc.Graph(id="graph"),
])

@app.callback(
    Output("graph", "figure"), 
    [Input("dropdown", "value")])
def display_color(color):
    fig = go.Figure(
        data=go.Bar(y=[2, 3, 1], marker_color=color))
    return fig

app.run_server(debug=True)
```

    Dash is running on http://127.0.0.1:8050/
    
     * Serving Flask app '__main__' (lazy loading)
     * Environment: production
    [31m   WARNING: This is a development server. Do not use it in a production deployment.[0m
    [2m   Use a production WSGI server instead.[0m
     * Debug mode: on
    


    An exception has occurred, use %tb to see the full traceback.
    

    SystemExit: 1
    


    C:\anaconda\envs\test3\lib\site-packages\IPython\core\interactiveshell.py:3425: UserWarning:
    
    To exit: use 'exit', 'quit', or Ctrl-D.
    
    


```python

```


```python
#### []
```


```python
covid19_top10 = pd.read_excel('data/covid_top10.xlsx', header = [0,1])
```


```python
covid19_top10
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>Unnamed: 0_level_0</th>
      <th colspan="9" halign="left">new_cases</th>
      <th>...</th>
      <th colspan="10" halign="left">new_tests</th>
    </tr>
    <tr>
      <th></th>
      <th>location</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>...</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>month</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>04</td>
      <td>81470.0</td>
      <td>116583.0</td>
      <td>33466.0</td>
      <td>8590.0</td>
      <td>99671.0</td>
      <td>18009.0</td>
      <td>104161.0</td>
      <td>117512.0</td>
      <td>139956.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>683294.0</td>
      <td>62214.0</td>
      <td>1472249.0</td>
      <td>77689.0</td>
      <td>3317307.0</td>
      <td>0.0</td>
      <td>718807.0</td>
      <td>5618576.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>05</td>
      <td>427662.0</td>
      <td>22114.0</td>
      <td>155746.0</td>
      <td>16355.0</td>
      <td>27534.0</td>
      <td>71440.0</td>
      <td>299345.0</td>
      <td>26044.0</td>
      <td>78768.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>677559.0</td>
      <td>2906826.0</td>
      <td>136443.0</td>
      <td>1899522.0</td>
      <td>191581.0</td>
      <td>7199301.0</td>
      <td>0.0</td>
      <td>2520931.0</td>
      <td>11899129.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>06</td>
      <td>887192.0</td>
      <td>13269.0</td>
      <td>394872.0</td>
      <td>29912.0</td>
      <td>7581.0</td>
      <td>135425.0</td>
      <td>241086.0</td>
      <td>9792.0</td>
      <td>27677.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>1087930.0</td>
      <td>4871627.0</td>
      <td>215368.0</td>
      <td>1511371.0</td>
      <td>306893.0</td>
      <td>8929059.0</td>
      <td>0.0</td>
      <td>2656857.0</td>
      <td>18014790.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>07</td>
      <td>1260444.0</td>
      <td>22995.0</td>
      <td>1110507.0</td>
      <td>51991.0</td>
      <td>6959.0</td>
      <td>198548.0</td>
      <td>191532.0</td>
      <td>39251.0</td>
      <td>19577.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>2051856.0</td>
      <td>10224316.0</td>
      <td>265632.0</td>
      <td>1423003.0</td>
      <td>407014.0</td>
      <td>8042857.0</td>
      <td>0.0</td>
      <td>3949332.0</td>
      <td>27454928.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>08</td>
      <td>1245787.0</td>
      <td>93921.0</td>
      <td>1995178.0</td>
      <td>66420.0</td>
      <td>21677.0</td>
      <td>174923.0</td>
      <td>153941.0</td>
      <td>174336.0</td>
      <td>33290.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>3452145.0</td>
      <td>23474944.0</td>
      <td>299905.0</td>
      <td>1831746.0</td>
      <td>367906.0</td>
      <td>7789748.0</td>
      <td>0.0</td>
      <td>5230736.0</td>
      <td>25348800.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>09</td>
      <td>902663.0</td>
      <td>284733.0</td>
      <td>2621418.0</td>
      <td>112212.0</td>
      <td>45647.0</td>
      <td>143656.0</td>
      <td>178397.0</td>
      <td>306330.0</td>
      <td>117765.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>5490691.0</td>
      <td>31888815.0</td>
      <td>633641.0</td>
      <td>2689063.0</td>
      <td>359178.0</td>
      <td>9520619.0</td>
      <td>0.0</td>
      <td>6879501.0</td>
      <td>26824516.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10</td>
      <td>724670.0</td>
      <td>808471.0</td>
      <td>1871498.0</td>
      <td>123080.0</td>
      <td>364569.0</td>
      <td>181746.0</td>
      <td>435468.0</td>
      <td>416490.0</td>
      <td>558947.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>7445662.0</td>
      <td>34599335.0</td>
      <td>810521.0</td>
      <td>4450539.0</td>
      <td>411629.0</td>
      <td>13747959.0</td>
      <td>0.0</td>
      <td>8965952.0</td>
      <td>35280518.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>11</td>
      <td>800273.0</td>
      <td>862510.0</td>
      <td>1278727.0</td>
      <td>128795.0</td>
      <td>922124.0</td>
      <td>188581.0</td>
      <td>669669.0</td>
      <td>462509.0</td>
      <td>618941.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>6601086.0</td>
      <td>31583912.0</td>
      <td>907567.0</td>
      <td>6160638.0</td>
      <td>455048.0</td>
      <td>15726155.0</td>
      <td>0.0</td>
      <td>9332998.0</td>
      <td>46214321.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>12</td>
      <td>1340095.0</td>
      <td>400792.0</td>
      <td>803865.0</td>
      <td>204315.0</td>
      <td>505612.0</td>
      <td>312551.0</td>
      <td>851411.0</td>
      <td>280078.0</td>
      <td>862499.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>9067261.0</td>
      <td>29731625.0</td>
      <td>644905.0</td>
      <td>4653508.0</td>
      <td>794686.0</td>
      <td>12575649.0</td>
      <td>0.0</td>
      <td>11407092.0</td>
      <td>52551708.0</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 31 columns</p>
</div>




```python
covid19_top10.set_index(covid19_top10.columns[0])[1:]
# 데이터
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">new_cases</th>
      <th>...</th>
      <th colspan="10" halign="left">new_tests</th>
    </tr>
    <tr>
      <th></th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
      <th>...</th>
      <th>Brazil</th>
      <th>France</th>
      <th>India</th>
      <th>Indonesia</th>
      <th>Italy</th>
      <th>Mexico</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>United Kingdom</th>
      <th>United States</th>
    </tr>
    <tr>
      <th>(Unnamed: 0_level_0, location)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>04</th>
      <td>81470.0</td>
      <td>116583.0</td>
      <td>33466.0</td>
      <td>8590.0</td>
      <td>99671.0</td>
      <td>18009.0</td>
      <td>104161.0</td>
      <td>117512.0</td>
      <td>139956.0</td>
      <td>888718.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>683294.0</td>
      <td>62214.0</td>
      <td>1472249.0</td>
      <td>77689.0</td>
      <td>3317307.0</td>
      <td>0.0</td>
      <td>718807.0</td>
      <td>5618576.0</td>
    </tr>
    <tr>
      <th>05</th>
      <td>427662.0</td>
      <td>22114.0</td>
      <td>155746.0</td>
      <td>16355.0</td>
      <td>27534.0</td>
      <td>71440.0</td>
      <td>299345.0</td>
      <td>26044.0</td>
      <td>78768.0</td>
      <td>717694.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>677559.0</td>
      <td>2906826.0</td>
      <td>136443.0</td>
      <td>1899522.0</td>
      <td>191581.0</td>
      <td>7199301.0</td>
      <td>0.0</td>
      <td>2520931.0</td>
      <td>11899129.0</td>
    </tr>
    <tr>
      <th>06</th>
      <td>887192.0</td>
      <td>13269.0</td>
      <td>394872.0</td>
      <td>29912.0</td>
      <td>7581.0</td>
      <td>135425.0</td>
      <td>241086.0</td>
      <td>9792.0</td>
      <td>27677.0</td>
      <td>843368.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>1087930.0</td>
      <td>4871627.0</td>
      <td>215368.0</td>
      <td>1511371.0</td>
      <td>306893.0</td>
      <td>8929059.0</td>
      <td>0.0</td>
      <td>2656857.0</td>
      <td>18014790.0</td>
    </tr>
    <tr>
      <th>07</th>
      <td>1260444.0</td>
      <td>22995.0</td>
      <td>1110507.0</td>
      <td>51991.0</td>
      <td>6959.0</td>
      <td>198548.0</td>
      <td>191532.0</td>
      <td>39251.0</td>
      <td>19577.0</td>
      <td>1924850.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>2051856.0</td>
      <td>10224316.0</td>
      <td>265632.0</td>
      <td>1423003.0</td>
      <td>407014.0</td>
      <td>8042857.0</td>
      <td>0.0</td>
      <td>3949332.0</td>
      <td>27454928.0</td>
    </tr>
    <tr>
      <th>08</th>
      <td>1245787.0</td>
      <td>93921.0</td>
      <td>1995178.0</td>
      <td>66420.0</td>
      <td>21677.0</td>
      <td>174923.0</td>
      <td>153941.0</td>
      <td>174336.0</td>
      <td>33290.0</td>
      <td>1458662.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>3452145.0</td>
      <td>23474944.0</td>
      <td>299905.0</td>
      <td>1831746.0</td>
      <td>367906.0</td>
      <td>7789748.0</td>
      <td>0.0</td>
      <td>5230736.0</td>
      <td>25348800.0</td>
    </tr>
    <tr>
      <th>09</th>
      <td>902663.0</td>
      <td>284733.0</td>
      <td>2621418.0</td>
      <td>112212.0</td>
      <td>45647.0</td>
      <td>143656.0</td>
      <td>178397.0</td>
      <td>306330.0</td>
      <td>117765.0</td>
      <td>1206239.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>5490691.0</td>
      <td>31888815.0</td>
      <td>633641.0</td>
      <td>2689063.0</td>
      <td>359178.0</td>
      <td>9520619.0</td>
      <td>0.0</td>
      <td>6879501.0</td>
      <td>26824516.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>724670.0</td>
      <td>808471.0</td>
      <td>1871498.0</td>
      <td>123080.0</td>
      <td>364569.0</td>
      <td>181746.0</td>
      <td>435468.0</td>
      <td>416490.0</td>
      <td>558947.0</td>
      <td>1926939.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>7445662.0</td>
      <td>34599335.0</td>
      <td>810521.0</td>
      <td>4450539.0</td>
      <td>411629.0</td>
      <td>13747959.0</td>
      <td>0.0</td>
      <td>8965952.0</td>
      <td>35280518.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>800273.0</td>
      <td>862510.0</td>
      <td>1278727.0</td>
      <td>128795.0</td>
      <td>922124.0</td>
      <td>188581.0</td>
      <td>669669.0</td>
      <td>462509.0</td>
      <td>618941.0</td>
      <td>4496449.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>6601086.0</td>
      <td>31583912.0</td>
      <td>907567.0</td>
      <td>6160638.0</td>
      <td>455048.0</td>
      <td>15726155.0</td>
      <td>0.0</td>
      <td>9332998.0</td>
      <td>46214321.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1340095.0</td>
      <td>400792.0</td>
      <td>803865.0</td>
      <td>204315.0</td>
      <td>505612.0</td>
      <td>312551.0</td>
      <td>851411.0</td>
      <td>280078.0</td>
      <td>862499.0</td>
      <td>6406683.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>9067261.0</td>
      <td>29731625.0</td>
      <td>644905.0</td>
      <td>4653508.0</td>
      <td>794686.0</td>
      <td>12575649.0</td>
      <td>0.0</td>
      <td>11407092.0</td>
      <td>52551708.0</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 30 columns</p>
</div>




```python
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
import plotly.graph_objects as go
import plotly.express as px


import pandas as pd 
pd.options.plotting.backend = "plotly" 

app = dash.Dash(__name__)
data = pd.read_excel('data/covid_top10.xlsx', header = [0,1]) #  분석 결과 가져오기
data = data.set_index(data.columns[0])[1:]                    # 첫번째 컬럼을 로우 인덱스로 변경

app.layout = html.Div([
    html.P("Type:"),
    dcc.Dropdown(
        id="dropdown",
        options=[
            {'label': x, 'value': x}
            for x in ['new_cases','new_deaths','new_tests']
        ],
        value='new_cases',
        clearable=False,
    ),
    dcc.Graph(id="graph"),
])

@app.callback(
    Output("graph", "figure"), 
    [Input("dropdown", "value")])
def display_graph(val):
    fig = data[val].plot()
    return fig

app.run_server(debug=True)
```

    Dash is running on http://127.0.0.1:8050/
    
    Dash is running on http://127.0.0.1:8050/
    
     * Serving Flask app '__main__' (lazy loading)
     * Environment: production
    [31m   WARNING: This is a development server. Do not use it in a production deployment.[0m
    [2m   Use a production WSGI server instead.[0m
     * Debug mode: on
    


    An exception has occurred, use %tb to see the full traceback.
    

    SystemExit: 1
    


    C:\anaconda\envs\test3\lib\site-packages\IPython\core\interactiveshell.py:3425: UserWarning:
    
    To exit: use 'exit', 'quit', or Ctrl-D.
    
    


```python

```
