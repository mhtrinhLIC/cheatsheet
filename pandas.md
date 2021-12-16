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
