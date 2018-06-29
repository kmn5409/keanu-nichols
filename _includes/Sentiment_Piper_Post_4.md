```python
if(len(list(df3)) == 2):
    df3.reset_index(level=0, inplace=True)
    df3.columns = ['augurmsgID','score','sentiment']
    for i in range(len(df3['augurmsgID'])):
        df3.loc[i,'augurmsgID'] = i+1
print(df3)

#for i in range(len(df4['score'])):
```

        augurmsgID     score sentiment
    0            1  0.140263  Positive
    1            2  0.140263  Positive
    2            3  0.624650  Positive
    3            4  0.624650  Positive
    4            5  0.148000  Positive
    5            6  0.148000  Positive
    6            7  0.285950  Positive
    7            8  0.285950  Positive
    8            9  0.254578  Positive
    9           10  0.254578  Positive

