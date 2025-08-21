```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

```python
df = pd.read_csv('country_wise_latest.csv')
df.head(2)
```

```python
df.shape
```

```python
df.info()
```

```python
df.isnull().sum()
```

#### We got no missing values and datatypes have no issues

```python
df.columns
```

```python
df.columns = df.columns.str.strip().str.lower().str.replace(' ','_')
```

```python
df.columns
```

```python
df.columns = df.columns.str.replace('/','')
```

```python
df.columns
```

#### Name for columns have been changed for better understanding.

## 1. Which countries are most affected by COVID-19?


```python
# country wise covid cases
Country_wise_impact = df[['countryregion','confirmed', 'deaths', 'recovered', 'active','new_cases']].sort_values(by = 'confirmed', ascending = False)
Country_wise_impact.head(10)
```

#### 1. US have more confirmed cases with some deaths.

## 2. How effective is recovery across different countries?


```python
# country wise recovery vs death comparison
df['Recovered/100 cases'] = (df['recovered']/df['confirmed']) * 100
df['Deaths/100 Cases'] = (df['deaths']/df['confirmed']) * 100

Recovery_vs_deaths = df[['countryregion', 'Recovered/100 cases','Deaths/100 Cases']].sort_values(by = 'Recovered/100 cases', ascending = False)
Recovery_vs_deaths.head(15)
```

```python
plt.figure(figsize=(10, 8))
df.sort_values('confirmed', ascending=False).head(10).plot(
    x='countryregion', y=['confirmed', 'recovered', 'deaths'], kind='bar', figsize=(12,6))
plt.title('Top 10 Countries by Confirmed Cases')
plt.ylabel('Number of Cases')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

```

#### 2. US and Brazil seems have Many confirmed cases with some recovery. and India plays a good role with good recoveries by huge populations.

## 3. What are the current trends (1-week changes)?

```python
weekly_rise = df[['countryregion', '1_week_change', '1_week_%_increase']].sort_values(by = '1_week_change', ascending = False).head(10)
weekly_rise
```

```python
weekly_rise_increase = df[['countryregion', '1_week_change', '1_week_%_increase']].sort_values(by = '1_week_%_increase', ascending = False).head(10)
weekly_rise_increase
```

```python
plt.figure(figsize=(10, 8))
sns.barplot(x = 'countryregion', y = '1_week_%_increase', data = weekly_rise_increase)
plt.title('1 week increased by country')
plt.xlabel('Country/Region')
plt.ylabel('%increase in week')
plt.xticks(rotation=60)
plt.show()
```

#### 3. The place called papua New Guinea seems more cases in a week.

## 4. Healthcare Outcomes

```python
df.columns
```

```python
Health_care = df[['countryregion','deaths__100_cases','recovered__100_cases']].sort_values(by = 'recovered__100_cases', ascending = False).head(10)
Health_care
```

```python
plt.figure(figsize=(10, 8))
sns.barplot(x = 'countryregion', y = 'deaths__100_cases', data = Health_care)
plt.title('Death rate by country')
plt.xlabel('Country/Region')
plt.ylabel('Death_Rate')
plt.xticks(rotation=60)
plt.show()
```

#### 4. The place called Holy see, Grenada, Dominica have good healthcare, got no deaths.

## 5.WHO Region Performance

```python
who_region_data= df.groupby('who_region')[[
    'confirmed', 'deaths', 'recovered', '1_week_change', '1_week_%_increase']].sum()

# Add outcome ratios
who_region_data['Recovered/100 cases'] = (who_region_data['recovered'] / who_region_data['confirmed']) * 100
who_region_data['Deaths/100 Cases'] = (who_region_data['deaths'] / who_region_data['confirmed']) * 100
```

```python
who_region_data['Recovered/100 cases']
```

```python
who_region_data['Deaths/100 Cases']
```

```python

```
