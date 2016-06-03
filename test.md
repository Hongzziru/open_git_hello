#Python StringIO를 통한 pickle test

###code

```python
import pickle
import StringIO

phone = {'a': 12, 'b': 34, 'c': 56, 'd': 78}
li = ['string',1234,0.12]
t = (phone, li)

f = open('test.txt','w')

pickle.dump(t,f)
f.close()

f = open('test.txt')


print pickle.load(f)

f2 = StringIO.StringIO()
pickle.dump(t, f2)
f2.flush()

ins = StringIO.StringIO(f2.getvalue())
print pickle.load(ins)
```

###Result
```
({'a': 12, 'c': 56, 'b': 34, 'd': 78}, ['string', 1234, 0.12])
({'a': 12, 'c': 56, 'b': 34, 'd': 78}, ['string', 1234, 0.12])
```
