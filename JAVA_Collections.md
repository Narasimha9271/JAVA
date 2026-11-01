# Java 21 — Generics and Collections

Think of a collection as a **container**, and generics as the **type label on that container**:

```text
List<String>
│    │
│    └── What may go inside? Strings.
└─────── How is it organized? A list.
```

We’ll use Java 21. Outputs below are **expected, not executed here**.

**The five memory cues**

> **List = Line**  
> **Set = Single copy**  
> **Map = Match a key to a value**  
> **Queue = Who goes next?**  
> **Deque = Double-ended queue**

---

**1. Generics: put a type label on your container**

Suppose you want a list of student names:

```java
List<String> names = new ArrayList<>();

names.add("Asha");
names.add("Ravi");

// names.add(100);  // Compile-time error
```

`<String>` means the list holds string references.

The compiler prevents adding an integer. When you retrieve an element, it knows the result is a `String`:

```java
String first = names.get(0);
```

Without a useful type parameter, mistakes can survive until execution.

**Remember: “Generics catch type mistakes before the program runs.”**

Read this declaration carefully:

```java
List<String> names = new ArrayList<>();
```

| Part | Meaning |
|---|---|
| `List` | Interface: the operations and behavior you need |
| `String` | Element type |
| `ArrayList` | Implementation: how that behavior is provided |
| `<>` | The compiler infers the type arguments |

An **interface** describes a contract. An **implementation** supplies the working machinery.

Think:

```text
List       = I need an ordered sequence.
ArrayList  = Implement it using a resizable array.
```

Use reference types in generic arguments:

```java
List<Integer> numbers;  // Valid
// List<int> numbers;   // Invalid
```

Adding an `int` to a `List<Integer>` uses boxing.

---

**2. Your own generic type**

A generic class lets one design work with different types.

```java
public class GenericDemo {
    static class Box<T> {
        private final T value;

        Box(T value) {
            this.value = value;
        }

        T get() {
            return value;
        }
    }

    public static void main(String[] args) {
        Box<String> name = new Box<>("Asha");
        Box<Integer> score = new Box<>(90);

        System.out.println(name.get());
        System.out.println(score.get());
    }
}
```

Expected output:

```text
Asha
90
```

`T` is a **type parameter**—a placeholder for a type.

Common naming conventions:

| Symbol | Usual meaning |
|---|---|
| `T` | Type |
| `E` | Element |
| `K` | Key |
| `V` | Value |

They are names, not special keywords.

**Memory cue: “Write the container once; choose its type when using it.”**

---

**3. Choose the collection by the question you need to answer**

Imagine managing students:

```text
Need the third registered student? ──► List

Need each course name only once? ────► Set

Need a student using their ID? ──────► Map

Need the next student to serve? ─────► Queue

Need to add/remove at both ends? ────► Deque
```

The simplified interface relationships are:

```text
Collection
├── List
├── Set
└── Queue
    └── Deque

Map — separate hierarchy
```

`Map` is part of the Collections Framework, but does not extend `Collection`.

Also distinguish:

- `Collection`: an interface.
- `Collections`: a utility class containing methods such as `sort` and `reverse`.

---

**4. `List`: a numbered line**

A list preserves positional order and allows duplicates.

```text
Index:  0       1       2
       Asha    Ravi    Asha
```

```java
List<String> names = new ArrayList<>();

names.add("Asha");
names.add("Ravi");
names.add("Asha");

System.out.println(names.get(1)); // Ravi

names.set(1, "Maya");
names.remove(0);

System.out.println(names);        // [Maya, Asha]
```

**Ordered does not mean sorted.** A list retains its sequence; it does not automatically alphabetize elements.

**`ArrayList` versus `LinkedList`**

```text
ArrayList:
[reference][reference][reference]...
       resizable backing array

LinkedList:
[node] ⇄ [node] ⇄ [node]
        linked nodes
```

| Need | Usually choose |
|---|---|
| Everyday list, indexed access, appending | `ArrayList` |
| Linked-list behavior with an already-positioned iterator | Consider `LinkedList` |
| Stack or queue operations | Usually `ArrayDeque` |

`ArrayList` provides O(1) indexed access and amortized O(1) appending. Inserting or removing near the beginning requires shifting elements. [Java 21 `ArrayList`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayList.html)

**Amortized O(1)** means that a sequence of appends averages constant work per append. An individual append may resize and copy the backing array.

A common misconception:

> “LinkedList insertion is always O(1).”

Updating links is O(1) once the location is known. Finding an indexed location can take O(n).

**Integer removal trap**

```java
List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30));

numbers.remove(1);                  // Removes index 1: value 20
numbers.remove(Integer.valueOf(10)); // Removes value 10
```

---

**5. `Set`: keep one of each**

A set prevents duplicate elements according to its equality or ordering rules.

```java
Set<String> courses = new HashSet<>();

courses.add("Java");
courses.add("SQL");
courses.add("Java");

System.out.println(courses.size()); // 2
```

Choose the implementation based on order:

| Implementation | Iteration order | Memory cue |
|---|---|---|
| `HashSet` | No guaranteed order | **Hash = lookup** |
| `LinkedHashSet` | Insertion order | **Linked = remembers arrival** |
| `TreeSet` | Sorted order | **Tree = sorted** |

```java
Set<Integer> sorted = new TreeSet<>();

sorted.add(30);
sorted.add(10);
sorted.add(20);

System.out.println(sorted); // [10, 20, 30]
```

**Connection to your previous lesson**

For hash-based sets, `equals()` and `hashCode()` work together:

```text
hashCode() → helps locate candidates
equals()   → decides equality
```

Equal objects must have equal hash codes. Equal hash codes do not necessarily mean equal objects.

Avoid changing fields used by `equals` or `hashCode` while an object is stored in a hash-based set or used as a map key. Otherwise, lookup can stop finding it where expected.

For `TreeSet`, comparison returning zero means “the same element for set purposes.” Its ordering should be consistent with `equals` to satisfy the normal set contract.

---

**6. `Map`: a dictionary**

A map associates a unique key with a value:

```text
Student ID      Student name
101 ──────────► Asha
102 ──────────► Ravi
```

```java
Map<Integer, String> students = new HashMap<>();

students.put(101, "Asha");
students.put(102, "Ravi");

System.out.println(students.get(101)); // Asha

students.put(101, "Maya");             // Replaces value for key 101
System.out.println(students.get(101)); // Maya
```

Keys are unique. Values can repeat.

| Implementation | Use when |
|---|---|
| `HashMap` | You need lookup without an ordering requirement |
| `LinkedHashMap` | You want predictable insertion order; it can also support access order |
| `TreeMap` | You need keys maintained in sorted order |

`HashMap` has no guaranteed iteration order. Its basic lookup/update operations are expected O(1) under suitable hash distribution; collisions and resizing matter. [Java 21 `HashMap`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html)

**Missing-key trap**

```java
students.get(999)
```

returns `null`.

Because `HashMap` can also store a null value, use:

```java
students.containsKey(999)
```

when you need to distinguish “missing key” from “key mapped to null.”

**Memory cue: “List asks where; Map asks which key.”**

---

**7. `Queue` and `Deque`: deciding who leaves next**

For a FIFO queue:

> **First In, First Out**

```text
Remove here                           Add here
    ↓                                     ↓
  [Asha] ───── [Ravi] ───── [Maya]
```

```java
Queue<String> line = new ArrayDeque<>();

line.offer("Asha");
line.offer("Ravi");

System.out.println(line.peek()); // Asha: inspect
System.out.println(line.poll()); // Asha: remove
System.out.println(line.poll()); // Ravi
System.out.println(line.poll()); // null: empty
```

Useful pairs:

| Operation | Special-result form | Exception form |
|---|---|---|
| Insert | `offer()` | `add()` |
| Remove head | `poll()` | `remove()` |
| Inspect head | `peek()` | `element()` |

`offer` can return `false` if a capacity-constrained queue cannot accept an element. `poll` and `peek` return `null` when empty.

**A queue is not always FIFO.**

A `PriorityQueue` chooses according to priority:

```java
Queue<Integer> priority = new PriorityQueue<>();

priority.offer(30);
priority.offer(10);
priority.offer(20);

System.out.println(priority.poll()); // 10
System.out.println(priority.poll()); // 20
System.out.println(priority.poll()); // 30
```

Natural ordering places the smallest element at the head. Supply a comparator to change that.

**Important:** iterating or printing a `PriorityQueue` does not guarantee sorted order. Repeatedly polling retrieves elements in priority order. [Java 21 `PriorityQueue`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/PriorityQueue.html)

**Deque = Double-Ended QUEue**

```text
add/remove ⇄ [Asha] [Ravi] [Maya] ⇄ add/remove
```

```java
Deque<String> deque = new ArrayDeque<>();

deque.addFirst("Asha");
deque.addLast("Ravi");

System.out.println(deque.removeFirst()); // Asha
System.out.println(deque.removeLast());  // Ravi
```

For a stack:

> **Last In, First Out**

```java
Deque<String> stack = new ArrayDeque<>();

stack.push("First");
stack.push("Second");

System.out.println(stack.pop()); // Second
```

Prefer `Deque` for ordinary stack usage rather than the older `Stack` class. `ArrayDeque` prohibits nulls and provides amortized O(1) operations at its ends. [Java 21 `ArrayDeque`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayDeque.html)

---

**8. Bounds: restrict which types are acceptable**

A bound says:

> “This type parameter must satisfy this requirement.”

```java
static <T extends Number> double twice(T value) {
    return value.doubleValue() * 2;
}
```

Examples:

```java
twice(10);    // Integer
twice(2.5);   // Double
// twice("Hi"); // Compile-time error
```

`Number` supplies methods such as `doubleValue`, so the compiler permits that call.

The same `extends` keyword is used for an interface bound:

```java
<T extends Comparable<T>>
```

**Memory cue: “A bound is an admission rule.”**

For this particular `twice` method, accepting a `Number` directly would also work. A named generic parameter becomes especially useful when you need to preserve relationships between input and output types.

---

**9. Invariance: why `List<Integer>` is not `List<Number>`**

An `Integer` is a `Number`, but this is invalid:

```java
List<Integer> integers = new ArrayList<>();

// List<Number> numbers = integers; // Does not compile
```

Why?

If it were allowed:

```java
numbers.add(3.5);
```

would put a `Double` into a list promised to contain integers.

```text
List<Integer> ──X──► List<Number>
```

Generic types are **invariant** in this sense: the subtype relationship between their arguments does not automatically become the same relationship between the parameterized types.

**Memory cue: “An integer is a number; an integer-only container is not an any-number container.”**

Wildcards provide controlled flexibility.

---

**10. Wildcards: `?`, `extends`, and `super`**

`?` means “some type that is not specified here.”

| Type | Meaning | Safe reading | Safe addition through this reference |
|---|---|---|---|
| `List<?>` | List of an unknown type | `Object` | No arbitrary non-null element |
| `List<? extends Number>` | List of some subtype of `Number` | `Number` | No arbitrary non-null number |
| `List<? super Integer>` | List of some supertype of `Integer` | `Object` | `Integer` values |

The compiler permits adding `null` in these cases, but the collection itself may reject null or reject all modifications.

**The key mnemonic: PECS**

> **Producer Extends, Consumer Super**

Think from the method’s perspective:

- A source **produces** values for your method to read → `extends`.
- A destination **consumes** values your method supplies → `super`.

```text
Source                         Destination
? extends T ─── read T ───► ? super T
```

Complete example:

```java
import java.util.ArrayList;
import java.util.List;

public class WildcardDemo {
    static <T> void copyInto(
            List<? extends T> source,
            List<? super T> destination) {

        for (T value : source) {
            destination.add(value);
        }
    }

    public static void main(String[] args) {
        List<Integer> source = List.of(10, 20);
        List<Number> destination = new ArrayList<>();

        copyInto(source, destination);

        System.out.println(destination);
    }
}
```

Expected output:

```text
[10, 20]
```

This method copies element references, not the objects themselves:

```text
source[0] ─────────► Integer object representing 10
destination[0] ────► same object
```

The method assumes separate source and destination collections and a destination that supports addition.

If there are `n` source elements and destination appends are amortized O(1), copying takes O(n) time. The destination gains O(n) references; extra working space is O(1).

**Important qualification:** `? extends T` does not make an object immutable. It restricts type-safe additions through that reference; operations such as removal may still be possible.

---

**11. Erasure: how Java implements generics**

The compiler checks generic types, then translates them primarily using **type erasure**:

- Type parameters are replaced by their bounds, or `Object`.
- Necessary casts are inserted.
- Bridge methods may be generated to preserve overriding behavior.

Java does not normally generate a separate runtime class for every type argument. Some generic declaration information remains in metadata. [Oracle’s type-erasure explanation](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)

For example:

```java
List<String> names = new ArrayList<>();
List<Integer> numbers = new ArrayList<>();

System.out.println(names.getClass() == numbers.getClass());
```

Expected output:

```text
true
```

Consequences:

```java
// new T();                     // Not allowed for an ordinary type parameter
// new List<String>[10];        // Not allowed
// value instanceof List<String> // Not allowed
```

But:

```java
value instanceof List<?>
```

is allowed.

You also cannot overload methods solely by:

```java
process(List<String> values)
process(List<Integer> values)
```

Both erase to the same parameter type.

**Memory cue: “The compiler checks the label; runtime objects usually share the same generic machinery.”**

Avoid **raw types** such as `List` without type arguments. They weaken checking and can move failures into runtime casts.

---

**12. Iterators: a cursor that moves through elements**

An **iterator** provides a controlled way to visit elements.

```text
[Asha] [Ravi] [Maya]
   ↑
 current traversal position
```

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();
    System.out.println(name);
}
```

For a collection, enhanced `for` uses iterator-style traversal:

```java
for (String name : names) {
    System.out.println(name);
}
```

**Removing while iterating**

Avoid structurally modifying an `ArrayList` directly inside enhanced `for`:

```java
for (String name : names) {
    if (name.equals("Ravi")) {
        names.remove(name); // Unsafe iteration pattern
    }
}
```

It can throw `ConcurrentModificationException`, even with only one thread.

Use the iterator’s removal operation:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    if (iterator.next().equals("Ravi")) {
        iterator.remove();
    }
}
```

Or, when supported:

```java
names.removeIf(name -> name.equals("Ravi"));
```

Here, `name -> ...` is a **lambda**: a short function that tests each element.

Fail-fast exceptions are a bug-detection aid, not a guaranteed way to enforce thread safety. Also, not every collection or iterator supports removal.

**Memory cue: “If the iterator is walking, let the iterator remove.”**

---

**13. `Comparable` versus `Comparator`**

Both define ordering, but the rule lives in different places.

> **Comparable: “I know my natural order.”**  
> **Comparator: “This separate rule compares two objects.”**

| Feature | `Comparable<T>` | `Comparator<T>` |
|---|---|---|
| Method | `compareTo(other)` | `compare(first, second)` |
| Rule location | In the class itself | Separate object/function |
| Purpose | Natural ordering | Alternative orderings |

Both return:

```text
Negative → first comes before second
Zero     → tied in this ordering
Positive → first comes after second
```

The result need not be exactly `-1`, `0`, or `1`.

`String` and `Integer` already implement natural ordering:

```java
List<Integer> scores =
        new ArrayList<>(List.of(90, 70, 80));

scores.sort(Comparator.naturalOrder());

System.out.println(scores); // [70, 80, 90]
```

Reverse it:

```java
scores.sort(Comparator.reverseOrder());
```

Expected order:

```text
[90, 80, 70]
```

**Different rules for the same names**

```java
List<String> names =
        new ArrayList<>(List.of("Ravi", "Bo", "Asha"));

names.sort(
        Comparator.comparingInt(String::length)
                  .thenComparing(Comparator.naturalOrder())
);

System.out.println(names);
```

Expected output:

```text
[Bo, Asha, Ravi]
```

`String::length` means “use each string’s length.”

The rule is:

1. Shorter names first.
2. For equal lengths, natural string order.

Natural string order is not the same as culturally appropriate dictionary order for every language.

For a custom class, natural ordering can be implemented with a method such as:

```java
@Override
public int compareTo(Student other) {
    return Integer.compare(this.id, other.id);
}
```

Use `Integer.compare`, not subtraction:

```java
return this.id - other.id; // Can overflow
```

For sorted sets and maps, comparison returning zero also determines whether elements or keys are treated as duplicates. Choose tie-breakers deliberately.

---

**14. One complete collections-and-sorting example**

```java
import java.util.*;

public class CollectionsDemo {
    public static void main(String[] args) {
        List<String> registrations =
                new ArrayList<>(List.of("Ravi", "Asha", "Ravi", "Bo"));

        Set<String> uniqueStudents =
                new LinkedHashSet<>(registrations);

        Map<String, Integer> scores = new LinkedHashMap<>();
        scores.put("Ravi", 80);
        scores.put("Asha", 95);
        scores.put("Bo", 80);

        Queue<String> waitingLine =
                new ArrayDeque<>(uniqueStudents);

        Deque<String> recentActions = new ArrayDeque<>();
        recentActions.push("Opened register");
        recentActions.push("Added scores");

        List<String> ranked = new ArrayList<>(uniqueStudents);
        ranked.sort(
                Comparator.<String>comparingInt(name -> scores.get(name))
                          .reversed()
                          .thenComparing(Comparator.naturalOrder())
        );

        System.out.println("Registrations: " + registrations);
        System.out.println("Unique: " + uniqueStudents);
        System.out.println("Scores: " + scores);
        System.out.println("Next student: " + waitingLine.poll());
        System.out.println("Latest action: " + recentActions.pop());
        System.out.println("Ranked: " + ranked);
    }
}
```

Expected output:

```text
Registrations: [Ravi, Asha, Ravi, Bo]
Unique: [Ravi, Asha, Bo]
Scores: {Ravi=80, Asha=95, Bo=80}
Next student: Ravi
Latest action: Added scores
Ranked: [Asha, Bo, Ravi]
```

The ranking sorts by descending score, then ascending name.

Every name has a score in this example. If a name were missing, `scores.get(name)` would return null, and unboxing it for the numeric comparison would throw `NullPointerException`.

---

**15. The practical selection and cost card**

Let `n` be the collection size. Assume constant-cost comparisons and well-behaved hashing where indicated.

| Structure | Main operation | Typical cost |
|---|---|---:|
| `ArrayList` | Indexed get | O(1) |
| `ArrayList` | Append | Amortized O(1) |
| `ArrayList` | Search or middle insertion/removal | O(n) |
| `LinkedList` | Indexed get | O(n) |
| `HashSet` / `HashMap` | Basic lookup/update | Expected O(1), assuming suitable hashing |
| `TreeSet` / `TreeMap` | Lookup/update | O(log n) |
| `ArrayDeque` | Add/remove at an end | Amortized O(1) |
| `PriorityQueue` | Offer/poll | O(log n) |
| `PriorityQueue` | Peek | O(1) |
| `ArrayList` sorting | General comparison sort | O(n log n) worst-case time; allow O(n) auxiliary space |

String comparisons and user-defined hash functions may themselves take nonconstant time. Include that cost when analyzing a real algorithm.

`List.sort` rearranges the list; it does not return a new sorted list. Its sort is stable: elements tied by the comparator retain their relative order.

**Three final pitfalls**

- `List.of(...)` is unmodifiable and rejects null. Wrap it in `new ArrayList<>(...)` when you need to change or sort it.
- Ordinary `ArrayList`, `HashMap`, and `ArrayDeque` are not automatically safe for concurrent mutation.
- Storing an object in a collection usually stores its reference, not a deep copy.

**Practice**

1. Choose a collection for unique email addresses while preserving arrival order.
2. Choose a collection for looking up a student by ID.
3. Choose a collection for repeatedly serving the smallest-priority number.
4. Explain why `List<Integer>` cannot be assigned to `List<Number>`.
5. Complete the mnemonic: **Producer ___, Consumer ___**.
6. Write a program that removes duplicate names, then prints them alphabetically.

**Revision line:** **Choose the container by behavior, use generics for type safety, use PECS for flexible method boundaries, and separate natural ordering from alternative comparison rules.**

