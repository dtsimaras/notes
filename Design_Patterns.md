# Design Patterns

## Description

This document contains notes and key points from *Head First Design Patterns – Second Edition* by Eric Freeman & Elisabeth Robson.

These notes are intended as a quick reference to review design principles and stay connected with the core ideas of the book. While helpful for review, they are most useful if you've already read and understood the original material.

A useful external resource for studying design patterns:  
[https://www.tutorialspoint.com/design_pattern/index.htm](https://www.tutorialspoint.com/design_pattern/index.htm)

---

## Table of Contents
- [Design Principles](#design-principles)
- [1. Strategy Pattern](#1-strategy-pattern)
- [2. Observer Pattern](#2-observer-pattern)
- [3. Decorator Pattern](#3-decorator-pattern)
- [4. Factory Pattern](#4-factory-pattern)


---

## Pattern Types

Design patterns are typically grouped into three categories based on their purpose:

- **Creational Patterns**  
  Handle object creation mechanisms, aiming to make a system independent of how its objects are created, composed, and represented.  
  > Examples: *Factory Method*, *Abstract Factory*, *Singleton*

- **Structural Patterns**  
  Focus on how classes and objects are composed to form larger structures. These patterns help ensure flexibility and maintainability in system architecture.  
  > Examples: *Decorator*, *Adapter*, *Composite*

- **Behavioral Patterns**  
  Concerned with how objects interact and communicate. These patterns assign responsibilities among objects to improve flexibility in communication.  
  > Examples: *Strategy*, *Observer*, *Command*

---


## Design Principles

- **Encapsulate what varies**  
  Identify aspects of your application that change and isolate them from what stays the same.

- **Program to an interface, not an implementation**  
  Depend on abstractions (interfaces or abstract classes), not concrete classes.

- **Favor composition over inheritance**  
  Use object composition to combine behaviors dynamically at runtime instead of relying on inheritance hierarchies.

- **Strive for loosely coupled designs**  
  Minimize dependencies between components to make the system more flexible and maintainable.

- **Open/Closed Principle (OCP)**  
  Classes should be open for extension, but closed for modification.

- **Dependency Inversion Principle (DIP)**  
  Depend on abstractions, not on concrete classes. High-level modules should not depend on low-level modules.

---

## Design Patterns

### 1. Strategy Pattern *(Behavioral)*

> Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

- **Use when:** You have multiple related algorithms or behaviors and need to switch between them at runtime.  
- **Principle in action:** Favor composition over inheritance.

```
          ┌─────────────────────┐                       ┌─────────────────┐
          │        Duck         │                       │   FlyBehavior   │
          │     <<abstract>>    │                       │  <<interface>>  │
          │                     │                       │                 │
          │ + display()         │                       │ + fly()         │
          │ + performFly()      │◆─────────────────────▶└─────────────────┘
          │ + performQuack()    │                                ▲
          │ + setFlyBehavior()  │                                │
          │ + setQuackBehavior()│                      ┌─────────┴────────┐
          └─────────────────────┘                      │                  │
                   ▲                          ┌─────────────────┐ ┌─────────────────┐
                   │                          │   FlyWithWings  │ │    FlyNoWay     │
         ┌─────────┴─────────┐                │                 │ │                 │
         │                   │                │ + fly()         │ │ + fly()         │
┌─────────────────┐ ┌─────────────────┐       └─────────────────┘ └─────────────────┘
│   MallardDuck   │ │   RubberDuck    │
│                 │ │                 │      ┌─────────────────┐
│ + display()     │ │ + display()     │      │  QuackBehavior  │
└─────────────────┘ └─────────────────┘      │  <<interface>>  │
                                             │                 │
                                             │ + quack()       │
                                             └─────────────────┘
                                                      ▲
                                                      │
                                              ┌───────┴───────┐
                                              │               │
                                      ┌─────────────────┐ ┌─────────────────┐
                                      │     Quack       │ │     Squeak      │
                                      │                 │ │                 │
                                      │ + quack()       │ │ + quack()       │
                                      └─────────────────┘ └─────────────────┘
```

---

### 2. Observer Pattern *(Behavioral)*

> Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

- **Use when:** You need a loosely coupled way for objects to communicate changes.  
- **Principle in action:** Strive for loosely coupled designs between interacting objects.

```
┌─────────────────────┐           ┌─────────────────┐
│       Subject       │           │     Observer    │
│    <<interface>>    │           │  <<interface>>  │
│                     │           │                 │
│ + registerObserver()│           │ + update()      │
│ + removeObserver()  │           └─────────────────┘
│ + notifyObservers() │                   ▲
└─────────────────────┘                   │
          ▲                               │
          │                               │
┌─────────────────────┐          ┌──────────────────┐
│     WeatherData     │          │ CurrentConditions│
│                     │          │   Display        │
│ + registerObserver()│          │                  │
│ + removeObserver()  │          │ + update()       │
│ + notifyObservers() │          │ + display()      │
│ + getTemperature()  │          └──────────────────┘
│ + getHumidity()     │
│ + getPressure()     │
└─────────────────────┘
```

---

### 3. Decorator Pattern *(Structural)*

> Attach additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality.

- **Use when:** You want to add features to objects at runtime without affecting other instances.  
- **Principle in action:** Favor composition over inheritance.

```
              ┌────────────────┐
              │    Beverage    │
              │  <<abstract>>  │
              │                │
              │ + description  │
              │ + cost()       │
              │   <<abstract>> │
              └────────────────┘
                       ▲
                       │
         ┌─────────────┴─────────────┐
         │                           │
┌─────────────────┐        ┌────────────────────┐
│    Espresso     │        │ CondimentDecorator │
│                 │        │   <<abstract>>     │
│ + cost()        │        │                    │
└─────────────────┘        │ + getDescription() │
                           │   <<abstract>>     │
                           └────────────────────┘
                                     ▲
                                     │
                         ┌───────────┴──────────┐
                         │                      │
                ┌───────────────────┐ ┌───────────────────┐
                │       Milk        │ │       Mocha       │
                │                   │ │                   │
                │ + cost()          │ │ + cost()          │
                │ + getDescription()│ │ + getDescription()│
                └───────────────────┘ └───────────────────┘
```

---

### 4. Factory Pattern *(Creational)*

> Define an interface for creating an object, but let subclasses decide which class to instantiate.

- **Simple Factory** (not an official pattern):  
  A single class with a method that creates instances based on input parameters.  
  *Use when:* You want to centralize object creation but don't need subclassing.

```
┌─────────────────┐    creates    ┌────────────────────┐
│   PizzaStore    │──────────────▶│ SimplePizzaFactory │
└─────────────────┘               └────────────────────┘
                                            │
                                            │ creates
                                            ▼
                                   ┌────────────────┐
                                   │     Pizza      │
                                   │  <<abstract>>  │
                                   └────────────────┘
                                            ▲
                                            │
                         ┌──────────────────┼─────────────────────┐
                         │                  │                     │
                 ┌─────────────────┐ ┌─────────────────┐ ┌──────────────────┐
                 │   CheesePizza   │ │   VeggiePizza   │ │  PepperoniPizza  │
                 └─────────────────┘ └─────────────────┘ └──────────────────┘  
```

- **Factory Method:**  
  Define an interface for creating an object, but let subclasses override the method to decide which class to instantiate.  
  *Use when:* You want to delegate instantiation to subclasses for greater flexibility.

```
          ┌─────────────────┐              ┌─────────────────┐
          │   PizzaStore    │              │      Pizza      │
          │   <<abstract>>  │              │   <<abstract>>  │
          │                 │              └─────────────────┘
          │ + orderPizza()  │                       ▲
          │ + createPizza() │                       │
          │   <<abstract>>  │                       │
          └─────────────────┘                       │
                   ▲                                │
                   │                                │
         ┌─────────┴─────────┐                      │
         │                   │                      │
┌─────────────────┐ ┌─────────────────┐             │
│   NYPizzaStore  │ │ChicagoPizzaStore│             │
│                 │ │                 │             │
│ + createPizza() │ │ + createPizza() │             │
└─────────────────┘ └─────────────────┘             │
         │                   │                      │
         │ creates           │ creates              │
         ▼                   ▼                      │
┌─────────────────┐ ┌─────────────────┐             │
│   NYStylePizza  │ │ChicagoStylePizza│─────────────┘
└─────────────────┘ └─────────────────┘
```

- **Abstract Factory:**
  Provide an interface for creating families of related or dependent objects without specifying their concrete classes.  
  *Use when:* You need to create families of related objects and want to ensure they work together.

```
          ┌─────────────────┐                    ┌──────────────────────┐
          │   PizzaStore    │                    │PizzaIngredientFactory│
          └─────────────────┘                    │   <<abstract>>       │
                   │                             │                      │
                   │ uses                        │ + createDough()      │
                   ▼                             │ + createSauce()      │
          ┌─────────────────┐                    │ + createCheese()     │
          │      Pizza      │                    └──────────────────────┘
          │   <<abstract>>  │                                ▲
          │                 │                                │
          │ + prepare()     │                     ┌──────────┴────────────┐
          └─────────────────┘                     │                       │
                   ▲                    ┌───────────────────┐ ┌────────────────────────┐
                   │                    │NYIngredientFactory│ │ChicagoIngredientFactory│
         ┌─────────┴────────┐           │                   │ │                        │
         │                  │           │ + createDough()   │ │ + createDough()        │
┌─────────────────┐ ┌─────────────────┐ │ + createSauce()   │ │ + createSauce()        │
│   CheesePizza   │ │   VeggiePizza   │ │ + createCheese()  │ │ + createCheese()       │
└─────────────────┘ └─────────────────┘ └───────────────────┘ └────────────────────────┘
                                                 │                         │
                                                 │ creates                 │ creates
                                                 ▼                         ▼
                                        ┌─────────────────┐      ┌──────────────────┐
                                        │ ThinCrustDough  │      │ ThickCrustDough  │
                                        │ MarinaraSauce   │      │ PlumTomatoSauce  │
                                        │ ReggianoCheese  │      │ MozzarellaCheese │
                                        └─────────────────┘      └──────────────────┘
```

- **Principle in action:** Program to an interface, not an implementation.

---

