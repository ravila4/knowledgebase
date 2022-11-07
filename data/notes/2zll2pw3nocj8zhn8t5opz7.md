
## Creating DataFrames

Specify header while reading a tab-delimited file:

```python
cols = ["col1", "col2", "col3" ]
df = pd.read_csv("data.tab", sep="\t", names=cols)
```

## Cleaning Data

Rename several DataFrame columns:

```python
df = df.rename(columns = {
    'col1 old name':'col1 new name',
    'col2 old name':'col2 new name',
    'col3 old name':'col3 new name',
})
```

Get a report of all duplicate records in a DataFrame, based on specific columns:

```python
dupes = df[df.duplicated(['col1', 'col2', 'col3'], keep=False)]
```

Remove duplicates by column, keeping first entry:

```python
df = df.drop_duplicates(subset='col', keep='first')
```

Reset a DataFrame's index to continuous integers, without saving old index (eg. after deleting records).

```python
df.reset_index(drop=True, inplace=True)
```

Clean up missing values in multiple DataFrame columns:

```python
df = df.fillna({
    'col1': 'missing',
    'col2': 999,
    'col3': 0,
    'col4': 'missing',
})
```

Rename several DataFrame columns:

```python
df = df.rename(columns = {
    'col1 old name':'col1 new name',
    'col2 old name':'col2 new name',
    'col3 old name':'col3 new name',
})
```


## Exploring Data

Sort DataFrame on a column:
```python
df.sort_values(by='col1', ascending=False)
```

## Indexing and Selecting Data

Accessing an index from a multi-index DataFrame:

```python
pandas.MultiIndex.get_level_values
```

## Grouping and Transforming Data

Group by two columns and count total in groups:

```python
df.groupby(['col1', 'col2']).count()
```

Group and then aggregate all columns with a function:

```python
df.groupby('col').agg(function)
```

Group and aggregate specific columns with specific functions:

```python
df.groupby('col').agg({"col1": np.sum, "col2": pd.Series.nunique})
```

Spreadsheet-style pivot with aggregation:

```python
table = pd.pivot_table(
    df, values='D', index=['A', 'B'], columns=['C'], aggfunc=np.sum)
```
