# 객체

**객체**란??

- 키(key)와 값(value)의 쌍으로 이루어진 데이터 구조
- 모든 객체는 속성(property)과 행위(method)를 가진다  
  &rarr; 속성, 필드, property는 같은 말
  &rarr; 함수, 메서드는 같은 말

- JavaScript는 프로토타입 기반 객체지향 언어이다
- 객체는 **prototype**을 통해 속성과 메서드를 상속받을 수 있다

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  // name과 age는 속성
}

Person.prototype.greet = function () {
  console.log(`Hello, my name is ${this.name}.`);
};
// greet는 메서드

const charlie = new Person("Charlie", 28);
charlie.greet(); // "Hello, my name is Charlie."
```

> Person.prototype을 사용하면 모든 Person 객체가 공유하는 메서드를 정의할 수 있다
> 메모리 절약 효과 (각 객체에 greet()을 개별적으로 저장하지 않음)  
> &rarr; prototype 에 저장된다

<br>

## Class

---

- ES6에서는 class 문법을 제공하여 객체지향 프로그래밍 스타일을 쉽게 구현할 수 있다

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}

const dave = new Person("Dave", 35);
dave.greet(); // "Hello, my name is Dave."
```

> 1. new 를 사용하면 새로운 객체 생성 (instance)  
>    &rarr; name = Dave 이고, age = 35인 새로운 객체 생성  
>    &rarr; Class 는 추상적이고, 객체(instance)는 구체적!!!
> 2. constructor 함수는 객체가 생성될 때 호출된다
> 3. greet()는 생성자 함수에 포함되지 않기에 호출되지는 않지만,  
>    prototype에 자동으로 추가된다

#### OOP에서 핵심 개념

1. 캡슐화(Encapsulation)

- JavaScript에서는 #을 사용해 private(비공개) 속성을 만들 수 있다
- 외부에 공개하지 않고 내부에서만 접근할 수 있게 만들 수 있다
- public, private

```javascript
class BankAccount {
  #balance = 0;
  // private 속성 : 클래스 내부에서만 접근 가능

  deposit(amount) {
    this.#balance += amount;
    console.log(`Deposited: $${amount}`);
  }

  getBalance() {
    return this.#balance;
  }
  // 외부로 값을 내보낼 때 사용할 함수
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
// console.log(account.#balance);
// 오류 발생 : 외부에서 바로 접근하지 못한다
```

2. 상속(Inheritance)

- extends 키워드를 사용하면 클래스를 상속할 수 있다
- 기존 클래스의 속성과 메서드를 재사용 가능

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    // 부모 클래스 Animal에서 constructor 호출
    // Dog클래스 내에 this.name 이라는 속성이 없지만 상속받아 사용할 수 있다
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name}(${this.breed}) barks.`);
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // "Buddy barks."
```

&rarr; 여기서 super()는 왜 호출해야 할까???

extends를 사용한 자식 클래스는  
 반드시 this를 사용하기 전에 super()를 호출해야 함  
 즉, extends 키워드로 부모 클래스를 상속한 경우,  
 자식 클래스의 constructor에서 super()를 호출하지 않으면 오류가 발생

<br>

---

📌 Property(속성) vs Prototype(프로토타입) 차이 정리

- Property (속성)
  - 객체의 개별적인 데이터 (자기 자신에게 있음)
  - 각 인스턴스(객체)마다 **독립적**으로 존재
- Prototype (프로토타입)

  - 객체가 상속받는 부모 객체 (**공유**되는 속성과 메서드)
  - 공유되는 속성(메서드) 저장
  - Person.prototype을 통해 설정됨
  - \_\_proto\_\_를 통해 참조됨

    예시를 들어보자!!!

  ```javascript
  function Person(name) {
    this.name = name; // 개별 속성
  }

  Person.prototype.sayHello = function () {
    return `Hello, my name is ${this.name}`;
  };
  //

  const p1 = new Person("Alice");
  const p2 = new Person("Bob");

  console.log(p1.sayHello()); // Hello, my name is Alice
  console.log(p2.sayHello()); // Hello, my name is Bob
  ```

  > sayHello는 prototype에 저장되므로 모든 객체가 공유  
  > p1.sayHello()와 p2.sayHello()는 같은 메서드를 참조
