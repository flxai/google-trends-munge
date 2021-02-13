# google-trends-munge

CLI to collect data from Google Trends in CSV files using [`google-trends-api`](https://github.com/pat310/google-trends-api), written in Node.js.

## Prerequisites

Install `google-trends-api`:

```
$ npm install google-trends-api
```

## Using data in Python

My motivation was to quickly sketch something that allows collection from Google Trends data and I had the impression that [`pytrends`](https://github.com/GeneralMills/pytrends) does not currently (2021) give confident results.

For quick usage within Jupyter Lab this snippet should give proper orientation. Be sure to replace `KEYWORD*`:

```python
!./gtrends KEYWORD1 KEYWORD2 > tmp.csv

df = pd.read_csv('tmp.csv', index_col='timestamp')
df.index = pd.to_datetime(df.index, unit='s')
df.plot()
plt.show()
```

