# The Four Pillars of OOP in TypeScript: Managing Complexity at Scale

## Introduction

Object-Oriented Programming (OOP) is not just a coding style — it is a philosophy for managing complexity. As applications grow, unstructured code becomes impossible to maintain. The four pillars of OOP — **Encapsulation**, **Abstraction**, **Inheritance**, and **Polymorphism** — give us a toolkit to organize logic, reduce duplication, and keep large TypeScript projects under control.

---

## 1. Encapsulation

**Encapsulation** means bundling data and the methods that operate on that data inside a single class, and restricting direct access to internal details using access modifiers (`private`, `protected`, `public`).

This prevents outside code from putting an object into an invalid state.

```typescript
class BankAccount {
  private balance: number;

  constructor(initialBalance: number) {
    this.balance = initialBalance;
  }

  deposit(amount: number): void {
    if (amount > 0) {
      this.balance += amount;
    }
  }

  getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // 1500

// account.balance = -99999; // Error — private property, cannot be touched
```

The `balance` is hidden. Outside code can only interact with it through controlled methods. This makes the class predictable and safe.

---

## 2. Abstraction

**Abstraction** means hiding complex implementation details and exposing only what is necessary. In TypeScript, this is achieved using `abstract` classes or `interface`s.

You define *what* a class must do, without specifying *how* it does it.

```typescript
abstract class PaymentProcessor {
  abstract processPayment(amount: number): string;

  printReceipt(amount: number): void {
    const result = this.processPayment(amount);
    console.log(`Receipt: ${result}`);
  }
}

class BkashPayment extends PaymentProcessor {
  processPayment(amount: number): string {
    return `Paid ${amount} BDT via bKash`;
  }
}

class StripePayment extends PaymentProcessor {
  processPayment(amount: number): string {
    return `Charged $${amount} via Stripe`;
  }
}

const payment = new BkashPayment();
payment.printReceipt(500); // Receipt: Paid 500 BDT via bKash
```

The `printReceipt` logic is shared. Each subclass only needs to implement its own version of `processPayment`. The complexity is hidden behind a clean interface.

---

## 3. Inheritance

**Inheritance** allows a class to reuse properties and methods from a parent class. This eliminates code duplication and creates a natural hierarchy.

```typescript
class Vehicle {
  constructor(public brand: string, public speed: number) {}

  move(): string {
    return `${this.brand} is moving at ${this.speed} km/h`;
  }
}

class Car extends Vehicle {
  constructor(brand: string, speed: number, public doors: number) {
    super(brand, speed);
  }

  describe(): string {
    return `${this.move()} with ${this.doors} doors`;
  }
}

class Motorcycle extends Vehicle {
  describe(): string {
    return `${this.move()} on two wheels`;
  }
}

const car = new Car("Toyota", 120, 4);
console.log(car.describe());
// Toyota is moving at 120 km/h with 4 doors
```

`Car` and `Motorcycle` both inherit `brand`, `speed`, and the `move()` method from `Vehicle`. Shared logic lives in one place.

---

## 4. Polymorphism

**Polymorphism** means "many forms." It allows different classes to be treated through the same interface, while each class implements the behavior in its own way.

```typescript
class Shape {
  area(): number {
    return 0;
  }
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  area(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
  }

  area(): number {
    return this.width * this.height;
  }
}

function printArea(shape: Shape): void {
  console.log(`Area: ${shape.area().toFixed(2)}`);
}

printArea(new Circle(5));      // Area: 78.54
printArea(new Rectangle(4, 6)); // Area: 24.00
```

The `printArea` function does not care whether it receives a `Circle` or a `Rectangle`. It just calls `.area()` and gets the right result. This makes it trivial to add new shapes later without changing any existing code.

---

## How the Four Pillars Work Together

In a large-scale project, these pillars complement each other:

- **Encapsulation** protects internal state and reduces unexpected bugs.
- **Abstraction** hides complexity and defines clean contracts between modules.
- **Inheritance** reduces code duplication by sharing common logic in base classes.
- **Polymorphism** makes the system extensible — new features can be added without modifying existing, tested code.

Together, they directly support the **SOLID principles** and keep codebases maintainable as they scale from a few hundred lines to tens of thousands.

---

## Conclusion

TypeScript's class system is purpose-built to support all four OOP pillars with strong typing. Encapsulation enforces boundaries, abstraction manages complexity, inheritance promotes reuse, and polymorphism enables flexibility. Mastering these concepts is not just about passing an assignment — it is the foundation of building software that can actually survive in production and evolve over time.