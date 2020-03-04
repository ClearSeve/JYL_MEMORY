# yield

```
def fun(start,end):
    while start < end:
        yield start
        start = start + 1

for n in fun(0,10): 
    print(n)
```
