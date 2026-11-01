# Java 21 — Foundations and Practical Learning

A complete collection of the course lessons covered in this chat, including the learning roadmap, worked examples, execution diagrams, expected outputs, exercises, and revision notes.

**Baseline:** Java 21, without preview features. **Status:** Roadmap extends into advanced topics; only the lessons reproduced below have been taught. Hexadecimal was intentionally skipped. Examples are educational; expected output is not a claim of execution. Historical lesson progress notes are preserved.

## Contents

- [Roadmap and computer foundations](#lesson-1)
- [Bits, bytes, and binary](#lesson-2)
- [Signed integers and overflow](#lesson-3)
- [Floating-point numbers and rounding](#lesson-4)
- [Unicode and text encodings](#lesson-5)
- [Working with your computer](#lesson-6)
- [Java execution and toolkit](#lesson-7)
- [Values, decisions, methods, and memory](#lesson-8)
- [Arrays, text, and shared objects](#lesson-9)
- [Mutation and equality quick reference](#lesson-10)

<a id="lesson-1"></a>

## 1. Roadmap and computer foundations

We’ll use **Java 21** for this course. Examples will avoid preview features unless explicitly labeled. Java 21 is an LTS release—“long-term support”—although update availability and support terms depend on your JDK provider. [Oracle’s support roadmap](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)

We’ll work toward four outcomes: writing Java confidently, explaining how programs execute, debugging problems systematically, and answering interview questions through understanding.

This response gives you the complete roadmap and **Lesson 1**. After that, we’ll take one manageable lesson at a time. Say **“next”** when you’re ready to continue.

**The learning roadmap**

Each numbered module contains several lessons. The levels mean:

- **Essential:** the foundation needed to write and understand everyday programs.
- **Intermediate:** deeper knowledge needed for reliable applications.
- **Advanced:** runtime behavior, concurrency, diagnostics, and performance.

Read the modules in order. We’ll introduce testing and debugging early, then deepen them as your programs grow.

| Module | Level | What we’ll learn |
|---|---|---|
| **1. Computer foundations** | Essential | CPU, cores, RAM, storage, and how they cooperate; instructions and data; what happens when an application starts and finishes. |
| **2. Representing information** | Essential → Intermediate | Bits, bytes, binary, hexadecimal; signed integers, overflow, floating-point representation and precision; Unicode, characters, UTF-8 versus UTF-16. |
| **3. Working with the operating system** | Essential | Files, directories, paths, terminals, commands, environment variables; operating systems, processes, threads, scheduling, and context switching. |
| **4. From source code to execution** | Essential | Source code, machine code, compilation, interpretation; Java language versus Java SE; JVM, JDK, and the role historically associated with the JRE; `.java` → `.class` → execution. |
| **5. Your Java toolkit** | Essential | `java`, `javac`, JShell, an IDE, and a debugger; Java 21 program startup and `main`; reading compiler errors, exceptions, and stack traces. |
| **6. Values and calculations** | Essential | Variables, initialization, primitive and reference types, literals, operators, precedence, conversions, casting, numeric promotion, and readable naming. |
| **7. Decisions and repetition** | Essential | Conditions, loops, tracing execution, boundary cases, and basic input validation; small tests and debugging exercises. |
| **8. Methods and execution memory** | Essential → Intermediate | Parameters, return values, scope and lifetime; conceptual addresses and references; call stacks and frames; recursion; stack versus heap; managed versus manual memory; stack overflow versus heap exhaustion. |
| **9. Arrays, text, and shared objects** | Essential | Arrays and multidimensional arrays; strings and `StringBuilder`; `null`, aliasing, mutation versus reassignment, identity versus equality; Java’s pass-by-value behavior; wrappers, boxing, and unboxing pitfalls. |
| **10. Object-oriented Java** | Essential → Intermediate | Classes, objects, fields, constructors, methods, `this`, `super`; access modifiers, `static`, `final`, and initialization order; encapsulation, abstraction, inheritance, polymorphism; interfaces versus abstract classes; composition; overloading versus overriding and runtime dispatch. |
| **11. Reliable object design** | Intermediate | `Object`, `equals`, `hashCode`, `toString`; immutability, defensive copying, shallow versus deep copying; enums, nested classes, records, and sealed types; design principles and patterns introduced through concrete problems. |
| **12. Exceptions and resource safety** | Essential → Intermediate | Checked and unchecked exceptions, useful error messages, exception propagation, try-with-resources; distinguishing variable scope, object reachability, and resource lifetime. |
| **13. Generics and collections** | Intermediate | Generic types, bounds, wildcards, invariance, and erasure; `List`, `Set`, `Map`, `Queue`, `Deque`; major implementations and selection criteria; iterators, `Comparable`, `Comparator`, and sorting. |
| **14. Data structures and algorithms** | Essential → Intermediate | Deriving Big O; arrays, linked structures, stacks, queues, hash tables, heaps, and trees; searching, sorting, recursion, and problem-solving patterns; connections to Java collections. A **heap data structure** is distinct from **JVM heap memory**. |
| **15. Functional Java and core APIs** | Intermediate | Lambdas, functional interfaces, method references, streams, and appropriate `Optional` usage; date/time APIs; `BigInteger`, `BigDecimal`, and numerical correctness. |
| **16. Files and application structure** | Intermediate | File I/O, buffering, NIO, encodings, resource management; packages, imports, classpath, JARs, and an introduction to modules; Maven, dependencies, project structure, packaging, and running an application. |
| **17. Practical development** | Essential → Intermediate | Unit and boundary testing, debugging, logging, Git; SQL foundations, JDBC, prepared statements, transactions; networking foundations before HTTP, client-server communication, and Java’s HTTP client; validation and avoiding hardcoded secrets. |
| **18. JVM execution and memory in depth** | Advanced | Runtime memory areas and thread ownership; frame local variables and operand stacks, heap, class metadata; class loading, linking, initialization, and loaders; object construction; instance versus static fields; string pooling and interning. |
| **19. Garbage collection and performance** | Advanced | GC roots, reachability, collection eligibility, retention leaks; strong, soft, weak, and phantom references; pauses, throughput, latency; JIT compilation, warmup, inlining, escape analysis; CPU caches, locality, virtual memory; heap dumps, thread dumps, GC logs, and profiling. |
| **20. Concurrency and the Java Memory Model** | Intermediate → Advanced | Concurrency versus parallelism; threads, tasks, executors, futures, `CompletableFuture`; races, atomicity, visibility, ordering, happens-before; `synchronized`, `volatile`, locks, atomics, safe publication, immutability, concurrent collections; deadlock, starvation, interruption, cancellation; Java 21 virtual threads and their workload limitations. |
| **21. Advanced integration and interview preparation** | Advanced | Annotations, reflection, serialization concerns; frameworks after their prerequisites; integrated projects, design tradeoffs, debugging investigations, and interview questions that test reasoning. |

In the JVM lessons, we will explicitly distinguish:

1. **Specification guarantees:** behavior a conforming Java implementation must preserve.
2. **Implementation details:** how a particular JVM, such as HotSpot, commonly works.
3. **Runtime-dependent behavior:** effects of version, settings, and optimization.

Memory diagrams will be conceptual models. They will not claim measured addresses, fixed object sizes, or mandatory physical allocation locations. The **Java Memory Model**, which governs interactions between threads, will be taught separately from diagrams of JVM memory areas.

**How we’ll practise**

Projects will grow with your understanding:

| Stage | Project | Main skills |
|---|---|---|
| Foundations | Unit converter and number analyser | Types, calculations, conditions, loops, methods |
| Objects | In-memory library catalogue | Classes, references, equality, collections |
| Reliable applications | File-backed expense tracker | Exceptions, resources, dates, `BigDecimal`, testing |
| Connected applications | Database-backed task manager with an HTTP integration | SQL, transactions, networking, dependencies |
| Advanced integration | Concurrent data-processing application | Task coordination, cancellation, thread safety, profiling |

Each lesson will explain the problem first, define unfamiliar words, develop intuition, then give precise behavior. Relevant lessons will include complete runnable Java examples, expected output, execution traces, common mistakes, edge cases, exercises, and revision notes.

For substantial problems, we’ll identify inputs, outputs, constraints, and edge cases before writing an algorithm. We’ll start with a simple correct solution, explain why it works, dry-run it, test it, then compare improvements.

Complexity analysis will define the input size and count the actual work. It will include copying, allocations, library operations, and recursion space where relevant; distinguish auxiliary space from output space; and state assumptions about expected, amortized, and worst-case costs.

The memory exercises will explicitly cover passing primitives and references, parameter reassignment versus object mutation, shared mutable objects, `==` versus `equals`, string literals versus constructed strings, shallow/deep copying, recursion, and collections retaining unwanted objects.

Practice answers will stay separate from worked examples. When you submit an answer, I’ll identify the specific misconception and update our progress checklist.

---

**Lesson 1 — What happens when a computer runs a program?**

**Goal:** Explain how the CPU, RAM, storage, and operating system work together.

This lesson teaches **general computer concepts**. The small Java example at the end connects them to the language; you do not need to understand all its syntax yet.

**1. The problem these parts solve**

Imagine a program that adds two numbers.

The computer needs somewhere to:

- Keep the program when it is not running.
- Hold the instructions and data being used.
- Perform the calculation.
- Coordinate the work and display the result.

Those jobs involve storage, RAM, the CPU, and the operating system.

**2. The essential vocabulary**

| Term | Simple meaning | More precise explanation |
|---|---|---|
| **Program** | Instructions describing work | Code that can be translated or otherwise processed into executable operations |
| **Data** | Information a program uses | Numbers, text, images, instructions represented as data, and other encoded information |
| **Storage** | Where files persist | Devices such as SSDs that retain data without ordinary operating power |
| **RAM** | Memory used during operation | Main memory holding accessible instructions and data; ordinary RAM loses its contents when power is removed |
| **CPU** | The processor that performs operations | Hardware that executes machine instructions |
| **Core** | An execution unit within a CPU | A processing core capable of executing an instruction stream; modern cores have additional internal complexity |
| **Operating system** | Software that manages the computer | Windows, for example, manages processes, memory, files, devices, and access to hardware |
| **Process** | An instance of a running program | An operating-system-managed execution environment with resources such as an address space |

A saved Java file is a file. It becomes part of a running computation only when appropriate tools process or execute it.

**3. An intuitive model**

Think of a person doing calculations at a desk:

| Analogy | Computer part |
|---|---|
| Filing cabinet containing instructions and records | Storage |
| Desk holding material currently in use | RAM |
| Person performing calculations | CPU |
| Office coordinator allocating work and equipment | Operating system |

The analogy helps explain different roles. It stops being accurate when taken literally: RAM does not understand its contents, the CPU does not reason like a person, and real computers move data through additional layers such as caches.

**4. Following a program through execution**

Suppose a program calculates `2 + 3` and displays the result.

| Stage | What happens conceptually |
|---|---|
| 1. Before launch | Program files are stored on an SSD or another storage device. |
| 2. Launch | The operating system creates a process and makes required code and data available in memory. Loading can happen incrementally. |
| 3. Execution | The CPU executes machine instructions that implement the program’s behavior. |
| 4. Calculation | The required arithmetic produces the value `5`. |
| 5. Output | Software passes the result through output facilities so that it appears in the terminal. |
| 6. Exit | The process finishes, and the operating system reclaims its process resources. |

This is a deliberately simplified execution model. Later we’ll add threads, virtual memory, caches, and Java’s runtime.

**An important distinction:** the CPU does not directly understand Java source text. Java tooling and the JVM participate in turning the program’s behavior into machine execution.

**5. A minimal Java example**

Save this as **`FirstProgram.java`**:

```java
public class FirstProgram {
    public static void main(String[] args) {
        System.out.println(2 + 3);
    }
}
```

From a terminal opened in the directory containing that file, run:

```powershell
javac FirstProgram.java
java FirstProgram
```

**Expected output—not executed here:**

```text
5
```

What the important lines mean:

| Code | Meaning for now |
|---|---|
| `public class FirstProgram` | Declares a class named `FirstProgram`; we’ll study classes later. |
| `public static void main(String[] args)` | Declares the conventional entry method used for this Java 21 program. |
| `2 + 3` | An expression: code that computes a value. Here, that value is `5`. |
| `System.out.println(...)` | Sends a value to standard output and ends the line. Standard output commonly appears in your terminal. |
| `{` and `}` | Group the code into blocks. |

You do not need to memorize `public`, `static`, or `String[]` today. Each will get its own explanation when its prerequisites are in place.

**Compilation and execution**

`javac FirstProgram.java` invokes the Java compiler. For this example, it checks the source and produces `FirstProgram.class`, containing Java bytecode.

`java FirstProgram` launches the Java runtime to load the class and invoke its entry method. The JVM implements the bytecode’s behavior using mechanisms that depend on the JVM implementation.

An optimizing implementation may simplify a constant calculation such as `2 + 3` before execution. The result is still `5`; we should not assume the physical CPU performs a separate addition operation for every expression written in source code.

**6. Common mistakes and their corrections**

| Mistake | Why it is wrong | Correction |
|---|---|---|
| “Saving a program runs it.” | Saving writes a file; it does not necessarily launch execution. | Compile and launch it using the appropriate tools. |
| “RAM and storage are the same.” | They serve different roles and have different persistence properties. | Think of RAM as working memory and storage as persistent file storage. |
| “More RAM always makes calculations faster.” | RAM capacity does not directly determine CPU arithmetic speed. | More RAM helps when memory capacity is the bottleneck. |
| “Four cores make every program four times faster.” | Work may be sequential or require coordination. | Multiple cores help when useful work can execute in parallel. |
| “The CPU reads Java source code directly.” | Java source is not the CPU’s machine instruction language. | Trace the compiler and runtime stages. |

Two useful edge cases:

- **Closing the terminal:** saving your source file does not save all temporary state from a running program.
- **Losing power:** unsaved values held only in ordinary RAM are lost; previously saved files generally persist.

**7. When this knowledge helps**

These distinctions help you ask better debugging questions:

- Is a calculation taking too long?
- Is the program using too much working memory?
- Is it waiting for storage or a network response?
- Did it fail before execution, during execution, or while saving output?

A slow application alone does not tell you which resource is responsible. Later, we’ll learn how to collect evidence.

Big O analysis adds little to this fixed calculation, so we will introduce it when programs process inputs whose sizes can grow.

**8. Comprehension check**

Answer in your own words:

1. How do RAM and storage differ?
2. What does the CPU execute?
3. Why doesn’t doubling the number of cores guarantee half the running time?
4. Is a saved `.java` file itself a running process?

**9. Coding exercise — your turn**

Modify `FirstProgram` to print the results of these calculations, one per line:

- `7 + 5`
- `7 - 5`
- `7 * 5`

Before running it:

1. Predict all three output lines.
2. Compile the program.
3. Run it.
4. Compare the actual output with your prediction.

If a command fails, paste the exact command and error text. If Java is not installed or available in your terminal yet, you can still submit your predictions.

**Revision note:** Storage keeps files; RAM holds working instructions and data; the CPU executes machine instructions; the operating system coordinates resources. Java source passes through compilation and runtime stages before its behavior becomes machine execution.

**Our current checklist**

- [x] Java version selected: **21**
- [x] Complete roadmap established
- [x] Lesson 1 introduced
- [ ] Lesson 1 understanding confirmed through your answers
- [ ] Weak topics identified from practice
- [ ] Remaining lessons

Send your answers and exercise when ready, or say **“next”** to continue with bits, bytes, and binary.

---

<a id="lesson-2"></a>

## 2. Bits, bytes, and binary

**Lesson 2 — Bits, bytes, and binary**

A computer must represent numbers, text, images, and instructions using physical hardware. **Bits provide the basic building blocks for those representations.**

Today’s goal is to understand bits and bytes, read small binary numbers, and connect them to Java. These are general computer concepts; the Java behavior is identified separately.

**1. What is a bit?**

A **bit** is a unit of information with two possible values:

```text
0
1
```

Hardware represents these values through distinguishable physical states. For example, electronic circuits can use different voltage ranges.

A light switch is a useful analogy: off or on. Its limit is that a bit is an abstract value, not necessarily a literal switch.

One bit provides two possibilities. Additional bits let us represent more combinations:

| Number of bits | Possible combinations | Count |
|---|---|---:|
| 1 | `0`, `1` | 2 |
| 2 | `00`, `01`, `10`, `11` | 4 |
| 3 | `000` through `111` | 8 |
| 4 | `0000` through `1111` | 16 |

Each added bit doubles the number of combinations:

```text
n bits → 2ⁿ distinct combinations
```

A **byte** contains **8 bits**, giving:

```text
2⁸ = 256 distinct combinations
```

That means 256 possible patterns—not automatically the numbers 1 through 256. The interpretation determines what each pattern means.

**2. Why binary place values work**

Our usual number system is **decimal**, or base 10. Each position is worth ten times the position to its right.

For example:

```text
347 = 3 × 100 + 4 × 10 + 7 × 1
```

**Binary** is base 2. It uses only `0` and `1`, and each position is worth twice the position to its right.

Consider the binary number `1101`:

| Position value | 8 | 4 | 2 | 1 |
|---|---:|---:|---:|---:|
| Binary digit | 1 | 1 | 0 | 1 |
| Contribution | 8 | 4 | 0 | 1 |

Add the contributions:

```text
1101 in binary = 8 + 4 + 1 = 13 in decimal
```

Read each `1` as “include this position’s value.”

Leading zeros do not change the value:

```text
1101 = 00001101
```

The second representation simply shows all eight positions of a byte.

**3. Converting decimal to binary**

Let’s represent decimal **22** using eight bits.

Start with the largest position value and decide whether to include it.

| Position value | Include it? | Amount remaining |
|---:|---|---:|
| 128 | No | 22 |
| 64 | No | 22 |
| 32 | No | 22 |
| 16 | Yes | 6 |
| 8 | No | 6 |
| 4 | Yes | 2 |
| 2 | Yes | 0 |
| 1 | No | 0 |

Therefore:

```text
22 = 16 + 4 + 2
22 in binary = 00010110
```

Check by converting back:

```text
0×128 + 0×64 + 0×32 + 1×16 + 0×8 + 1×4 + 1×2 + 0×1
= 22
```

We’ll automate this conversion after learning loops. For now, understanding the positions matters more than writing the algorithm.

**4. The same bits can have different meanings**

A bit pattern does not identify its own meaning.

For example:

```text
01000001
```

Depending on the agreed representation, it could represent:

- The unsigned integer `65`.
- The letter `A` in ASCII and UTF-8.
- Part of an instruction, image, or larger value.

An **encoding** is a set of rules that connects information to representations. We’ll study text encodings separately.

Think of the digits `2026`: they could mean a year, an item number, or a quantity. Context supplies the interpretation.

**5. Java example: different ways to write the same number**

A **literal** is a value written directly in source code.

In Java, a whole-number literal starting with `0b` is binary.

Save this as `BinaryBasics.java`:

```java
public class BinaryBasics {
    public static void main(String[] args) {
        System.out.println(13);
        System.out.println(0b1101);
        System.out.println(0b0000_1101);
        System.out.println(Integer.toBinaryString(13));
    }
}
```

Compile and run:

```powershell
javac BinaryBasics.java
java BinaryBasics
```

**Expected output—not executed here:**

```text
13
13
13
1101
```

The important statements:

| Code | Explanation |
|---|---|
| `13` | A decimal integer literal |
| `0b1101` | A binary integer literal with the same value |
| `0b0000_1101` | The same binary value; the underscore improves readability |
| `Integer.toBinaryString(13)` | Produces text showing the binary representation of positive integer `13` |

The compiler recognizes the literal’s notation. The decimal and binary literals above describe the **same integer value**.

Printing an integer normally uses decimal notation. To display its binary digits, this example explicitly requests a binary text representation.

`Integer.toBinaryString` omits leading zeros for positive values. It does not promise an eight-character result.

For now, treat it as a library method—a named operation supplied by Java. We’ll explain classes, methods, and returned text in later lessons.

**6. What eight bits can represent**

For an **unsigned integer**, all bits contribute to a nonnegative value.

Eight bits give this range:

```text
Minimum: 00000000 = 0
Maximum: 11111111 = 255
```

There are 256 values because zero counts as one possibility.

**Java-specific distinction:** Java’s `byte` type is a **signed** eight-bit integer. Its range is:

```text
-128 through 127
```

That is also 256 possible values. Java uses a representation called **two’s complement**, which we’ll explain in the signed-integer lesson.

For now, remember:

> Eight bits provide 256 patterns. The interpretation determines their values.

**7. Common mistakes and edge cases**

| Mistake | Why it fails | Correction |
|---|---|---|
| “A byte holds a maximum value of 256.” | There are 256 patterns; unsigned counting starts at zero. | The largest unsigned eight-bit value is 255. |
| “`1101` is automatically binary in Java.” | Without a prefix, Java reads this literal as decimal. | Write `0b1101`. |
| “`0b102` is valid.” | Binary allows only digits `0` and `1`. | Convert the intended value using valid binary digits. |
| “Leading zeros always have no effect in Java source.” | A leading `0` on a multi-digit integer literal can indicate **octal**, or base 8. | Use ordinary decimal notation or an explicit `0b` prefix. |
| “A bit pattern tells us whether it is text or a number.” | Meaning comes from the representation rules. | Identify the type or encoding. |

For example, Java interprets `010` as octal, whose decimal value is **8**. Avoid leading zeros on decimal integer literals.

Two boundary cases:

- Binary `0` represents zero.
- Binary `11111111` represents 255 **when interpreted as unsigned**; Java’s signed `byte` interpretation of that pattern is −1.

**8. When binary is useful**

You’ll use this understanding when learning:

- Integer ranges and overflow.
- File sizes and memory capacity.
- Character encodings.
- Network formats.
- Bit flags and bitwise operations.

For everyday calculations, decimal literals are usually easier to read. Binary notation helps when individual bit positions matter.

**9. Comprehension check**

Answer without running code first:

1. How many distinct patterns can **5 bits** represent?
2. What decimal value does binary `1010` represent?
3. Why can eight bits represent both unsigned values `0` through `255` and signed values `-128` through `127`?
4. What will Java print for `System.out.println(0b1001);`?

**10. Coding exercise**

Write a complete program named `BinaryPractice` that:

1. Prints binary literals representing decimal **6**, **10**, and **22**.
2. Prints the binary text representation of decimal **19**, using `Integer.toBinaryString`.
3. Prints `0b0011` and `0b11` on separate lines.

Predict every output line before running it. Then explain why the final two lines match.

**Revision note:** A bit has two possible values; a byte contains eight bits. Binary position values double from right to left. A group of `n` bits has `2ⁿ` patterns, and the encoding determines what those patterns mean.

**Progress:** Java 21 selected; Lessons 1 and 2 introduced. Practice understanding is still awaiting your answers. Next: **hexadecimal and how it relates to binary**.

---

<a id="lesson-3"></a>

## 3. Signed integers and overflow

We’ll mark **hexadecimal as skipped** and move to **signed integers and overflow**.

**Lesson 3 — Integer ranges and overflow**

**Goal:** Understand why Java integers have limits and what happens when a calculation exceeds them.

This builds on one idea from Lesson 2: **a fixed number of bits provides a fixed number of patterns.**

**1. The problem: representing negative numbers**

Programs need negative values for temperatures, account balances, and differences.

A **signed integer** represents whole numbers that can be negative, zero, or positive.

Java provides these primitive integer types. A **primitive type** is a built-in type for a basic value.

| Java type | Width | Minimum | Maximum |
|---|---:|---:|---:|
| `byte` | 8 bits | −128 | 127 |
| `short` | 16 bits | −32,768 | 32,767 |
| `int` | 32 bits | −2³¹ | 2³¹ − 1 |
| `long` | 64 bits | −2⁶³ | 2⁶³ − 1 |

These widths and ranges are **Java guarantees**, independent of whether your computer has a 32-bit or 64-bit processor.

For most ordinary whole-number calculations, start with `int`. Use `long` when the required range is larger.

**2. How signed values fit into bits**

Java’s signed integer types use **two’s complement**.

An intuitive model is a circular counter: after the largest value, the next value wraps around to the smallest.

The analogy has a limit: the computer does not necessarily contain a physical counter. It describes the result of fixed-width integer arithmetic.

For an eight-bit two’s-complement value, the position values are:

| Position | Leftmost | | | | | | | Rightmost |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Weight | **−128** | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

The leftmost bit has a **negative weight**.

For example:

```text
00000101 = 4 + 1 = 5

11111111 = -128 + 64 + 32 + 16 + 8 + 4 + 2 + 1
         = -1

10000000 = -128

01111111 = 127
```

The leftmost bit indicates whether the signed value is negative, but you cannot simply remove it and read the remaining bits as the magnitude.

For example, `10000001` represents **−127**, not −1.

There are 128 negative values, one zero, and 127 positive values:

```text
128 + 1 + 127 = 256 values
```

That explains the asymmetric range.

**3. What is overflow?**

**Integer overflow** occurs when the mathematical result falls outside the range of the integer type used for the calculation.

For Java `int` addition:

```text
2,147,483,647 + 1
```

The mathematical answer is:

```text
2,147,483,648
```

But that is too large for `int`. Java’s ordinary integer addition wraps around, producing:

```text
-2,147,483,648
```

**Ordinary integer addition does not automatically throw an exception on overflow.**

**4. A complete Java example**

Save this as `IntegerLimits.java`:

```java
public class IntegerLimits {
    public static void main(String[] args) {
        int largest = Integer.MAX_VALUE;

        System.out.println(largest);
        System.out.println(largest + 1);

        long widerResult = (long) largest + 1;
        System.out.println(widerResult);

        long tooLate = largest + 1;
        System.out.println(tooLate);
    }
}
```

Run:

```powershell
javac IntegerLimits.java
java IntegerLimits
```

**Expected output—not executed here:**

```text
2147483647
-2147483648
2147483648
-2147483648
```

The important statements:

- `int largest = ...` declares a variable—a named place for a value—and initializes it with the largest `int`.
- `Integer.MAX_VALUE` supplies that maximum value.
- `(long) largest` is a **cast**: an explicit conversion to another type.
- `long widerResult` can hold a larger range than `int`.

**5. Why the last result is still negative**

The type of the destination variable does not determine the type of an earlier calculation.

| Expression | How Java evaluates it | Result |
|---|---|---:|
| `largest + 1` | Both operands are `int`; addition uses `int` | −2,147,483,648 |
| `(long) largest + 1` | One operand is `long`; addition uses `long` | 2,147,483,648 |
| Assign `largest + 1` to `long` | Perform `int` addition first, then convert its result | −2,147,483,648 |

An **operand** is a value an operator acts on. In `a + b`, `a` and `b` are operands.

Execution of the last assignment is conceptually:

```text
largest + 1
     ↓ int addition overflows
-2147483648
     ↓ conversion to long preserves this value
-2147483648
```

Converting the result afterward cannot recover the intended mathematical answer.

Another correct way to make this addition use `long` is:

```java
long result = largest + 1L;
```

The suffix `L` makes `1L` a `long` literal.

**6. Detecting overflow when it must be reported**

If an incorrect wrapped result would be unacceptable, Java provides checked arithmetic methods:

```java
public class CheckedAddition {
    public static void main(String[] args) {
        System.out.println(Math.addExact(20, 30));

        System.out.println(Math.addExact(Integer.MAX_VALUE, 1));
    }
}
```

**Expected behavior—not executed here:**

```text
50
```

Then the second call throws an `ArithmeticException`, indicating integer overflow. The exact stack-trace details depend on the runtime and source line numbers.

An **exception** signals that execution cannot continue normally at that operation. We’ll learn how to handle exceptions later.

**7. Common mistakes and corrections**

| Mistake | Why it fails | Correction |
|---|---|---|
| “Java makes an `int` wider when needed.” | Its width remains 32 bits. | Choose a suitable type before calculating. |
| “Assigning to `long` prevents overflow.” | The calculation may already have used `int`. | Convert an operand before the operation. |
| “Using `long` removes all limits.” | `long` also has a fixed range. | Check bounds, use checked arithmetic, or later use `BigInteger`. |
| “Every negative result means overflow.” | Negative numbers can be valid results. | Compare the result with the operation and input constraints. |

One additional Java-specific trap:

```java
byte result = 100 + 20;
```

This compiles because the compiler can evaluate this constant expression and verify that `120` fits in `byte`.

However:

```java
byte first = 100;
byte second = 20;
byte result = first + second;
```

This does **not** compile: addition promotes these `byte` operands to `int`, and Java will not implicitly narrow that result back to `byte`.

We’ll revisit numeric promotion systematically later. For now, use `int` for ordinary arithmetic.

**8. Edge cases worth remembering**

- Adding `1` to `Integer.MAX_VALUE` wraps to `Integer.MIN_VALUE`.
- Subtracting `1` from `Integer.MIN_VALUE` wraps to `Integer.MAX_VALUE`.
- Multiplication can overflow too.
- Negating `Integer.MIN_VALUE` still produces `Integer.MIN_VALUE`: its positive counterpart does not fit in `int`.

Use `Math.addExact`, `Math.subtractExact`, or `Math.multiplyExact` when the relevant overflow must cause an exception. For integers beyond `long`’s range, we’ll later study `BigInteger`.

**9. Comprehension check**

1. Why does an eight-bit signed integer have a maximum of 127 rather than 128?
2. Why can `long result = a + b;` still contain an overflowed result when `a` and `b` are `int`?
3. What does converting an operand to `long` **before** addition change?

**10. Coding exercise**

A shop sells **50,000 items**, each costing **50,000 units**.

Write a complete program that:

1. Stores both inputs in `int` variables.
2. Calculates their product into a `long` variable.
3. Ensures the multiplication itself uses `long`.
4. Prints the total.

Predict the result before running it. Then explain where you performed the conversion and why.

**Revision note:** Fixed-width integers have limited ranges. Java’s ordinary integer arithmetic can wrap on overflow. Use a sufficiently wide type **before the calculation**, or checked arithmetic when overflow must be reported.

**Progress:** Hexadecimal skipped. Signed integers and overflow introduced; understanding awaits practice. Next: **floating-point numbers and why `0.1 + 0.2` can produce a surprising result**.

---

<a id="lesson-4"></a>

## 4. Floating-point numbers and rounding

**Lesson 4 — Floating-point numbers and rounding**

Integers work for counting items, but programs also need values such as `1.75` metres or `36.6` degrees. Java provides **`float` and `double`** for calculations involving fractions.

The key idea: **these types represent many values approximately.** Understanding that approximation helps you avoid incorrect calculations and comparisons.

**1. Why some fractions need rounding**

In decimal, one-third has an endless representation:

```text
1 ÷ 3 = 0.333333333...
```

If you have room for only six digits after the decimal point, you might store:

```text
0.333333
```

That is close to one-third, but it is not exactly one-third.

Binary has a similar limitation. Its fractional positions represent halves, quarters, eighths, and so on:

| Binary position after the point | Value |
|---|---:|
| First | 1/2 = 0.5 |
| Second | 1/4 = 0.25 |
| Third | 1/8 = 0.125 |
| Fourth | 1/16 = 0.0625 |

Therefore:

```text
Binary 0.1  = decimal 0.5
Binary 0.01 = decimal 0.25
Binary 0.11 = decimal 0.75
```

But decimal `0.1` needs an endless sequence of binary fractional digits:

```text
Decimal 0.1 = binary 0.0001100110011...
```

A fixed-size type cannot keep an infinite sequence. It stores a nearby representable value.

**Some fractions, such as `0.5` and `0.25`, are exact in binary. Others, such as `0.1`, are not.**

**2. What “floating point” means**

Think of scientific notation:

```text
12345 = 1.2345 × 10⁴
```

It separates a value into significant digits and a scale.

Floating-point representation uses a similar idea, with powers of **2**. Its main components are:

- **Sign:** positive or negative.
- **Significand:** the significant binary digits.
- **Exponent:** the power of two that scales those digits.

This allows a wide range of magnitudes with a limited number of significant digits. It does **not** reserve a fixed number of digits after the decimal point.

Java defines `float` using the 32-bit IEEE 754 binary32 format and `double` using the 64-bit binary64 format. These are language guarantees. [Java 21 specification](https://docs.oracle.com/javase/specs/jls/se21/html/jls-4.html#jls-4.2.3)

| Type | Approximate significant decimal precision | Example |
|---|---:|---|
| `float` | About 6–7 digits | `float temperature = 36.6f;` |
| `double` | About 15–16 digits | `double temperature = 36.6;` |

Those precision figures are rough guides, not promises of exact decimal storage.

For ordinary approximate calculations, **use `double` by default**. A decimal-point literal such as `36.6` has type `double`; the suffix `f` requests `float`.

**3. A complete Java example**

Save this as `FloatingPointBasics.java`:

```java
public class FloatingPointBasics {
    public static void main(String[] args) {
        double first = 0.1;
        double second = 0.2;
        double sum = first + second;

        System.out.println(sum);
        System.out.println(sum == 0.3);
        System.out.println(0.5 + 0.25);

        double integerDivision = 5 / 2;
        double fractionalDivision = 5.0 / 2;

        System.out.println(integerDivision);
        System.out.println(fractionalDivision);
    }
}
```

Compile and run:

```powershell
javac FloatingPointBasics.java
java FloatingPointBasics
```

**Expected output—not executed here:**

```text
0.30000000000000004
false
0.75
2.0
2.5
```

Let’s trace the important operations:

| Statement or expression | What happens |
|---|---|
| `double first = 0.1;` | Stores the `double` approximation of decimal `0.1`. |
| `first + second` | Adds the stored values and rounds the result to a representable `double`. |
| `sum == 0.3` | Checks exact numerical equality with the `double` approximation of decimal `0.3`. They differ. |
| `0.5 + 0.25` | Both inputs and the result are exactly representable. |
| `5 / 2` | Both operands are integers, so integer division produces `2`. |
| Assign that `2` to `double` | Converts it to `2.0`; the discarded fraction is not recovered. |
| `5.0 / 2` | Uses floating-point division and produces `2.5`. |

This revisits the previous lesson: **the destination variable does not change how an earlier calculation is performed.**

During compilation, Java determines expression types and converts literals to their represented values. During execution, arithmetic follows those types. The example’s results do not depend on a particular physical memory layout.

**4. Comparing approximate results**

The operator `==` asks whether two numerical values are exactly equal. It does not ask whether they are “close enough.”

For approximate measurements, you may instead compare the difference against a suitable **tolerance**—the amount of difference you are willing to accept.

```java
public class ApproximateComparison {
    public static void main(String[] args) {
        double actual = 0.1 + 0.2;
        double expected = 0.3;
        double tolerance = 0.000000001;

        boolean closeEnough =
                Math.abs(actual - expected) <= tolerance;

        System.out.println(closeEnough);
    }
}
```

**Expected output—not executed here:**

```text
true
```

Here:

- `boolean` holds either `true` or `false`.
- `Math.abs(...)` gives the absolute value, so the difference is nonnegative.
- `<=` means “less than or equal to.”

**There is no universal tolerance.** It must fit the problem’s units, scale, and accuracy requirements. Large and small magnitudes may require a relative tolerance; we’ll revisit that later.

Exact equality remains useful when exact equality is what the problem requires.

**5. Range and precision are different**

**Range** describes how large or small a value can be. **Precision** describes how finely distinguishable nearby values are.

As floating-point magnitudes grow, the gaps between representable values generally grow too. Eventually, adding `1` may leave a large `double` unchanged.

This means a type can have enough range for a number while lacking enough precision for the calculation you need.

More bits improve the situation, but switching from `float` to `double` does not make every decimal fraction exact.

**6. Important edge cases**

Floating-point arithmetic includes special values. Unlike integer division by zero, floating-point division by zero can produce infinity or `NaN`, meaning “Not a Number.” Floating-point overflow can produce infinity, and sufficiently tiny results can round to zero. [Java 21 floating-point operations](https://docs.oracle.com/javase/specs/jls/se21/html/jls-4.html#jls-4.2.4)

| Expression | Result |
|---|---|
| `1.0 / 0.0` | `Infinity` |
| `-1.0 / 0.0` | `-Infinity` |
| `0.0 / 0.0` | `NaN` |
| `Double.NaN == Double.NaN` | `false` |

To test for `NaN`, use `Double.isNaN(value)`.

Java also has positive and negative floating-point zero. We’ll explore their significance when numerical edge cases become relevant.

**7. Choosing an appropriate representation**

| Requirement | Usual choice |
|---|---|
| Count whole items | `int` or `long` |
| Calculate approximate measurements | `double` |
| Work with an API requiring single precision | `float` |
| Preserve decimal amounts and apply explicit decimal rounding | `BigDecimal`, covered later |

For money, the representation must match the application’s rules. A whole-number count of minor units can work for a fixed scale; `BigDecimal` supports decimal calculations with explicit rounding decisions.

Formatting an approximate number to two decimal places changes its displayed text. It does not repair the stored value or earlier arithmetic.

**8. Comprehension check**

1. Why can binary floating point represent `0.5` exactly but not `0.1`?
2. Why does `double result = 5 / 2;` produce `2.0`?
3. Does switching to `double` make every decimal calculation exact?
4. Why should a tolerance depend on the problem?

**9. Coding exercise**

Write `AveragePractice` to calculate the average of these three integer scores:

```text
7, 8, 10
```

Your program should:

1. Store the scores in three `int` variables.
2. Calculate their total.
3. Divide using floating-point arithmetic.
4. Print the average.

Before running it, predict what would happen if you divided the total by `3` instead of `3.0`.

**Revision note:** `double` provides a wide range with finite precision. Some decimal fractions require approximation. Choose the arithmetic type before calculating, and choose comparison rules that fit the problem.

**Progress:** Lessons 1–4 introduced; hexadecimal skipped; practice answers pending. Next: **how computers represent text—Unicode, characters, UTF-8, and UTF-16**.

---

<a id="lesson-5"></a>

## 5. Unicode and text encodings

**Lesson 5 — How computers represent text**

A computer stores bits. To store a name, message, or emoji, it needs rules connecting text to numbers and those numbers to stored data.

Today’s goal is to distinguish **Unicode, UTF-8, UTF-16, and Java’s `char` and `String` types**. Hexadecimal remains skipped; we’ll use ordinary decimal numbers.

**1. Unicode gives characters identities**

Imagine two computers exchanging a message. If they disagree about what the stored numbers mean, the receiver may display incorrect text.

**Unicode** provides a shared standard for representing text from different writing systems. It assigns numerical identifiers called **code points**.

Examples:

| Text | Unicode code point, written in decimal |
|---|---:|
| `A` | 65 |
| `é` | 233 |
| `₹` | 8377 |
| `😀` | 128512 |

A code point identifies an element of text. It does not specify its font or exact appearance.

A useful analogy is a catalogue: each entry has an identifying number. The limit of the analogy is that several entries can combine to produce what a reader perceives as one character.

**2. Encoding turns those identities into stored data**

An **encoding** defines how text is represented using units of data.

Two important Unicode encodings are:

| Encoding | Basic unit | Units used for one Unicode scalar value* |
|---|---|---|
| UTF-8 | 8-bit unit: one byte | 1–4 bytes |
| UTF-16 | 16-bit unit | 1–2 units |

*A Unicode scalar value is a code point excluding a reserved range used for UTF-16 surrogate units. You do not need to memorize that range.*

Both encodings can represent all Unicode text. Their representations differ. [Unicode encoding FAQ](https://www.unicode.org/faq/utf_bom.html)

For these individual examples:

| Text | UTF-8 bytes | UTF-16 units |
|---|---:|---:|
| `A` | 1 | 1 |
| `é` | 2 | 1 |
| `₹` | 3 | 1 |
| `😀` | 4 | 2 |

**Encoding** converts text to a representation such as bytes. **Decoding** interprets that representation as text.

The reader must use the appropriate encoding to recover the intended text.

**3. What Java’s `char` actually holds**

A Java `char` holds **one 16-bit UTF-16 code unit**.

That is enough for `A`:

```java
char letter = 'A';
```

But the emoji `😀` requires two UTF-16 units, called a **surrogate pair**. It does not fit in a single `char`. Java provides code-point operations using `int` to handle the wider range. [Java 21 `Character` documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Character.html)

Use a `String` for text:

```java
String greeting = "Hello";
String emoji = "😀";
```

Notice the quotation marks:

- `'A'` is a `char` literal.
- `"A"` is a `String` literal.
- `"Hello"` is also a `String` literal.

A `String` is an object representing text. A variable of type `String` holds a **reference** to that object—a value through which the program can access it.

**4. A complete Java example**

Save this file as **UTF-8**, named `TextBasics.java`:

```java
import java.nio.charset.StandardCharsets;

public class TextBasics {
    public static void main(String[] args) {
        String text = "A😀";

        int units = text.length();
        int points = text.codePointCount(0, text.length());
        byte[] encoded = text.getBytes(StandardCharsets.UTF_8);

        System.out.println(units);
        System.out.println(points);
        System.out.println(encoded.length);
    }
}
```

Compile and run:

```powershell
javac -encoding UTF-8 TextBasics.java
java TextBasics
```

**Expected output—not executed here:**

```text
3
2
5
```

Here is why:

| Measurement | Calculation | Result |
|---|---|---:|
| UTF-16 units | `A` uses 1; `😀` uses 2 | 3 |
| Code points | `A` and `😀` | 2 |
| UTF-8 bytes | `A` uses 1; `😀` uses 4 | 5 |

The important statements:

- `import ...` lets us use the short name `StandardCharsets`, which supplies standard encoding definitions.
- `text.length()` returns the number of UTF-16 code units.
- `codePointCount(0, text.length())` counts code points from position zero up to, but excluding, the ending position.
- `getBytes(StandardCharsets.UTF_8)` creates a byte array containing the UTF-8 representation.
- `byte[]` means an array of byte values. An **array** groups values in an indexed sequence.
- Arrays use `.length`; strings use the method `.length()`.

Java’s string indexing follows UTF-16 units. That describes the API’s behavior; it does not establish a particular physical object layout. [Java 21 `String` documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html)

**5. Following the references and execution**

After the assignments, this conceptual diagram describes the relationships:

```text
main's current method frame
    text ───────────────► String S1: "A😀"
    units = 3
    points = 2
    encoded ────────────► Byte array B1: 5 elements
```

`S1` and `B1` are teaching labels, not memory addresses.

The program reads `S1` and creates `B1`; encoding does not modify the string. Each method call returns its result to `main`, which continues with the next statement.

When `main` returns, its local references disappear. If nothing else retains `B1`, the array becomes eligible for garbage collection; eligibility does not guarantee immediate collection. We should not infer the literal string’s reachability solely from the local `text` variable.

During compilation, `-encoding UTF-8` tells the compiler how to read the **source file**. During execution, `getBytes(UTF_8)` separately encodes the **string’s contents**. These are two distinct encoding steps.

**6. One visible character can contain several code points**

Consider an accented letter. It can sometimes be represented as:

```text
one precomposed accented letter
```

or:

```text
a base letter + a combining accent
```

They can look alike while containing different code-point sequences.

A **grapheme cluster** groups code points that generally behave as one user-perceived character. Some emoji sequences also contain several code points.

Consequently, these counts answer different questions:

- **Bytes:** how much encoded data will be stored or transmitted?
- **UTF-16 units:** how many Java string positions are used?
- **Code points:** how many Unicode code points occur?
- **Grapheme clusters:** how many user-perceived text units occur?

Even code-point counting is not a universal answer to “How many visible characters are there?”

**7. Common mistakes and edge cases**

| Mistake | Correction |
|---|---|
| “Every character uses one byte.” | Byte count depends on the text and encoding. |
| “Every Java `char` holds a complete character.” | A `char` holds one UTF-16 unit. |
| “`length()` gives the visible character count.” | It counts UTF-16 units. |
| “UTF-8 only supports English.” | It supports all Unicode text. |
| “A three-unit string occupies exactly six bytes of RAM.” | Encoding units do not determine the complete object’s physical memory usage. |

An empty string, `""`, has zero units, zero code points, and zero bytes when encoded as UTF-8.

A Java string can also contain an unpaired surrogate—a UTF-16 unit without its required partner. Malformed text needs explicit error-handling decisions when encoding or decoding; we’ll cover those during file I/O.

For files and network messages, follow the specified encoding. UTF-8 is a common choice when you control the format. For editor cursor movement or visible-character limits, grapheme boundaries matter.

**8. Comprehension check**

1. What is the difference between a code point and an encoding?
2. Why does `"😀".length()` return `2`?
3. Why can the number of UTF-8 bytes differ from a string’s length?
4. Does counting code points always count user-perceived characters?

**9. Coding exercise**

Adapt `TextBasics` to examine:

```java
String text = "A₹😀";
```

Before running it, predict:

1. Its UTF-16 unit count.
2. Its code-point count.
3. Its UTF-8 byte count.

Then repeat with an empty string.

**Revision note:** Unicode identifies text elements; encodings represent them as data. Java `char` values are UTF-16 units, and string length, code-point count, and encoded byte count can differ.

**Progress:** Lessons 1–5 introduced; hexadecimal skipped; practice understanding remains unverified. Next: **files, directories, paths, terminals, and commands**.

---

<a id="lesson-6"></a>

## 6. Working with your computer

We’ll cover the whole module in one connected lesson, using **Windows, PowerShell, and Java 21**.

The central question is:

> When you type `java MyProgram`, how does text in a file become work performed by the CPU?

We’ll build the answer from files and paths through processes, threads, and execution. All outputs below are **expected or illustrative; I have not executed these examples here**.

---

**1. Files and directories: how stored information is organized**

A **file** is a named collection of data managed by a file system. Examples include source code, photographs, documents, and compiled programs.

A file has content and associated information, such as its size, timestamps, and access permissions.

A **directory**, also called a folder, organizes named entries for files and other directories.

A **file system** provides the rules and structures for storing, naming, finding, and accessing these items.

Imagine this directory structure:

```text
JavaCourse
└── Lesson6
    ├── src
    │   └── ComputerDemo.java
    ├── out
    │   └── ComputerDemo.class
    └── data
        └── message.txt
```

Here:

- `JavaCourse`, `Lesson6`, `src`, `out`, and `data` are directories.
- `ComputerDemo.java` contains Java source text.
- `ComputerDemo.class` contains compiled Java bytecode and related information.
- `message.txt` contains text used by the program.

The indentation shows containment. It does not describe the physical arrangement of data on an SSD.

**A filename extension is a convention, not a conversion.**

Changing `photo.jpg` to `photo.txt` does not convert an image into readable text. Likewise, renaming `.java` to `.class` does not compile Java.

A **text file** contains data interpreted through a character encoding, such as UTF-8. A `.class` file uses a structured binary format. Both ultimately contain bytes.

**Common mistake:** saving source as `ComputerDemo.java.txt` because an editor adds `.txt`.

**Correction:** check the actual filename, including its extension.

---

**2. Paths: how you identify a file**

A **path** describes where to find an item.

Suppose the course directory is:

```text
C:\JavaCourse\Lesson6
```

The full location of the source file is:

```text
C:\JavaCourse\Lesson6\src\ComputerDemo.java
```

Here:

- `C:` identifies a Windows drive.
- `\` separates path components.
- The remaining components identify directories and the filename.

**Absolute paths**

A fully qualified absolute path identifies a location without needing your current directory:

```text
C:\JavaCourse\Lesson6\data\message.txt
```

Windows also supports network paths such as:

```text
\\server\share\folder\message.txt
```

**Relative paths**

A relative path is interpreted against a base location, usually the current working directory:

```text
data\message.txt
```

If your current directory is:

```text
C:\JavaCourse\Lesson6
```

that relative path identifies:

```text
C:\JavaCourse\Lesson6\data\message.txt
```

If your current directory changes, the same relative path can identify a different location.

Two special components are:

| Component | Meaning |
|---|---|
| `.` | Current directory |
| `..` | Parent directory |

Examples, assuming the current directory is `C:\JavaCourse\Lesson6`:

| Relative path | Resolved location |
|---|---|
| `.\data\message.txt` | `C:\JavaCourse\Lesson6\data\message.txt` |
| `.\src` | `C:\JavaCourse\Lesson6\src` |
| `..\notes.txt` | `C:\JavaCourse\notes.txt` |

A Windows subtlety: `C:notes.txt` is drive-relative; it is not the same as `C:\notes.txt`. For now, use clearly relative paths or fully qualified paths. [PowerShell path documentation](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_path_syntax)

**The most important Java consequence:**

> A relative file path is generally resolved from the program’s working directory, not automatically from the directory containing its `.java` or `.class` file.

This explains many “file not found” errors.

**Check yourself:** if the current directory is `C:\JavaCourse\Lesson6\src`, where does `..\data\message.txt` point?

---

**3. Terminal versus shell versus command**

These terms describe different parts of the interaction.

| Term | Role |
|---|---|
| **Terminal** | Provides the interface for typing input and displaying textual output |
| **Shell** | Interprets command syntax and coordinates commands |
| **Command** | An instruction you ask the shell to perform |
| **Argument** | Information supplied to a command |
| **Option** | An argument that changes a command’s behavior |

For example, Windows Terminal can host PowerShell.

PowerShell is the shell interpreting:

```powershell
javac -encoding UTF-8 ComputerDemo.java
```

The pieces are:

```text
javac                 command
-encoding UTF-8       option and its value
ComputerDemo.java     source-file argument
```

The terminal displays the interaction. PowerShell interprets the command. `javac` performs compilation.

**Not every command starts a separate program.**

PowerShell commands such as `Get-Location` are typically executed within the PowerShell process. An external executable such as `javac.exe` normally runs as a separate process.

**Useful PowerShell commands**

| Purpose | Command |
|---|---|
| Show current location | `Get-Location` |
| List directory entries | `Get-ChildItem` |
| Change directory | `Set-Location .\src` |
| Move to the parent directory | `Set-Location ..` |
| Read a text file | `Get-Content -LiteralPath .\data\message.txt` |
| Check whether a path exists | `Test-Path -LiteralPath .\data\message.txt` |
| Find the Java command | `Get-Command java` |

`cd` is a convenient PowerShell alias for `Set-Location`.

**Paths containing spaces**

Quote a path so the shell treats it as one argument:

```powershell
Set-Location 'C:\My Java Course'
```

To invoke an executable whose quoted path contains spaces, PowerShell uses the call operator `&`:

```powershell
& 'C:\Program Files\Java\jdk-21\bin\java.exe' -version
```

That installation path is illustrative; your JDK may be somewhere else.

**Output, errors, and exit status**

A command-line program commonly communicates through:

- **Standard input:** incoming data.
- **Standard output:** ordinary output.
- **Standard error:** diagnostic output.
- **Exit code:** a numeric completion status.

Java exposes the first three through `System.in`, `System.out`, and `System.err`.

After an external command finishes, PowerShell’s `$LASTEXITCODE` reports its exit code:

```powershell
javac ComputerDemo.java
$LASTEXITCODE
```

Zero conventionally means success. Nonzero usually signals failure or another documented outcome.

Successful compilation commonly prints nothing. Silence does not mean nothing happened—look for the generated class file and check the exit code.

---

**4. Environment variables: configuration inherited by programs**

An **environment variable** is a named value available in a process’s environment.

Examples include:

```text
PATH
JAVA_HOME
TEMP
```

These can supply executable search locations, tool configuration, or temporary-directory locations.

In PowerShell, inspect one using:

```powershell
$env:JAVA_HOME
```

Create a course-specific variable for the current shell:

```powershell
$env:JAVA_TUTOR_NAME = 'Learner'
```

An external program launched afterward normally inherits that value.

The inheritance relationship is conceptually:

```text
PowerShell process
    JAVA_TUTOR_NAME = Learner
             │
             │ starts child process
             ▼
Java process
    JAVA_TUTOR_NAME = Learner
```

Each process has its own environment. Updating the shell’s environment does not automatically update a Java process that is already running. Likewise, a child process does not ordinarily rewrite its parent’s environment. [PowerShell environment-variable documentation](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_Environment_Variables)

**Shell variables and environment variables differ**

```powershell
$courseName = 'Java'
```

creates an ordinary PowerShell variable.

```powershell
$env:COURSE_NAME = 'Java'
```

sets an environment variable that can be inherited by newly launched programs.

A Java local variable is different again:

```java
int count = 10;
```

It belongs to the Java program’s execution, not the environment-variable configuration.

**What `PATH` does**

When you enter:

```powershell
java -version
```

PowerShell must locate the command. External executable discovery uses the directories listed in `PATH`, subject to the shell’s command-resolution rules.

Inspect those directories on Windows:

```powershell
$env:PATH -split ';'
```

If several Java installations exist, the executable found may not be the one you intended.

Check:

```powershell
Get-Command java -All
Get-Command javac -All
java -version
javac -version
```

The compiler and runtime can come from different installations.

**What `JAVA_HOME` does**

`JAVA_HOME` conventionally points to the JDK root directory:

```text
C:\Program Files\Java\jdk-21
```

Tools that understand this convention can use it to find Java.

Setting `JAVA_HOME` alone does not guarantee that the shell’s `java` command changes. That command might still resolve through another `PATH` entry.

Keep these three names separate:

| Name | Purpose |
|---|---|
| `PATH` | Helps locate external executables |
| `JAVA_HOME` | Convention used by tools to identify a Java installation |
| Java classpath | Tells Java where to locate application classes and libraries |

---

**5. The operating system: coordinating and protecting resources**

An **operating system**, such as Windows, manages the computer’s resources and supplies services to applications.

Its responsibilities include:

| Resource or problem | Operating-system responsibility |
|---|---|
| CPU access | Schedule runnable threads |
| Memory | Manage address spaces, mappings, and protection |
| Files | Provide access to file systems |
| Devices | Coordinate access through device drivers |
| Security | Enforce permissions and access rules |
| Processes | Create, manage, and terminate execution environments |

The privileged core of the OS is called the **kernel**.

Applications usually execute in **user mode**, where access to protected resources is restricted. When they need services such as file access, they use interfaces that ultimately request work from the OS.

A **system call** is a controlled entry into an operating-system service.

For example:

```text
Java asks to read a file
        ↓
Java library/runtime support
        ↓
Operating-system file services
        ↓
File data becomes available to the program
```

The data might already be cached in RAM, so a file read does not always require a fresh SSD operation.

A useful analogy is a building manager allocating rooms and controlling access. The limitation is that many activities happen concurrently, and the OS is itself executable software.

---

**6. Program versus process**

A **program** is code and associated data describing work.

A **process** is an operating-system-managed instance of execution.

For example, launching the same Java application twice normally creates two processes:

```text
Stored class files
        │
        ├── launch → Java process A
        │
        └── launch → Java process B
```

Each process has resources such as an identifier, a virtual address space, an environment, and threads. Windows calls its process identifier a **PID**. [Microsoft’s process and thread overview](https://learn.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads)

**What is a virtual address space?**

It is the address space through which a process accesses memory.

The OS and hardware translate virtual addresses to backing memory. A process does not normally see every other process’s memory as directly accessible.

Two processes can therefore use the same numerical virtual address without referring to the same physical memory.

Processes can deliberately communicate or share memory using appropriate mechanisms. Ordinary Java references do not automatically connect objects across separate JVM processes.

**A process is not the same as a window**

- One application can use several processes.
- A background process may have no visible window.
- Several windows can belong to one process.

Task Manager gives you a useful view of processes, but the visible application layout is not a reliable process diagram.

---

**7. Threads: execution inside a process**

A **thread** is an execution flow within a process.

A process supplies resources; threads perform work using those resources.

Conceptually:

```text
Java process
├── Shared resources
│   ├── Java objects
│   ├── Loaded code and runtime data
│   └── Open resources
│
├── Main thread
│   └── Its current calls and execution state
│
└── Worker thread
    └── Its current calls and execution state
```

Each thread has its own execution context and call stack. Threads within a process can access shared objects, which makes communication convenient but also creates opportunities for conflicting updates.

A **call stack** tracks active method calls. A **stack frame** contains the state associated with one active call.

For example:

```text
main calls calculate
    calculate calls multiply
```

Conceptual active frames:

```text
Top:     multiply(...)
         calculate(...)
Bottom:  main(...)
```

When `multiply` returns, its frame is removed from the active call sequence and `calculate` resumes.

That is a method return, not a thread switch.

**Concurrency versus parallelism**

- **Concurrency:** several activities make progress over overlapping periods.
- **Parallelism:** activities execute simultaneously on different execution resources.

On one logical CPU, two runnable threads can take turns:

```text
Time →  A  A  B  B  A  B
```

On two logical CPUs, they may execute at the same time:

```text
CPU 1 → A  A  A
CPU 2 → B  B  B
```

A **logical CPU** is an execution resource exposed to the OS scheduler. Hardware multithreading can expose more logical CPUs than physical cores; that does not make every logical CPU equivalent to a separate full core.

For this lesson, we use Java **platform threads**, which are typically backed by OS threads. Java 21 also provides virtual threads, whose scheduling adds another layer; we will study that separately.

---

**8. Scheduling: deciding who gets CPU time**

A **scheduler** selects runnable threads to execute.

A useful simplified state model is:

| State | Meaning |
|---|---|
| Ready | Can execute but is waiting for CPU access |
| Running | Currently executing |
| Waiting | Cannot proceed until some event occurs |
| Finished | Execution has ended |

These are conceptual OS states, not an exact reproduction of Java’s `Thread.State` enumeration.

Suppose a thread requests file data and must wait:

```text
Running → Waiting → Ready → Running
```

While it waits, another thread can use the CPU.

A thread can also lose CPU access while still runnable. This is **preemption**: the OS interrupts its execution so another thread can run.

Scheduling decisions depend on factors such as priorities, available processors, and waiting events. You should not assume that starting thread A before thread B guarantees that A prints first.

**CPU-bound versus I/O-bound work**

- **CPU-bound:** progress is mainly limited by computation.
- **I/O-bound:** progress is mainly limited by waiting for files, networks, or devices.

More threads do not automatically speed up CPU-bound work. Once available CPU resources are busy, extra runnable threads can add overhead.

Concurrency can help I/O-bound applications perform other useful work while some operations wait.

---

**9. Context switching: how execution changes hands**

A **context switch** transfers execution from one thread to another.

The execution context includes items such as:

- The **instruction pointer**: where execution should continue.
- **Registers**: small, fast CPU storage locations.
- Stack-related state and other thread information.

Conceptually:

```text
Thread A is running
        ↓
Save enough of A's execution state
        ↓
Select thread B
        ↓
Restore B's execution state
        ↓
Continue B
```

Windows can switch because a running thread needs to wait, its scheduling interval expires, or a higher-priority thread becomes ready. Waiting threads are not eligible to run until the relevant conditions change. [Microsoft’s context-switch documentation](https://learn.microsoft.com/en-us/windows/win32/procthread/context-switches)

**What a switch does not mean**

The OS does not ordinarily copy the thread’s entire stack and all the process’s objects elsewhere on every switch. They remain in memory; the saved context lets execution resume.

Switching between threads in different processes can also require changing address-space context.

**Why switching has a cost**

Saving and restoring state takes work. Switching can also disturb useful cached data and address translations, reducing performance after execution resumes.

The exact cost depends on hardware, workload, and OS behavior. There is no universal fixed duration.

**Three operations that are easy to confuse**

| Operation | What changes |
|---|---|
| Method call | Active calls within the current thread |
| System call | Execution enters an OS service |
| Context switch | A different thread takes over execution |

A system call may lead to waiting and a context switch, but it does not have to.

---

**10. Source code, machine code, compilation, and interpretation**

**Source code** is the program text written in a programming language.

For example:

```java
System.out.println(2 + 3);
```

**Machine code** consists of instructions directly executed by a particular processor architecture.

Different processor architectures, such as x86-64 and ARM64, have different machine instruction sets.

**Compilation** translates code into another representation while checking and potentially transforming it.

The target does not have to be machine code. Java’s `javac` translates source into JVM class files containing **bytecode**.

**Bytecode** consists of instructions for the JVM’s execution model.

**Interpretation** executes a representation through an interpreter. In a bytecode interpreter, runtime software examines bytecode operations and performs their effects.

The CPU still executes machine instructions—the interpreter itself is implemented using executable machine code.

**Java’s usual execution route**

```text
ComputerDemo.java
       │
       │ javac
       ▼
ComputerDemo.class
       │
       │ JVM loads and executes
       ▼
Machine-level execution
       │
       ▼
CPU performs work
```

`javac` reads Java source and produces class files. [Java 21 compiler documentation](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javac.html)

**JIT compilation**

A **just-in-time compiler** translates code during execution.

HotSpot, a common JVM implementation, can combine interpretation with compilation. It gathers execution information and compiles selected code into machine code, potentially optimizing it further as the program runs. [Java 21 HotSpot documentation](https://docs.oracle.com/en/java/javase/21/vm/java-hotspot-virtual-machine-performance-enhancements.html)

This helps explain **warmup**: performance can change as execution information is collected and optimized code becomes available.

The exact compilation decisions are implementation- and configuration-dependent. Java does not guarantee that a particular method will be compiled after a fixed number of calls.

Also, optimized machine execution need not mirror source statements one-for-one. The implementation must preserve the required observable behavior.

**Is Java compiled or interpreted?**

Both mechanisms can participate:

```text
Source → compiled to bytecode
Bytecode → interpreted and/or compiled for execution
```

Calling Java simply “interpreted” misses an important part of its execution model.

**Why bytecode can be portable**

The same compatible class file can run on suitable JVMs for different operating systems and CPU architectures.

Portability still has limits: the required Java version, libraries, native dependencies, and OS-specific assumptions must be satisfied.

---

**11. JDK, JVM, and the two commands you use**

| Term | Meaning |
|---|---|
| Java language | Rules for writing Java programs |
| Java SE | Standard platform specifications and APIs |
| JVM | Execution environment implementing the JVM model |
| JDK | Development kit containing tools such as `javac` and `java` |
| JRE | Historical name associated with a runtime distribution without the full development toolset |

For your course, use a Java 21 JDK.

These commands have different jobs:

```powershell
javac ComputerDemo.java
```

Compile the source.

```powershell
java ComputerDemo
```

Launch the class using the Java runtime.

In class-launch mode, supply the class name, not `ComputerDemo.class`.

Java 21 also supports launching a suitable single source file directly:

```powershell
java ComputerDemo.java
```

That is a separate source-file mode that compiles the source for execution. We’ll use explicit compile-then-run commands here so you can inspect the stages. [Java 21 launcher documentation](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)

---

**12. Integrated lab: connect files, paths, environment, and execution**

Our program will:

1. Accept a file path as a command-line argument.
2. Show its process and thread information.
3. Read an environment variable.
4. Show how the file path resolves.
5. Read and print the file.

First create a course directory in PowerShell:

```powershell
$courseDir = Join-Path $env:USERPROFILE 'Documents\JavaCourse\Lesson6'

New-Item -ItemType Directory -Force -Path $courseDir
Set-Location -LiteralPath $courseDir

New-Item -ItemType Directory -Force -Path src, out, data

Set-Content -LiteralPath .\data\message.txt -Value 'Hello from the file.' -Encoding UTF8

$env:JAVA_TUTOR_NAME = 'Learner'
```

`Join-Path` builds a path from components. `New-Item` creates directories here. `Set-Content` writes the practice file; rerunning it replaces that file’s contents.

Save the following as `src\ComputerDemo.java`:

```java
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class ComputerDemo {
    public static void main(String[] args) throws Exception {
        if (args.length != 1) {
            System.err.println(
                    "Usage: java -cp out ComputerDemo <file-path>");
            System.exit(2);
        }

        Path input = Path.of(args[0]);
        String learner = System.getenv("JAVA_TUTOR_NAME");

        System.out.println("PID: " + ProcessHandle.current().pid());
        System.out.println(
                "Thread: " + Thread.currentThread().getName());
        System.out.println(
                "Working directory: " + Path.of("").toAbsolutePath());
        System.out.println("Learner: " + learner);
        System.out.println("Input path: " + input.toAbsolutePath());

        String content =
                Files.readString(input, StandardCharsets.UTF_8);

        System.out.println("File content:");
        System.out.print(content);
    }
}
```

Some syntax is new; here is what you need for this lab:

| Code | Meaning |
|---|---|
| `String[] args` | Array containing command-line arguments |
| `args.length != 1` | Checks whether the number of arguments is not one |
| `args[0]` | Gets the first argument; indexing starts at zero |
| `System.exit(2)` | Ends the process with exit code 2 |
| `Path.of(...)` | Creates a Java representation of a path |
| `System.getenv(...)` | Reads an environment variable; returns `null` if absent |
| `ProcessHandle.current().pid()` | Gets the current process identifier |
| `Thread.currentThread().getName()` | Gets the executing Java thread’s name |
| `Files.readString(...)` | Reads and decodes the entire file |
| `throws Exception` | Allows failures to propagate from this teaching example |

A path object describes a location; creating it does not itself create or open the file.

Now compile:

```powershell
javac -encoding UTF-8 -d .\out .\src\ComputerDemo.java
```

`-d .\out` selects the destination directory for generated class files.

Run:

```powershell
java -cp .\out ComputerDemo .\data\message.txt
```

`-cp .\out` tells the launcher where to find the application class.

Illustrative output:

```text
PID: <a number assigned to this run>
Thread: main
Working directory: <your course directory>
Learner: Learner
Input path: <your course directory>\data\message.txt
File content:
Hello from the file.
```

Some file-writing tools include an initial encoding marker, so a text file’s exact contents can include an invisible leading character. The visible message above remains the intended result.

**Trace the entire launch**

| Step | What happens |
|---|---|
| 1 | PowerShell interprets the command and locates `java`. |
| 2 | Windows starts the Java process with arguments, an environment, and a working directory. |
| 3 | The Java runtime initializes and locates `ComputerDemo` using the classpath. |
| 4 | The class is loaded, linked, and initialized as needed. |
| 5 | Java invokes `main` with the application arguments. |
| 6 | The code constructs a path and requests a file read. |
| 7 | Runtime and OS services supply the file data; UTF-8 decoding produces a string. |
| 8 | Output is written and displayed through the terminal. |
| 9 | `main` returns. With no remaining non-daemon application threads keeping it alive, normal JVM shutdown proceeds. |

The JVM can have internal service threads even though you wrote only a `main` method.

**What changes if you run from another directory?**

Move to the parent:

```powershell
Set-Location ..
```

Now run:

```powershell
java -cp .\Lesson6\out ComputerDemo .\Lesson6\data\message.txt
```

Both paths now start from the parent directory.

Java finding your class does not automatically make its data paths correct. Classpath resolution and application file-path resolution serve different purposes.

**Complexity of this file read**

Let `n` be the number of input bytes. For ordinary processing, reading and decoding the whole file takes time proportional to `n`, with memory proportional to the decoded content plus temporary buffers. The returned string is output space; additional buffering is auxiliary space and depends on the implementation.

For very large files, processing incrementally can reduce retained memory. We’ll study that during file I/O.

---

**13. A small thread example**

Save this as `ThreadDemo.java`:

```java
public class ThreadDemo {
    static class Worker extends Thread {
        @Override
        public void run() {
            for (int i = 1; i <= 3; i++) {
                System.out.println("Worker " + i);
            }
        }
    }

    public static void main(String[] args)
            throws InterruptedException {

        Thread worker = new Worker();
        worker.start();

        for (int i = 1; i <= 3; i++) {
            System.out.println("Main " + i);
        }

        worker.join();
        System.out.println("Finished");
    }
}
```

Compile and run:

```powershell
javac ThreadDemo.java
java -cp . ThreadDemo
```

For now:

- `Worker extends Thread` defines a thread class.
- `run()` contains its work.
- `start()` starts separate execution of that work.
- `join()` waits for the worker to finish.
- `throws InterruptedException` acknowledges that waiting can be interrupted.

One possible output is:

```text
Main 1
Worker 1
Worker 2
Main 2
Worker 3
Main 3
Finished
```

Another valid output prints all the worker lines before the main lines.

**The ordering between threads is nondeterministic.**

Within each thread, its numbers appear in order. `Finished` appears after both loops because `main` completes its own loop and then waits for the worker.

A subtle mistake is calling:

```java
worker.run();
```

That is an ordinary method call on the current thread. It does not start a new thread.

Conceptually, after `start()`:

```text
Main thread                         Worker thread
    main frame                          run frame
    worker ───────────────► Worker object
```

This diagram labels relationships, not physical addresses or guaranteed allocation locations. The running thread’s lifecycle also keeps relevant runtime state reachable; clearing one local reference would not stop the thread.

---

**14. Debug by identifying the failing stage**

Different errors point to different layers.

| Symptom | Likely layer | First check |
|---|---|---|
| `java` is not recognized | Shell command discovery | `Get-Command java` and `PATH` |
| `javac` is not recognized | Compiler discovery | JDK installation and compiler path |
| Source file not found | Compiler input path | Current directory and source filename |
| Syntax/type error | Source compilation | Compiler message and reported line |
| Could not find main class | Class lookup | Class name and classpath |
| `UnsupportedClassVersionError` | Runtime compatibility | Compiler target versus runtime version |
| `NoSuchFileException` | Application file access | Resolved data-file path |
| Environment value is `null` | Process configuration | Variable spelling and launch environment |
| Output still shows old behavior | Build/run workflow | Whether compilation succeeded and which class file ran |
| Thread output changes across runs | Scheduling | Whether the program requires explicit coordination |

An especially common mistake is:

1. Compile successfully.
2. Edit the source.
3. Compilation fails.
4. Run the old class file.
5. Assume the new source executed.

A failed compilation does not necessarily remove an earlier class file. Always inspect compilation results before running.

---

**15. Practice: connect the concepts**

Try these without immediately checking the earlier explanations.

1. Your working directory is `C:\course`, but your class is in `C:\course\out`. The program reads `data.txt`. Where does it normally look?
2. Why can changing `JAVA_HOME` leave `java -version` unchanged?
3. What is the difference between a saved program and a process?
4. Which resources are shared by threads, and which execution state belongs to each thread?
5. Can two threads be concurrent without being parallel?
6. Why is a method call not a context switch?
7. Why can Java be described as both compiled and interpreted?
8. Why does `worker.run()` behave differently from `worker.start()`?

For a practical exercise, modify `ComputerDemo` so it prints the file’s UTF-16 unit count using `content.length()`.

Test:

- A normal text file.
- An empty file.
- A filename containing spaces.
- A missing file.
- A launch with no argument.
- A launch from a different working directory.

Before each run, predict which stage will succeed or fail.

**Revision note:** Files hold data; paths locate it; the shell interprets commands; environment variables configure launched processes. The OS manages resources and schedules threads. Java source compiles to bytecode, and a JVM implements its execution using mechanisms such as interpretation and JIT compilation.

**Progress:** This complete module is now introduced in depth. Hexadecimal remains skipped, and practical understanding is awaiting your answers or lab results. The next course topic is **variables, values, references, scope, and lifetime**, which will build directly on the process and thread model above.

---

<a id="lesson-7"></a>

## 7. Java execution and toolkit

We’ll combine both modules into one practical lesson: **how Java runs, which tools you use, and how to diagnose problems**.

We’ll use **Java 21 without preview features**. The commands are for PowerShell. All outputs are **expected outputs, not results executed here**.

---

**1. Separate the language, platform, and tools**

The word “Java” is often used for several different things.

| Term | What it means | Example |
|---|---|---|
| **Java language** | Rules for writing valid Java source code | Classes, methods, loops, types |
| **Java SE** | Java Standard Edition: the standard platform specifications and APIs | Strings, collections, files, threads |
| **JVM** | The Java Virtual Machine execution model, and implementations of that model | A runtime that loads and executes class files |
| **JDK** | Java Development Kit: tools and runtime components for developing Java applications | `javac`, `java`, `jshell`, `jdb` |
| **JRE** | Java Runtime Environment: historically, a runtime distribution for running applications without the complete development toolkit | JVM implementation plus runtime libraries and support files |
| **IDE** | Integrated Development Environment: an application that brings development tools together | Editor, project management, compiler integration, debugger |

Two distinctions matter immediately:

**The Java language is not the standard library.**

For example:

```java
int count = 5;
```

uses language features.

```java
Math.abs(-5)
```

calls a method supplied by the standard library.

**The JVM is not the entire JDK.**

The JDK includes development tools and runtime components. The JVM provides the execution machinery; it is not your editor or source compiler.

Historically, a common simplified diagram was:

```text
JDK
├── Development tools
└── Runtime environment
    ├── JVM
    └── Standard libraries and supporting components
```

Treat that as a description of responsibilities. Modern distributions do not necessarily contain a separate nested `jre` directory or offer a separate JRE download. Applications can also ship tailored runtime images.

For this course, a **Java 21 JDK** gives you the tools we need.

---

**2. Source code, bytecode, and machine code**

These are three different representations of a program.

**Source code** is the text you write:

```java
int result = 7 + 5;
```

**Bytecode** is an instruction representation used by the JVM. It is stored in class files along with information describing classes, methods, fields, and other runtime requirements.

**Machine code** contains instructions executed by a physical processor architecture, such as x86-64 or ARM64.

The usual Java workflow is:

```text
Source file                  Class file
Example.java ── javac ──► Example.class
                              │
                              ▼
                        JVM implementation
                              │
                              ▼
                      Machine-level execution
                              │
                              ▼
                             CPU
```

A `.class` file is a structured binary file. Opening it in a text editor will not show ordinary Java source.

Also, **one source file does not necessarily produce exactly one class file**. A source file containing multiple or nested classes can produce several class files.

---

**3. What compilation actually does**

A **compiler** translates a program from one representation into another.

Conceptually, `javac`:

1. Reads the source text.
2. Recognizes its grammatical structure.
3. Checks names, types, and other language rules.
4. Produces class files if compilation succeeds.

For example:

```java
int count = "five";
```

is rejected because a string cannot be assigned to an `int`.

But:

```java
int count = 5;
```

is valid.

Compilation does not generally execute your application’s `main` method. It checks and translates the application.

**Successful compilation does not prove that the program is correct.**

A program can compile and then:

- Receive invalid input.
- Attempt to open a missing file.
- Divide an integer by zero.
- Produce the wrong answer because its algorithm is incorrect.

The compiler cannot establish every property of every possible execution.

`javac` specifically translates Java source into class files for the JVM. [Java 21 compiler documentation](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javac.html)

---

**4. Interpretation and JIT compilation**

An **interpreter** implements program operations by examining and executing an intermediate representation.

In Java’s case, a bytecode interpreter processes bytecode instructions. The physical CPU executes the interpreter’s machine instructions.

A **JIT compiler**, meaning *just-in-time compiler*, translates selected code into machine code during execution.

A common JVM implementation, HotSpot, can combine these techniques:

```text
Bytecode
   │
   ├── Interpreted execution
   │
   └── Runtime profiling and JIT compilation
                         │
                         ▼
                 Compiled machine code
```

**Profiling** here means gathering information about execution—for example, which methods execute frequently.

**Optimization** transforms execution to improve performance while preserving required observable behavior.

Possible optimizations include:

- Inlining a method’s work into its caller.
- Eliminating calculations whose results are unused.
- Simplifying operations when runtime information permits it.

This is why short benchmark results can be misleading: execution may change as the JVM warms up.

These are implementation strategies. Java does not guarantee that every method will be JIT-compiled or specify a universal number of calls before compilation. [Java 21 HotSpot documentation](https://docs.oracle.com/en/java/javase/21/vm/java-hotspot-virtual-machine-performance-enhancements.html)

**Interview question:** “Is Java compiled or interpreted?”

A useful answer is:

> Java source is normally compiled into bytecode. A JVM implementation can interpret bytecode and compile code into machine instructions during execution.

That explains the stages instead of attaching one label to the entire system.

---

**5. Verify the tools you are actually using**

In PowerShell:

```powershell
java -version
javac -version
jshell --version
```

For our course, each should identify a Java 21 tool, although the vendor name and update number can differ.

Check executable locations:

```powershell
Get-Command java
Get-Command javac
Get-Command jshell
```

Why check all three?

Your computer can have several JDK installations. The `java` runtime might come from one installation and `javac` from another.

An IDE can also use a different JDK from your terminal.

A common failure is compiling with a newer Java version and running with an older one. That can produce:

```text
UnsupportedClassVersionError
```

The class file exists, but the selected runtime cannot support its class-file version.

---

**6. A complete application: source to execution**

Create a file named `LearningApp.java`:

```java
public class LearningApp {
    public static void main(String[] args) {
        if (args.length != 2) {
            System.out.println(
                    "Usage: java LearningApp <name> <number>");
            return;
        }

        String name = args[0];
        int number = Integer.parseInt(args[1]);
        int result = doubleValue(number);

        System.out.println("Hello, " + name);
        System.out.println("Result: " + result);
    }

    static int doubleValue(int value) {
        return value * 2;
    }
}
```

We’ll use small numbers here. As you learned earlier, ordinary `int` multiplication can overflow for sufficiently large inputs.

Compile:

```powershell
javac LearningApp.java
```

For this source, successful compilation creates:

```text
LearningApp.class
```

Run:

```powershell
java -cp . LearningApp Narasimha 12
```

Expected output:

```text
Hello, Narasimha
Result: 24
```

The command means:

| Part | Meaning |
|---|---|
| `java` | Launch the Java runtime |
| `-cp .` | Search the current directory for application classes |
| `LearningApp` | Launch this class |
| `Narasimha` | First application argument |
| `12` | Second application argument |

The **classpath** tells Java where to locate application classes and libraries. It is different from `PATH`, which helps your shell locate executable programs.

In this launch mode, use the class name:

```powershell
java -cp . LearningApp
```

Not:

```powershell
java LearningApp.class
```

---

**7. Understand every part of `main`**

Our entry method is:

```java
public static void main(String[] args)
```

| Part | Meaning |
|---|---|
| `public` | The method is accessible from outside its class |
| `static` | It belongs to the class and can be invoked without constructing a `LearningApp` object |
| `void` | It does not return a value to its caller |
| `main` | The method name recognized by this standard launch convention |
| `String[]` | An array of strings |
| `args` | The parameter name; another valid name could be used |

An **array** contains an indexed sequence of elements. The first index is zero.

For:

```powershell
java -cp . LearningApp Narasimha 12
```

the argument array contains:

```text
args[0] = "Narasimha"
args[1] = "12"
args.length = 2
```

Neither the class name nor `-cp .` is included in this application argument array.

The number arrives as text:

```java
Integer.parseInt(args[1])
```

converts `"12"` into integer `12`.

If you need an argument containing spaces, quote it:

```powershell
java -cp . LearningApp "Narasimha Rao" 12
```

Expected output:

```text
Hello, Narasimha Rao
Result: 24
```

The launcher uses the conventional public static entry method for this Java 21 application. [Java 21 launcher documentation](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)

**Version boundary:** Java 21 has preview alternatives involving simpler entry methods. We are not enabling them. Use the conventional form above throughout this course.

**Important:** classes without `main` are valid. Most classes in a larger application are helpers or domain classes; they are not independently launched applications.

---

**8. What happens before `main` executes?**

The runtime must establish the classes needed for execution.

Three terms describe important stages:

| Stage | Purpose |
|---|---|
| **Loading** | Find a class’s binary representation and create its runtime representation |
| **Linking** | Verify and prepare the class, and resolve symbolic references as required |
| **Initialization** | Execute the class initialization logic required by Java’s rules |

Linking includes:

- **Verification:** checks class-file structure and bytecode constraints.
- **Preparation:** establishes static fields and their initial default values, subject to specified constant handling.
- **Resolution:** connects symbolic references, such as a method name and descriptor, to runtime entities.

Resolution can occur lazily. Do not imagine that the JVM eagerly loads and completely resolves every class your application might ever use before starting `main`.

These stages are specified by the JVM; their implementation is more detailed than this introductory model. [JVM loading, linking, and initialization specification](https://docs.oracle.com/javase/specs/jvms/se21/html/jvms-5.html)

Here is a runnable demonstration of initialization before `main`:

```java
public class StartupDemo {
    static {
        System.out.println("Class initialization");
    }

    public static void main(String[] args) {
        System.out.println("Inside main");
    }
}
```

Run:

```powershell
javac StartupDemo.java
java -cp . StartupDemo
```

Expected output:

```text
Class initialization
Inside main
```

The `static { ... }` block is a **static initializer**.

Therefore, “`main` is the application entry method” does not mean “no Java initialization code can execute before its first statement.”

---

**9. Follow the method calls and references**

Return to `LearningApp`.

Just before `doubleValue` returns, the conceptual active calls are:

```text
Current thread's active calls

doubleValue frame
    value = 12
    about to return 24

main frame
    args ─────► Array A1
    name ─────► String S1: "Narasimha"
    number = 12
    result: awaiting the method's returned value
```

The argument array also references strings:

```text
Array A1
    [0] ─────► String S1: "Narasimha"
    [1] ─────► String S2: "12"
```

Notice that `name` and `args[0]` refer to the same string in this example. Assigning the reference does not copy the string’s contents.

Execution proceeds as follows:

| Step | Effect |
|---|---|
| Call `doubleValue(number)` | Copies the integer value `12` into parameter `value` |
| Evaluate `value * 2` | Produces `24` |
| Return | Completes the method call |
| Resume `main` | Assigns the returned value to `result` |
| Print | Displays the greeting and result |

After the method returns, its active frame is gone.

When `main` returns, its locals stop participating in execution. Objects are eligible for garbage collection only when they are no longer reachable through relevant references. Ending one variable’s scope alone does not prove an object is unreachable, and eligibility does not imply immediate collection.

The labels `A1`, `S1`, and `S2` are conceptual identifiers. They are not memory addresses or claims about physical placement.

---

**10. Inspect bytecode with `javap`**

The JDK includes a tool called `javap` that can display class information and disassemble bytecode.

Run:

```powershell
javap -c LearningApp
```

For the `doubleValue` method, a typical `javac` result contains instructions resembling:

```text
0: iload_0
1: iconst_2
2: imul
3: ireturn
```

Their roles are:

| Instruction | Meaning |
|---|---|
| `iload_0` | Load the integer from local-variable slot zero |
| `iconst_2` | Push integer constant two |
| `imul` | Multiply the two integers |
| `ireturn` | Return an integer |

Bytecode uses an **operand stack**, a working stack for values used by instructions.

Conceptually:

```text
Instruction      Operand stack afterward

iload_0          [12]
iconst_2         [12, 2]
imul             [24]
ireturn          result passed to caller
```

This is the JVM instruction model. Optimized machine code need not physically reproduce every bytecode stack operation.

`javap` is useful when you want to see what compilation produced. It is not necessary for every ordinary coding task.

---

**11. JShell: experiment without a complete application**

JShell is Java’s interactive tool for trying declarations, statements, and expressions.

It is a **REPL**:

```text
Read → Evaluate → Print → Repeat
```

Start it:

```powershell
jshell
```

Enter:

```java
int total = 10;
int count = 3;
total / count
(double) total / count
```

The expression results are:

```text
3
3.3333333333333335
```

JShell may display temporary variable names such as `$1`; those names depend on the session.

You can define a method:

```java
int twice(int value) {
    return value * 2;
}
```

Then invoke it:

```java
twice(7)
```

Result:

```text
14
```

Useful JShell commands:

| Command | Purpose |
|---|---|
| `/help` | Show help |
| `/vars` | Show variables |
| `/methods` | Show declared methods |
| `/list` | Show entered snippets |
| `/reset` | Clear the session state |
| `/exit` | Leave JShell |

JShell supports quick language and API experiments; it is not a replacement for developing and testing complete applications. [Java 21 JShell introduction](https://docs.oracle.com/en/java/javase/21/jshell/introduction-jshell.html)

**A common trap:** earlier snippets remain in the session. Code may appear to work only because a variable was defined ten minutes ago.

Use `/reset` when checking whether an example is self-contained.

---

**12. What an IDE adds**

An IDE brings several tools into one application:

- A source editor.
- Code completion and navigation.
- Error highlighting.
- Build integration.
- Run configurations.
- A debugger.
- Test execution.

The IDE does not change Java’s language rules. It coordinates tools and provides a richer interface.

Before running a project, verify these settings:

| Setting | What to check |
|---|---|
| Project JDK | Java 21 |
| Language level | Java 21, without preview features |
| Main class | The class containing the intended entry method |
| Program arguments | Arguments passed to `main` |
| Working directory | Base directory for relative application paths |
| Runtime JDK | The Java installation used to execute the application |

An IDE can compile through its own compiler integration or a build tool. Do not assume its Build button literally executes the same `javac` command you used in PowerShell.

**Program arguments versus JVM options**

These belong in different places:

```powershell
java -Xmx256m -cp . LearningApp Narasimha 12
```

- `-Xmx256m` is a JVM option controlling maximum Java heap size.
- `Narasimha 12` are application arguments.

You do not need that memory option for this lesson; it illustrates the distinction.

---

**13. Compiler errors, runtime exceptions, and logic errors**

These require different debugging approaches.

| Category | When it appears | Example |
|---|---|---|
| Compiler error | While translating source | Unknown variable name |
| Launch or linkage failure | While finding or preparing code | Main class not found |
| Runtime exception | During execution | Invalid numeric input |
| Logic error | Program runs but behaves incorrectly | Integer division used for an average |

**Compiler-error example**

```java
public class CompileDemo {
    public static void main(String[] args) {
        int total = 10;
        System.out.println(totel);
    }
}
```

The variable is declared as `total` but used as `totel`.

A representative diagnostic is:

```text
error: cannot find symbol
        System.out.println(totel);
                           ^
  symbol:   variable totel
```

Read it in this order:

1. Which file and line?
2. What category of error?
3. Which token is highlighted?
4. What name or type did the compiler expect?

Correction:

```java
System.out.println(total);
```

Fix the first meaningful error and compile again. One missing brace or semicolon can produce several later messages.

Also check the line before the reported location: the parser may only discover the problem after the original mistake.

---

**14. Runtime exceptions and stack traces**

An **exception** is an object representing an exceptional condition that disrupts normal execution.

If a method does not handle an exception, it propagates to its caller. If it remains uncaught, the thread terminates and a diagnostic is commonly printed.

Save this as `CrashDemo.java`:

```java
public class CrashDemo {
    public static void main(String[] args) {
        showResult();
    }

    static void showResult() {
        int answer = divide(10, 0);
        System.out.println(answer);
    }

    static int divide(int total, int count) {
        return total / count;
    }
}
```

Run:

```powershell
javac CrashDemo.java
java -cp . CrashDemo
```

Expected diagnostic for this exact source layout:

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at CrashDemo.divide(CrashDemo.java:12)
    at CrashDemo.showResult(CrashDemo.java:7)
    at CrashDemo.main(CrashDemo.java:3)
```

Line numbers change if you change the source layout.

Read the trace from the top:

| Part | Meaning |
|---|---|
| `main` | Name of the thread |
| `ArithmeticException` | Exception type |
| `/ by zero` | Diagnostic message |
| `divide(...:12)` | Where this failure occurred |
| `showResult(...:7)` | The caller of `divide` |
| `main(...:3)` | The caller of `showResult` |

The active call chain at failure was:

```text
main → showResult → divide
```

The trace lists the most recent call first.

`System.out.println(answer)` is never reached because `divide` does not return normally.

**How to correct it**

First decide what the inputs mean.

If `count` represents a number of people, zero is invalid. Validate that requirement before dividing:

```java
if (count <= 0) {
    throw new IllegalArgumentException(
            "count must be greater than zero");
}
```

This reports the actual input rule. It does not silently invent a result.

Later, we’ll teach how application boundaries can catch exceptions and display useful messages without exposing raw traces to end users.

For longer traces:

- Look for the first relevant line in your code.
- Inspect the exception type and message.
- Follow `Caused by:` sections when present.
- Do not assume that the final line identifies the original cause.

---

**15. A debugger: inspect execution instead of guessing**

A debugger lets you pause an application and inspect its state.

| Feature | Purpose |
|---|---|
| Breakpoint | Pause when execution reaches a selected location |
| Step over | Execute the current statement without entering its method calls |
| Step into | Follow execution into a method call |
| Step out | Continue until the current method returns |
| Resume | Continue until the next pause or completion |
| Variables view | Inspect values available in the selected frame |
| Call-stack view | Inspect active method calls |
| Watch expression | Evaluate an expression while paused |

A breakpoint is normally reached **before** the selected statement executes.

Use this complete program:

```java
public class AverageDemo {
    public static void main(String[] args) {
        int total = 10;
        int count = 3;
        double average = total / count;

        System.out.println(average);
    }
}
```

Expected output:

```text
3.0
```

Suppose the intended result is approximately `3.3333333333333335`.

A useful debugging sequence is:

1. Set a breakpoint on `double average = total / count;`.
2. Start debugging.
3. Inspect `total`: `10`.
4. Inspect `count`: `3`.
5. Evaluate `total / count`: `3`.
6. Step over the assignment.
7. Inspect `average`: `3.0`.

The input values are correct. The calculation uses integer division because both operands are integers.

Correction:

```java
double average = (double) total / count;
```

Expected output after recompilation:

```text
3.3333333333333335
```

This is a **logic error**, not a compiler error or exception.

The debugger helps you locate the first point where actual behavior differs from your expectation.

**Command-line debugging**

The JDK also provides `jdb`:

```powershell
javac -g AverageDemo.java
jdb AverageDemo
```

The `-g` option includes debugging information, including local-variable information.

At the debugger prompt, commands such as these support an investigation:

```text
stop in AverageDemo.main
run
list
next
locals
where
cont
quit
```

Use `next` until the declarations you want to inspect have executed. `locals` shows available local variables; `where` shows the call stack. IDE debuggers provide graphical controls for the same core activities. [Java 21 debugger documentation](https://docs.oracle.com/en/java/javase/21/docs/specs/man/jdb.html)

---

**16. A reliable everyday workflow**

Use this sequence:

```text
Understand the requirement
        ↓
Write a small implementation
        ↓
Compile
        ↓
Fix compiler errors
        ↓
Run with a known input
        ↓
Compare expected and actual behavior
        ↓
Inspect failures with a debugger or focused output
        ↓
Correct and test again
```

Test different input categories.

For `LearningApp`:

| Input | Expected behavior |
|---|---|
| `Narasimha 12` | Prints result `24` |
| `"Narasimha Rao" 12` | Preserves the name as one argument |
| No arguments | Prints usage and returns |
| `Narasimha twelve` | `NumberFormatException` with the current implementation |
| `Narasimha 0` | Prints result `0` |
| `Narasimha -4` | Prints result `-8` |

Be aware of stale class files:

> Editing source does not update bytecode. If compilation fails, an older class file may still exist and run.

A successful run of an old file does not validate your latest source.

---

**17. Practice and interview checks**

Answer these in your own words:

1. Why is a `.class` file not ordinary CPU machine code?
2. What is the difference between Java SE and the JDK?
3. Why can a class compile successfully without a `main` method?
4. Why can initialization code execute before `main`?
5. What does `static` allow the launcher to do?
6. Why can a program compile successfully and still throw an exception?
7. What does the first `at ...` line in a stack trace usually identify?
8. How would you distinguish a logic error from a runtime exception?
9. Why might an application behave differently in an IDE and a terminal?

For implementation practice, change `LearningApp` to:

- Accept **two integer arguments**.
- Use a separate method to add them.
- Print the result.
- Print usage when the argument count is incorrect.

Then:

1. Compile and run it from PowerShell.
2. Try its addition method in JShell.
3. Set a debugger breakpoint inside the method.
4. Inspect its parameters and call stack.
5. Test zero, negative numbers, and invalid text.
6. Explain whether large integers could overflow.

No solution is included yet so you can work through the decisions.

**Revision note:** `javac` turns source into class files; `java` launches execution through a JVM. JShell supports quick experiments, an IDE coordinates development tools, and a debugger exposes execution state. Diagnose problems by first identifying whether they occur during compilation, startup, execution, or result checking.

**Progress:** Modules 4 and 5 have now been taught together. Java 21 remains our baseline; hexadecimal is skipped; understanding will be confirmed through your exercises.

---

<a id="lesson-8"></a>

## 8. Values, decisions, methods, and memory

These three modules fit together:

- **Values and calculations:** represent information and compute results.
- **Decisions and repetition:** choose actions and repeat work.
- **Methods and execution memory:** organize work and understand how calls and objects behave.

We’ll use **Java 21**, build from small examples to a complete problem, and then trace references and recursive calls.

All outputs below are **expected outputs; I have not executed these examples here**. Save each complete example using its public class name, then compile and run it:

```powershell
javac Example.java
java -cp . Example
```

---

**1. Variables, values, and types**

A program needs to remember information while it works.

A **value** is information such as `25`, `true`, or a reference to an object.

A **variable** is a typed storage location that can hold a value.

A **type** determines which values are allowed and which operations make sense.

```java
int age = 25;
```

Read this as:

> Declare a variable named `age`, whose type is `int`, and initialize it with the value `25`.

| Part | Meaning |
|---|---|
| `int` | Type |
| `age` | Variable name |
| `=` | Assignment |
| `25` | Initial value |
| `;` | Ends the statement |

A useful analogy is a labeled container. Its limitation is that Java variables are part of the language model; their physical storage can change through optimization.

**Declaration, initialization, and reassignment**

```java
int score;       // Declaration
score = 80;      // First assignment
score = 90;      // Reassignment
```

A declaration can include an initializer:

```java
int score = 80;
```

Assignment replaces the variable’s current value:

```java
int count = 5;
count = count + 1;
```

The right-hand side is evaluated first:

```text
Read count:       5
Calculate 5 + 1:  6
Assign to count:  6
```

`=` means assignment, not mathematical equality.

---

**2. Local variables must be initialized before use**

A **local variable** is declared inside a method or another local block.

This does not compile:

```java
int score;
System.out.println(score);
```

Java requires the compiler to establish that a local variable has been assigned before it is read.

Correct:

```java
int score = 0;
System.out.println(score);
```

However, **fields**—variables declared as members of a class—and array elements receive default values.

| Type category | Default field/array-element value |
|---|---|
| Integer types | Zero |
| Floating-point types | Positive zero |
| `char` | The zero-valued UTF-16 unit |
| `boolean` | `false` |
| Reference types | `null` |

Do not transfer that rule to ordinary local variables. Java distinguishes these initialization rules. [Java 21 variables and initial values](https://docs.oracle.com/javase/specs/jls/se21/html/jls-4.html#jls-4.12.5)

Also, `null` is not an empty string, an empty array, or zero. It represents the absence of an object reference.

---

**3. Primitive types versus reference types**

Java has eight primitive types:

| Type | Represents | Example |
|---|---|---|
| `byte` | 8-bit signed integer | `byte level = 3;` |
| `short` | 16-bit signed integer | `short year = 2026;` |
| `int` | 32-bit signed integer | `int count = 100;` |
| `long` | 64-bit signed integer | `long population = 8_000_000_000L;` |
| `float` | 32-bit binary floating-point value | `float ratio = 0.5f;` |
| `double` | 64-bit binary floating-point value | `double temperature = 36.6;` |
| `char` | One UTF-16 code unit | `char grade = 'A';` |
| `boolean` | `true` or `false` | `boolean ready = true;` |

There is no language guarantee that a `boolean` variable occupies exactly one physical bit.

**A primitive variable holds a primitive value.**

```java
int first = 10;
int second = first;
second = 20;
```

Now:

```text
first  = 10
second = 20
```

The assignment copied the integer value.

**A reference variable holds a reference value.**

```java
int[] first = {10};
int[] second = first;
```

An array is an object. Both variables now refer to the same array:

```text
first  ──┐
         ├──► Array A1: [10]
second ──┘
```

The assignment copied a reference, not the array.

```java
second[0] = 20;
```

Changes that shared array:

```text
first  ──┐
         ├──► Array A1: [20]
second ──┘
```

Therefore, `first[0]` is now also `20`.

A reference is not a portable numerical memory address you can perform arithmetic on. JVMs can represent references differently and can move objects while preserving program behavior.

---

**4. Literals and readable names**

A **literal** is a value written directly in source code.

```java
42
3.5
true
'A'
"Hello"
null
```

Useful details:

```java
long distance = 3_000_000_000L;
float fraction = 0.25f;
double average = 7.5;
String message = "Hello";
```

- `L` marks a `long` integer literal.
- `f` marks a `float` literal.
- A decimal-point literal such as `7.5` normally has type `double`.
- Underscores can separate digits for readability.
- Single quotes delimit `char` literals.
- Double quotes delimit string literals.

Use names that reveal purpose:

```java
int numberOfStudents = 30;
double averageScore = 82.5;
boolean hasPermission = true;
```

Prefer these over:

```java
int x = 30;
double a = 82.5;
boolean b = true;
```

Short names such as `i` can be reasonable for a tiny loop index.

Common conventions:

| Item | Convention | Example |
|---|---|---|
| Variable or method | `lowerCamelCase` | `totalScore`, `calculateAverage` |
| Class | `UpperCamelCase` | `ScoreAnalyzer` |
| Named constant | `UPPER_SNAKE_CASE` | `MAX_SCORE` |

Include units when useful:

```java
long timeoutMillis = 5000;
```

That is clearer than `timeout = 5000`.

---

**5. Operators: calculating and comparing**

An **operator** performs an operation on values called **operands**.

| Category | Operators | Example |
|---|---|---|
| Arithmetic | `+ - * / %` | `total / count` |
| Comparison | `< <= > >=` | `score >= 50` |
| Equality | `== !=` | `count == 0` |
| Boolean logic | `&& || !` | `ready && permitted` |
| Assignment | `= += -= *= /= %=` | `total += score` |
| Increment/decrement | `++ --` | `i++` |
| Conditional expression | `? :` | `passed ? "Pass" : "Fail"` |
| Bit operations | `& \| ^ ~ << >> >>>` | Operations on bit patterns |

We’ll leave detailed bit manipulation for a later lesson.

**Integer division and remainder**

```java
7 / 2     // 3
7 % 2     // 1
```

Integer division truncates toward zero:

```java
-7 / 2    // -3
-7 % 2    // -1
```

For ordinary integer values, checking evenness is:

```java
number % 2 == 0
```

This also works for negative even numbers.

For a fractional result:

```java
double result = 7.0 / 2;   // 3.5
```

**Assignment versus equality**

```java
count = 5;    // Change count
count == 5    // Ask whether count equals 5
```

A subtle bug is:

```java
if (ready = true) {
    // ...
}
```

This can compile because assigning `true` produces a boolean value. It changes `ready` and then tests `true`.

Write:

```java
if (ready) {
    // ...
}
```

**Reference equality**

For object references, `==` checks whether both refer to the same object, or are both `null`.

For string content, normally use:

```java
"yes".equals(answer)
```

This also safely returns `false` if `answer` is `null`.

---

**6. Precedence, grouping, and evaluation order**

**Precedence** determines how an expression is grouped.

```java
2 + 3 * 4
```

means:

```java
2 + (3 * 4)
```

and produces `14`.

Parentheses change the grouping:

```java
(2 + 3) * 4
```

produces `20`.

A useful partial ordering, highest first:

| Priority | Operators |
|---|---|
| Higher | Unary operators such as `!` and unary `-` |
| | `* / %` |
| | `+ -` |
| | `< <= > >=` |
| | `== !=` |
| | `&&` |
| | `||` |
| | `?:` |
| Lower | Assignment operators |

Use parentheses when they make intent easier to read.

**Precedence is not the same as evaluation order.**

Java evaluates operands and method arguments according to defined ordering rules, including left-to-right operand evaluation. Do not introduce changes to variables inside complicated expressions merely because the ordering is defined. [Java 21 expression evaluation](https://docs.oracle.com/javase/specs/jls/se21/html/jls-15.html#jls-15.7)

Prefer:

```java
count++;
int total = count + price;
```

over expressions that mix several increments and calculations.

**String concatenation can surprise you**

```java
System.out.println("Total: " + 2 + 3);
System.out.println("Total: " + (2 + 3));
```

Expected output:

```text
Total: 23
Total: 5
```

The first expression joins text left to right. The second calculates the sum before joining it to the text.

---

**7. Conversions, casting, and numeric promotion**

A **conversion** changes a value’s representation or type according to Java’s rules.

A **cast** explicitly requests a permitted conversion.

**Widening**

```java
int count = 100;
long larger = count;
```

Every `int` value fits exactly in `long`.

But widening does not always mean preserving every significant digit:

```java
int original = 16_777_217;
float approximate = original;
```

That integer cannot be represented exactly as a `float`.

**Narrowing**

```java
double price = 19.95;
int wholePart = (int) price;
```

`wholePart` becomes `19`. This truncates toward zero; it does not round to the nearest integer.

Integer narrowing can discard higher-order bits:

```java
int original = 130;
byte narrowed = (byte) original;
```

`narrowed` becomes `-126`.

A cast is not an overflow-validation operation.

**Numeric promotion**

For ordinary binary arithmetic such as addition:

1. If an operand is `double`, arithmetic uses `double`.
2. Otherwise, if an operand is `float`, it uses `float`.
3. Otherwise, if an operand is `long`, it uses `long`.
4. Otherwise, it uses `int`.

Thus, arithmetic on `byte`, `short`, and `char` commonly produces `int`. [Java 21 conversion and promotion rules](https://docs.oracle.com/javase/specs/jls/se21/html/jls-5.html)

```java
byte first = 10;
byte second = 20;
int total = first + second;
```

**Convert before the calculation**

```java
int first = 50_000;
int second = 50_000;

long wrong = first * second;
long correct = (long) first * second;
```

The first multiplication uses `int` and overflows before assignment to `long`.

**Compound assignment includes conversion**

```java
byte count = 127;
count += 1;
```

This compiles and produces `-128`.

By contrast:

```java
count = count + 1;
```

does not compile without an explicit narrowing conversion because `count + 1` has type `int`.

Do not assume `+=` protects against overflow.

**Runnable calculation example**

```java
public class CalculationDemo {
    public static void main(String[] args) {
        int total = 10;
        int count = 3;

        System.out.println(total / count);
        System.out.println((double) total / count);
        System.out.println((int) 9.8);
        System.out.println(2 + 3 * 4);
        System.out.println((2 + 3) * 4);
    }
}
```

Expected output:

```text
3
3.3333333333333335
9
14
20
```

---

**8. Conditions: choosing what happens**

A condition must produce a `boolean`.

```java
if (score >= 50) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}
```

Java does not treat integer zero as `false` or other integers as `true`.

This is invalid:

```java
if (count) {
    // ...
}
```

Write the actual condition:

```java
if (count > 0) {
    // ...
}
```

**Multiple alternatives**

```java
if (score < 0 || score > 100) {
    System.out.println("Invalid score");
} else if (score >= 80) {
    System.out.println("Excellent");
} else if (score >= 50) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}
```

The order matters. The first matching branch executes.

If you test `score >= 50` before `score >= 80`, a score of `90` is already caught by the first branch.

**Short-circuit evaluation**

For `&&`, Java skips the right operand if the left operand is false:

```java
text != null && !text.isEmpty()
```

If `text` is `null`, Java does not call `isEmpty()`.

For `||`, Java skips the right operand if the left operand is true.

This makes guards possible:

```java
count != 0 && total / count > 2
```

Do not replace `&&` with `&` casually. With boolean operands, `&` evaluates both sides.

**A compact conditional value**

```java
String outcome = score >= 50 ? "Pass" : "Fail";
```

Use this for a simple choice of value. Prefer `if` for larger blocks of work.

---

**9. `switch`: choosing among distinct alternatives**

A `switch` is useful when choosing among named or numbered cases.

Java 21 supports this form:

```java
public class SwitchDemo {
    public static void main(String[] args) {
        int option = 2;

        String action = switch (option) {
            case 1 -> "Create";
            case 2 -> "View";
            case 3 -> "Exit";
            default -> "Unknown option";
        };

        System.out.println(action);
    }
}
```

Expected output:

```text
View
```

The arrow form does not fall through into the next case.

Older colon-style `switch` statements can fall through when a `break` is omitted. We’ll use arrows when they express the decision clearly.

Use `if` for general conditions such as ranges. Use `switch` for selecting among distinct alternatives.

---

**10. Loops: repeating work**

Each execution of a loop body is an **iteration**.

Choose the loop form based on the problem:

| Form | Typical use |
|---|---|
| `for` | Counted or indexed repetition |
| `while` | Repeat while a condition remains true |
| `do-while` | Execute at least once, then test |
| Enhanced `for` | Visit each element without needing its index |

**Basic `for` loop**

```java
for (int i = 0; i < 3; i++) {
    System.out.println(i);
}
```

Execution order:

```text
Initialize once
    ↓
Check condition
    ↓ true
Execute body
    ↓
Perform update
    ↓
Check condition again
```

Expected output:

```text
0
1
2
```

**`while`**

```java
int remaining = 3;

while (remaining > 0) {
    System.out.println(remaining);
    remaining--;
}
```

Expected output:

```text
3
2
1
```

If the condition is false initially, the body runs zero times.

**`do-while`**

```java
int remaining = 0;

do {
    System.out.println(remaining);
    remaining--;
} while (remaining > 0);
```

Expected output:

```text
0
```

The body runs once before the first condition check.

**Enhanced `for`**

```java
int[] scores = {70, 80, 90};

for (int score : scores) {
    System.out.println(score);
}
```

The local variable `score` receives each element value. Assigning a different value to this primitive local does not replace the corresponding array element.

**`break` and `continue`**

- `break` exits the nearest loop.
- `continue` skips the remainder of the current iteration.

In a basic `for` loop, `continue` still proceeds to the update expression.

These behaviors are part of Java’s statement rules. [Java 21 loops and control flow](https://docs.oracle.com/javase/specs/jls/se21/html/jls-14.html)

---

**11. Trace a loop rather than guessing**

Consider:

```java
int sum = 0;

for (int i = 1; i <= 3; i++) {
    sum += i;
}

System.out.println(sum);
```

Trace:

| `i` at condition check | `i <= 3` | `sum` before body | `sum` after body |
|---:|---|---:|---:|
| 1 | true | 0 | 1 |
| 2 | true | 1 | 3 |
| 3 | true | 3 | 6 |
| 4 | false | 6 | Body not executed |

Expected output:

```text
6
```

The useful reasoning is:

> After each completed iteration, `sum` contains the sum of all integers processed so far.

This is a **loop invariant**: a property maintained as the loop progresses.

**Common loop bugs**

| Bug | Result | Correction |
|---|---|---|
| `i <= array.length` | Eventually accesses past the final element | Use `i < array.length` |
| Missing progress update | Loop may never terminate | Ensure progress toward termination |
| Semicolon after `while (...)` | Creates an empty loop body | Remove unintended semicolon |
| Resetting `sum` inside the loop | Loses earlier contributions | Initialize before the loop |
| Using the wrong starting value | Skips or repeats a case | Check the smallest valid input |

**Complexity**

If a loop processes `n` elements once and performs constant work per element, its running time is **O(n)**.

If it keeps only a fixed number of counters, its auxiliary space is **O(1)**.

Two nested loops are not automatically O(n²). Count their actual repetitions. An outer loop of `n` iterations with an inner loop of exactly three iterations performs `3n` iterations: O(n).

---

**12. Methods: name a task and give it a contract**

A **method** groups operations behind a name.

```java
static int add(int first, int second) {
    return first + second;
}
```

| Part | Meaning |
|---|---|
| `static` | Can be called through the class without creating an instance |
| `int` | Return type |
| `add` | Method name |
| `first`, `second` | Parameters |
| `return` | Supplies the result and completes this call |

A **parameter** is a variable declared by the method.

An **argument** is a value supplied when calling it.

```java
int answer = add(10, 20);
```

Here, `10` and `20` are arguments. Their values initialize `first` and `second`.

A method’s **contract** describes:

- What inputs it accepts.
- What result it produces.
- Whether it changes objects.
- What happens for invalid input.

For example:

> `average(scores)` accepts a nonempty array of scores from 0 through 100 and returns their arithmetic mean.

That contract guides both implementation and testing.

**Returning versus printing**

```java
return result;
```

passes a value to the caller.

```java
System.out.println(result);
```

writes output.

A method that only prints an answer does not automatically provide that answer to another calculation.

Prefer returning results from calculation methods. The caller can then print, store, compare, or combine them.

---

**13. Worked problem: calculate a validated average**

**Requirement**

Calculate the average of a nonempty array of integer scores. Every score must be between 0 and 100.

**Inputs and outputs**

- Input: an `int[]`.
- Output: a `double`.
- Invalid input: `null`, an empty array, or an out-of-range score.

**What to notice first**

1. An empty array would make the denominator zero.
2. Integer division would discard the fractional part.
3. Validation must check every score.
4. A `long` accumulator avoids ordinary `int` sum overflow.

**Algorithm**

1. Reject missing or empty input.
2. Start a total at zero.
3. Visit each score.
4. Reject scores outside the allowed range.
5. Add valid scores to the total.
6. Divide using floating-point arithmetic.

Complete program:

```java
public class ScoreAnalyzer {
    static double average(int[] scores) {
        if (scores == null || scores.length == 0) {
            throw new IllegalArgumentException(
                    "Provide at least one score");
        }

        long total = 0;

        for (int score : scores) {
            if (score < 0 || score > 100) {
                throw new IllegalArgumentException(
                        "Each score must be between 0 and 100");
            }

            total += score;
        }

        return (double) total / scores.length;
    }

    public static void main(String[] args) {
        int[] scores = {70, 80, 95};
        System.out.println(average(scores));
    }
}
```

Expected output:

```text
81.66666666666667
```

`throw` signals failure using an exception. It stops normal execution of this method.

**Dry run**

| Score | Total before | Total after |
|---:|---:|---:|
| 70 | 0 | 70 |
| 80 | 70 | 150 |
| 95 | 150 | 245 |

Then:

```text
245 ÷ 3 = approximately 81.66666666666667
```

**Why it works**

After each iteration, `total` is the sum of the validated scores processed so far. After all elements are processed, it is the complete sum. Dividing by the number of scores produces the mean.

**Complexity**

Let `n` be the number of scores.

- Valid input: O(n) time, because each element is visited once.
- Invalid input may fail earlier; worst-case time remains O(n).
- Auxiliary space: O(1).
- Output: one `double`, O(1).
- The existing input array is not copied.

**Test cases**

| Input | Expected result |
|---|---|
| `{70, 80, 95}` | Approximately `81.66666666666667` |
| `{0}` | `0.0` |
| `{100}` | `100.0` |
| `{0, 100}` | `50.0` |
| `{}` | Exception |
| `null` | Exception |
| `{-1, 50}` | Exception |
| `{50, 101}` | Exception |

Testing around `0` and `100` checks the boundaries. Testing `-1` and `101` checks just outside them.

---

**14. Java always passes arguments by value**

This is one of the most important concepts in the course.

> Java copies an argument’s value into the method’s parameter.

For a primitive argument, it copies the primitive value.

For an object argument, it copies the reference value.

Complete example:

```java
public class PassingDemo {
    static void changeNumber(int value) {
        value = 99;
    }

    static void changeArray(int[] values) {
        values[0] = 20;
        values = new int[] {30};
        values[0] = 40;
    }

    public static void main(String[] args) {
        int number = 10;
        changeNumber(number);
        System.out.println(number);

        int[] first = {10};
        int[] second = first;

        changeArray(first);

        System.out.println(first[0]);
        System.out.println(second[0]);

        second = new int[] {50};
        second[0] = 60;

        System.out.println(first[0]);
        System.out.println(second[0]);
    }
}
```

Expected output:

```text
10
20
20
20
60
```

**Primitive argument**

At the start of `changeNumber`:

```text
main:
    number = 10

changeNumber:
    value = 10
```

Reassigning `value` does not change `number`.

**Reference argument**

At the start of `changeArray`:

```text
main:
    first  ──┐
    second ──┼──► Array A1: [10]
             │
changeArray: │
    values ──┘
```

After:

```java
values[0] = 20;
```

the shared object changes:

```text
first, second, values ──► Array A1: [20]
```

After:

```java
values = new int[] {30};
```

only the parameter is reassigned:

```text
main:
    first  ──┐
    second ──┴──► Array A1: [20]

changeArray:
    values ─────► Array A2: [30]
```

Changing A2 to `[40]` does not affect A1.

When `changeArray` returns, its parameter disappears from the active call. A2 has no escaping reference in this example and can be collected once unreachable. Collection is not necessarily immediate.

Later, reassigning `second` separates the caller’s references too:

```text
first  ─────► Array A1: [20]
second ─────► Array A3: [60]
```

**Mutation changes an object. Reassignment changes which value a variable holds.**

---

**15. Scope, lifetime, and reachability are different**

**Scope** is the region of source code where a name can be used.

```java
if (true) {
    int score = 80;
    System.out.println(score);
}

// score cannot be used here
```

**Lifetime** concerns how long a particular variable or runtime entity exists during execution.

Each method invocation has its own parameters and local-variable state.

**Reachability** concerns whether an object can still be accessed through live reference paths relevant to garbage collection.

An object can outlive the method that created it:

```java
static int[] createScores() {
    int[] local = {70, 80};
    return local;
}
```

The method returns a reference to the array.

```java
int[] scores = createScores();
```

Conceptually:

```text
While createScores runs:

createScores.local ──► Array A1: [70, 80]

After it returns:

caller.scores ───────► Array A1: [70, 80]
```

The local variable is gone, but the array remains reachable.

**Resource lifetime is another concern**

A file handle or database connection may need explicit closing even when Java manages the memory of its wrapper object.

Garbage collection is not a substitute for timely resource cleanup. We’ll use try-with-resources for that.

Also, an object may become unreachable before the end of a source-level scope if its reference will never be used again. Scope alone does not dictate exact garbage-collection timing.

---

**16. Call stacks and frames**

When one method calls another, the caller must wait for a result or an exception.

A **frame** represents the state of an active method invocation.

Conceptually, a frame includes local variables, an operand stack, and information supporting completion of the call.

The JVM defines a stack for each thread. In its abstract runtime model, frames belong to method invocations; objects and arrays belong to the shared heap. Physical implementation is left flexible. [JVM runtime areas and frames](https://docs.oracle.com/javase/specs/jvms/se21/html/jvms-2.html#jvms-2.5)

For:

```text
main calls average
```

we can draw:

```text
Top:     average frame
             scores ──► Array A1
             total = ...

Bottom:  main frame
             scores ──► Array A1
```

The references are separate parameter/local values, but they designate the same array.

When `average` returns:

1. The returned `double` is supplied to the caller.
2. The active `average` invocation completes.
3. `main` resumes after the call.

If an exception propagates instead, execution does not resume as though the method returned a normal result.

---

**17. Recursion: a method calls itself**

Recursion solves a problem using smaller instances of the same problem.

Two requirements are essential:

1. A **base case** that stops recursion.
2. Progress toward that base case.

Consider the sum:

```text
sumTo(3) = 3 + 2 + 1
```

Recursive definition:

```text
sumTo(0) = 0
sumTo(n) = n + sumTo(n - 1), for n > 0
```

Complete program:

```java
public class RecursionDemo {
    static long sumTo(int n) {
        if (n < 0) {
            throw new IllegalArgumentException(
                    "n must be nonnegative");
        }

        if (n == 0) {
            return 0;
        }

        return n + sumTo(n - 1);
    }

    public static void main(String[] args) {
        System.out.println(sumTo(3));
    }
}
```

Expected output:

```text
6
```

**Calls grow first**

```text
sumTo(3) needs 3 + sumTo(2)
sumTo(2) needs 2 + sumTo(1)
sumTo(1) needs 1 + sumTo(0)
sumTo(0) returns 0
```

At the deepest point:

```text
Top:     sumTo frame: n = 0
         sumTo frame: n = 1, waiting for result
         sumTo frame: n = 2, waiting for result
         sumTo frame: n = 3, waiting for result
Bottom:  main frame
```

Each invocation has a separate `n`.

**Then results return**

```text
sumTo(0) returns 0
sumTo(1) returns 1 + 0 = 1
sumTo(2) returns 2 + 1 = 3
sumTo(3) returns 3 + 3 = 6
```

**Complexity and alternatives**

For nonnegative input `n`, the recursive version makes `n + 1` calls.

| Approach | Time | Auxiliary space | Main tradeoff |
|---|---:|---:|---|
| Recursion | O(n) | O(n) | Clear recurrence; stack grows with input |
| Loop | O(n) | O(1) | Avoids recursive stack growth |
| Formula | O(1) | O(1) | Requires the arithmetic identity and safe types |

A loop version:

```java
static long sumToIterative(int n) {
    if (n < 0) {
        throw new IllegalArgumentException("n must be nonnegative");
    }

    long total = 0;

    for (long i = 1; i <= n; i++) {
        total += i;
    }

    return total;
}
```

The `long` loop counter avoids wrapping when incrementing beyond the largest possible positive `int` input.

A formula version:

```java
static long sumToFormula(int n) {
    if (n < 0) {
        throw new IllegalArgumentException("n must be nonnegative");
    }

    return (long) n * (n + 1L) / 2;
}
```

The conversions occur before arithmetic that could overflow `int`.

These complexity statements count fixed-width arithmetic operations. Each version returns one `long`, so output space is O(1).

For this particular problem, the formula is preferable. Recursion becomes more useful when the problem itself has a recursive structure, such as traversing a tree.

---

**18. Stack versus heap: use the model carefully**

| Aspect | JVM stack | JVM heap |
|---|---|---|
| Main role | Active method execution | Objects and arrays |
| Ownership in JVM model | Per thread | Shared among threads |
| Typical contents in conceptual diagrams | Frames, parameter/local values, operand stacks | Object state and array elements |
| How usage ends | Calls complete and frames cease being active | Objects become unreachable and may be reclaimed |

Avoid these inaccurate shortcuts:

```text
“All primitives live on the stack.”
“All references live on the stack.”
“Every object must be physically allocated on the heap.”
```

A primitive can be an object field or array element. A reference can also be stored inside another object.

The JVM specification gives an abstract execution model. Optimizations can eliminate allocations, keep values in registers, or transform execution while preserving required behavior.

Also, the stack is not the only alternative to the heap. Runtime memory includes class-related data, native allocations, generated code, and other areas we’ll study later.

---

**19. Managed memory versus manual memory management**

In Java, you normally request objects and let the runtime reclaim unreachable object memory.

```java
int[] values = new int[100];
```

You do not ordinarily call a Java equivalent of `free(values)`.

In manual memory-management systems, code may be responsible for explicitly releasing allocated memory. Mistakes can include releasing memory too early, releasing it twice, or never releasing it.

Java’s managed memory avoids many such errors, but it does not remove all memory problems.

For example, a collection can keep references to objects your application no longer needs:

```text
Long-lived collection
    ├──► useful object
    ├──► obsolete object
    └──► obsolete object
```

Those obsolete objects are still reachable. The collector cannot infer that your application no longer wants them.

That is a common form of Java memory leak: **unwanted retention**.

---

**20. Stack overflow versus heap exhaustion**

| Failure | Typical cause | Typical diagnostic |
|---|---|---|
| Stack overflow | Too many active nested calls | `StackOverflowError` |
| Java heap exhaustion | Unable to allocate required object memory | `OutOfMemoryError`, often with `Java heap space` |

A recursion error can come from:

- No base case.
- A base case that is never reached.
- Correct recursion whose depth is simply too large.

Even a mathematically valid call such as `sumTo` with a very large positive input can exhaust the stack.

Heap exhaustion can come from:

- Retaining too many objects.
- Creating an enormous array.
- Processing a dataset larger than the configured heap can support.

`OutOfMemoryError` does not always mean the Java heap is full; it can also describe other allocation failures.

Increasing memory limits is not automatically the correct fix. First identify whether the algorithm, retention behavior, or resource requirement is the problem.

Java does not guarantee tail-call optimization, so do not rely on recursive calls being converted into constant-space loops.

---

**21. Practice and debugging checks**

Try these without immediately looking back.

**Predict the result**

```java
int first = 5;
int second = first;
second++;
System.out.println(first);
```

```java
int[] first = {5};
int[] second = first;
second[0]++;
System.out.println(first[0]);
```

Explain why the two assignments have different consequences.

**Find the loop bug**

```java
int[] values = {10, 20, 30};

for (int i = 0; i <= values.length; i++) {
    System.out.println(values[i]);
}
```

Identify the first invalid index and the correction.

**Explain the method behavior**

Why can a method change an array element visible to its caller, but cannot replace the caller’s reference merely by reassigning its parameter?

**Implementation exercise**

Write:

```java
static int countPassing(int[] scores)
```

Requirements:

- Reject `null`.
- Reject any score outside 0–100.
- Treat 50 or above as passing.
- Return zero for an empty array.
- Return the count instead of printing inside the method.

Test:

```text
{}                 → 0
{49}               → 0
{50}               → 1
{0, 50, 100}       → 2
{-1}               → exception
{101}              → exception
null               → exception
```

Before coding, state the algorithm and predict the time and auxiliary-space complexity.

**Revision note:** Variables hold values; reference assignments copy references. Conditions choose paths, loops repeat work, and methods create separate invocations with their own parameters and local state. Java passes every argument by value. Scope, object reachability, and resource lifetime are different concepts.

**Progress:** Modules 6–8 are introduced. Your practice answers will tell us which topics are secure and which need reinforcement.

---

<a id="lesson-9"></a>

## 9. Arrays, text, and shared objects

The easiest way to remember this module is to keep **three rules** in mind:

> **Assignment copies a value.**  
> **A reference value identifies an object.**  
> **Changing an object and changing a reference are different operations.**

We’ll apply these rules to arrays, strings, methods, and wrapper objects.

All examples use **Java 21**. Outputs are expected, not executed here.

---

**1. Think of a reference as an arrow**

Consider:

```java
int age = 25;
int[] scores = {70, 80, 90};
```

Conceptually:

```text
age = 25

scores ─────► Array A1
              [70, 80, 90]
```

- `age` holds an integer value.
- `scores` holds a reference to an array object.

The arrow is a teaching aid. It is not a claim about a physical address or how the JVM stores references.

**Memory cue: “The variable holds the arrow; the object holds the data.”**

This cue applies to reference variables. Primitive variables hold primitive values.

---

**2. Arrays: a fixed number of numbered slots**

An array groups elements of one component type.

```java
int[] scores = {70, 80, 90};
```

Its positions are numbered from zero:

| Index | Value |
|---:|---:|
| 0 | 70 |
| 1 | 80 |
| 2 | 90 |

Read or change an element:

```java
System.out.println(scores[0]);  // 70
scores[1] = 85;
```

Now the array contains:

```text
[70, 85, 90]
```

**Length is fixed; elements can change.**

```java
scores.length   // 3
```

You cannot resize that array object. To hold more elements, create another array or later use a resizable collection such as `ArrayList`.

Another way to create an array:

```java
int[] scores = new int[3];
```

This creates three elements initialized to zero:

```text
[0, 0, 0]
```

Reference-array elements initially contain `null`:

```java
String[] names = new String[3];
```

```text
[null, null, null]
```

**Boundary rule**

For an array of length `n`, valid indices are:

```text
0 through n - 1
```

Therefore:

```java
scores[3]
```

is invalid for a three-element array and throws `ArrayIndexOutOfBoundsException`.

An empty array is valid:

```java
int[] empty = new int[0];
```

Its length is zero, and it has no valid element index.

**Memory cue: “Length is the count; the last index is count minus one.”**

---

**3. Aliasing: two arrows to the same object**

**Aliasing** means accessing the same object through different references.

```java
int[] first = {10, 20};
int[] second = first;
```

Assignment copies the reference:

```text
first  ──┐
         ├──► Array A1: [10, 20]
second ──┘
```

Now:

```java
second[0] = 99;
```

changes A1:

```text
first  ──┐
         ├──► Array A1: [99, 20]
second ──┘
```

Both variables see the change because there is only one array.

**Reassignment changes the arrow**

```java
second = new int[] {7, 8};
```

Now:

```text
first  ─────► Array A1: [99, 20]

second ─────► Array A2: [7, 8]
```

A1 was not modified by this reassignment.

**Memory cue:**

```text
second[0] = 99;           Change the object.
second = new int[]{7, 8}; Change the arrow.
```

A complete example:

```java
import java.util.Arrays;

public class ArraySharing {
    public static void main(String[] args) {
        int[] first = {10, 20};
        int[] second = first;

        second[0] = 99;
        System.out.println(Arrays.toString(first));

        second = new int[] {7, 8};

        System.out.println(Arrays.toString(first));
        System.out.println(Arrays.toString(second));
    }
}
```

Expected output:

```text
[99, 20]
[99, 20]
[7, 8]
```

`Arrays.toString` produces a readable representation of the elements. Printing an array directly does not normally display its contents.

---

**4. Copying an array is different from copying its reference**

To create an independent copy of a primitive array:

```java
int[] original = {10, 20};
int[] copy = original.clone();

copy[0] = 99;
```

Result:

```text
original ─────► Array A1: [10, 20]
copy     ─────► Array A2: [99, 20]
```

But copying an array of references copies those references. It does not automatically copy the referenced objects.

This becomes especially important with multidimensional arrays.

---

**5. Multidimensional arrays are arrays of arrays**

```java
int[][] grid = {
    {10, 20},
    {30, 40}
};
```

Read an element using two indices:

```java
grid[1][0]   // 30
```

Read that as:

> Select row 1, then select element 0 from that row.

The conceptual structure is:

```text
grid ──► Outer array
          [0] ──► Row R1: [10, 20]
          [1] ──► Row R2: [30, 40]
```

Rows can have different lengths:

```java
int[][] rows = {
    {10, 20, 30},
    {40}
};
```

This is often called a **jagged array**.

Loop using each row’s own length:

```java
for (int row = 0; row < rows.length; row++) {
    for (int column = 0; column < rows[row].length; column++) {
        System.out.println(rows[row][column]);
    }
}
```

A row can also be `null`:

```java
int[][] rows = new int[2][];
```

This creates the outer array, but no inner arrays yet.

**The shallow-copy trap**

```java
int[][] original = {
    {10, 20},
    {30, 40}
};

int[][] copy = original.clone();
copy[0][0] = 99;
```

`original[0][0]` also becomes `99`.

Why? Only the outer array was copied:

```text
original ──► Outer O1 ──┐
                       ├──► shared Row R1: [99, 20]
copy     ──► Outer O2 ──┘
```

To independently copy this `int[][]`, copy each row too:

```java
int[][] copy = new int[original.length][];

for (int row = 0; row < original.length; row++) {
    copy[row] =
            original[row] == null ? null : original[row].clone();
}
```

For arrays containing more complex mutable objects, the required depth of copying depends on the structure.

**Memory cue: “Copying a container does not automatically copy everything inside it.”**

---

**6. Strings: text that cannot be mutated**

A `String` is **immutable**: once created, its text does not change.

```java
String first = "Java";
String second = first;

first = first.concat(" 21");
```

Conceptually:

```text
Before:

first  ──┐
         ├──► String S1: "Java"
second ──┘

After:

first  ─────► String S2: "Java 21"
second ─────► String S1: "Java"
```

The original string was not modified. `first` was reassigned to the result.

A common mistake:

```java
String text = "Java";
text.concat(" 21");

System.out.println(text);
```

Output:

```text
Java
```

The returned result was ignored.

Correction:

```java
text = text.concat(" 21");
```

String methods preserve the original string’s content. A method may return another string or reuse an existing one when appropriate; do not assume every call allocates a new object. [Java 21 `String` documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html)

**Useful operations**

| Expression | Meaning |
|---|---|
| `text.length()` | Number of UTF-16 code units |
| `text.isEmpty()` | Whether length is zero |
| `text.charAt(0)` | First UTF-16 code unit |
| `text.substring(1, 3)` | Units at positions 1 and 2 |
| `text.contains("av")` | Whether that sequence occurs |
| `text.equals(other)` | Whether string contents match |

Remember from our Unicode lesson: `length()` does not necessarily count visible characters.

**Memory cue: “String edits return a result; keep the result if you need it.”**

---

**7. Identity versus equality**

**Identity:** are these references pointing to the same object?

**Equality:** do these objects represent equal values according to their class’s rules?

For references:

```java
first == second
```

tests identity.

For strings:

```java
first.equals(second)
```

compares content.

Example:

```java
String first = "Java";
String second = new String("Java");
```

```text
first  ─────► String S1: "Java"
second ─────► String S2: "Java"
```

Therefore:

```java
first == second       // false
first.equals(second)  // true
```

The explicit constructor is useful for this demonstration; ordinary code should normally use the literal directly.

**Why `==` sometimes seems to work with strings**

```java
String first = "Java";
String second = "Java";
```

String literals are interned, so these references designate the same string object.

But this does not make `==` the correct content comparison.

**Arrays need a different comparison**

```java
int[] first = {1, 2};
int[] second = {1, 2};
```

| Expression | Result |
|---|---|
| `first == second` | `false` |
| `first.equals(second)` | `false` |
| `Arrays.equals(first, second)` | `true` |

Arrays do not override `equals` to compare their elements.

For nested arrays, `Arrays.deepEquals` compares nested contents. [Java 21 array utilities](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Arrays.html)

**Memory cue: “`==` asks same object; content comparison depends on the type.”**

---

**8. `StringBuilder`: text you can change**

Use `StringBuilder` when constructing text through repeated updates.

```java
StringBuilder builder = new StringBuilder("Java");
builder.append(" 21");
```

The builder’s content changes:

```text
builder ──► Builder B1: "Java 21"
```

If two variables refer to that builder, both observe mutations.

```java
public class TextDemo {
    public static void main(String[] args) {
        String first = "Java";
        String second = first;

        first = first.concat(" 21");

        System.out.println(first);
        System.out.println(second);

        StringBuilder builder = new StringBuilder("Java");
        StringBuilder alias = builder;

        alias.append(" 21");

        System.out.println(builder);
        System.out.println(alias);
    }
}
```

Expected output:

```text
Java 21
Java
Java 21
Java 21
```

**Convert the completed text to a string**

```java
String result = builder.toString();
```

Later builder changes do not mutate `result`.

| Operation | Effect |
|---|---|
| `append(...)` | Add text at the end |
| `insert(...)` | Insert text |
| `delete(...)` | Remove a range |
| `setCharAt(...)` | Replace a UTF-16 unit |
| `toString()` | Obtain a string representing current content |

A `StringBuilder` is mutable and does not provide built-in synchronization for shared concurrent use. Also, its `equals` method does not compare textual content. [Java 21 `StringBuilder` documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/StringBuilder.html)

For content comparison:

```java
firstBuilder.toString().equals(secondBuilder.toString())
```

**Which should you choose?**

- Use `String` for ordinary text values.
- Use `+` for a simple expression such as `"Hello, " + name`.
- Use `StringBuilder` when repeatedly accumulating text, especially in a loop.

**Memory cue: “String is a finished text value; StringBuilder is an editable draft.”**

---

**9. `null`: no object is referenced**

Compare:

```java
String first = null;
String second = "";
```

Conceptually:

```text
first  ──► no object

second ──► String S1: ""
```

An empty string is a real object with zero-length content.

Likewise:

```java
int[] missing = null;
int[] empty = new int[0];
```

These are different states.

```java
missing.length  // NullPointerException
empty.length    // 0
```

Before using a possibly missing reference, decide the intended behavior.

```java
if (text != null && !text.isEmpty()) {
    System.out.println(text);
}
```

Because `&&` short-circuits, `isEmpty()` is not called when `text` is `null`.

For null-safe equality between two possibly null references:

```java
java.util.Objects.equals(first, second)
```

This delegates to the object’s equality rules when appropriate; it does not turn array equality into element-by-element comparison.

**Memory cue: “Empty means an object with no contents; null means no object.”**

---

**10. Pass-by-value: the method gets its own copy of the arrow**

Java always passes arguments by value.

For a primitive, the copied value is a number, boolean, or other primitive.

For an object, the copied value is a reference.

```java
public class MethodDemo {
    static void change(int[] values) {
        values[0] = 99;
        values = new int[] {500};
    }

    public static void main(String[] args) {
        int[] numbers = {10};

        change(numbers);

        System.out.println(numbers[0]);
    }
}
```

Expected output:

```text
99
```

At method entry:

```text
main frame:
    numbers ────┐
                ├──► Array A1: [10]
change frame:   │
    values ─────┘
```

After mutation:

```text
numbers, values ──► Array A1: [99]
```

After parameter reassignment:

```text
main:
    numbers ──► Array A1: [99]

change:
    values ───► Array A2: [500]
```

Reassigning `values` does not reassign `numbers`.

When `change` returns, its active frame is removed. A2 has not escaped this method and can be collected once unreachable. A1 remains reachable through `numbers`.

These are conceptual execution diagrams, not guaranteed physical layouts. Collection eligibility does not guarantee immediate collection.

**Memory cue: “A method receives a copied arrow, not control over your variable.”**

The same rule explains:

| Parameter type | What the method can do |
|---|---|
| `int` | Change its own local parameter |
| `int[]` | Mutate the shared array or reassign its local reference |
| `String` | Read the immutable string or reassign its local reference |
| `StringBuilder` | Mutate the shared builder or reassign its local reference |

---

**11. Wrappers: object forms of primitive values**

Wrapper classes let primitive values participate where objects are required.

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

For example, generic collections use reference types:

```java
ArrayList<Integer>
```

not:

```java
ArrayList<int>   // Invalid
```

We’ll teach collections later.

**Boxing: primitive → wrapper**

```java
Integer boxed = 10;
```

Java automatically boxes the integer value.

**Unboxing: wrapper → primitive**

```java
int number = boxed;
```

Java extracts the primitive value.

**Pitfall 1: unboxing `null`**

```java
Integer count = null;
int result = count;
```

This throws `NullPointerException`.

The same hidden unboxing occurs here:

```java
count + 1
```

Use a primitive when absence is not meaningful, or handle missing values explicitly.

**Pitfall 2: wrapper identity**

```java
Integer first = 100;
Integer second = 100;
```

Boxing can reuse wrapper objects. Some identities are guaranteed for specified small constant values; others must not be relied upon.

Therefore, do not use wrapper `==` to express value equality. These boxing and unboxing rules are specified by Java. [Java 21 conversions](https://docs.oracle.com/javase/specs/jls/se21/html/jls-5.html#jls-5.1.7)

Use:

```java
first.equals(second)
```

or, if either may be null:

```java
java.util.Objects.equals(first, second)
```

**Pitfall 3: different wrapper types**

```java
Integer.valueOf(10).equals(Long.valueOf(10))
```

is `false`: these are different wrapper types.

**Pitfall 4: wrappers are immutable**

```java
Integer first = 10;
Integer second = first;
first++;
```

`first++` unboxes, increments, and boxes the result before reassigning `first`. It does not modify the original wrapper.

Result:

```text
first  represents 11
second represents 10
```

**Memory cue: “Box to use an object; unbox to calculate; check null before unboxing.”**

---

**12. Remember the main performance costs**

Let `n` be the number of array elements or text units.

| Operation | Typical cost |
|---|---|
| Access an array element by index | O(1) |
| Visit every array element | O(n) |
| Copy an array | O(n) time and O(n) output space |
| Compare two equal-length primitive arrays | O(n) worst case |
| Visit a jagged array | O(r + N), for `r` rows and `N` total elements |
| Build `n` text units by repeated small appends | Typically O(n) total with amortized buffer growth |
| Create the final string from a builder | Allow O(n) copying time and output space |

Repeatedly building a longer string with:

```java
text = text + piece;
```

can repeatedly copy the accumulated prefix, producing quadratic total work for many small pieces. A builder avoids that repeated-prefix pattern.

These are algorithmic cost models, not exact timing or memory-size guarantees.

---

**13. Your one-minute revision card**

| Concept | Remember |
|---|---|
| Array | Fixed number of mutable slots |
| Multidimensional array | Array whose elements refer to arrays |
| Aliasing | Multiple arrows to the same object |
| Mutation | Change the object |
| Reassignment | Change the arrow |
| `String` | Immutable text |
| `StringBuilder` | Mutable text construction |
| `null` | No object reference |
| Reference `==` | Same object? |
| `.equals()` | Equality defined by the class |
| Pass-by-value | Copy the value—including a reference value |
| Boxing/unboxing | Primitive ↔ wrapper |

**Practice—predict before running**

1. What is printed?

```java
int[] a = {5};
int[] b = a;
b[0] = 8;
System.out.println(a[0]);
```

2. What is printed?

```java
String a = "Hi";
String b = a;
a = a + "!";
System.out.println(b);
```

3. What is printed?

```java
StringBuilder a = new StringBuilder("Hi");
StringBuilder b = a;
b.append("!");
System.out.println(a);
```

4. Why does this fail?

```java
Integer value = null;
System.out.println(value + 1);
```

**Coding exercise:** Write a method that takes an `int[]` and returns a new array containing twice each value, without changing the original. Reject `null`; allow an empty array. For this exercise, assume every doubled value fits in `int`.

Explain which objects are shared, which are new, and the time and space complexity.

**Progress:** Arrays, text, shared objects, and wrapper pitfalls are introduced. Your predictions will help confirm whether mutation versus reassignment is clear.

---

<a id="lesson-10"></a>

## 10. Mutation and equality quick reference

**An array, a `String`, and a `StringBuilder` are all objects.** Whether an object can be changed depends on its type.

- **Mutation:** change the existing object.
- **Reassignment:** make a variable refer to a different object.

**1. Mutation differences—in one example**

```java
public class MutationDemo {
    static class Box {
        int value = 10;
    }

    public static void main(String[] args) {
        // Custom mutable object
        Box box = new Box();
        Box boxAlias = box;
        box.value = 20;
        System.out.println(boxAlias.value);   // 20

        // Array: elements can change; length cannot
        int[] numbers = {10};
        int[] arrayAlias = numbers;
        numbers[0] = 20;
        System.out.println(arrayAlias[0]);    // 20

        // String: its text cannot change
        String text = "Hi";
        String textAlias = text;
        text = text + "!";                    // Reassignment
        System.out.println(textAlias);       // Hi
        System.out.println(text);            // Hi!

        // StringBuilder: its contents can change
        StringBuilder builder = new StringBuilder("Hi");
        StringBuilder builderAlias = builder;
        builder.append("!");                  // Mutation
        System.out.println(builderAlias);    // Hi!
    }
}
```

Expected output—not executed here:

```text
20
20
Hi
Hi!
Hi!
```

The concept:

```text
MUTATION
a ──┐                          a ──┐
    ├──► object: 10    →           ├──► same object: 20
b ──┘                          b ──┘

REASSIGNMENT
a ──┐                          a ─────► different object
    ├──► original      →
b ──┘                          b ─────► original
```

| Type | What can change? |
|---|---|
| Custom object | Depends on class design; `Box` above is mutable |
| Array | Elements can change; array length stays fixed |
| `String` | Existing text cannot change |
| `StringBuilder` | Existing contents and length can change |

**Remember:** changing a shared object is visible through its aliases. Reassigning one variable does not reassign the others.

---

**2. `==` versus `equals()`**

> **Primitives: `==` compares values.**  
> **References: `==` compares identity.**  
> **`equals()` follows the class’s equality rules.**

```java
int x = 10;
int y = 10;
System.out.println(x == y);       // true: primitive values

String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);       // false: different objects
System.out.println(a.equals(b));  // true: same string content
```

`new String(...)` is used here to deliberately create distinct objects; ordinary code usually uses string literals.

**Important: `equals()` does not automatically mean “compare contents.”**

The implementation inherited from `Object` compares identity. Classes such as `String` override it to define value equality. Your own class must override it if you want custom value equality.

---

**3. Which equality operation should you use?**

| Situation | Operation | Meaning |
|---|---|---|
| Primitive numbers | `a == b` | Numeric equality; floating-point accuracy still matters |
| References | `a == b` | Same object, or both `null` |
| Strings | `a.equals(b)` | Same exact string content |
| Strings ignoring case | `a.equalsIgnoreCase(b)` | Case-insensitive comparison |
| Possibly null objects | `Objects.equals(a, b)` | Null-safe use of the class’s `equals` |
| One-dimensional arrays | `Arrays.equals(a, b)` | Corresponding elements are equal |
| Nested arrays | `Arrays.deepEquals(a, b)` | Recursively compare nested array contents |
| String versus builder | `text.contentEquals(builder)` | Same character sequence |
| Two builders | `a.toString().equals(b.toString())` | Same text |
| Custom objects | Override `equals()` | Define what equality means for that class |

For object-array elements, `Arrays.equals` uses their equality rules; it does not recursively compare nested arrays.

**Examples together**

```java
import java.util.Arrays;
import java.util.Objects;

// These statements can go inside main:

System.out.println("Java".equals("java"));            // false
System.out.println("Java".equalsIgnoreCase("java"));  // true

System.out.println(Objects.equals(null, null));       // true
System.out.println(Objects.equals(null, "Java"));     // false

int[] a = {1, 2};
int[] b = {1, 2};

System.out.println(a == b);                          // false
System.out.println(a.equals(b));                     // false
System.out.println(Arrays.equals(a, b));             // true

int[][] first = {{1, 2}};
int[][] second = {{1, 2}};

System.out.println(Arrays.equals(first, second));     // false
System.out.println(Arrays.deepEquals(first, second)); // true

StringBuilder left = new StringBuilder("Hi");
StringBuilder right = new StringBuilder("Hi");

System.out.println(left.equals(right));              // false
System.out.println(left.toString().equals(
        right.toString()));                         // true
System.out.println("Hi".contentEquals(left));         // true
```

**Three traps to remember**

1. **`null.equals(...)` throws `NullPointerException`.** Use `Objects.equals` when either value may be null.
2. **String literals and boxed numbers can share objects.** Do not let an occasional `true` from reference `==` convince you it compares content.
3. **If you override `equals()`, also override `hashCode()`.** Equal objects must produce equal hash codes for hash-based collections to work correctly.

**Memory line:** **“Same object? `==`. Same meaning? Choose the type’s equality operation.”**

---


