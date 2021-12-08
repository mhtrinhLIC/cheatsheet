# Add a column
The `.values` is important otherwise you may end up with a bunch of `NaN`
```python
dfIndex=pd.Series(range(len(res)))
res['dfIndex'] = dfIndex.values  
```
