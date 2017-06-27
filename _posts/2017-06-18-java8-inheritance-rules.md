---
layout: post
title: Inheritance rules in Java 8
---

Inheritance is one of the most core and important principle of Object oriented languages. Each OO language has
its own way of implementing rules of inheritance and specifically rules of multi inheritance. This blog post is
aimed at providing clarity around how Java 8 handles multi inheritance situations and how does it resolve conflict
in such situations.

Java 8 supports multiple inheritance through default methods of interfaces or through combining a method of a class with default methods of interfaces. This brings us to a question of default method conflict resolution.

## If multiple parents have the same method signature then which method's implementation is invoked in the implementing class?

There are multiple rules that helps us solve this conflict. The order of precedence for these is as follows:

## Rule 1 - Classes take higher precedence then interfaces.

Given following code

```
public interface Animal {
    default String say() {
        return "From Animal interface";
    }
}  

public class Dog {
    public String say() {
        return "From Dog Class";
    }
}

public class BullDog extends Dog implements Animal{

}
```
Here class BullDog extends Dog class and implements Animal interface. Both class Dog and interface Animal has a method named say. So when we run following test it passes.

```
@Test
public void shouldInheritFromClassOverInterface() {
    BullDog max = new BullDog();
    Assert.assertEquals("From Dog Class", max.say());
}
```    
This shows that method implementation from Dog class is taking precedence over default method in Animal interface.

## Rule 2 - Derived interfaces or sub-interfaces take higher precedence than the ones higher up in the hierarchy.
Given following code
```
public interface Fruit {
    default String color() {
        return "Default Color";
    }
}

public interface Apple extends Fruit {
    default String color() {
        return "Red";
    }
}

public class OpalApple implements Apple{

}
```
Here class OpalApple implements Apple interface which extends Fruit interface. Both interfaces have a method named color.
So when we run following test it passes.

```
@Test
public void shouldInheritFromLowerInterface() {
    OpalApple appleA = new OpalApple();
    Assert.assertEquals("Red", appleA.color());
}
```
This tells us that default method implementation from interface Apple takes precedence over interface Fruit.

## Rule 3 - In case the above rules are not able to resolve the conflict then the implementing class has to override the method with its own implementation.
Given following code
```
public interface A {
    default String hello() {
        return "Hello from A";
    }
}

public interface B {
    default String hello() {
        return "Hello from B";
    }
}

public class C implements A, B {
    public String hello() {
        return A.super.hello();
    }
}
```
Here class C implements both interface A and B. Both have same default method hello. If class c doesn't provide its own implementation then you will see following error while compiling.

```
Error:(3, 8) java: class C inherits unrelated defaults
for hello() from types A and B.
```

When class C provides its own implementation of hello it passes following tests.
```
@Test
public void shouldProvideItsOwnImplementation() {
    C cObj= new C();
    Assert.assertEquals("Hello From A", cObj.hello());
}
```
This example also shows how to call specific implementation from super class.

## How does Java 8 solve Diamond problem in case of default methods in interfaces?
Following code illustrates diamond problem.

```
public interface A {
    default String hello() {
        return "Hello From A";
    }
}

public interface B extends A {
    default String hello() {
        return "Hello From B";
    }
}

public interface C extends A {
    default String hello() {
        return "Hello From C";
    }
}

public class D implements B, C {
    public String hello() {
        return "Hello From D";
    }
}
```
Interface A is the parent interface. B and C interfaces both extends A. While class D implements both B and C interfaces. In this scenario if B or C doesn't have default method named hello then class D will not have to provides implementation of hello. It automatically inherits from interface A. But if B and C both has default methods named hello then class D has to provide its own implementation.
To prove when we run following test, it passes.

```
@Test
public void shouldProvideItsOwnImplementation() {
    D dObj= new D();
    Assert.assertEquals("Hello From D", dObj.hello());
}
```

Hopefully above examples clears understanding of inheritance rules in java 8.

## References
[https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)
[https://www.javabrahman.com/java-8/java-8-multiple-inheritance-of-behavior-from-interfaces-using-default-methods/](https://www.javabrahman.com/java-8/java-8-multiple-inheritance-of-behavior-from-interfaces-using-default-methods/)

-- Lav
