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
