# google-trends-reconstruct-daily-precision

CLI to collect data from Google Trends in CSV files using [`google-trends-api`](https://github.com/pat310/google-trends-api), written in Node.js.

**Important** Google Trend's API has a limitation of lowering data's resolution for longer query periods. I implemented an approach that circumvents this shortcoming, which is described [here](https://gist.github.com/flxai/fdd287a94f835d6f9ed5bef18f1bb33a).


## Prerequisites

Install `google-trends-api`:

```console
$ npm install google-trends-api
```

## Regular usage

To invoke the CLI and collect data, use the following command. Be sure to replace `KEYWORD*`:

```console
$ ./gtrends KEYWORD1 KEYWORD2
```

Use redirection of standard output to persist results in a file:

```console
$ ./gtrends KEYWORD1 KEYWORD2 > $(date -I)-KEYWORD1-KEYWORD2.csv
```

### Usage hints

The metric provided by Google is normalized using min max scaling and multiplied by 100.
Therefore, all the results have at least one entry with a score of 100.
To observe these values within a population it is important to query them bundled up in a single query as to not loose their relation to one another.
The resolution to these values could be increased by compound queries that query single keywords and scale them according to an approximated factor from the relation of the common query.

**Warning:** For now the maximum time range is queried. This leads Google to mostly output a monthly frequency. Resolution could be increased to daily information by yearly compound queries that scale using the factors for the whole time window. Please see [this notebook](https://gist.github.com/flxai/fdd287a94f835d6f9ed5bef18f1bb33a) for an implementation.


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

This is equivalent to the following usage of `pytrends` (see e.g. the [notebook]() mentioned earlier):

```python
kw_list = ['KEYWORD1', 'KEYWORD2']
pytrends = TrendReq(hl='en-US', tz=360)
pytrends.build_payload(kw_list, cat=0, timeframe='all', geo='', gprop='')
df = pytrends.interest_over_time()
```

