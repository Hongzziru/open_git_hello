#Design Pattern - Prototype Pattern

###정리
>생성 디자인 패턴 중 하나로, 비용이 많은 객체의 생성이나 비슷한 객체의 지속적 생성에 적용 가능하다. 예를들어, DB에서 동일한 데이터셋을 가져와 한두가지의 속성값만 변화시켜야하는 작업이 있을 경우 변화 작업시마다 DB에 접근하기보단 한번 가져온 데이터셋에 대하여 Clone을 해서 작업을 진행하는 것이 자원 관리에 더 효과적이다. 두번째 예로는, 게임의 몬스터를 발생시키는 경우, 비슷한 속성을 가진 몬스터를 여러번 발생 시킬 때 새로운 객체를 계속 생성하여 속성을 일일이 할당하기 보단 하나의 객체를 정의하여 Clone한다면 자원 관리를 효과적으로 할 수 있다.

###Data 예제

* DB Data Schema
  
  ```python
  class DbData:
    def __init__(self):
      self.a = None
      self.b = None
      self.c = None
      print 'Create object'
    def loadData(self):
      self.a = 'a'
      self.b = 'b'
      self.c = 'c'
    def getData(self):
      return self.a, self.b, self.c
  ```
  
* Prototype
  프로토타입을 발생시킬 객체를 등록하는 메소드, Clone 메소드가 존재한다.
  
  ```python
  class Prototype:
    def __init__(self,):
      print 'create proto'
      self.datas = {}
    def registerPrototype(self, name, data):
      print 'register complete'
      self.datas[name] = data
    def deletePrototype(self, name):
      del self.datas[name]
    def clone(self, name):
      cloneData = deepcopy(self.datas[name])
      return cloneData
  ```

* Test
  a 데이터 셋을 생성한 뒤 loadData, (DB에 접근해서 데이터를 가져온다고 가정)를 한다. 그 후 prototype 객체를 생성하여 a 데이터셋을 프로토타입에 등록하고, clone 메소드를 통해 a와 동일한 데이터 셋 clone1, clone2를 생성한다.
 
  ```python
  a = DbData()
  a.loadData()
  print a
  prototype = Prototype()
  prototype.registerPrototype('a', a)
  clone1 = prototype.clone('a')
  clone2 = prototype.clone('a')
  print a
  print '**'
  print clone1
  print clone2
  ```

* Result
  객체를 deepcopy하여 동일한 데이터 셋을 사용할 수 있다. 
  ```python
  Create object
  <__main__.DbData instance at 0x02AAF0D0>
  create proto
  register complete
  <__main__.DbData instance at 0x02AAF0D0>
  **
  <__main__.DbData instance at 0x02AAFAF8>
  <__main__.DbData instance at 0x02AAFB48>
  
  a : 0x02AAF0D0
  clone1 : 0x02AAFAF8
  clone2 : 0x02AAFB48
  ```
