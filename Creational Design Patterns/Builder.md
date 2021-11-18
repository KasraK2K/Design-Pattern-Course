# Builder

---

## Intent
**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.
&nbsp;&nbsp;

![[builder_1.png]]
&nbsp;&nbsp;

## Problem
Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with lots of parameters. Or even worse: scattered all over the client code.
&nbsp;&nbsp;

![[factory_2.png]]

_In most cases most of the parameters will be unused, making **the constructor calls pretty ugly**._

![[factory_3.png]]

I explain with an example:

```javascript
class Address {
  constructor(zip, street) {
    this.zip = zip;
    this.street = street;
  }
}

class User {
  constructor(name, age, phone, address) {
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.address = address;
  }
}

// This is an upgly example
const user = new User(
  'Alex',
  undefined,
  undefined,
  new Address(1234, 'street 1')
);
```
&nbsp;&nbsp;

## Solution

The Builder pattern suggests that you extract the object construction code out of its own class and move it to separate objects called _builders_.

![[factory_4.png]]

Let me show you solution in code:

```javascript
class Address {
  constructor(zip, street) {
    this.zip = zip;
    this.street = street;
  }
}

class User {
  constructor(name) {
    this.name = name;
  }
}

class UserBuilder {
  constructor(name) {
    this.user = new User(name);
  }

  setAge(age) {
    this.user.age = age;
    return this;
  }

  setPhone(phone) {
    this.user.phone = phone;
    return this;
  }

  setAddress(address) {
    this.user.address = address;
    return this;
  }

  build() {
    return this.user;
  }
}

const userBuilder = new UserBuilder('Alex')
  .setAge(35)
  .setPhone('1234567890')
  .setAddress(new Address(12345, 'street 1'))
  .build();
```
&nbsp;&nbsp;

There is another example to solve this problem in javascript:

```javascript
class Address {
  constructor(zip, street) {
    this.zip = zip;
    this.street = street;
  }
}

class User {
  constructor(name, { age, phone, address } = {}) {
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.address = address;
  }
}

const user = new User('Alex', {
  age: 35,
  phone: '1234567890',
  address: new Address(12345, 'street 1'),
});
```