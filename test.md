#Design Pattern - Builder Pattern

###정리
>객체의 생성은 변수 및 다양한 정보를 설정해야 객체가 생성된다. 만약 객체 생성에 필요한 변수와 정보의 양이 늘어난다면 객체의 생성이 매우 번거롭게 된다. 그래서 Builder pattern의 Builder는 객체 생성 과정의 복잡성을 넘기는 것이 포인트

###Cafe 예제

* 생성되는 Cafe 객체 클래스
  Cafe객체를 생성하기 위한 모든 정보를 정의하고 있다.

  ```python
  #objcet
  class Cafe(object):
    def __init__(self, name):
      self.name = name
      self.address = None
      self.info = None
      self.cofees = []

    def view(self):
      print 'Cafe name : ', self.name, '   Cafe address : ', self.address, '   Cafe info : ', self.info,
  ```
  
* Cafe 객체의 Builder Interface 
  Python에선 굳이 필요 없지만, Java에 인터페이스로 필요
  
  ```python
  #   builder interface
  class CafeBuilder(object):
    def setAddress(self):
      raise
    def setInfo(self):
      raise
    def addCofee(self):
      raise
  ```

* Cafe 객체를 실제로 정의하는 Builder의 구현체인 클래스들
  클래스 안에 모든 값이 정의되어야 하지만 테스트를 위해 set메소드로 변수를 넘겨주었다. 세부적인 과정.
 
  ```python
  class Acafe(CafeBuilder):
  def __init__(self):
    self.cafe = Cafe('A')
  def setAddress(self, address):
    self.cafe.address = address
  def setInfo(self, info):
    self.cafe.info = info
  def addCoffee(self, coffee):
    self.cafe.cofees.append(coffee)

  class Bcafe(CafeBuilder):
    def __init__(self):
      self.cafe = Cafe('B')
    def setAddress(self, address):
      self.cafe.address = address
    def setInfo(self, info):
      self.cafe.info = info
    def addCoffee(self, coffee):
      self.cafe.cofees.append(coffee)
  ```

* Builder를 관리하기 위한 Director
  create 메소드로 객체의 정보를 set한다.포괄적인 과정.

  ```python
  #   Director
  class CafeManufacturer(object):
    def __init__(self):
      self.builder = None

    def create(self, address, info, coffee):
      assert not self.builder is None, 'no defined builder'

      self.builder.setAddress(address)
      self.builder.setInfo(info)
      self.builder.addCoffee(coffee)
      return self.builder.cafe
  ```
  
* Test 예제
  CafeManufacturer, Director를 생성하고, Director의 builder 변수를 통해 실제 객체를 생성하는 A, Bcafe객체를 생성한다.그 후 Director의 create 메소드를 통해서 객체의 정보를 정의하여 객체 생성을 완료하게 된다.

  ```python
  manufacturer = CafeManufacturer()
  manufacturer.builder = Acafe()
  acafe = manufacturer.create('a', 'a', 'a')
  acafe.view()
  print
  manufacturer.builder = Bcafe()
  bcafe = manufacturer.create('b', 'b', 'b')
  bcafe.view()
  ```
* Result

  ```
  Cafe name :  A    Cafe address :  a    Cafe info :  a
  Cafe name :  B    Cafe address :  b    Cafe info :  b
  ```
