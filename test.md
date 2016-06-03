#Backtracking 기본 알고리즘

* Permutations of String [link](http://www.geeksforgeeks.org/write-a-c-program-to-print-all-permutations-of-a-given-string/)

```python
 
def toString(List):
    return ''.join(List)
def permute(a,l,r):
    if l==r:
        print toString(a)
    else:
        for i in xrange(l,r+1):
            a[l], a[i] = a[i], a[l]
            permute(a, l+1, r)
            a[l], a[i] = a[i], a[l]
str = 'ADD'
permute(list(str), 0, len(str)-1)
```
backtracking 알고리즘은 모든 경우의 수를 검사하는 알고리즘으로써 효율은 최악일 수 있으나 모든 경우를 검사하기 때문에 풀지 못하는 문제는 거의 없다.
