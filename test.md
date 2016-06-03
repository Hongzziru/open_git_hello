#Dynamic Programming

###Min Cost Path [link](http://www.geeksforgeeks.org/dynamic-programming-set-6-min-cost-path/)
```python
/* A Naive recursive implementation of MCP(Minimum Cost Path) problem */
def minCost(cost, m, n):
   if (n < 0 or m < 0):
      return 1
   elif (m == 0 and n == 0):
      return cost[m][n]
   else:
      return cost[m][n] + min( minCost(cost, m-1, n-1),
                               minCost(cost, m-1, n),
                               minCost(cost, m, n-1))

#/* A utility function that returns minimum of 3 integers */
def min(x, y, z):
   if (x < y):
      return x if x < z else z
   else:
      return y if y < z else z

#/* Driver program to test above functions */
cost = [[1,2,3],
        [4,8,2],
        [1,5,3]]
print minCost(cost,2,2)


Output : 8
```

* Min Cost Path tree
```
mC refers to minCost()
                                    mC(2, 2)
                          /            |           \
                         /             |            \             
                 mC(1, 1)           mC(1, 2)             mC(2, 1)
              /     |     \       /     |     \           /     |     \ 
             /      |      \     /      |      \         /      |       \
       mC(0,0) mC(0,1) mC(1,0) mC(0,1) mC(0,2) mC(1,1) mC(1,0) mC(1,1) mC(2,0) 
```
   * 위 알고리즘은 3x3 Matrix에서 M(2,2)에서 시작하여 탐색 가능한 모든 경로를 탐색하여 가중치가 가장 적은 path를 선택하게 된다.


###비행기게임 - Max Coin Algorithm 문제 
>map은 N x 5 로 구성되어 있다. 비행기는 정 중앙에서 출발하게 되며 1턴에 좌,우 또는 대기할 수 있다. 1턴에서 map은 계속해서 내려오게 된다.

```   
   0 : 아무것도 아님
   1 : coin
   2 : 적
   ▲ : 비행기

      [  [0,1, 2 ,1,1],
         [1,2, 1 ,0,1],
         [1,2, 1 ,1,1],
         [0,0, 1 ,0,0],
         [1,2, 2 ,2,2],
         [0,1, 0 ,1,1],
         [1,1, 1 ,1,1],
         [1,2, 0 ,1,1],
         [1,1, 1 ,1,1] ]
               ▲

비행기는 현재 위치 다음에 이동할 수 있는 경로에 모두 2가 있는 상태라면 폭탄을 1회 사용하여 현재 위치에서 5 x 5 에 해당하는 모든 2를 제거할 수 있다.

ex)
             2, 2, 2
                ▲

폭탄은 1회 사용 가능이므로 그다음에 동일한 상황가 맞닥뜨린다면 -1을 출력한다.

위의 제한된 상황에서 비행기가 최대로 획득할 수 있는 코인의 수를 출력하여라.
```


* 진행중인 알고리즘
   현재까지 나의 알고리즘은  2를 만나게되면 2를 0으로 치환하여 max 값을 구하게 된다.
```python
map = [[0,1,2,1,1],
       [1,2,1,0,1],
       [1,2,1,1,1],
       [0,0,1,0,0],
       [1,2,2,2,2],
       [0,1,0,1,1],
       [1,1,1,1,1],
       [1,2,0,1,1],
       [1,1,1,1,1]]

tc2 = [[0 for x in range(len(map[1]))] for x in range(len(map))]

def max(x, y, z):
    if x > y:
        return x if x > z else z
    else:
        return y if y > z else z

def maxCost(map, x, y):
    if x < 0 or y < 0 or y > len(map[0])-1:
        return 0
    else:
        if map[x][y] == 2:
            map[x][y] = 0
        return map[x][y] + max(maxCost(map, x-1, y-1), maxCost(map, x-1, y), maxCost(map, x-1, y+1))

print 'result', max(maxCost(map, len(map)-1, 1), maxCost(map, len(map)-1,2),maxCost(map,len(map)-1,3))
```
