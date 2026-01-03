# 1. Single Responsibility Principle (SRP)

단일 책임 원칙은 클래스가 변경되어야 할 단 하나의 이유만 가져야 한다고 말합니다. 다시 말해, 클래스는 오직 한 가지 책임만 져야 합니다.
```
// Without SRP  
class User {  
  constructor(name) {  
    this.name = name;  
  }  
  
  saveToDatabase() {  
    // Save user to the database  
  }  
  
  sendEmail() {  
    // Send welcome email to the user  
  }  
}  
  
// With SRP  
class User {  
  constructor(name) {  
    this.name = name;  
  }  
}  
  
class UserRepository {  
  saveToDatabase(user) {  
    // Save user to the database  
  }  
}  
  
class EmailService {  
  sendWelcomeEmail(user) {  
    // Send welcome email to the user  
  }  
}

```

관심사를 분리함으로써, 각 클래스는 이제 다음과 같은 단일 책임을 갖게 됩니다:

- 사용자 데이터 관리
- 데이터베이스에 저장
- 이메일 발송

이렇게 하면 각 클래스는 특정 작업에 집중할 수 있으며, 코드의 유지보수성과 가독성이 향상됩니다.

# 2. 개방-폐쇄 원칙 Open/Closed Principle (OCP)

개방-폐쇄 원칙은 소프트웨어 엔티티가 수정에는 닫혀 있지만 확장에는 열려 있어야 한다고 말합니다. 이 원칙은 추상화와 인터페이스를 사용하도록 장려합니다.

```
// Without OCP  
class Square {  
constructor(side) {  
this.side = side;  
}  
}  
  
class AreaCalculator {  
calculateSquareArea(square) {  
return square.side * square.side;  
}  
}  
  
// With OCP  
class Shape {  
calculateArea() {}  
}  
  
class Square extends Shape {  
constructor(side) {  
super();  
this.side = side;  
}  
  
calculateArea() {  
return this.side * this.side;  
}  
}  
  
class Circle extends Shape {  
constructor(radius) {  
super();  
this.radius = radius;  
}  
  
calculateArea() {  
return Math.PI * this.radius * this.radius;  
}  
}
```
기존 코드를 수정하지 않고도 새로운 기능을 추가할 수 있게 함으로써, 다음과 같은 이점을 제공합니다:

- 기존 코드의 안정성 유지
- 새로운 형태나 기능의 쉬운 추가
- 코드의 유연성과 확장성 향상

예를 들어, 새로운 도형을 추가할 때 AreaCalculator 클래스를 수정하지 않고도 쉽게 확장할 수 있습니다. 이는 다형성과 인터페이스를 활용하여 달성할 수 있습니다.


# 3. 리스코프 치환 원칙 Liskov Substitution Principle (LSP)

리스코프 치환 원칙은 부모 클래스의 객체를 자식 클래스의 객체로 대체해도 프로그램의 정확성에 영향을 주지 않아야 한다고 말합니다.

```
// Without LSP  
class Bird {  
fly() {  
// Fly logic  
}  
}  
  
class Penguin extends Bird {  
// Penguins can't fly  
fly() {  
throw new Error('Penguins can\'t fly');  
}  
}  
  
// With LSP  
class Bird {  
move() {}  
}  
  
class FlyingBird extends Bird {  
fly() {  
// Fly logic  
}  
}  
  
class Penguin extends Bird {  
// Penguins can't fly  
move() {}  
}
```
LSP를 준수함으로써, 서브클래스를 기본 클래스와 상호 교환 가능하게 보장할 수 있습니다.

# 4. 인터페이스 분리 원칙 Interface Segregation Principle (ISP)

인터페이스 분리 원칙은 클래스가 사용하지 않는 인터페이스를 구현하도록 강제해서는 안 된다고 말합니다. JavaScript에는 엄격한 인터페이스가 없지만, 유사한 구조를 만들 수 있습니다.

```
// Without ISP  
class Worker {  
work() {  
// Work logic  
}  
  
eat() {  
// Eat logic  
}  
}  
  
// With ISP  
class Workable {  
work() {}  
}  
  
class Eatable {  
eat() {}  
}  
  
class Worker implements Workable, Eatable {  
work() {  
// Work logic  
}  
  
eat() {  
// Eat logic  
}  
}
```

인터페이스를 더 작은 단위로 나눔으로써, 클래스는 자신에게 필요한 것만 구현할 수 있습니다.

# 5. 의존성 역전 원칙 Dependency Inversion Principle (DIP)

의존성 역전 원칙은 상위 수준 모듈이 하위 수준 모듈에 직접 의존해서는 안 되며, 둘 다 추상화에 의존해야 한다고 말합니다.

```
// Without DIP  
class LightBulb {  
turnOn() {  
// Turn on logic  
}  
  
turnOff() {  
// Turn off logic  
}  
}  
  
class Switch {  
constructor(bulb) {  
this.bulb = bulb;  
}  
  
operate() {  
// Operate the bulb  
if (/* some condition */) {  
this.bulb.turnOn();  
} else {  
this.bulb.turnOff();  
}  
}  
}  
  
// With DIP  
class Switchable {  
turnOn() {}  
  
turnOff() {}  
}  
  
class LightBulb implements Switchable {  
turnOn() {  
// Turn on logic  
}  
  
turnOff() {  
// Turn off logic  
}  
}  
  
class Switch {  
constructor(device) {  
this.device = device;  
}  
  
operate() {  
// Operate the device  
if (/* some condition */) {  
this.device.turnOn();  
} else {  
this.device.turnOff();  
}  
}  
}
```
이제 Switch 클래스는 구체적인 구현이 아니라 추상화(Switchable)에 의존합니다.

# 결론

JavaScript와 Node.js에 SOLID 원칙을 적용하면 더욱 모듈화되고, 확장 가능하며, 유지보수가 용이한 코드를 만들 수 있습니다. 이러한 원칙을 이해하고 구현함으로써, 개발자들은 강력하고 유연한 소프트웨어 아키텍처를 만들 수 있습니다.

-출처-
https://codewithpawan.medium.com/understanding-solid-principles-in-javascript-and-node-js-9abb09760049