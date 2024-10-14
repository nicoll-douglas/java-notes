# Java Fundamentals

## Table of Contents

1. [Variables, Data Types & Classes](#1-variables-data-types--classes)
   1. [Variables](#11-variables)
   2. [Primitive Types](#12-primitive-types)
   3. [Reference Types (Objects)](#13-reference-types-objects)
   4. [Enums](#14-enums)
   5. [Wrapper Classes](#15-wrapper-classes)
   6. [Class Constructors](#16-class-constructors)
2. [Type Casting](#2-type-casting)
   1. [Implicit Casting (Widening)](#21-implicit-casting-widening)
   2. [Explicit Casting (Narrowing)](#22-explicit-casting-narrowing)
   3. [Boolean Contexts](#23-boolean-contexts)
   4. [Upcasting](#24-upcasting)
   5. [Downcasting](#25-downcasting)
3. [Operators & Keywords](#3-operators--keywords)
   1. [Equality](#31-equality)
   2. [`null`](#32-null)
   3. [`switch`](#33-switch)
   4. [`instanceof`](#34-instanceof)
   5. [XOR](#35-xor)
4. [Loops](#4-loops)
   1. [For-each Loop](#41-for-each-loop)
5. [Arrays](#5-arrays)
   1. [About Arrays](#51-about-arrays)
   2. [Creating Arrays](#52-creating-arrays)
6. [Functions](#6-functions)
   1. [Method Overloading](#61-method-overloading)
   2. [Lambda Expressions](#62-lambda-expressions)
   3. [Method References](#63-method-references)
7. [OOP](#7-oop)
   1. [Inheritance](#71-inheritance)
   2. [Encapsulation](#72-encapsulation)
   3. [Polymorphism](#73-polymorphism)
   4. [Abstraction](#74-abstraction)
   5. [Interfaces](#75-interfaces)
   6. [Association](#76-association)
8. [Packages](#8-packages)
   1. [About Packages](#81-about-packages)
   2. [Access Modifiers](#82-access-modifiers)
9. [Exception Handling](#9-exception-handling)
   1. [Checked Exceptions](#91-checked-exceptions)
   2. [Unchecked Exceptions](#92-unchecked-exceptions)
   3. [Errors](#93-errors)

## 1. Variables, Data Types & Classes

### 1.1 Variables

- Variables **must** be initiliased before utilisation
- When uninitialised fields are read from objects they take on their default values

```java
int i; // declared
i = 10; // initialised (assigned)

int j = 10; // in one step

int x, y, z = 10; // in one line
```

### 1.2 Primitive Types

#### Key Points:

- All primitive types passed by value

#### Integer Types:

```java
byte b = 12; // 1 byte
short s = 236; // 2 bytes
int i = 153; // 4 bytes
long l = 100L; // 8 bytes
```

- int and long default to int, specify L to declare as long

#### Float Types:

```java
float f = 100.23f; // 4 bytes
double d = 100.23; // 8 bytes
```

- float and double default to double, specify f to declare as float

#### Non-numeric Types:

```java
boolean b = true; // 1 bit
char c = 'c'; // 2 bytes (\u0000 to \uFFFF so UTF-16)
```

### 1.3 Reference Types (Objects)

#### Key Points:

- All reference types passed by reference

#### Strings:

```java
String s = "hello world";
```

- When initiliased with a string literal, string object lives in the string pool
- Any other string literals declared that have the same value return the same reference
- Strings that are instantiated have a separate reference in the heap
- Strings are immutable (but can be reassigned normally)

#### Objects:

```java
Object o = new Object();
```

### 1.4 Enums

#### Key Notes:

- Enum is a special type (similar to a class) to denote a group of constants and is denoted with uppercase starting letter
- Enums can have fields, methods, constructors, nested classes, interfaces and enums like other classes
- Type safe so you can only assign enum values to variables of the same enum type
- Enums are immutable, cannot add or remove values once created (implicitly `public`, `static` and `final`, but can be `private` if not at the top level)
- Cannot instantiate new enums, instances are created when the enum is loaded
- Enums inherit methods from the `java.lang.Enum` class
- Java also provides several built in enums (e.g `java.time.Month`)

#### Example:

```java
public enum Day {
  MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}
```

#### Fields, Methods & Constructors in Enums

- Fields in Enums can store additional information about each constant
- Enum constructors are used to initialise these values
- Enum constructors are implicitly `private` and cannot take on any other access modifier
- Enum constructors are called once per constant
- Enums can have instance and static methods to be called on the constants or the enum itself
- For instance members, `this` refers to the current constant

E.g:

```java
public enum Fruit {
  // constants use the Fruit constructor
  APPLE("Red"),
  BANANA("Yellow");

  public final String color; // stores color of fruit constant

  Fruit(String color) {
    this.color = color;
  }
}

Fruit fruit = Fruit.APPLE;
fruit.color; // returns "Red"
```

#### Advanced Enums

- Enums can implement interfaces much like classes
- Enums can also have their own abstract methods
- Each enum constant must provide its own custom implementation of all the abstract methods from their source

##### Example:

```java
public enum Operation {
  // if using a constructor, just utilise the syntax as normal to initiliase the instance fields
  ADD {
    @Override
    public int apply(int x, int y) {
      return x + y;
    }
  },
  SUBTRACT {
    @Override
    public int apply(int x, int y) {
      return x - y;
    }
  }

  public abstract int apply(int x, int y);
}
```

### 1.5 Wrapper Classes

#### Key Notes:

- Each primitive type in Java has a wrapper class when you need object representations
- Each wrapper class has a method to return the primitive value representation (e.g `.intValue()` for `Integer`)
- Wrapper classes can also provide useful utility methods for operating on their primitives (e.g `Integer.parseInt(String)`)
- Some wrapper classes cache values (e.g integer-type wrappers cache values in the range -128 and 127 so they return the same reference)

#### Usage example:

```java
// must use wrapper class in the generic (array lists only allow reference types)
ArrayList<Integer> numbers = new ArrayList<>();
```

#### Value Caching:

- Integer-type wrappers (`Byte`, `Short`, `Integer`, `Long`) cache objects in the value range -128 to 127 so they return the same reference
- `Character` caches character objects in the range 0 to 127
- `Boolean` caches `true` and `false` (same reference returned)
- No caching occures for `Float` and `Double`

#### Autoboxing & Unboxing

- Lets you immediately/automatically convert from a primitive to a wrapper or vice-versa

e.g:

```java
// Autoboxing
Integer num = 5; // int converted to Integer instance

// Unboxing
int value = num; // Integer instance converted to primitive value
```

### 1.6 Class Constructors

#### Key Notes:

- When instantiating a new object, if a field is not initialised by the constructor, it takes on its default value

#### Default Class Constructors

- If a class doesn't define a constructor, a default is generated by the compiler
- Default class constructors initialise instance variables to their default values and takes no arguments
- It implicitly calls `super()` for the corresponding parent constructor if the class is a sub-class

#### Constructor Chaining

- When a class has an overloaded constructor, it can use `this()` to invoke the corresponding constructor as a shorthand

## 2. Type Casting

### 2.1 Implicit Casting (Widening)

#### Key Points:

- Converting from smaller-size type to larger size-type that results in no data loss and can accomodate it
- When adding characters or numbers with strings, they get upgraded to a string to return a string

#### Valid Casts:

1. **byte → short → int → long → float → double**
2. **char → int → long → float → double**

- Cannot convert from byte or short to char as char is unsigned so data loss would occur due to limited range
- Can convert from long or big int to float because precision loss is not critical (float range contains entire range of longs and ints)

### 2.2 Explicit Casting (Narrowing)

#### Key Points:

- Converting from larger-size type to smaller size-type that results in data loss since it can't accomodate it
- Any data outside the size range gets "chopped off" and then converted by according value

#### Casts:

1. **double → float → long → int → char → short → byte**
2. **byte → char**
3. **short → char**

### 2.3 Boolean Contexts

- In a boolean context such as an `if` conditional or ternary operator, expression **must** evaluate to a boolean
- In such scenarios, automatic conversion **does not** occur for non-boolean types and throws an error (unlike JS)

### 2.4 Upcasting

#### Key Points:

- The processing of converting a reference of a subclass type to a reference of its superclass type
- It is a form of implicit casting so happens automatically (every object of a subclass is also an object of a superclass)
- After upcasting, you can only access the fields and methods defined in the superclass (unless overriden in the subclass, then the overriden method is inferred and used at runtime AKA run-time polymorphism)
- In this regard you can treat objects of subclasses as objects of their superclass to call their overriden methods (perhaps when working with collections, method params or interfaces)

#### Example:

```java
class Animal {
  void makeSound() {
    System.out.println("Animal makes a sound");
  }
}

class Dog extends Animal {
  void bark() {
    System.out.println("Dog barks");
  }
}

Dog dog = new Dog(); // dog has access to .makeSound() and .bark()
Animal a = dog; // Dog object reference is upcasted, now only has access to .makeSound() since it is now an Animal reference

Animal b = new Dog(); // immediate upcasting of a Dog object reference
```

### 2.5 Downcasting

#### Key Notes:

- The process of converting a reference of a superclass type to a reference of a subclass type
- It is a form of explicit casting, happens manually
- Can be unsafe and throw a `ClassCastException` if the object being downcasted is not an instance of the expected subclass
- Only valid when the object actually belongs to the subclass type
- Typically used when you know the actual object type at runtime and need to access subclass-specific fields and methods

#### Example:

```java
class Animal {
  void makeSound() {
    System.out.println("Animal makes a sound");
  }
}

class Dog extends Animal {
  void bark() {
    System.out.println("Dog barks");
  }
}

Animal animal = new Dog(); // immediately upcasted, animal only has access to .makeSound()
Dog dog = (Dog) animal; // animal object is downcasted, now has access to .bark() as well since it was an upcasted Dog
```

#### Safe Downcasting:

```java
Animal animal = new Dog();

if (animal instanceof Dog) {
    Dog dog = (Dog) animal;  // safe downcast
    dog.bark();
} else {
    System.out.println("The animal is not a dog.");
}
```

## 3. Operators & Keywords

### 3.1 Equality

- `==` and `!=` operators compare primitives by value and objects by reference
- String literals that are equal return the same reference from the string pool
- Comparison can be made with `.equals()` methods that may perform custom implementations
- When doing arithmetic comparison, smaller types are [implicitly casted](#implicit-casting-widening) or "promoted" upwards for accurate comparison

### 3.2 `null`

- `null` indicates the absence of an object reference
- `null` **can only** be assigned to reference types
- `null == null` evaluates to `true`
- `null` gets "cast" to string when concatenating with a string

### 3.3 `switch`

#### Key Points:

- Switch statements are compared by value
- For strings or wrappers, they are compared with `.equals()`
- Case statement values must be **implicitly castable** if primitive otherwise have to be explicitly casted
- Cannot switch on reference types unless `.equals()` method overriden meaningfully

#### Valid expression types that can be switched on:

- **byte, short, int** (and their wrappers)
- **char** (and wrapper)
- **String** (both literal and Object)
- **enum** values

### 3.4 `instanceof`

- `instanceof` returns `true` if the object is an instance of the specified class
- Returns `true` if the object implements the specified interface
- Returns `false` if the object is `null`

### 3.5 XOR

```java
boolean a = true;
boolean b = false;
a ^ b // returns exclusive or (XOR) value
```

## 4. Loops

### 4.1 For-each Loop

#### Key Points:

- Used to iterate over an array or a collection that implements the `Iterable` interface
- The loop variable is a local copy and cannot be modified meaningfully unless it is a mutable object

#### Syntax:

```java
for (type element : collection) {
  // code
}
```

## 5. Arrays

### 5.1 About Arrays

- Arrays can only store items of the same type
- Array lengths are fixed and cannot be changed (final field `.length` on the array)
- The array class is a dynamically created runtime class and extends `Object`
- Inherites basic methods such as `.getClass()`, `.equals()`
- Arrays implicitly implement the `Cloneable` and `Serializable` interfaces (allows for cloning and serialization)
- Use the `Arrays` class for more useful static methods that operate on arrays

### 5.2 Creating Arrays

#### Method/Case 1:

- Array values default to the default value of their type (i.e `0` for int, `false` for boolean and `null` for objects)

```java
int[] numbers = new int[5]; // array of size 5: {0, 0, 0, 0, 0}
```

#### Method/Case 2:

```java
int[] numbers = {1, 2, 3, 4, 5};
```

#### Method/Case 3:

- `new` keyword must be used to initialise anonymous arrays

```java
someMethod(new int[]{1, 4}); // uses new keyword
```

## 6. Functions

### 6.1 Method Overloading

#### Key Points:

- Methods have signatures which is the method name and types/number of parameters
- Methods can be "overloaded" to accept different parameters

#### Example:

```java
// method that gets called depends on the arguments

static int add(int x, int y) {
  return x + y;
}

static double add(double x, double y) {
  return x + y;
}
```

### 6.2 Lambda Expressions

#### Key Points:

- Anonmyous functions typically passed as arguments to methods (like JS arrow functions)
- Lamda expressions can also be returned from methods (as long as there is a matching return type)
- Parameter types don't have to be specified if they can be inferred from the interface
- Can be stored in variables if type is a functional interface (interface with exactly one abstract method)
- Override methods do not count towards the abstract method count in an interface

#### Common Functional Interfaces:

- **Supplier\<T>** (method: get, takes none, returns any)
- **BiFunction\<T, U, R>** (method: apply, takes any 2, returns any)
- **Function\<T, R>** (method: apply, takes any, returns any)
- **Predicate\<T>** (method: test, takes any, returns bool)
- **Consumer\<T>** (method: accept, takes any, returns none)

#### Assigment Example:

```java
Predicate<Integer> greaterThan5 = (x) -> x > 5; // param type inferred from interface
greaterThan5.test(3) // test is the abstract method, returns false
```

### 6.3 Method References

#### Key Points:

- Can be a lambda shorthand since it simply calls a predefined method

#### Types:

1. Reference to static method (`ClassName::staticMethod`)
2. Reference to instance method (`instance::instanceMethod`)
3. Reference to instance method of arbitrary object of particular type (`ClassName::instanceMethod`)
4. Reference to constructor (`ClassName::new`)

#### Examples:

##### Static method:

```java
Function<Double, Double> sqrtRef = Math::sqrt;
// same as (x) -> Math.sqrt(x)
```

##### Instance method:

```java
Consumer<String> printlnRef = System.out::println;
// instance method of PrintStream object System.out
// same as (s) -> System.out.println(s)
```

##### Instance method, arbitrary object:

```java
Function<String, Integer> lengthRef = String::length;
// same as (s) -> s.length()
```

##### Constructor:

```java
Supplier<Object> constructorRef = Object::new;
// same as () -> new Object()
```

## 7. OOP

### 7.1 Inheritance

#### Key Points:

- Allows classes to inherit fields and methods from other classes
- The `extends` keyword allows classes to inherit all instance methods and properties from its parent class (super class)
- Static methods and fields are also available on the subclass
- Static methods and fields that are "inherited" cannot be overriden, if redeclared in the subclass, it becomes its own separate method
- Subclasses must use the `super` keyword in the constructor
- Inheritance represents a unidirectional "is-a" relationship, where if A inerhits/extends B, an A is a B but a B is not an A

#### Example:

```java
class Animal {
  public int legCount;

  public Animal(int legCount) {
    this.legCount = legCount;
  }
}

class Dog extends Animal {
  public Dog() {
    super(2);
  }
}

Dog d = new Dog();
d.legCount; // evaluates to 2, Dog inherits legCount field
```

### 7.2 Encapsulation

#### Key Points:

- Data fields and methods are bundled into a singular class
- Access to fields and methods can be restricted to hide the internal state of the object

#### Example:

```java
class Person {
  private String name;

  public Person(String name) {
    this.name = name;
  }

  public String getName() {
    return this.name;
  }

  public void setName(String name) {
    this.name = name;
  }
}

Person p = new Person("Bob");

p.name; // throws exception, name field can only be used internally
p.getName(); // get method provides access, returns "Bob",
p.setName("John"); // set method allows modification, sets p.name to "John",
```

### 7.3 Polymorphism

#### Key Points:

- Allows one interface to be used for a general class of actions
- In simpler terms an interface or method can take on many forms on a need-to basis

#### Compile-time Polymorphism (Static)

- Achieved by method overloading

#### Run-time Polymorphism (Dynamic)

- Achieved by method overriding
- Happens during runtime where the object determines which method to invoke

e.g:

```java
class Animal {
  public void makeSound() {
    System.out.println("Animal makes a sound");
  }
}

class Dog extends Animal {
  @Override
  public void makeSound() {
    System.out.println("Dog barks");
  }
}

Animal dog = new Dog();
Animal a = new Animal();

dog.makeSound(); // "Dog barks", overriden method used despite being upcasted (run-time polymorphism)
a.makeSound();  // "Animal makes a sound", object type is Animal
```

### 7.4 Abstraction

#### Key Points:

- Abstraction hides the complex implementation details of the object and only exposes the essential features of the object
- Achieved with **abstract classes** or **interfaces**

#### Abstract Classes

- Abstract classes are classes that cannot be instantiated and are specified with the `abstract` keyword
- They may have both abstract methods (unimplemented) and concrete methods (implemented)
- Abstract methods should be implemented by an adopting class
- If not all abstract methods implemented, then the class **must** be defined with `abstract`

#### Example of abstraction:

```java
abstract class Animal {
  public abstract void makeSound();
}

// class is not abstract since all methods implemented
class Dog extends Animal {
  // method is given concrete implementation
  @Override
  public void makeSound() {
    System.out.println("Dog barks");
  }
}
```

### 7.5 Interfaces

#### Key Points:

- Defines a set of methods as a characteristic that a class can implement
- Abstract methods are implicitly `public` and **cannot** be defined otherwise; only static methods may have other modifiers
- Classes can implement multiple interfaces (comma separation) allowing Java to overcome limitation of single inheritance (a class can only extend one other class)
- Constants in interfaces are implicitly `public`, `static` and `final`
- The `@FunctionalInterface` directive can be used to indicate a functional interface you define
- Interfaces can inherit from other interfaces like classes with the `extends` keyword

#### Types of Methods:

- **Abstract** - a method with no body; must be given implementations in a class otherwise the class has to be defined with `abstract`
- **Static** - a method that can be called on the interface itself
- **Default** - a method with a default implementation; can be overriden by the implementing class (declared with the `default` keyword)

### 7.6 Association

#### Types of Association

##### General Association

- One class uses or interacts with another class or knows about it
- Can be unidirectional or bidirectional and one class may depend on another
- General association means the objects have independent lifecycles
- They communicate or interact with each other but there is no ownership

##### Aggregation

- Type of association that is represented by a "has-a" relationship with **loose coupling**
- A class A object can **aggregate** class B objects but if object A is removed, the class B objects can still exist independently (they are significant enough)
- The part can exist independently of the whole

##### Composition

- Type of association that is represented by a "has-a" relationship with **strong coupling** and **ownernship** ("contained-by" relationship)
- The lifetime of the part depends on the whole
- If the whole is deleted, the parts are also deleted
- The part cannot exist independently of the whole

## 8. Packages

### 8.1 About Packages

#### Key Points:

- Groups related classes, interfaces and sub-packages together
- Provides namespacing and access control
- Think of and structure packages like folders (packages create organisation)

#### Syntax:

##### /myPackageName/\*

```java
package myPackageName;

// imports

// class definition
```

##### /myPackageName/subPackage/\*

```java
package myPackageName.subPackage;

// imports

// class definition
```

#### Common Packages:

- `java.lang` - Contains fundamental classes like `String`, `Math`, `Object`, etc. (implicitly imported)
- `java.util` - Contains utility classes like `ArrayList`, `HashMap`, `Date`, etc.
- `java.io` - Contains classes for file input/output operations (`File`, `BufferedReader`, etc.).
- `java.nio` - Provides classes for more efficient I/O operations (New I/O API).
- `java.net` - Contains networking classes like `Socket`, `URL`, etc.
- `java.sql` - Provides classes for working with databases (JDBC).
- `javax.swing` - Contains classes for building GUI applications (e.g., `JFrame`, `JButton`).

### 8.2 Access Modifiers

#### Key Points:

- Used to specify the visibility of methods, variables and constructors (also called members)
- Controls where they can be accessed from
- Top level classes can only specified as public or package-private by default
- Nested classes can be specified with any modifier (since they count as members)

#### Main Modifiers:

##### `public`

- The member can be accessed from any other class, inside or outside the package

##### `protected`

- The member is accessible to all classes within the same package and by all subclasses (even if the class is in a different package)

##### `private`

- The member is only accessible within the same class (or class body)
- Private fields and methods cannot be accessed from any other class including subclasses

##### **Default** (no modifier)

- The member can be accessed from any other class in the package, but not from outside the package
- Is also referred to as "package-private"

#### Summary

- A summary of the level of the access a program has to members of a class with the specified modifier

| Modifier    | Within Class | Same Package | Subclass (Different Package) | Outside Package |
| ----------- | ------------ | ------------ | ---------------------------- | --------------- |
| `public`    | Yes          | Yes          | Yes                          | Yes             |
| `protected` | Yes          | Yes          | Yes                          | No              |
| default     | Yes          | Yes          | No                           | No              |
| `private`   | Yes          | No           | No                           | No              |

## 9. Exception Handling

### 9.1 Checked Exceptions

#### Key Notes:

- Exceptions that are checked at compile-time
- If a method can throw a checked exception, it **must** be handled by a `try-catch` block or declare it in the method's `throws` clause
- Exception classes inherit from `java.lang.Exception`

#### Example:

```java
import java.io.*;

try {
  // throws checked exception if file.txt doesn't exist during compilation
  FileReader file = new FileReader("file.txt");
} catch (FileNotFoundException e) {
  e.printStackTrace();
}
```

#### `try-catch`

- In `try-catch`, multiple `catch` blocks needed for each type of exception if they don't belong to the same class hierarchy or can use bar operator (`|`)
- If they belong to the same hierarchy, more specific exception can be specified first
- If there is no `try-catch` block, the exception propagates up through the call stack

#### `throws`

- `throws` used in a method declaration to indicate that it might throw a checked exception
- Can include multiple exception types with a comma separated list
- When a subclass overrides a parent's method with a `throws` clause, it **cannot declare new checked exceptions**
- Can only declare the same exceptions, their subclasses or handle the declared exceptions (also applies to abstract classes and interfaces)
- Meaning the method cannot also throw other kinds of expcetions

E.g

```java
public void readFile() throws IOException {
  // code that might throw an IOException
}
```

#### Try-With-Resources

- Simplifies resource management, such as closing files, sockets, or database connections, by automatically closing resources when the try block exits
- Resources that implement the `AutoCloseable` or `Closeable` interface can be used in a try-with-resources block

E.g

```java
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line = reader.readLine();
    System.out.println(line);
} catch (IOException e) {
    e.printStackTrace();
}
// No need for a finally block; reader is automatically closed
```

### 9.2 Unchecked Exceptions

#### Key Notes:

- Exceptions that occur during runtime and are not checked at compile-time
- Usually indicate bugs such as logic errors

#### Example:

```java
int[] arr = new int[5];
System.out.println(arr[10]); // throws ArrayIndexOutOfBoundsException at runtime
```

### 9.3 Errors

#### Key Notes:

- Errors are serious problems that are outside of the control of the program
- Occurs in situations where the application cannot recover and usually signal a fatal issue with the JVM
- Error classes inherit from `java.lang.Error`

#### Common Errors:

- **OutOfMemoryError** - Thrown when the JVM runs out of memory
- **StackOverflowError** - Thrown when the stack of a thread is exhausted (e.g infinite recursion)

## 10. Collections

### 10.1 Hierarchies

#### Iterable "Types"

<img src="images/collections-iterable.png" width="700" alt="Iterable Hierarchy" />

#### Map "Types"

<img src="images/collections-map.png" width="700" alt="Map Hierarchy" />