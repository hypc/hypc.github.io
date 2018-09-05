---
title: Python：获取两个日期中间的日期、月份或年
date: 2018-09-04 14:58:51
tags: [python]
---

```python
def date_range(start_dt, end_dt):
    """
    :type start_dt: date | datetime
    :type end_dt: date | datetime
    :rtype:
    """
    start_date = start_dt.strftime('%Y%m%d')
    end_date = end_dt.strftime('%Y%m%d')
    while start_date <= end_date:
        yield start_date
        start_dt += timedelta(days=1)
        start_date = start_dt.strftime('%Y%m%d')


def month_range(start_dt, end_dt):
    """
    :type start_dt: date | datetime
    :type end_dt: date | datetime
    :rtype:
    """
    start_month = start_month2 = start_dt.strftime('%Y%m')
    end_month = end_dt.strftime('%Y%m')
    while start_month <= end_month:
        yield start_month
        while start_month == start_month2:
            start_dt += timedelta(days=28)
            start_month2 = start_dt.strftime('%Y%m')
        start_month = start_month2


def year_range(start_dt, end_dt):
    """
    :type start_dt: date | datetime
    :type end_dt: date | datetime
    :rtype:
    """
    start_year = start_year2 = start_dt.strftime('%Y')
    end_year = end_dt.strftime('%Y')
    while start_year <= end_year:
        yield start_year
        while start_year == start_year2:
            start_dt += timedelta(days=365)
            start_year2 = start_dt.strftime('%Y')
        start_year = start_year2
```
