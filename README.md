# Sage Data API Client

This is the official Sage Python data API client. Its main goal is to make writing queries and working with the results easy. It does this by:

1. Providing a simple query function which talks to the data API.
2. Providing the results in an easy to use [Pandas](https://pandas.pydata.org) data frame.

## Installation

```sh
pip install sage-data-client
```

## Usage Examples

### Query API

```python
import sage_data_client

# query and load data into pandas data frame
df = sage_data_client.query(
    start="-1h",
    filter={
        "name": "env.temperature",
    }
)

# print results in data frame
print(df)

# meta columns are expanded into meta.fieldname. for example, here we print the unique nodes
print(df["meta.node"].unique())

# print stats of the temperature data grouped by node + sensor.
print(df.groupby(["meta.node", "meta.sensor"]).value.agg(["size", "min", "max", "mean"]))
```

```python
import sage_data_client

# query and load data into pandas data frame
df = sage_data_client.query(
    start="-1h",
    filter={
        "name": "env.raingauge.*",
    }
)

# print number of results of each name
print(df.groupby(["meta.node", "name"]).size())
```

### Load results from file

If we have saved the results of a query to a file `data.json`, we can also load using the `load` function as follows:

```python
import sage_data_client

# load results from local file
df = sage_data_client.load("data.json")

# print number of results of each name
print(df.groupby(["meta.node", "name"]).size())
```

## Reference

The `query` function accepts the following arguments:

* `start`. Absolute or relative start timestamp. (**required**)
* `end`. Absolute or relative end timestamp.
* `tail`. Limit results to `tail` most recent values per series.
* `filter`. Key-value patterns to filter data on.
