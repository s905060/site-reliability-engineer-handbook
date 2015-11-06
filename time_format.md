#Time format
```
from datetime import date
from datetime import datetime
```
```
datetime.strptime('2012-11-14 14:32:30', '%Y-%m-%d %H:%M:%S')
```
```
today = date.today()
year = today.year
month = "%02d" % today.month
day = "%02d" % today.day
```