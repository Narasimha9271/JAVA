# Java OOP — Video-by-Video Study Guide

Original explanations, extra concepts, tips, mnemonics, exercises, and revision summaries for the seven lessons in [Kunal Kushwaha’s Java OOP playlist](https://www.youtube.com/playlist?list=PL9gnSGHSqcno1G3XjUbwzXHL8_EttOuKk).

The study guide below is preserved from our chat. Examples are independent teaching snippets; repeated class names belong to separate examples. Add the relevant imports and a class or method wrapper when running individual snippets. Output comments describe expected behavior and are not a claim that the examples were executed.

## Study guide

I can teach you the playlist’s topics in the format you requested:

1. **Detailed explanations with Java examples**
2. **Extra concepts that help you understand and apply them**
3. **Tips, techniques, common mistakes, and mnemonics**
4. **A revision summary**

One limitation: I can’t reproduce everything spoken in these copyrighted videos or provide a near-verbatim replacement. I also couldn’t retrieve complete transcripts, so I can’t claim to cover every remark or example. **What follows is an original study guide organized around the seven lessons’ published topics.**

I’ll assume you understand variables, loops, arrays, and basic methods. The examples use conventional Java syntax; where newer Java changes an older teaching rule, I’ll point that out.

The course follows this sequence:

| Video | Main topics |
|---|---|
| [1](https://www.youtube.com/watch?v=BSVKUk58K6U) | Classes, objects, constructors, `this`, wrappers, `final`, garbage collection |
| [2](https://www.youtube.com/watch?v=_Ya6CN13t8k) | Packages, `static`, nested classes, singleton |
| [3](https://www.youtube.com/watch?v=46T2wD3IuhM) | Inheritance, polymorphism, encapsulation, abstraction |
| [4](https://www.youtube.com/watch?v=W145DXs8fFg) | Access control, standard packages, `Object` |
| [5](https://www.youtube.com/watch?v=rgHZa7-Dibg) | Abstract classes, interfaces, annotations |
| [6](https://www.youtube.com/watch?v=OY2lPr8h93U) | Generics, custom lists, comparison, lambdas, exceptions, cloning |
| [7](https://www.youtube.com/watch?v=9ogGan-R1pc) | Collections, `Vector`, enums |

Treat each lesson as a separate study session. Read the code, predict its behavior, and then run it.

---

## VIDEO 1 — Classes, objects, constructors, and keywords

### 1. Detailed teaching

**Why do we need classes?**

Suppose you store information about three books:

```java
String[] titles = {"Java Basics", "Clean Design", "Algorithms"};
int[] pages = {200, 350, 500};
boolean[] borrowed = {false, true, false};
```

This works, but the relationship between the arrays depends on matching indexes. If you rearrange one array without rearranging the others, the data becomes inconsistent.

A class groups related data and behavior:

```java
class Book {
    String title;
    int pages;
    boolean borrowed;

    void borrow() {
        borrowed = true;
    }
}
```

The fields describe a book’s **state**. The method describes **behavior**.

A class defines a type. An object is an instance of that type.

```java
Book first = new Book();
Book second = new Book();

first.title = "Java Basics";
second.title = "Algorithms";
```

These are two separate objects. Changing `first.title` does not change `second.title`.

Objects have:

- **State:** current field values.
- **Behavior:** operations they support.
- **Identity:** which particular object they are.

Two books can have identical field values while still being different objects.

**Understand this statement carefully**

```java
Book first = new Book();
```

| Part | Meaning |
|---|---|
| `Book` | Declared type of the variable |
| `first` | Reference variable |
| `new Book()` | Creates and initializes a `Book` object |
| `=` | Stores the resulting reference in `first` |

**The reference variable is not the object.**

```java
Book other = first;
other.title = "Updated title";

System.out.println(first.title); // Updated title
```

Both variables refer to the same object:

```text
first ──┐
        ├──> Book object
other ──┘
```

Assignment copied the reference. It did not copy the book.

**What happens without `new`?**

```java
Book book;
```

Inside a method, this declares an uninitialized local variable. You cannot use it until it is assigned.

```java
Book book = null;
```

This explicitly stores a null reference. There is no book to access:

```java
book.borrow(); // NullPointerException
```

**Default field values**

Instance fields receive default values before constructor initialization:

| Field type | Default |
|---|---|
| Integer numeric types | `0` |
| Floating-point types | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` |
| Reference types | `null` |

Local variables do **not** automatically receive usable default values.

```java
class Example {
    int count; // Defaults to 0

    void show() {
        int local;
        // System.out.println(local); // Compilation error
    }
}
```

**Constructors**

A constructor establishes the initial state of a new object.

```java
class Book {
    String title;
    int pages;

    Book(String title, int pages) {
        this.title = title;
        this.pages = pages;
    }
}
```

Usage:

```java
Book book = new Book("Java Basics", 200);
```

Constructor rules:

- Its name matches the class name.
- It has no return type—not even `void`.
- It can accept parameters.
- It can be overloaded.
- It is not inherited.

This is a method, not a constructor:

```java
void Book() {
}
```

**Default constructor versus no-argument constructor**

If an ordinary class declares no constructors, the compiler supplies a default constructor.

```java
class Book {
}
```

Therefore:

```java
new Book();
```

is allowed.

Once you declare a constructor, the compiler does not also generate a default constructor:

```java
class Book {
    Book(String title) {
    }
}

// new Book(); // Compilation error
```

A no-argument constructor that **you write** is not the compiler-generated default constructor.

**The `this` keyword**

`this` refers to the current object.

```java
Book(String title) {
    this.title = title;
}
```

Here:

- `this.title` means the object’s field.
- `title` means the constructor parameter.

Without `this`:

```java
title = title;
```

you assign the parameter to itself.

**Constructor overloading and chaining**

You can provide several ways to initialize an object:

```java
class Book {
    String title;
    int pages;

    Book() {
        this("Untitled", 0);
    }

    Book(String title) {
        this(title, 0);
    }

    Book(String title, int pages) {
        this.title = title;
        this.pages = pages;
    }
}
```

`this(...)` invokes another constructor in the same class.

The benefit is centralized initialization: all paths eventually reach the constructor that assigns the fields.

For code compatible with older Java versions, put `this(...)` or `super(...)` first. Java 25 introduced restricted support for statements before these calls, so “it must always be the first statement” is no longer a universal rule. [Oracle’s constructor update](https://docs.oracle.com/en/java/javase/25/language/flexible-constructor-bodies.html)

**Primitive values versus objects**

```java
int pages = 200;
Book book = new Book("Java Basics", 200);
```

An `int` variable holds a primitive value. A `Book` variable holds a reference.

Avoid memorizing “all primitives are on the stack.” A primitive field can be part of an object; a primitive array contains primitive elements. Storage depends on context and JVM implementation.

A useful conceptual model is:

- Method execution uses stack frames.
- Objects and arrays belong to the JVM heap model.
- Reference values connect variables and object fields to objects.

**Wrapper classes**

Wrappers provide object types corresponding to primitives:

| Primitive | Wrapper |
|---|---|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

```java
Integer boxed = 25; // Autoboxing
int value = boxed;  // Unboxing
```

Wrappers are useful with generics:

```java
ArrayList<Integer> numbers = new ArrayList<>();
```

You cannot use `ArrayList<int>`.

Wrappers are immutable. Also, unboxing `null` fails:

```java
Integer number = null;
int value = number; // NullPointerException
```

**The `final` keyword**

For variables, `final` means the variable can be assigned only once.

```java
final int limit = 10;
// limit = 20; // Compilation error
```

For reference variables, it fixes the reference:

```java
final Book book = new Book("Java", 200);

book.pages = 250; // Allowed
// book = new Book("Python", 300); // Not allowed
```

**A final reference does not automatically make the object immutable.**

**Garbage collection**

Java automatically manages memory for objects that are no longer reachable in ways that keep them alive.

```java
Book a = new Book("Java", 200);
Book b = a;

a = null;
```

The object is still reachable through `b`.

```java
b = null;
```

Assuming nothing else references it, it can become eligible for collection. That does not mean it is immediately collected.

Do not use `finalize()` as a destructor or dependable cleanup mechanism. Finalization was deprecated for removal in Java 18. Files and similar resources need explicit lifecycle management, commonly try-with-resources. [Oracle release notes](https://www.oracle.com/java/technologies/javase/18-relnote-issues.html)

### 2. Extra content you need

**Java is always pass-by-value.**

For object arguments, the copied value is a reference.

```java
static void change(Book book) {
    book.title = "Changed";
}

static void replace(Book book) {
    book = new Book("Replacement", 100);
}
```

Given:

```java
Book original = new Book("Original", 200);

change(original);
System.out.println(original.title); // Changed

replace(original);
System.out.println(original.title); // Still Changed
```

`change` uses its copied reference to modify the shared object.

`replace` changes only its own parameter variable.

Also, validate constructor inputs so that objects begin in valid states:

```java
Book(String title, int pages) {
    if (title == null || title.isBlank()) {
        throw new IllegalArgumentException("Title is required");
    }
    if (pages < 0) {
        throw new IllegalArgumentException("Pages cannot be negative");
    }

    this.title = title;
    this.pages = pages;
}
```

### 3. Tips, techniques, and mnemonics

- **Class = definition; object = instance; reference = connection.**
- **`this` = this particular object.**
- **Final reference = fixed connection, potentially changeable object.**
- Draw arrows whenever two variables might share an object.
- For constructors, check **name, parameters, initialization**.
- Never infer wrapper value equality from `==`; use `.equals()` when comparing wrapper objects by value.

**Practice:** Create a `Movie` class with three constructors. Assign one movie reference to another variable and demonstrate shared state.

### 4. Summary

A class groups state and behavior. Objects are instances, and reference variables refer to them. Constructors initialize objects; `this` identifies the current object or chains constructors. Wrappers represent primitive values as objects. `final` restricts reassignment. Garbage collection handles memory, while resource cleanup needs deliberate handling.

---

## VIDEO 2 — Packages, `static`, nested classes, and singleton

### 1. Detailed teaching

**Packages**

Packages organize types and help prevent name collisions.

```java
package learning.library;

public class Book {
}
```

The fully qualified name is:

```text
learning.library.Book
```

Another package can have its own `Book`.

Imports let you use short names:

```java
import learning.library.Book;
```

An import does not copy code or create an object.

Important details:

- `java.lang` is implicitly imported.
- Types in your own package generally need no import.
- `import java.util.*;` does not import subpackages.
- `company` and `company.tools` are separate packages; their names do not grant special access privileges.

**Instance members versus static members**

An instance member belongs to a particular object. A static member belongs to the class.

```java
class Visitor {
    String name;
    static int createdCount;

    Visitor(String name) {
        this.name = name;
        createdCount++;
    }
}
```

```java
Visitor a = new Visitor("Asha");
Visitor b = new Visitor("Ravi");

System.out.println(a.name);               // Asha
System.out.println(b.name);               // Ravi
System.out.println(Visitor.createdCount); // 2
```

Each visitor has a name; the class shares the counter.

More precisely, static state belongs to a loaded class. Different class loaders can load separate copies of a class.

**Static methods**

```java
class NumberTools {
    static int square(int value) {
        return value * value;
    }
}
```

Call it using the class name:

```java
int result = NumberTools.square(5);
```

A static method has no current instance, so it cannot directly use `this` or an instance field.

```java
class Visitor {
    String name;

    static void printName() {
        // System.out.println(name); // Which visitor?
    }
}
```

It can access instance members through an explicit object:

```java
static void printName(Visitor visitor) {
    System.out.println(visitor.name);
}
```

An instance method can access both instance state and static members.

**Static initialization**

```java
class Settings {
    static int timeout;

    static {
        timeout = 30;
    }
}
```

Static field initializers and static blocks run during class initialization, in textual order.

They do not run for every object you create.

**Understanding `System.out.println`**

```java
System.out.println("Hello");
```

Break it down:

- `System`: a class in `java.lang`.
- `out`: a static field referring to a `PrintStream`.
- `println(...)`: an instance method invoked on that stream.

This combines class-level access with an object method call.

**Nested classes**

A nested class is declared inside another class.

```java
class Library {
    static class Catalog {
    }

    class Shelf {
    }
}
```

A static nested class has no implicit enclosing `Library` instance:

```java
Library.Catalog catalog = new Library.Catalog();
```

An inner class is associated with an enclosing instance:

```java
Library library = new Library();
Library.Shelf shelf = library.new Shelf();
```

Use an inner class when that association is meaningful. Otherwise, a static nested class often avoids unnecessary coupling.

**Singleton**

A singleton controls access to one instance within its intended scope.

A simple eager version:

```java
final class AppSettings {
    private static final AppSettings INSTANCE = new AppSettings();

    private AppSettings() {
    }

    static AppSettings getInstance() {
        return INSTANCE;
    }
}
```

```java
AppSettings first = AppSettings.getInstance();
AppSettings second = AppSettings.getInstance();

System.out.println(first == second); // true
```

The ingredients are:

1. A private constructor.
2. A stored instance.
3. An access method.

### 2. Extra content you need

**Static does not mean thread-safe.**

```java
createdCount++;
```

involves reading, calculating, and writing. Multiple threads can interfere.

Similarly, safely constructing a singleton does not automatically make its mutable fields safe for concurrent use.

For lazy initialization, the holder pattern is useful:

```java
final class AppSettings {
    private AppSettings() {
    }

    private static class Holder {
        private static final AppSettings INSTANCE = new AppSettings();
    }

    static AppSettings getInstance() {
        return Holder.INSTANCE;
    }
}
```

The JVM’s class initialization mechanism safely initializes the holder when needed.

Also, avoid making everything static. If different objects need different data or replaceable behavior, instances are usually a better fit.

### 3. Tips, techniques, and mnemonics

- **Static = shared by the class; instance = specific to an object.**
- Ask: “Does this operation need one particular object’s state?”
- Access static members through the class name.
- **Inner needs an outer; static nested does not.**
- Singleton does not mean “one object across every server in a distributed application.”

**Practice:** Build a `Ticket` class with a per-ticket ID and a shared creation counter.

### 4. Summary

Packages organize types. Static members belong to the class, while instance members belong to objects. Static methods need explicit references to access instance state. Nested classes group related types. A singleton controls instance creation, but shared mutable state still requires careful design.

---

## VIDEO 3 — The four OOP principles

### 1. Detailed teaching

**Inheritance**

Inheritance lets a subclass specialize a superclass.

```java
class Device {
    private final String name;

    Device(String name) {
        this.name = name;
    }

    String getName() {
        return name;
    }

    void start() {
        System.out.println("Device starting");
    }
}

class Printer extends Device {
    Printer(String name) {
        super(name);
    }

    void print() {
        System.out.println(getName() + " is printing");
    }
}
```

`Printer extends Device` represents an **is-a** relationship.

A printer is a device.

`super(name)` invokes a superclass constructor. Constructors are not inherited, but superclass initialization is part of constructing the subclass object.

A superclass’s private fields remain part of the object’s state, but subclass code cannot directly access them.

**Forms of inheritance**

| Form | Example | Java classes |
|---|---|---|
| Single | `Printer extends Device` | Supported |
| Multilevel | `LaserPrinter extends Printer extends Device` | Supported |
| Hierarchical | Several subclasses extend `Device` | Supported |
| Multiple direct class inheritance | One class extends two classes | Not supported |

A class may implement multiple interfaces, which you will study in Video 5.

**Reference type versus runtime object type**

```java
Device device = new Printer("Office printer");
```

The declared reference type is `Device`. The runtime object is a `Printer`.

The declared type determines which members are accessible through that expression:

```java
device.start(); // Allowed
// device.print(); // Not available through Device
```

The object has not lost its printer behavior. The reference type limits what the compiler permits directly.

**Polymorphism**

Polymorphism allows one common type to represent different implementations.

```java
class Device {
    void start() {
        System.out.println("Device starting");
    }
}

class Printer extends Device {
    @Override
    void start() {
        System.out.println("Printer warming up");
    }
}
```

```java
Device device = new Printer();
device.start(); // Printer warming up
```

For an overridden instance method, the runtime object determines which implementation executes. [Oracle’s polymorphism explanation](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)

**Overloading versus overriding**

Overloading means the same method name with different parameter lists:

```java
void print(String text) {
}

void print(int number) {
}

void print(String text, int copies) {
}
```

Overriding means a subclass supplies a new implementation of an inherited instance method.

| Property | Overloading | Overriding |
|---|---|---|
| Parameters | Must differ | Same signature |
| Inheritance required? | No | Yes |
| Main selection step | Compile time | Runtime dispatch |
| Return type alone sufficient? | No | Must obey overriding rules |

Example of an important distinction:

```java
static void inspect(Object value) {
    System.out.println("Object version");
}

static void inspect(String value) {
    System.out.println("String version");
}
```

```java
Object value = "hello";
inspect(value); // Object version
```

Overload selection uses the expression’s compile-time type.

**Fields and static methods behave differently**

Fields are not dynamically dispatched:

```java
class Parent {
    int value = 10;
}

class Child extends Parent {
    int value = 20;
}

Parent object = new Child();
System.out.println(object.value); // 10
```

Static methods can be hidden, but they are not overridden through runtime instance dispatch.

**`final` with inheritance**

```java
final class Utility {
}
```

A final class cannot be subclassed.

```java
class Device {
    final void identify() {
    }
}
```

A final method cannot be overridden.

Do not choose `final` because you assume it guarantees a performance improvement. Use it to express design constraints.

**Encapsulation**

Encapsulation brings state and behavior together and controls how state changes.

```java
class Volume {
    private int level;

    public void setLevel(int level) {
        if (level < 0 || level > 100) {
            throw new IllegalArgumentException("Expected 0 to 100");
        }
        this.level = level;
    }

    public int getLevel() {
        return level;
    }
}
```

The purpose is to preserve an invariant:

```text
0 <= level <= 100
```

Adding getters and setters mechanically does not guarantee good encapsulation. Operations should protect meaningful rules.

**Abstraction**

Abstraction exposes the operations a caller needs while hiding unnecessary implementation details.

```java
printer.print();
```

The caller need not understand how the printer queues pages, communicates with hardware, or manages buffers.

The distinction:

- **Encapsulation:** control access and preserve valid state.
- **Abstraction:** present a useful, simpler view of behavior.

They often work together.

### 2. Extra content you need

**Composition: the has-a relationship**

```java
class Engine {
    void start() {
        System.out.println("Engine starting");
    }
}

class Car {
    private final Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void start() {
        engine.start();
    }
}
```

A car **has an** engine. It is not an engine.

Composition is often preferable when you want to reuse a capability without claiming a subtype relationship.

**Substitutability**

A subclass should honor the expectations established by its superclass.

If a base type promises an operation, a subtype that arbitrarily rejects it can make the hierarchy misleading.

Before using inheritance, ask:

> Can callers use this subtype wherever the parent is expected without breaking the parent’s contract?

### 3. Tips, techniques, and mnemonics

- **Is-a → consider inheritance. Has-a → consider composition.**
- **Overload = different inputs. Override = different implementation.**
- **Reference type checks access; runtime object selects overridden behavior.**
- Four principles: **A PIE** — Abstraction, Polymorphism, Inheritance, Encapsulation.
- Use `@Override` whenever you intend to override a method.

**Practice:** Create `Notification` with `EmailNotification` and `SmsNotification` subclasses. Store both in a `Notification[]` and invoke `send()`.

### 4. Summary

Inheritance models specialization. Polymorphism lets callers use common types with different implementations. Overloading is resolved using compile-time information; overridden instance methods dispatch using the runtime object. Encapsulation protects state, and abstraction simplifies usage. Composition models collaborating objects.

---

## VIDEO 4 — Access control, packages, and `Object`

### 1. Detailed teaching

**Access modifiers**

For ordinary member access, remember:

| Modifier | Declaring class | Same package | Subclass in another package | Unrelated class in another package |
|---|---:|---:|---:|---:|
| `private` | Yes | No | No | No |
| No modifier | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes, with restrictions | No |
| `public` | Yes | Yes | Yes | Yes |

This is the introductory view; nested types have additional private-access privileges, and accessibility of the enclosing type also matters. [Oracle’s access-control guide](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

**Package-private** means no access modifier:

```java
class Helper {
}
```

Do not write `default class Helper`.

For ordinary top-level classes, choose `public` or package-private. Top-level classes cannot be `private` or `protected`.

**The subtle part of `protected`**

In a different package, subclass access to an inherited protected instance member is restricted by the receiver type.

```java
// package first;
public class Parent {
    protected int value;
}
```

```java
// package second;
public class Child extends Parent {
    void inspect(Child child, Parent parent) {
        System.out.println(this.value);  // Allowed
        System.out.println(child.value); // Allowed

        // System.out.println(parent.value); // Not allowed here
    }
}
```

Being a subclass does not grant unrestricted access to the protected state of every `Parent` object.

**Useful standard packages**

| Package | Typical purpose |
|---|---|
| `java.lang` | Fundamental types such as `String`, `Object`, wrappers |
| `java.util` | Collections and utilities |
| `java.io` | Stream-oriented I/O |
| `java.nio.file` | Paths and file operations |
| `java.net` | Networking |
| `java.time` | Dates and times |
| `java.awt` | Desktop graphics and UI infrastructure |

Treat applet examples in older material as historical rather than a starting point for new applications.

**The `Object` class**

Every Java class ultimately inherits from `Object`, directly or indirectly.

Several methods are especially important:

| Method | Purpose |
|---|---|
| `toString()` | Text representation |
| `equals(Object)` | Equality comparison |
| `hashCode()` | Hash value used by hash-based structures |
| `getClass()` | Runtime class information |
| `clone()` | Protected copying mechanism with a specific contract |

`Object` also supplies monitor methods such as `wait()`, `notify()`, and `notifyAll()`, which belong in a later concurrency lesson.

**`==` versus `equals()`**

For references, `==` asks whether the references identify the same object.

```java
String first = new String("Java");
String second = new String("Java");

System.out.println(first == second);      // false
System.out.println(first.equals(second)); // true
```

`equals()` expresses the equality defined by the class.

The default implementation inherited from `Object` uses identity. Custom value-like classes often override it.

**Designing equality**

Suppose a product is identified by its integer ID:

```java
final class Product {
    private final int id;

    Product(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (!(other instanceof Product)) {
            return false;
        }

        Product product = (Product) other;
        return id == product.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }

    @Override
    public String toString() {
        return "Product{id=" + id + "}";
    }
}
```

The fundamental hashing rule is:

```text
Equal objects must have equal hash codes.
Equal hash codes do not necessarily mean equal objects.
```

A hash code is not guaranteed to be unique or to represent a memory address. `Object`’s equality, hashing, and cloning contracts are documented in its [API reference](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Object.html).

**`instanceof` versus `getClass()`**

```java
Object value = "hello";

System.out.println(value instanceof String);        // true
System.out.println(value.getClass() == String.class); // true
```

With inheritance:

- `instanceof` checks compatibility with a type.
- `getClass()` identifies the exact runtime class.
- `null instanceof SomeType` is false.
- Calling `getClass()` through null throws `NullPointerException`.

### 2. Extra content you need

**Do not mutate hash keys in ways that change equality.**

Imagine putting an object into a `HashSet`, then changing the ID used by `hashCode()`.

The object remains stored according to its old hash placement, while future lookups use its new hash. Operations such as `contains()` can then behave unexpectedly.

Prefer stable, immutable identity fields for keys.

For null-safe equality:

```java
Objects.equals(first, second);
```

For arrays:

```java
Arrays.equals(firstArray, secondArray);
```

Ordinary array `.equals()` does not compare all elements.

### 3. Tips, techniques, and mnemonics

- **Access expands:** private → package → protected → public.
- **`==` = same object; `.equals()` = class-defined equality.**
- **Equal → same hash. Same hash ↛ necessarily equal.**
- Generate `equals()` and `hashCode()` together, then review which fields define identity.
- Start fields as private and expose only operations that callers need.

**Practice:** Put two separate `Product` objects with the same ID into a `HashSet`. With the implementation above, the set should contain one logical product.

### 4. Summary

Access modifiers establish boundaries. Packages organize APIs. `Object` provides shared foundational behavior. Reference identity and logical equality differ. Equality and hashing must agree, especially when objects enter sets and maps.

---

## VIDEO 5 — Abstract classes, interfaces, and annotations

### 1. Detailed teaching

**Abstract classes**

An abstract class can provide shared state and implementation while leaving some behavior for subclasses.

```java
abstract class Shape {
    private final String color;

    Shape(String color) {
        this.color = color;
    }

    String getColor() {
        return color;
    }

    abstract double area();
}

class Circle extends Shape {
    private final double radius;

    Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

```java
Shape shape = new Circle("Blue", 2);
System.out.println(shape.area());
```

You cannot directly instantiate `Shape`:

```java
// new Shape("Blue"); // Compilation error
```

An abstract class:

- Can have constructors.
- Can have fields.
- Can have concrete methods.
- Can have static methods.
- May contain zero or more abstract methods.
- Cannot itself be `final`.

An abstract method cannot be `private`, `static`, or `final`, because its purpose requires implementation through overriding.

A constructor cannot be abstract. An abstract class’s constructor still runs when a concrete subclass is constructed. [Oracle’s abstract-class guide](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)

**Interfaces**

An interface defines a type and a behavioral contract.

```java
interface Printable {
    void print();
}

class Report implements Printable {
    @Override
    public void print() {
        System.out.println("Printing report");
    }
}
```

The method declaration above is implicitly `public abstract`. Therefore, the implementation must be public.

```java
Printable document = new Report();
document.print();
```

Code using `Printable` can work with any implementation that fulfills its contract.

**Multiple interfaces**

```java
interface Scannable {
    void scan();
}

class OfficeMachine implements Printable, Scannable {
    @Override
    public void print() {
        System.out.println("Printing");
    }

    @Override
    public void scan() {
        System.out.println("Scanning");
    }
}
```

One class can implement several interfaces.

An interface can extend other interfaces:

```java
interface MultiFunction extends Printable, Scannable {
}
```

**Interface members**

Modern interfaces can contain:

- Abstract methods.
- Default methods.
- Static methods.
- Private helper methods.
- Constants.
- Nested types.

Interface fields are implicitly `public static final`:

```java
interface Limits {
    int MAX_ITEMS = 100;
}
```

A default method supplies an instance implementation:

```java
interface Printable {
    void print();

    default void printTwice() {
        print();
        print();
    }
}
```

A static method belongs to the interface:

```java
interface Printable {
    void print();

    static boolean isSupported(String format) {
        return "PDF".equals(format);
    }
}
```

Call it as:

```java
Printable.isSupported("PDF");
```

Static interface methods are not inherited as ordinary instance methods by implementing classes.

**Default-method conflicts**

If unrelated interfaces provide conflicting default methods, the implementing class must resolve the conflict:

```java
interface A {
    default String label() {
        return "A";
    }
}

interface B {
    default String label() {
        return "B";
    }
}

class Combined implements A, B {
    @Override
    public String label() {
        return A.super.label() + B.super.label();
    }
}
```

**Abstract class versus interface**

| Question | Abstract class | Interface |
|---|---|---|
| Instance fields? | Yes | No |
| Constructors? | Yes | No |
| Shared implementation? | Yes | Yes, through suitable methods |
| Multiple direct inheritance by a class? | One superclass | Multiple interfaces |
| Typical purpose | Shared base with state | Capability or contract |

Do not memorize “interfaces contain only abstract methods.” That is outdated.

**Annotations**

Annotations attach metadata to declarations and other supported locations.

```java
@Override
public String toString() {
    return "Report";
}
```

Useful examples:

- `@Override`: compiler checks that overriding is valid.
- `@Deprecated`: marks an API as deprecated.
- `@SuppressWarnings`: suppresses a named warning.
- `@FunctionalInterface`: checks the functional-interface requirement.

Annotations do not automatically execute behavior. Their effect depends on the compiler, tools, or runtime code that interprets them.

### 2. Extra content you need

**Depend on a contract and supply the implementation**

```java
interface MessageSender {
    void send(String message);
}

class AlertService {
    private final MessageSender sender;

    AlertService(MessageSender sender) {
        this.sender = sender;
    }

    void alert() {
        sender.send("Attention required");
    }
}
```

`AlertService` does not create a specific email or SMS sender internally. Its dependency is supplied through its constructor.

This makes implementations replaceable and simplifies testing. It is the basic idea of constructor dependency injection.

### 3. Tips, techniques, and mnemonics

- **Abstract class = shared base. Interface = promised capability.**
- A class **extends** a class and **implements** interfaces.
- An interface **extends** interfaces.
- Use small interfaces whose operations belong together.
- `@Override` catches misspelled method names and incorrect parameter lists.

**Practice:** Create a `PaymentProcessor` interface and two implementations. Make a checkout class accept the interface through its constructor.

### 4. Summary

Abstract classes combine partial implementation with shared state. Interfaces define contracts that many classes can implement. Default methods permit some shared behavior in interfaces. Annotations communicate metadata. Programming against interfaces makes implementations easier to replace.

---

## VIDEO 6 — Generics, custom lists, lambdas, exceptions, and copying

### 1. Detailed teaching

This lesson contains several substantial topics. Study them individually.

**Generics**

Generics let a type or method work with specified types while preserving compile-time checks.

```java
class Box<T> {
    private T value;

    void set(T value) {
        this.value = value;
    }

    T get() {
        return value;
    }
}
```

```java
Box<String> box = new Box<>();
box.set("Java");

String text = box.get();
// box.set(123); // Compilation error
```

`T` is a type parameter. `String` is a type argument.

Common naming conventions:

- `T`: type.
- `E`: element.
- `K`: key.
- `V`: value.

These are conventions, not special keywords.

**Why not use `Object` everywhere?**

An `Object` field can hold unrelated values, requiring casts when retrieving them. Generics express the intended type relationship and catch many mistakes earlier.

**Building a small dynamic list**

An array has fixed length. A dynamic list creates a larger array when necessary.

It tracks two different quantities:

- **Size:** number of stored elements.
- **Capacity:** length of the backing array.

```java
import java.util.Arrays;

class TinyList<T> {
    private Object[] data = new Object[2];
    private int size;

    public void add(T value) {
        if (size == data.length) {
            data = Arrays.copyOf(data, data.length * 2);
        }
        data[size++] = value;
    }

    @SuppressWarnings("unchecked")
    public T get(int index) {
        checkIndex(index);
        return (T) data[index];
    }

    public void set(int index, T value) {
        checkIndex(index);
        data[index] = value;
    }

    public T remove(int index) {
        T removed = get(index);

        int moved = size - index - 1;
        if (moved > 0) {
            System.arraycopy(data, index + 1, data, index, moved);
        }

        data[--size] = null;
        return removed;
    }

    public int size() {
        return size;
    }

    private void checkIndex(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index: " + index);
        }
    }
}
```

Usage:

```java
TinyList<String> names = new TinyList<>();

names.add("Asha");
names.add("Ravi");
names.add("Meera"); // Triggers growth

System.out.println(names.get(1)); // Ravi
names.remove(1);
System.out.println(names.get(1)); // Meera
```

Why clear the vacated slot?

```java
data[--size] = null;
```

Otherwise, the backing array unnecessarily retains a reference to the removed element.

Why the unchecked cast?

The backing array stores `Object` references. Under correct generic usage, the class’s methods preserve the rule that its elements are `T`. Avoid raw types, which can undermine that guarantee.

This is an educational list, not a complete replacement for `ArrayList`.

**Type erasure and restrictions**

Java implements generics largely through type erasure: type parameters are translated to their bounds or `Object`, with compiler-inserted checks and casts as needed.

Consequences include restrictions such as:

```java
// new T();
// new T[10];
// value instanceof List<String>
```

These are not permitted in ordinary generic code. Primitive type arguments are also unavailable. [Oracle’s generic restrictions](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html)

**Generic invariance**

Even though `Integer` extends `Number`:

```java
List<Integer>
```

is not a subtype of:

```java
List<Number>
```

Otherwise, code could insert a `Double` into a list intended to contain integers.

**Wildcards**

```java
List<? extends Number>
```

means a list of some unknown type that is `Number` or a subtype.

You can read elements as `Number`, but cannot safely add an arbitrary non-null number because the actual element type might be `Integer`, `Double`, or something else.

```java
static double sum(List<? extends Number> values) {
    double total = 0;

    for (Number value : values) {
        total += value.doubleValue();
    }

    return total;
}
```

For a destination that can accept integers:

```java
static void addDefaults(List<? super Integer> destination) {
    destination.add(10);
    destination.add(20);
}
```

When reading from that destination, the generally safe type is `Object`.

Remember **PECS: Producer Extends, Consumer Super**. Oracle describes the same distinction in terms of input and output roles. [Wildcard guidelines](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html)

**Comparing objects**

`Comparable<T>` defines a class’s natural ordering:

```java
class ExamResult implements Comparable<ExamResult> {
    private final int score;

    ExamResult(int score) {
        this.score = score;
    }

    int getScore() {
        return score;
    }

    @Override
    public int compareTo(ExamResult other) {
        return Integer.compare(score, other.score);
    }
}
```

A comparison returns:

- Negative: comes before.
- Zero: same position under this ordering.
- Positive: comes after.

Avoid:

```java
return score - other.score;
```

because subtraction can overflow.

`Comparator<T>` provides an external ordering:

```java
Comparator<ExamResult> descending =
    Comparator.comparingInt(ExamResult::getScore).reversed();
```

A class can have one natural ordering and many alternative comparators.

**Lambda expressions**

A lambda supplies an implementation for a functional interface.

```java
@FunctionalInterface
interface Operation {
    int apply(int left, int right);
}
```

```java
Operation add = (left, right) -> left + right;
Operation multiply = (left, right) -> left * right;

System.out.println(add.apply(4, 5));      // 9
System.out.println(multiply.apply(4, 5)); // 20
```

A functional interface has one relevant abstract method. Default and static methods do not count toward that abstract-method total.

A lambda that captures a local variable requires that variable to be final or effectively final:

```java
int multiplier = 3;
IntUnaryOperator triple = value -> value * multiplier;

// Reassigning multiplier would make the capture invalid.
```

**Exception handling**

Exceptions represent abnormal completion of an operation.

```java
try {
    int value = Integer.parseInt("abc");
    System.out.println(value);
} catch (NumberFormatException exception) {
    System.out.println("Enter a valid integer");
} finally {
    System.out.println("Attempt finished");
}
```

| Keyword | Role |
|---|---|
| `try` | Encloses an operation that may fail |
| `catch` | Handles a matching exception |
| `finally` | Performs cleanup when control leaves the construct |
| `throw` | Throws a particular exception |
| `throws` | Declares possible exceptions in a method signature |

`finally` normally executes during ordinary control flow and exception unwinding. It is not guaranteed after abrupt JVM termination.

**Checked versus unchecked exceptions**

Checked exceptions generally must be caught or declared.

```java
static void load() throws IOException {
    // ...
}
```

`RuntimeException` subclasses are unchecked:

```java
throw new IllegalArgumentException("Invalid quantity");
```

`Error` subclasses are also unchecked and generally represent problems application code should not casually catch.

Catch specific exceptions before broader ones.

**Custom exceptions**

```java
class InvalidQuantityException extends Exception {
    InvalidQuantityException(String message) {
        super(message);
    }
}
```

```java
static void validateQuantity(int quantity)
        throws InvalidQuantityException {

    if (quantity <= 0) {
        throw new InvalidQuantityException(
            "Quantity must be greater than zero"
        );
    }
}
```

Extending `Exception` creates a checked exception unless you extend one of its unchecked subclasses.

**Copying objects**

Assignment creates an alias:

```java
Profile second = first;
```

A shallow copy creates a new outer object but shares referenced nested objects.

A deep copy duplicates the mutable state that needs to be independent.

```java
class Profile {
    private final int[] scores;

    Profile(int[] scores) {
        this.scores = scores.clone();
    }

    Profile(Profile other) {
        this.scores = other.scores.clone();
    }

    void setScore(int index, int score) {
        scores[index] = score;
    }

    int getScore(int index) {
        return scores[index];
    }
}
```

```java
Profile first = new Profile(new int[]{70, 80});
Profile second = new Profile(first);

second.setScore(0, 99);

System.out.println(first.getScore(0));  // 70
System.out.println(second.getScore(0)); // 99
```

Because the array contains primitives, cloning it copies all its element values. Cloning an array of objects would still share the referenced elements.

The older `Object.clone()` mechanism normally performs a shallow field copy. Classes using that mechanism typically implement `Cloneable` and expose an appropriate `clone()` method. Copy constructors often express the intended copying behavior more clearly.

### 2. Extra content you need

**Try-with-resources**

Use it for resources implementing `AutoCloseable`:

```java
try (BufferedReader reader =
         Files.newBufferedReader(Path.of("notes.txt"))) {

    System.out.println(reader.readLine());
}
```

The resource is closed when the block finishes, including ordinary exception paths. This example must be inside code that catches or declares `IOException`. [Oracle’s resource-management guide](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

**Dynamic-list performance**

| Operation | Typical cost |
|---|---|
| Indexed get/set | O(1) |
| Append with spare capacity | O(1) |
| Append that grows the array | O(n) |
| Append averaged across many operations | Amortized O(1) |
| Remove from middle | O(n) |

**Defensive copying**

Copy mutable input when callers should not retain control of your internal state. Similarly, avoid returning your internal mutable array directly.

### 3. Tips, techniques, and mnemonics

- **PECS:** Producer Extends, Consumer Super.
- **Comparable = natural ordering; Comparator = alternative ordering.**
- **`throw` performs; `throws` declares.**
- **Alias = same object; shallow = shared children; deep = independent mutable state.**
- Suppress warnings narrowly and only when you understand why the operation is safe.
- Do not swallow exceptions with empty `catch` blocks.
- Avoid returning from `finally`, which can hide a result or exception.

**Practice:** Extend `TinyList<T>` with `contains`. Then write a comparator for objects, a lambda for a calculation, and a copy constructor for a class containing a mutable list.

### 4. Summary

Generics express type relationships and improve compile-time checking. Dynamic lists manage size and capacity. Wildcards describe flexible inputs and outputs. Comparators define ordering, while lambdas implement functional interfaces. Exceptions structure failure handling. Copying requires an explicit decision about which mutable state should be shared.

---

## VIDEO 7 — Collections, `Vector`, and enums

### 1. Detailed teaching

**The Collections Framework**

The framework provides interfaces, implementations, and algorithms for working with groups of objects.

Its major abstractions include:

| Type | Main idea |
|---|---|
| `List` | Ordered sequence, duplicates allowed |
| `Set` | Unique elements |
| `Queue` | Elements awaiting processing |
| `Deque` | Operations at both ends |
| `Map` | Key-value associations |

`Map` belongs to the framework but does not extend `Collection`.

Many queues use FIFO order, but not all. A priority queue uses priority ordering.

**Program to an interface**

```java
List<String> names = new ArrayList<>();
```

The variable promises `List` behavior while `ArrayList` supplies the implementation.

```java
names.add("Asha");
names.add("Ravi");
names.add("Asha");

System.out.println(names.size()); // 3
```

A set removes duplicates according to its equality rules:

```java
Set<String> names = new HashSet<>();

names.add("Asha");
names.add("Asha");

System.out.println(names.size()); // 1
```

A map associates keys and values:

```java
Map<Integer, String> users = new HashMap<>();

users.put(101, "Asha");
users.put(102, "Ravi");

System.out.println(users.get(101)); // Asha
```

Putting another value under the same key replaces the previous association.

**Choosing implementations**

| Need | Common starting point |
|---|---|
| Indexed sequence | `ArrayList` |
| Unique values with no ordering requirement | `HashSet` |
| Unique values preserving insertion order | `LinkedHashSet` |
| Sorted unique values | `TreeSet` |
| Lookup by key | `HashMap` |
| Keys in sorted order | `TreeMap` |
| Stack or FIFO queue without concurrency | `ArrayDeque` |
| Processing by priority | `PriorityQueue` |

Avoid assuming `LinkedList` is faster merely because you insert or remove elements. Finding a position can cost O(n), and memory locality also matters.

**`Collection` versus `Collections`**

- `Collection`: an interface.
- `Collections`: a utility class.

```java
Collections.sort(names);
Collections.reverse(names);
```

These examples require a suitable mutable list.

**`Vector`**

`Vector` is an older resizable-array implementation whose operations include synchronization.

```java
List<Integer> values = new Vector<>();
values.add(10);
```

For ordinary single-threaded list usage, `ArrayList` is the usual choice. The official Java learning material recommends modern alternatives to legacy types such as `Vector` and `Stack`. [Java Collections Framework guide](https://dev.java/learn/api/collections-and-streams/collections-framework/intro/)

Synchronization of individual methods does not make a multi-step operation atomic:

```java
if (!values.contains(10)) {
    values.add(10);
}
```

Another thread could act between those calls.

Choose a concurrency approach based on the entire operation you need to protect.

**Enums**

Enums represent a fixed set of named values.

```java
enum OrderStatus {
    NEW,
    PAID,
    SHIPPED,
    DELIVERED,
    CANCELLED
}
```

```java
OrderStatus status = OrderStatus.PAID;
```

This is safer than unrestricted strings:

```java
String status = "PAIDD"; // Typo is still a valid String
```

An enum prevents assignment of an arbitrary unrelated string.

**Enums are classes with instances**

They can contain fields, constructors, and methods:

```java
enum Priority {
    LOW(1),
    MEDIUM(2),
    HIGH(3);

    private final int rank;

    Priority(int rank) {
        this.rank = rank;
    }

    public int getRank() {
        return rank;
    }
}
```

```java
System.out.println(Priority.HIGH.getRank()); // 3
```

You cannot construct additional constants using `new Priority(...)`.

Useful operations:

```java
Priority.HIGH.name();        // "HIGH"
Priority.HIGH.ordinal();     // 2
Priority.values();           // All constants
Priority.valueOf("HIGH");    // Priority.HIGH
```

`valueOf` is case-sensitive and throws an exception for an unknown name.

Enums implicitly extend `Enum`, cannot explicitly extend another class, and can implement interfaces. [Oracle’s enum guide](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)

### 2. Extra content you need

**Do not persist `ordinal()` as a business identifier.**

Reordering enum constants changes their ordinals.

If external data needs a stable code, define one explicitly:

```java
enum OrderStatus {
    NEW("N"),
    PAID("P"),
    SHIPPED("S");

    private final String code;

    OrderStatus(String code) {
        this.code = code;
    }

    String getCode() {
        return code;
    }
}
```

**Specialized enum collections**

Use `EnumSet` for sets of enum constants and `EnumMap` for maps with enum keys:

```java
Set<OrderStatus> active =
    EnumSet.of(OrderStatus.NEW, OrderStatus.PAID);
```

**Removing while iterating**

Avoid structural changes through the collection itself during ordinary iteration:

```java
// Potential ConcurrentModificationException:
for (String name : names) {
    if (name.isBlank()) {
        names.remove(name);
    }
}
```

For a suitable mutable collection:

```java
names.removeIf(String::isBlank);
```

An iterator’s supported `remove()` method is another option.

**Unmodifiable does not necessarily mean deeply immutable**

```java
List<Book> books = List.of(book);
```

The list cannot have elements added or removed through its API. The contained `Book` object may still be mutable.

### 3. Tips, techniques, and mnemonics

- **List = sequence; Set = uniqueness; Map = association.**
- Ask about **ordering, duplicates, lookup, and concurrency** before choosing a collection.
- **Enum = a named set of allowed values.**
- Comparing enum constants with `==` is appropriate.
- Do not rely on `HashMap` or `HashSet` iteration order.
- Remember that `remove(1)` on a `List<Integer>` removes the element at index 1. To remove the value 1, use `remove(Integer.valueOf(1))`.

**Practice:** Store orders in a map keyed by ID. Give each order an enum status. Find all orders whose status belongs to an `EnumSet` of active states.

### 4. Summary

Collection interfaces describe different storage needs, and implementations provide different behavior and performance. `Vector` is legacy and does not solve every concurrency problem. Enums model fixed choices with type safety and can carry behavior and stable codes.

---

## Put all seven lessons together

Build a small **library management application** with these requirements:

| Feature | Concepts practiced |
|---|---|
| `Book` with validated constructor inputs | Classes, objects, constructors |
| Private fields and controlled borrowing methods | Encapsulation |
| Packages for models and services | Organization and access control |
| `Borrowable` interface | Abstraction and interfaces |
| Different borrowable item implementations | Polymorphism |
| `Map<String, Book>` indexed by book ID | Generics and collections |
| `BookStatus` enum | Fixed, valid states |
| Sorting books by title | Comparators and lambdas |
| Exception when an unavailable book is borrowed | Exception handling |
| Equality based on stable book ID | `equals()` and `hashCode()` |
| Copies that do not expose internal mutable lists | Defensive copying |

For each feature, use this study loop:

1. **Explain** the concept aloud without looking.
2. **Predict** what a small example will do.
3. **Run** the example.
4. **Change** one detail and explain the new behavior.
5. **Apply** it to the library project.

Your strongest checkpoint is this: **you can explain why the code behaves as it does, including when it fails to compile, throws an exception, or unexpectedly shares state.**
