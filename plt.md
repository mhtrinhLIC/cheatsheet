
### Default figure siz and font
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
