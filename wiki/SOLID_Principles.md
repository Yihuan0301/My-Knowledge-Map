# SOLID Principles

SOLID 是面向对象设计的五个基本原则，帮助开发者创建更易维护、可扩展和健壮的软件系统。

SOLID is an acronym for five fundamental principles of object-oriented design that help developers create more maintainable, scalable, and robust software systems.

> **Note**: The code examples below are simplified for educational purposes to illustrate object-oriented concepts. They may not represent the most idiomatic JavaScript patterns but are designed to be clear and accessible across different programming backgrounds.

## Overview

- **S** - Single Responsibility Principle (单一职责原则)
- **O** - Open/Closed Principle (开放封闭原则)
- **L** - Liskov Substitution Principle (里氏替换原则)
- **I** - Interface Segregation Principle (接口隔离原则)
- **D** - Dependency Inversion Principle (依赖倒置原则)

## Single Responsibility Principle (SRP)

**单一职责原则**

### Definition
A class should have only one reason to change, meaning it should have only one job or responsibility.

一个类应该只有一个改变的理由，意味着它应该只有一个工作或职责。

### Example

**Bad:**
```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
  
  save() {
    // Save to database
  }
  
  sendEmail() {
    // Send email logic
  }
  
  generateReport() {
    // Generate user report
  }
}
```

**Good:**
```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class UserRepository {
  save(user) {
    // Save to database
  }
}

class EmailService {
  sendEmail(user, message) {
    // Send email logic
  }
}

class UserReportGenerator {
  generate(user) {
    // Generate user report
  }
}
```

## Open/Closed Principle (OCP)

**开放封闭原则**

### Definition
Software entities should be open for extension but closed for modification.

软件实体应该对扩展开放，对修改封闭。

### Example

**Bad:**
```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
}

class AreaCalculator {
  calculate(shape) {
    if (shape instanceof Rectangle) {
      return shape.width * shape.height;
    }
    // Need to modify this class every time we add a new shape
  }
}
```

**Good:**
```javascript
class Shape {
  area() {
    throw new Error('Method must be implemented');
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }
  
  area() {
    return this.width * this.height;
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  
  area() {
    return Math.PI * this.radius * this.radius;
  }
}

class AreaCalculator {
  calculate(shape) {
    return shape.area();
  }
}
```

## Liskov Substitution Principle (LSP)

**里氏替换原则**

### Definition
Objects of a superclass should be replaceable with objects of its subclasses without breaking the application.

超类的对象应该可以被其子类的对象替换而不破坏应用程序。

### Example

**Bad:**
```javascript
class Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error('Penguins cannot fly!');
  }
}
```

**Good:**
```javascript
class Bird {
  move() {
    console.log('Moving');
  }
}

class FlyingBird extends Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  swim() {
    console.log('Swimming');
  }
}
```

## Interface Segregation Principle (ISP)

**接口隔离原则**

### Definition
No client should be forced to depend on methods it does not use. Many client-specific interfaces are better than one general-purpose interface.

不应该强迫客户端依赖它不使用的方法。多个特定于客户端的接口优于一个通用接口。

### Example

**Bad:**
```javascript
class Worker {
  work() {}
  eat() {}
  sleep() {}
}

class Robot extends Worker {
  work() {
    console.log('Working');
  }
  
  eat() {
    // Robots don't eat - forced to implement unused method
    throw new Error('Robots do not eat');
  }
  
  sleep() {
    // Robots don't sleep - forced to implement unused method
    throw new Error('Robots do not sleep');
  }
}
```

**Good:**
```javascript
class Workable {
  work() {}
}

class Eatable {
  eat() {}
}

class Sleepable {
  sleep() {}
}

class Human extends Workable {
  work() {
    console.log('Working');
  }
}

// Implement only needed interfaces
Object.assign(Human.prototype, Eatable.prototype, Sleepable.prototype);

class Robot extends Workable {
  work() {
    console.log('Working');
  }
  // Only implements what it needs
}
```

## Dependency Inversion Principle (DIP)

**依赖倒置原则**

### Definition
High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

高层模块不应该依赖低层模块。两者都应该依赖抽象。抽象不应该依赖细节。细节应该依赖抽象。

### Example

**Bad:**
```javascript
class MySQLDatabase {
  save(data) {
    console.log('Saving to MySQL');
  }
}

class UserService {
  constructor() {
    this.database = new MySQLDatabase(); // Direct dependency
  }
  
  saveUser(user) {
    this.database.save(user);
  }
}
```

**Good:**
```javascript
// Abstraction
class Database {
  save(data) {
    throw new Error('Method must be implemented');
  }
}

class MySQLDatabase extends Database {
  save(data) {
    console.log('Saving to MySQL');
  }
}

class MongoDatabase extends Database {
  save(data) {
    console.log('Saving to MongoDB');
  }
}

class UserService {
  constructor(database) {
    this.database = database; // Depends on abstraction
  }
  
  saveUser(user) {
    this.database.save(user);
  }
}

// Usage
const mysqlDb = new MySQLDatabase();
const userService = new UserService(mysqlDb); // Inject dependency
```

## Benefits of SOLID Principles

1. **可维护性 (Maintainability)**: Code is easier to understand and modify
2. **可扩展性 (Scalability)**: New features can be added with minimal changes
3. **可测试性 (Testability)**: Code is easier to unit test
4. **可重用性 (Reusability)**: Components can be reused in different contexts
5. **降低耦合 (Reduced Coupling)**: Components are loosely coupled

## Related Concepts

- Design Patterns
- Clean Architecture
- Test-Driven Development (TDD)
- Domain-Driven Design (DDD)

## References

- Robert C. Martin (Uncle Bob) - Original author of SOLID principles
- "Clean Architecture" by Robert C. Martin
- "Design Patterns: Elements of Reusable Object-Oriented Software" by Gang of Four

## See Also

- [Home](Home.md) - Return to Wiki home
