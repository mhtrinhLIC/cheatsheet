
### Default figure size and font
``` python
import matplotlib.pyplot as plt

plt.rcParams["figure.figsize"] = (20,10) 
plt.rcParams.update({'font.size': 14})    
```


### Change tick frequency
``` python
from matplotlib.ticker import (MultipleLocator, AutoMinorLocator)

plt.gca().yaxis.set_minor_locator(MultipleLocator(0.1)) 
plt.grid()
plt.grid(which="minor",alpha=0.2)
```

For dates:
``` python
import matplotlib.dates as mdates

plt.gca().yaxis.set_minor_locator(mdates.DayLocator(interval=5)) 
plt.grid()
plt.grid(which="minor",alpha=0.2)
```
See https://www.earthdatascience.org/courses/use-data-open-source-python/use-time-series-data-in-python/date-time-types-in-pandas-python/customize-dates-matplotlib-plots-python/


### Rotate date xlabel
```python
plt.gcf().autofmt_xdate()
```

### Change the date format on axis
```python
from matplotlib.dates import DateFormatter

plt.gca().xaxis.set_major_formatter(DateFormatter("%b-%d"))
```

### Get color array
```python
cmap = plt.get_cmap("tab10")
col0 = cmap(0)
```
See also: https://stackoverflow.com/questions/42086276/get-default-line-colour-cycle


### Get last line color
```python
lastCol = plt.gca().lines[-1].get_color()
```

### Legend at the end of line plot
```python

# Do all your plot with label in
# Store all your labels in array: labelList

ax=plt.gca()
for line, name in zip(ax.lines, labelList):
    y = line.get_ydata()[-1]
    x = line.get_xdata()[-1]
    text = ax.annotate(name,
                       xy=(x, y),
                       xytext=(0, 0),
                       color=line.get_color(),
                       textcoords="offset points")
```


### Multi plot
```python
fig=plt.figure(figsize=(20,18))  # Optional: define figure size
ax1 = plt.subplot(211)  # in a 2 rows, 1 cols setup, figure 1, : we call it ax1
ax2 = plt.subplot(212)  # in a 2 rows, 1 cols setup, figure 2, : we call it ax2

plt.sca(ax1)  # We now plot on to ax1 
plt.plot(....)
plt.legend( ...)

plt.sca(ax2)  # We now plot on to ax2
plt.scatter(...)

plt.show()
```

### Shaded density plot
Require sns
```python
import seaborn as sns

sns.distplot(l.spike, hist = False, kde = True,
                 kde_kws = {'shade': True, 'linewidth': 3}, 
                  label = "chudleigh")
sns.distplot(c.spike, hist = False, kde = True,
                 kde_kws = {'shade': True, 'linewidth': 3}, 
                  label = "crv")
```
