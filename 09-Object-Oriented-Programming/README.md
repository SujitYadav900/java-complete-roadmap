# 09 — Object-Oriented Programming

> **Experience level: 🟡 Basic+ (6 months – 1 year)**

OOP is the **heart of Java**. This module teaches classes and objects, the **four pillars** (encapsulation, inheritance, polymorphism, abstraction), constructors, and interfaces — the knowledge that separates "writes Java code" from "thinks in objects."

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Classes-and-Objects.md](Classes-and-Objects.md) | Blueprints and instances, the foundation of OOP |
| [Constructors.md](Constructors.md) | Initializing objects: default, parameterized, this() |
| [Encapsulation.md](Encapsulation.md) | private fields, getters/setters, data protection |
| [Inheritance.md](Inheritance.md) | extends, super, IS-A relationships |
| [Polymorphism.md](Polymorphism.md) | Overloading & overriding, dynamic dispatch |
| [Abstraction.md](Abstraction.md) | abstract classes and hiding complexity |
| [Interfaces.md](Interfaces.md) | Contracts, multiple inheritance, default methods |
| [OOPS-Design.md](OOPS-Design.md) | Applying OOP to design clean, extensible systems |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Model real-world things as classes and objects.
2. ✅ Encapsulate data with private fields and accessors.
3. ✅ Use inheritance to reuse and extend behavior.
4. ✅ Apply polymorphism (overloading + overriding) correctly.
5. ✅ Choose abstract classes vs interfaces wisely.
6. ✅ Design small systems using OOP principles.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Classes & Objects] --> B[Constructors]
    B --> C[Encapsulation]
    C --> D[Inheritance]
    D --> E[Polymorphism]
    E --> F[Abstraction]
    F --> G[Interfaces]
    G --> H[OOPS Design]
    H --> I[Exercises]
```

---

## 🚀 The Four Pillars at a Glance

| Pillar | Question It Answers | Keyword |
|--------|---------------------|---------|
| Encapsulation | How do I protect data? | `private` + getters/setters |
| Inheritance | How do I reuse code? | `extends` + `super` |
| Polymorphism | How do I write flexible code? | overriding + overloading |
| Abstraction | How do I hide complexity? | `abstract` + interfaces |

---

## 📌 Quick Preview

```java
class Animal {
    String name;
    Animal(String name) { this.name = name; }
    void speak() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    Dog(String name) { super(name); }
    @Override
    void speak() { System.out.println(name + " says Woof!"); }
}

public class Preview {
    public static void main(String[] args) {
        Animal a = new Dog("Rex");
        a.speak();
    }
}
```

You'll understand every line by the end of this module.

---

## Next Module

➡️ [10 — Exception Handling](../10-Exception-Handling/README.md) — handle errors gracefully.