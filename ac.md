# Ac

Connect time for all the users
To display connect time for all the users use –p as shown below. Please note that this indicates the cumulative connect time for the individual users.
```
$ ac -p
        john                              3.64
        madison                           0.06
        sanjay                            88.17
        nisha                             105.92
        ramesh                            111.42
        total 309.21
```

Connect time for a specific user
To get a connect time report for a specific user, execute the following:
```
$ ac -d sanjay
Jul  2  total       12.85
Aug 25  total        5.05
Sep  3  total        1.03
Sep  4  total        5.37
Dec 24  total        8.15
Dec 29  total        1.42
Today   total        2.95
```