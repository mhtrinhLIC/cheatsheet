# Display all rows
```python
def p(df):
    with pd.option_context(
            'display.max_rows', None,      # All rows
            'display.max_columns', None,   # All cols
            'display.max_colwidth',20,     # Col width
            'precision', 2,                # Number of floating digit to display
            'display.float_format',  '{:,}'.format,  # 1,200,000
        ):
        display(df)
```

Or:
```python
pd.set_option('display.max_colwidth')
display(df)
pd.reset_option('display.max_colwidth')
```

# Timezone
```python
import pytz
from datetime import datetime 
s="2022-15-15T15:10:26.654544Z"
dt=datetime.strptime(s,'%Y-%m-%dT%H:%M:%S.%fZ')

# Tell that the date in the string is UTC:
utcDt=dt.replace(tzinfo=pytz.utc)

# Change the dt to local (or another) timezone
localDt=utcDt.astimezone(pytz.timezone("Pacific/Auckland"))

# Show the time in your timezone
localDt.strftime("%d %b %Y, %H:%m")

```


# Add a column
The `.values` is important otherwise you may end up with a bunch of `NaN`
```python
dfIndex=pd.Series(range(len(res)))
res['dfIndex'] = dfIndex.values  
```

# Modifying a cell of a DataFrame
Use `.loc` or `.iloc`

# Modify a section of a DataFrame, via subsetting
* Get the subset with `.copy()` that you work with
* Make the change to that subset (with `.loc` or `iloc`)
* Assign that subset back to your original df 

```python
# Subset and copy
w=df[startDtStr:endDtStr].copy()

# Modify your w cells
...
w.iat[i,iweight] = currentWeight
...

# Assign it back
df[startDtStr:endDtStr] = w

```

# Subsetting non-uniq date
Your date must not be an index. If it's the case, create a new column.
```
df['date'] = df.index # If your date in an index
df[df.date.between(start,end)]
```

# Make long/big integer display as normal and not scientific notation
```
df['mycolumn'] = df['mycolumn'].astype("Int64")
```
Alternatively, you can even force it as string to be sure. See https://stackoverflow.com/a/61154379
