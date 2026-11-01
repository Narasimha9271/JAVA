# Java Short Notes

Personal revision notes covering computer foundations, Java execution and memory, arrays and strings, and OOP videos 1–4. Converted from the supplied pasted text; repeated explanations and examples are retained.

Examples are independent snippets. Repeated class names belong to separate examples; add imports and a suitable class or method wrapper as needed. Outputs are expected illustrations, not execution results.

## Contents

- [Files, operating systems, processes, and threads](#foundations)
- [Java execution, JVM, JRE, and JDK](#execution)
- [Values, types, operators, and conversions](#values)
- [Pass-by-value and memory](#memory)
- [Arrays, strings, wrappers, and references](#arrays)
- [Mutation and equality revision](#revision)
- [Object-oriented programming](#oop)
- [Video 1 — Classes, objects, constructors, and keywords](#video-1)
- [Video 2 — Packages, static, and nested classes](#video-2)
- [Video 3 — The four OOP principles](#video-3)
- [Video 4 — Access control, packages, and Object](#video-4)

[Full OOP study guide](README_OOPS.md) · [Java foundations](README.md)

<a id="foundations"></a>

## Files, operating systems, processes, and threads

A file is a named collection of data managed by a file system. Examples include source code, photographs, documents, and compiled programs.

A file has content and associated information, such as its size, timestamps, and access permissions.

A directory, also called a folder, organizes named entries for files and other directories.

A file system provides the rules and structures for storing, naming, finding, and accessing these items.

| Term | Role |
|---|---|
| **Terminal** | Provides the interface for typing input and displaying textual output |
| **Shell** | Interprets command syntax and coordinates commands |
| **Command** | An instruction you ask the shell to perform |
| **Argument** | Information supplied to a command |
| **Option** | An argument that changes a command’s behavior |

- the terminal is the visual window,
- the shell is the engine running behind the scenes
- the command line is the literal text space where you type

| Name | Purpose |
|---|---|
| `PATH` | Helps locate external executables |
| `JAVA_HOME` | Convention used by tools to identify a Java installation |
| Java classpath | Tells Java where to locate application classes and libraries |

### The operating system: coordinating and protecting resources

An operating system, such as Windows, manages the computer’s resources and supplies services to applications.

Its responsibilities include:

| Resource or problem | Operating-system responsibility |
| --- | --- |
| CPU access | Schedule runnable threads |
| Memory | Manage address spaces, mappings, and protection |
| Files | Provide access to file systems |
| Devices | Coordinate access through device drivers |
| Security | Enforce permissions and access rules |
| Processes | Create, manage, and terminate execution environments |

The privileged core of the OS is called the kernel.

Applications usually execute in user mode, where access to protected resources is restricted. When they need services such as file access, they use interfaces that ultimately request work from the OS.

A system call is a controlled entry into an operating-system service.

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

- A kernel is the central core program of an Operating System. It acts as a bridge between computer hardware and software applications

| Feature | User Mode | Kernel Mode |
| --- | --- | --- |
| Privilege Level | Unprivileged / Restricted mode (Mode Bit: 1) | Privileged / Master / Supervisor mode (Mode Bit: 0) |
| Hardware Access | No direct access to hardware (CPU, disk, network, I/O) | Full, unrestricted access to all hardware and physical resources |
| Memory Access | Isolated private virtual address space per process; cannot touch kernel memory | Shared single virtual address space; can access all user and kernel memory |
| What Runs Here | User applications, web browsers, text editors, daemons | The core operating system kernel and critical device drivers |

### Program   vs   process

A program is code and associated data describing work.

A process is an operating-system-managed instance of execution.

### What is a virtual address space?

It is the address space through which a process accesses memory.

The OS and hardware translate virtual addresses to backing memory. A process does not normally see every other process’s memory as directly accessible.

Two processes can therefore use the same numerical virtual address without referring to the same physical memory.

Processes can deliberately communicate or share memory using appropriate mechanisms. Ordinary Java references do not automatically connect objects across separate JVM processes.

### A process is not the same as a window

- One application can use several processes.
- A background process may have no visible window.
- Several windows can belong to one process.

Task Manager gives you a useful view of processes, but the visible application layout is not a reliable process diagram.

### Threads: execution inside a process

A thread is an execution flow within a process.

A process supplies resources; threads perform work using those resources.

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

A call stack tracks active method calls. A stack frame contains the state associated with one active call.

### Concurrency versus parallelism

- Concurrency: several activities make progress over overlapping periods.
- Parallelism: activities execute simultaneously on different execution resources.

### Scheduling: deciding who gets CPU time

A scheduler selects runnable threads to execute.

A useful simplified state model is:

| State | Meaning |
| --- | --- |
| Ready | Can execute but is waiting for CPU access |
| Running | Currently executing |
| Waiting | Cannot proceed until some event occurs |
| Finished | Execution has ended |

Running → Waiting → Ready → Running

While it waits, another thread can use the CPU.

A thread can also lose CPU access while still runnable. This is preemption: the OS interrupts its execution so another thread can run.

Scheduling decisions depend on factors such as priorities, available processors, and waiting events. You should not assume that starting thread A before thread B guarantees that A prints first.

### CPU-bound versus I/O-bound work

- CPU-bound: progress is mainly limited by computation.
- I/O-bound: progress is mainly limited by waiting for files, networks, or devices.

More threads do not automatically speed up CPU-bound work. Once available CPU resources are busy, extra runnable threads can add overhead.

Concurrency can help I/O-bound applications perform other useful work while some operations wait.

### Context switching: how execution changes hands

A context switch transfers execution from one thread to another.

The execution context includes items such as:

- The instruction pointer: where execution should continue.
- Registers: small, fast CPU storage locations.
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

Windows can switch because a running thread needs to wait, its scheduling interval expires, or a higher-priority thread becomes ready. Waiting threads are not eligible to run until the relevant conditions change

### What a switch does not mean

The OS does not ordinarily copy the thread’s entire stack and all the process’s objects elsewhere on every switch. They remain in memory; the saved context lets execution resume.

Switching between threads in different processes can also require changing address-space context.

### Why switching has a cost

Saving and restoring state takes work. Switching can also disturb useful cached data and address translations, reducing performance after execution resumes.

The exact cost depends on hardware, workload, and OS behavior. There is no universal fixed duration.

### Three operations that are easy to confuse

| Operation | What changes |
| --- | --- |
| Method call | Active calls within the current thread |
| System call | Execution enters an OS service |
| Context switch | A different thread takes over execution |

A system call may lead to waiting and a context switch, but it does not have to.

<a id="execution"></a>

## Java execution, JVM, JRE, and JDK

A just-in-time compiler translates code during execution.

HotSpot, a common JVM implementation, can combine interpretation with compilation. It gathers execution information and compiles selected code into machine code, potentially optimizing it further as the program runs.

### Is Java compiled or interpreted?

Both mechanisms can participate:

```text
Source → compiled to bytecode
Bytecode → interpreted and/or compiled for execution
```

Calling Java simply “interpreted” misses an important part of its execution model.

Java is both compiled and interpreted.

Calling it strictly one or the other misses the hybrid execution model that makes the language unique.Instead of choosing between translating everything to machine code upfront (like C or C++) or translating it line-by-line at runtime (like traditional scripting languages),

Java uses a two-stage pipeline.

Phase 1: Source to Bytecode (The Ahead-of-Time Compilation)

- When you write Java code (.java files) and run the Java compiler (javac), your human-readable source code is compiled into bytecode (.class files).What it is: Bytecode is an intermediate, platform-independent instruction set. It is not machine code (which your CPU directly understands), but it is much lower-level and structured than your original source code.Why it matters: This is where Java achieves its famous "Write Once, Run Anywhere" capability. The bytecode remains identical whether you run it on Windows, macOS, or Linux, because it targets the Java Virtual Machine (JVM) rather than a specific physical processor.

Phase 2: Bytecode Execution (Interpretation + JIT Compilation)

- Once you execute your program, the JVM takes over using a combination of two mechanisms:
1. The Interpreter:  The JVM reads the bytecode instructions and translates them into native machine instructions one line at a time as the program runs. Because interpretation happens on the fly, starting up a purely interpreted program is fast, but running it continuously can be slow due to the overhead of translation.
2. The Just-In-Time (JIT) Compiler: To solve the performance bottleneck of pure interpretation, modern JVMs include a JIT compiler. As the program runs, the JVM monitors which methods or loops are executed frequently (called "hot spots").When it identifies a hot spot, the JIT compiler compiles that specific piece of bytecode directly into native machine code for your specific CPU.That native code is then cached, so future executions of that code run at native hardware speed, bypassing the interpreter entirely.

| Term | Meaning |
|---|---|
| Java language | Rules for writing Java programs |
| Java SE | Standard platform specifications and APIs |
| JVM | Execution environment implementing the JVM model |
| JDK | Development kit containing tools such as `javac` and `java` |
| JRE | Historical name associated with a runtime distribution without the full development toolset |

```text
JDK
├── Development tools
└── Runtime environment
    ├── JVM
    └── Standard libraries and supporting components
	
```

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

### What compilation actually does

A compiler translates a program from one representation into another.

Conceptually, javac:

1. Reads the source text.
2. Recognizes its grammatical structure.
3. Checks names, types, and other language rules.
4. Produces class files if compilation succeeds.

For example:

```java
int count = "five";
```

is rejected because a string cannot be assigned to an int.

But:

```java
int count = 5;
```

is valid.

Compilation does not generally execute your application’s main method. It checks and translates the application.

Successful compilation does not prove that the program is correct.

A program can compile and then:

- Receive invalid input.
- Attempt to open a missing file.
- Divide an integer by zero.
- Produce the wrong answer because its algorithm is incorrect.

The compiler cannot establish every property of every possible execution.

javac specifically translates Java source into class files for the JVM. Java 21 compiler documentation

### Interpretation and JIT compilation

An interpreter implements program operations by examining and executing an intermediate representation.

In Java’s case, a bytecode interpreter processes bytecode instructions. The physical CPU executes the interpreter’s machine instructions.

A JIT compiler, meaning just-in-time compiler, translates selected code into machine code during execution.

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
Profiling here means gathering information about execution—for example, which methods execute frequently.
```

Optimization transforms execution to improve performance while preserving required observable behavior.

Possible optimizations include:

- Inlining a method’s work into its caller.
- Eliminating calculations whose results are unused.
- Simplifying operations when runtime information permits it.

This is why short benchmark results can be misleading: execution may change as the JVM warms up.

These are implementation strategies. Java does not guarantee that every method will be JIT-compiled or specify a universal number of calls before compilation. Java 21 HotSpot documentation

### Interview question: “Is Java compiled or interpreted?”

A useful answer is:

Java source is normally compiled into bytecode. A JVM implementation can interpret bytecode and compile code into machine instructions during execution.

That explains the stages instead of attaching one label to the entire system.

### 1. JVM (Java Virtual Machine)

- What it is: The runtime engine that actually executes Java bytecode.
- What it does:
- Loads class files and verifies bytecode.
- Converts bytecode into native machine-specific instructions for your CPU using an interpreter and a Just-In-Time (JIT) compiler.
- Manages computer memory and handles automatic garbage collection.
- Platform dependency: Platform-dependent. A different JVM build is required for Windows, macOS, or Linux (even though the Java bytecode it runs remains the same everywhere).
- Who needs it: It is internal to the JRE and JDK; you rarely install a raw JVM by itself.

### 2. JRE (Java Runtime Environment)

- What it is: An installation package that provides the minimum requirements to run an already compiled Java application.
- What it contains:
- The JVM.
- Core class libraries and standard packages (such as java.lang, java.util, and java.io).
- Supporting runtime files and binaries.
- Limitation: It cannot compile Java source code (.java) into bytecode because it lacks development tools.
- Modern Note: Since Java 11, Oracle and most OpenJDK distributors no longer offer a separate, standalone JRE download; developers and end-users typically install a full JDK instead.

### 3. JDK (Java Development Kit)

- What it is: A comprehensive software development kit used to write, compile, debug, and run Java programs.
- What it contains:
- The JRE (which includes the JVM and core libraries).
- Development tools such as the Java compiler (javac), archiver (jar), debugger (jdb), and documentation generator (javadoc).

<a id="values"></a>

## Values, types, operators, and conversions

A program needs to remember information while it works.

A value is information such as 25, true, or a reference to an object.

A variable is a typed storage location that can hold a value.

A type determines which values are allowed and which operations make sense.

### 2. Local variables must be initialized before use

A local variable is declared inside a method or another local block.

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

However, fields—variables declared as members of a class—and array elements receive default values.

| Type category | Default field/array-element value |
| --- | --- |
| Integer types | Zero |
| Floating-point types | Positive zero |
| char | The zero-valued UTF-16 unit |
| boolean | false |
| Reference types | null |

Do not transfer that rule to ordinary local variables. Java distinguishes these initialization rules. Java 21 variables and initial values

Also, null is not an empty string, an empty array, or zero. It represents the absence of an object reference.

### 3. Primitive types versus reference types

Java has eight primitive types:

| Type | Represents | Example |
| --- | --- | --- |
| byte | 8-bit signed integer | byte level = 3; |
| short | 16-bit signed integer | short year = 2026; |
| int | 32-bit signed integer | int count = 100; |
| long | 64-bit signed integer | long population = 8_000_000_000L; |
| float | 32-bit binary floating-point value | float ratio = 0.5f; |
| double | 64-bit binary floating-point value | double temperature = 36.6; |
| char | One UTF-16 code unit | char grade = 'A'; |
| boolean | true or false | boolean ready = true; |

There is no language guarantee that a boolean variable occupies exactly one physical bit.

A primitive variable holds a primitive value.

```java
int first = 10;
int second = first;
second = 20;
```

Now:

```text
first  = 10
second = 20
The assignment copied the integer value.
```

A reference variable holds a reference value.

```java
int[] first = {10};
int[] second = first;
```

```text
An array is an object. Both variables now refer to the same array:
first  ──┐
         ├──► Array A1: [10]
```

second ──┘

The assignment copied a reference, not the array.

```java
second[0] = 20;
```

```text
Changes that shared array:
first  ──┐
         ├──► Array A1: [20]
```

second ──┘

Therefore, first[0] is now also 20.

A reference is not a portable numerical memory address you can perform arithmetic on. JVMs can represent references differently and can move objects while preserving program behavior.

### Common conventions:

| Item | Convention | Example |
| --- | --- | --- |
| Variable or method | lowerCamelCase | totalScore, calculateAverage |
| Class | UpperCamelCase | ScoreAnalyzer |
| Named constant | UPPER_SNAKE_CASE | MAX_SCORE |

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

### 7. Conversions, casting, and numeric promotion

A conversion changes a value’s representation or type according to Java’s rules.

A cast explicitly requests a permitted conversion.

### Widening

```java
int count = 100;
long larger = count;
```

Every int value fits exactly in long.

But widening does not always mean preserving every significant digit:

```java
int original = 16_777_217;
float approximate = original;
```

That integer cannot be represented exactly as a float.

### Narrowing

```java
double price = 19.95;
int wholePart = (int) price;
```

wholePart becomes 19. This truncates toward zero; it does not round to the nearest integer.

Integer narrowing can discard higher-order bits:

```java
int original = 130;
byte narrowed = (byte) original;
```

narrowed becomes -126.

A cast is not an overflow-validation operation.

### Numeric promotion

For ordinary binary arithmetic such as addition:

1. If an operand is double, arithmetic uses double.
2. Otherwise, if an operand is float, it uses float.
3. Otherwise, if an operand is long, it uses long.
4. Otherwise, it uses int.

Thus, arithmetic on byte, short, and char commonly produces int. Java 21 conversion and promotion rules

```java
byte first = 10;
byte second = 20;
int total = first + second;
```

### Convert before the calculation

```java
int first = 50_000;
int second = 50_000;

long wrong = first * second;
long correct = (long) first * second;
```

The first multiplication uses int and overflows before assignment to long.

### Compound assignment includes conversion

```java
byte count = 127;
```

count += 1;

This compiles and produces -128.

By contrast:

```java
count = count + 1;
```

does not compile without an explicit narrowing conversion because count + 1 has type int.

Do not assume += protects against overflow.

### Runnable calculation example

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

**Expected output:**

```text
3
3.3333333333333335
9
14
20
```

### Short-circuit evaluation

For &&, Java skips the right operand if the left operand is false:

text != null && !text.isEmpty()

If text is null, Java does not call isEmpty().

For ||, Java skips the right operand if the left operand is true.

This makes guards possible:

count != 0 && total / count > 2

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

<a id="memory"></a>

## Pass-by-value and memory

### Most Imp:

### 14. Java always passes arguments by value

This is one of the most important concepts in the course.

Java copies an argument’s value into the method’s parameter.

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

**Expected output:**

```text
10
20
20
20
60
```

### Primitive argument

At the start of changeNumber:

```text
main:
    number = 10

changeNumber:
    value = 10
Reassigning value does not change number.
```

### Reference argument

At the start of changeArray:

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
Changing A2 to [40] does not affect A1.
```

When changeArray returns, its parameter disappears from the active call. A2 has no escaping reference in this example and can be collected once unreachable. Collection is not necessarily immediate.

Later, reassigning second separates the caller’s references too:

```text
first  ─────► Array A1: [20]
second ─────► Array A3: [60]
```

Mutation changes an object. Reassignment changes which value a variable holds.

### The Big Rule to Remember

Copying a primitive: Java copies the value itself. Changes inside the method stay inside the method.

Copying an object/array: Java copies the address (remote control). If the method uses that remote to change what's inside the object/array (values[0] = 20), everyone looking at that object sees the change.

### Call stacks and frames

When one method calls another, the caller must wait for a result or an exception.

A frame represents the state of an active method invocation.

Conceptually, a frame includes local variables, an operand stack, and information supporting completion of the call.

The JVM defines a stack for each thread. In its abstract runtime model, frames belong to method invocations; objects and arrays belong to the shared heap. Physical implementation is left flexible.

| Aspect | JVM stack | JVM heap |
|---|---|---|
| Main role | Active method execution | Objects and arrays |
| Ownership in JVM model | Per thread | Shared among threads |
| Typical contents in conceptual diagrams | Frames, parameter/local values, operand stacks | Object state and array elements |
| How usage ends | Calls complete and frames cease being active | Objects become unreachable and may be reclaimed |

### Rule 1: Local variables live on the stack

Any variable declared directly inside a method (whether it's a primitive or a reference) lives on the stack. These variables are temporary and get wiped out the second the method finishes running.

```java
public class StackDemo {
    public static void main(String[] args) {
        int age = 25;                  // 1. Local primitive -> lives on the STACK
        String name = "Alice";         // 2. Local reference -> the remote control 'name' lives on the STACK
        
        calculateBonus(500);           // Calling a method creates a new stack frame
    }

    static void calculateBonus(int base) { // 3. Method parameter 'base' -> lives on the STACK
        int bonus = 50;                    // 4. Local primitive inside this method -> lives on the STACK
    } // When this method ends, 'base' and 'bonus' vanish from the stack immediately.
}
```

### Rule 2: Fields inside objects and array elements live on the heap

When variables belong to an object or an array, they live on the heap because the object or array itself lives on the heap.

```java
class Car {
    int doors = 4;                    // Field primitive -> lives on the HEAP (inside the Car object)
    String model = "Sedan";           // Field reference -> lives on the HEAP (inside the Car object)
}

public class HeapDemo {
    public static void main(String[] args) {
        Car myCar = new Car();        // 'myCar' reference is on the stack, BUT the actual Car object 
                                      // (including its 'doors' and 'model' fields) lives on the HEAP.

        int[] scores = {90, 85, 88};  // 'scores' reference is on the stack, BUT the array box 
                                      // and all its numbers (90, 85, 88) live on the HEAP.
    }
}
```

### Putting It All Together (The Big Picture)

Look at this single line:

```java
int[] numbers = {10, 20};
```

The variable numbers (the remote control) is a local variable, so it lives on the stack.

The actual array box {10, 20} and its elements (10 and 20) live on the heap.

### MYTHS:

“All primitives live on the stack.”

“All references live on the stack.”

“Every object must be physically allocated on the heap.”

### 1. “All primitives live on the stack.” ❌ False

The Reality: Only local primitives (variables created inside a method) live on the stack.

The Exception: If a primitive is part of an object (like a field: class Player { int score; }) or inside an array (int[] scores = ...), it lives on the heap, because the object or array itself lives on the heap.

### 2. “All references live on the stack.” ❌ False

The Reality: Just like primitives, reference variables only live on the stack if they are local variables inside a method.

The Exception: If an object contains a reference to another object (for example, a Car object that has an Engine reference field), that reference lives on the heap right inside the Car object.

### 3. “Every object must be physically allocated on the heap.” ❌ False (technically)

The Reality: Conceptually and historically, yes—objects live on the heap. However, modern Java Virtual Machines (JVMs) are extremely smart.

The Exception (Escape Analysis): If a method creates a small object, and the JVM realizes that the object never leaves that method (it doesn't "escape"), the JVM can optimize it. It might allocate that object directly on the stack (or even optimize the object away entirely into CPU registers) to make your code run faster.

### The Two Worlds of Java Data Types

Every single variable or data type in Java falls into one of two buckets:

1. The Primitives (The 8 Non-Reference Types)

These hold raw, direct values. They do not use remote controls. There are only 8 of them in the entire Java language:

Whole numbers: byte, short, int, long

Decimals: float, double

True/False: boolean

Single letters: char

2. Reference Types (Everything Else!)

If it is not one of those 8 primitives, it is a reference type. Every single one of these uses the "remote control" memory model:

Strings: (String name = "Alice";)

Arrays: (int[] numbers, String[] words)

Custom classes you create: (Car myCar, Person user, Dog myDog)

Built-in Java classes: (Scanner, Random, ArrayList, etc.)

The Golden Rule to Spot a Reference

Does it start with a capital letter? (Like String, Scanner, Car) $\rightarrow$ It's a reference type. (By Java naming convention, class names start with uppercase letters).

Does it have square brackets ([])? $\rightarrow$ It's a reference type (an array).

Is it entirely lowercase? (int, double, boolean, etc.) $\rightarrow$ It's a primitive.

### 1. The Wrapper Family Tree

Every primitive has a corresponding wrapper class. Notice that wrapper classes always start with a capital letter:

| Primitive | Wrapper class |
|---|---|
| int | Integer |
| double | Double |
| boolean | Boolean |
| char | Character |
| long | Long |

### 2. Autoboxing and Unboxing (Java's Magic Trick)

In older versions of Java, converting between a primitive and a wrapper was annoying. Today, Java does it automatically behind the scenes:

Autoboxing (Primitive $\rightarrow$ Object): Java automatically packs a primitive into an object.

```java
Integer wrappedNum = 5; // Java automatically converts int '5' into an Integer object
```

Unboxing (Object $\rightarrow$ Primitive): Java automatically unpacks the object back into a primitive.

```java
int normalNum = wrappedNum; // Java automatically extracts the '5' out of the object
```

### 3. The Big Surprise: Wrapper Objects are Immutable

With arrays or custom objects (like Car), we saw that they are mutable—you can change what's inside them (like values[0] = 20) using the reference.

Wrapper classes (Integer, Double, Boolean, etc.) are completely different. They are immutable, meaning once an Integer object is created, its value can never be changed.

If you try to change an Integer, Java doesn't actually modify the old object. Instead, it destroys the old object and creates a brand-new one in memory!

**What happens if you pass an Integer to a method?**

Remember our changeNumber method with primitives? Watch what happens with an Integer:

```java
public class WrapperDemo {
    static void changeIt(Integer num) {
        num = 99; // Looks like it changes the value, right?
    }

    public static void main(String[] args) {
        Integer myNumber = 10;
        changeIt(myNumber);
        System.out.println(myNumber); // Prints 10!
    }
}
```

Why did it stay 10?

myNumber is a reference pointing to an Integer object holding 10.

When you pass it to changeIt(myNumber), Java copies the reference (the remote control).

Inside changeIt, when you do num = 99;, Java says, "Ah, they want a new number 99!" It creates a brand-new Integer object holding 99 and points the local num remote at it.

Meanwhile, the original myNumber remote back in main is still pointing safely at the original 10 object.

**Summary**

- Primitives: Pass-by-value (copying the raw number). Changes don't escape.
- Arrays / Custom Objects: Pass-by-reference-value (copying the remote control). Changing what's inside the object affects everyone.
- Wrapper Classes (Integer, Double, etc.): Pass-by-reference-value, but they are immutable. Because you can't change what's inside them, any attempt to change them just creates a new object, leaving the original variable completely untouched (making them behave a lot like primitives!).

### EXPLAIN:

```java
public class Main {
    static void changeIt(Integer num) {
        num = 99; // Looks like it changes the value, right?
    }
    static void changeArr(int[] arr){
        arr[0]=20;
        arr = new int[] {30};
        arr[0]=40;
    }

    public static void main(String[] args) {
        int myNumber = 10;
        changeIt(myNumber);
        System.out.println(myNumber); // Prints 10!

        int vals[] = {10};
        changeArr(vals);
        System.out.println(vals[0]);
        vals = new int[]{40};
        vals[0] = 50;
        System.out.println(vals[0]);
        
    }
}
```

### Step 1: Give the Direct Definition

"Java is strictly pass-by-value, no exceptions. This means Java always passes a copy of the variable's value into a method, never the actual variable itself."

### Step 2: Address the Confusion (Primitives vs. Objects)

"People get confused because when we pass objects or arrays, it feels like pass-by-reference. But you have to remember what that 'value' actually is:

For a primitive, the value is the raw number (like 10).

For an object or array, the value is the memory address (the reference/remote control).

In both cases, Java is copying that value and handing it to the method."

### Step 3: The Ultimate Proof (The Reassignment Test)

This is how you mathematically or logically prove to an interviewer that Java cannot be pass-by-reference.

"Here is how you can prove Java is pass-by-value: If Java were pass-by-reference, reassigning the parameter variable inside a method would change the caller's variable."

Look at what happens in changeArr from your code:

```java
static void changeArr(int[] arr){
    arr[0] = 20;            // Modifies the object on the heap
    arr = new int[] {30};   // REASSIGNS the local variable 'arr'
    arr[0] = 40;            // Modifies the new object
}
```

"If Java were pass-by-reference, doing arr = new int[] {30} would overwrite the original vals variable in main so that vals now points to {30}.

But it doesn't! After changeArr finishes, vals[0] is 20, not 30. Why? Because arr inside the method was just a copy of the reference. Reassigning the copy did nothing to the original vals variable in main."

### Step 4: Summarize for the Interviewer

Wrap it up in one neat package:

Mutating state (arr[0] = 20) works because both the original variable and the copy point to the same object on the heap.

Reassigning the variable (arr = new int[] {30}) fails to affect the caller because the variable itself was passed by value (as a copy).

### IMP:

```java
public class Main {
   

    public static void main(String[] args) {
        int first = 5;
        int second = first;
        second++;
        System.out.println(first);//5

        int[] first1 = {5};
        int[] second1 = first1;
        second1[0]++;
        System.out.println(first1[0]);//6
        
    }
}
```

<a id="arrays"></a>

## Arrays, strings, wrappers, and references

Remember this picture first:

```text
reference variable ─────► object
```

For arrays, strings, builders, and wrappers, a variable holds a reference to an object.

Most of today’s behavior follows from three questions:

1. Which object does this variable refer to?
2. Can that object change?
3. Am I changing the object or replacing the reference?

All examples use Java 21. Outputs are expected, not executed here.

### 1. Arrays: a fixed number of numbered slots

An array groups values of one element type.

```java
int[] marks = {70, 80, 90};
```

Picture:

```text
marks ──► Array A1

          index:   0    1    2
          value:  70   80   90
```

**Memory aid: “Length counts; index starts at zero.”**

marks.length   // 3

marks[0]       // 70

marks[2]       // 90

The final valid index is:

marks.length - 1

marks[3] throws ArrayIndexOutOfBoundsException.

You can change an element:

```java
marks[1] = 85;
```

Now A1 contains:

```text
[70, 85, 90]
```

An array’s length is fixed after creation. To obtain a different length, create another array.

### Creating an initially empty-of-values array

```java
int[] marks = new int[3];
```

Its elements receive default values:

```text
[0, 0, 0]
```

Compare:

```java
String[] names = new String[3];
```

Its elements start as:

```text
[null, null, null]
```

The array exists, but it does not yet reference any name strings.

### Runnable example

```java
import java.util.Arrays;

public class ArrayDemo {
    public static void main(String[] args) {
        int[] marks = {70, 80, 90};
        marks[1] = 85;

        System.out.println(marks.length);
        System.out.println(marks[0]);
        System.out.println(Arrays.toString(marks));
    }
}
```

**Expected output:**

```text
3
70
[70, 85, 90]
```

Arrays.toString produces readable element text. Printing an array reference directly does not normally display its elements.

Use arrays when a fixed-size indexed sequence fits the problem. Later, we’ll use ArrayList when convenient resizing matters.

### 2. Aliasing: two references, one object

An alias is another reference through which the same object can be accessed.

```java
int[] first = {10, 20};
int[] second = first;
```

Picture:

```text
first  ──┐
         ├──► Array A1: [10, 20]
second ──┘
```

Assignment copied the reference. It did not copy the array.

```java
second[0] = 99;
System.out.println(first[0]);
```

**Expected output:**

```text
99
```

Both references lead to the changed object.

**Memory aid: “Two remotes, one television.”**

Either remote can change the same television. The analogy stops at access: Java references are values governed by language rules, not physical remote controls.

To create a separate array:

```java
int[] second = first.clone();
```

Now:

```text
first  ─────► Array A1: [10, 20]
second ─────► Array A2: [10, 20]
```

For a primitive int[], the copied elements are independent integer values.

### 3. Mutation versus reassignment

This distinction is worth repeating until it feels obvious.

```java
second[0] = 99;
```

Mutation: change the object reached through second.

```text
second = new int[]
```

*This snippet ends here in the supplied notes.*

The easiest way to remember this module is to keep three rules in mind:

Assignment copies a value.

A reference value identifies an object.

Changing an object and changing a reference are different operations.

We’ll apply these rules to arrays, strings, methods, and wrapper objects.

All examples use Java 21. Outputs are expected, not executed here.

### 1. Think of a reference as an arrow

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

- age holds an integer value.
- scores holds a reference to an array object.

The arrow is a teaching aid. It is not a claim about a physical address or how the JVM stores references.

**Memory cue: “The variable holds the arrow; the object holds the data.”**

This cue applies to reference variables. Primitive variables hold primitive values.

### 2. Arrays: a fixed number of numbered slots

An array groups elements of one component type.

```java
int[] scores = {70, 80, 90};
```

Its positions are numbered from zero:

| Index | Value |
| --- | --- |
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

Length is fixed; elements can change.

scores.length   // 3

You cannot resize that array object. To hold more elements, create another array or later use a resizable collection such as ArrayList.

Another way to create an array:

```java
int[] scores = new int[3];
```

This creates three elements initialized to zero:

```text
[0, 0, 0]
```

Reference-array elements initially contain null:

```java
String[] names = new String[3];
```

```text
[null, null, null]
```

### Boundary rule

For an array of length n, valid indices are:

0 through n - 1

Therefore:

scores[3]

is invalid for a three-element array and throws ArrayIndexOutOfBoundsException.

An empty array is valid:

```java
int[] empty = new int[0];
```

Its length is zero, and it has no valid element index.

**Memory cue: “Length is the count; the last index is count minus one.”**

### 3. Aliasing: two arrows to the same object

Aliasing means accessing the same object through different references.

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

### Reassignment changes the arrow

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

**Expected output:**

```text
[99, 20]
[99, 20]
[7, 8]
```

Arrays.toString produces a readable representation of the elements. Printing an array directly does not normally display its contents.

### 4. Copying an array is different from copying its reference

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

### 5. Multidimensional arrays are arrays of arrays

```java
int[][] grid = {
    {10, 20},
    {30, 40}
};
```

Read an element using two indices:

grid[1][0]   // 30

Read that as:

Select row 1, then select element 0 from that row.

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

This is often called a jagged array.

Loop using each row’s own length:

```java
for (int row = 0; row < rows.length; row++) {
    for (int column = 0; column < rows[row].length; column++) {
        System.out.println(rows[row][column]);
    }
}
```

A row can also be null:

```java
int[][] rows = new int[2][];
```

This creates the outer array, but no inner arrays yet.

### The shallow-copy trap

```java
int[][] original = {
    {10, 20},
    {30, 40}
};

int[][] copy = original.clone();
copy[0][0] = 99;
```

original[0][0] also becomes 99.

Why? Only the outer array was copied:

```text
original ──► Outer O1 ──┐
                       ├──► shared Row R1: [99, 20]
copy     ──► Outer O2 ──┘
```

To independently copy this int[][], copy each row too:

```java
int[][] copy = new int[original.length][];

for (int row = 0; row < original.length; row++) {
    copy[row] =
            original[row] == null ? null : original[row].clone();
}
```

For arrays containing more complex mutable objects, the required depth of copying depends on the structure.

**Memory cue: “Copying a container does not automatically copy everything inside it.”**

### 6. Strings: text that cannot be mutated

A String is immutable: once created, its text does not change.

```java
String first = "Java";
String second = first;

first = first.concat(" 21");
```

Conceptually:

Before:

```text
first  ──┐
         ├──► String S1: "Java"
second ──┘
```

After:

```text
first  ─────► String S2: "Java 21"
second ─────► String S1: "Java"
```

The original string was not modified. first was reassigned to the result.

A common mistake:

```java
String text = "Java";
text.concat(" 21");

System.out.println(text);
```

**Output:**

```text
Java
```

The returned result was ignored.

Correction:

```java
text = text.concat(" 21");
```

String methods preserve the original string’s content. A method may return another string or reuse an existing one when appropriate; do not assume every call allocates a new object. Java 21 String documentation

### Useful operations

| Expression | Meaning |
| --- | --- |
| text.length() | Number of UTF-16 code units |
| text.isEmpty() | Whether length is zero |
| text.charAt(0) | First UTF-16 code unit |
| text.substring(1, 3) | Units at positions 1 and 2 |
| text.contains("av") | Whether that sequence occurs |
| text.equals(other) | Whether string contents match |

Remember from our Unicode lesson: length() does not necessarily count visible characters.

**Memory cue: “String edits return a result; keep the result if you need it.”**

### 7. Identity versus equality

Identity: are these references pointing to the same object?

Equality: do these objects represent equal values according to their class’s rules?

For references:

first == second

tests identity.

For strings:

first.equals(second)

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

first == second       // false

first.equals(second)  // true

The explicit constructor is useful for this demonstration; ordinary code should normally use the literal directly.

### Why == sometimes seems to work with strings

```java
String first = "Java";
String second = "Java";
```

String literals are interned, so these references designate the same string object.

But this does not make == the correct content comparison.

### Arrays need a different comparison

```java
int[] first = {1, 2};
int[] second = {1, 2};
```

| Expression | Result |
| --- | --- |
| first == second | false |
| first.equals(second) | false |
| Arrays.equals(first, second) | true |

Arrays do not override equals to compare their elements.

For nested arrays, Arrays.deepEquals compares nested contents. Java 21 array utilities

**Memory cue: “== asks same object; content comparison depends on the type.”**

### 8. StringBuilder: text you can change

Use StringBuilder when constructing text through repeated updates.

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

**Expected output:**

```text
Java 21
Java
Java 21
Java 21
```

Convert the completed text to a string

```java
String result = builder.toString();
```

Later builder changes do not mutate result.

| Operation | Effect |
| --- | --- |
| append(...) | Add text at the end |
| insert(...) | Insert text |
| delete(...) | Remove a range |
| setCharAt(...) | Replace a UTF-16 unit |
| toString() | Obtain a string representing current content |

A StringBuilder is mutable and does not provide built-in synchronization for shared concurrent use. Also, its equals method does not compare textual content. Java 21 StringBuilder documentation

For content comparison:

firstBuilder.toString().equals(secondBuilder.toString())

### Which should you choose?

- Use String for ordinary text values.
- Use + for a simple expression such as "Hello, " + name.
- Use StringBuilder when repeatedly accumulating text, especially in a loop.

**Memory cue: “String is a finished text value; StringBuilder is an editable draft.”**

### 9. null: no object is referenced

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

missing.length  // NullPointerException

empty.length    // 0

Before using a possibly missing reference, decide the intended behavior.

```java
if (text != null && !text.isEmpty()) {
    System.out.println(text);
}
```

Because && short-circuits, isEmpty() is not called when text is null.

For null-safe equality between two possibly null references:

java.util.Objects.equals(first, second)

This delegates to the object’s equality rules when appropriate; it does not turn array equality into element-by-element comparison.

**Memory cue: “Empty means an object with no contents; null means no object.”**

### 10. Pass-by-value: the method gets its own copy of the arrow

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

**Expected output:**

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

main:

```text
    numbers ──► Array A1: [99]

change:
    values ───► Array A2: [500]
```

Reassigning values does not reassign numbers.

When change returns, its active frame is removed. A2 has not escaped this method and can be collected once unreachable. A1 remains reachable through numbers.

These are conceptual execution diagrams, not guaranteed physical layouts. Collection eligibility does not guarantee immediate collection.

**Memory cue: “A method receives a copied arrow, not control over your variable.”**

The same rule explains:

| Parameter type | What the method can do |
| --- | --- |
| int | Change its own local parameter |
| int[] | Mutate the shared array or reassign its local reference |
| String | Read the immutable string or reassign its local reference |
| StringBuilder | Mutate the shared builder or reassign its local reference |

### 11. Wrappers: object forms of primitive values

Wrapper classes let primitive values participate where objects are required.

| Primitive | Wrapper |
| --- | --- |
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

For example, generic collections use reference types:

ArrayList<Integer>

not:

ArrayList<int>   // Invalid

We’ll teach collections later.

### Boxing: primitive → wrapper

```java
Integer boxed = 10;
```

Java automatically boxes the integer value.

### Unboxing: wrapper → primitive

```java
int number = boxed;
```

Java extracts the primitive value.

### Pitfall 1: unboxing null

```java
Integer count = null;
int result = count;
```

This throws NullPointerException.

The same hidden unboxing occurs here:

count + 1

Use a primitive when absence is not meaningful, or handle missing values explicitly.

### Pitfall 2: wrapper identity

```java
Integer first = 100;
Integer second = 100;
```

Boxing can reuse wrapper objects. Some identities are guaranteed for specified small constant values; others must not be relied upon.

Therefore, do not use wrapper == to express value equality. These boxing and unboxing rules are specified by Java. Java 21 conversions

Use:

first.equals(second)

or, if either may be null:

java.util.Objects.equals(first, second)

### Pitfall 3: different wrapper types

Integer.valueOf(10).equals(Long.valueOf(10))

is false: these are different wrapper types.

### Pitfall 4: wrappers are immutable

```java
Integer first = 10;
Integer second = first;
first++;
```

first++ unboxes, increments, and boxes the result before reassigning first. It does not modify the original wrapper.

Result:

```text
first  represents 11
second represents 10
```

**Memory cue: “Box to use an object; unbox to calculate; check null before unboxing.”**

### 12. Remember the main performance costs

Let n be the number of array elements or text units.

| Operation | Typical cost |
| --- | --- |
| Access an array element by index | O(1) |
| Visit every array element | O(n) |
| Copy an array | O(n) time and O(n) output space |
| Compare two equal-length primitive arrays | O(n) worst case |
| Visit a jagged array | O(r + N), for r rows and N total elements |
| Build n text units by repeated small appends | Typically O(n) total with amortized buffer growth |
| Create the final string from a builder | Allow O(n) copying time and output space |

Repeatedly building a longer string with:

```java
text = text + piece;
```

can repeatedly copy the accumulated prefix, producing quadratic total work for many small pieces. A builder avoids that repeated-prefix pattern.

These are algorithmic cost models, not exact timing or memory-size guarantees.

### 13. Your one-minute revision card

| Concept | Remember |
| --- | --- |
| Array | Fixed number of mutable slots |
| Multidimensional array | Array whose elements refer to arrays |
| Aliasing | Multiple arrows to the same object |
| Mutation | Change the object |
| Reassignment | Change the arrow |
| String | Immutable text |
| StringBuilder | Mutable text construction |
| null | No object reference |
| Reference == | Same object? |
| .equals() | Equality defined by the class |
| Pass-by-value | Copy the value—including a reference value |
| Boxing/unboxing | Primitive ↔ wrapper |

### Practice—predict before running

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

<a id="revision"></a>

## Mutation and equality revision

An array, a String, and a StringBuilder are all objects. Whether an object can be changed depends on its type.

- Mutation: change the existing object.
- Reassignment: make a variable refer to a different object.

### 1. Mutation differences—in one example

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
| --- | --- |
| Custom object | Depends on class design; Box above is mutable |
| Array | Elements can change; array length stays fixed |
| String | Existing text cannot change |
| StringBuilder | Existing contents and length can change |

**Remember: changing a shared object is visible through its aliases. Reassigning one variable does not reassign the others.**

### 2. == versus equals()

Primitives: == compares values.

References: == compares identity.

equals() follows the class’s equality rules.

```java
int x = 10;
int y = 10;
System.out.println(x == y);       // true: primitive values

String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);       // false: different objects
System.out.println(a.equals(b));  // true: same string content
new String(...) is used here to deliberately create distinct objects; ordinary code usually uses string literals.
```

**Important: equals() does not automatically mean “compare contents.”**

The implementation inherited from Object compares identity. Classes such as String override it to define value equality. Your own class must override it if you want custom value equality.

### 3. Which equality operation should you use?

| Situation | Operation | Meaning |
| --- | --- | --- |
| Primitive numbers | a == b | Numeric equality; floating-point accuracy still matters |
| References | a == b | Same object, or both null |
| Strings | a.equals(b) | Same exact string content |
| Strings ignoring case | a.equalsIgnoreCase(b) | Case-insensitive comparison |
| Possibly null objects | Objects.equals(a, b) | Null-safe use of the class’s equals |
| One-dimensional arrays | Arrays.equals(a, b) | Corresponding elements are equal |
| Nested arrays | Arrays.deepEquals(a, b) | Recursively compare nested array contents |
| String versus builder | text.contentEquals(builder) | Same character sequence |
| Two builders | a.toString().equals(b.toString()) | Same text |
| Custom objects | Override equals() | Define what equality means for that class |

For object-array elements, Arrays.equals uses their equality rules; it does not recursively compare nested arrays.

### Examples together

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
```

System.out.println(left.toString().equals(

```java
        right.toString()));                         // true
System.out.println("Hi".contentEquals(left));         // true
```

### Three traps to remember

1. null.equals(...) throws NullPointerException. Use Objects.equals when either value may be null.
2. String literals and boxed numbers can share objects. Do not let an occasional true from reference == convince you it compares content.
3. If you override equals(), also override hashCode(). Equal objects must produce equal hash codes for hash-based collections to work correctly.

**Memory line: “Same object? ==. Same meaning? Choose the type’s equality operation.”**

| Memory Area | What goes there? | Lifespan |
| --- | --- | --- |
| Stack | Local primitive values & local reference variables (remotes) | Only lasts as long as the method runs |
| Heap | Objects, arrays, and all fields inside objects (primitives & references) | Lasts until the Garbage Collector deletes it |

<a id="oop"></a>

## Object-oriented programming

<a id="video-1"></a>

## Video 1 — Classes, objects, constructors, and keywords

- A class groups related data and behavior:

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

The fields describe a book’s state. The method describes behavior.

- A class defines a type. An object is an instance of that type.

Objects have:

- State: current field values.
- Behavior: operations they support.
- Identity: which particular object they are.

```java
Book first = new Book();
```

| Part         | Meaning |
|---|---|
| `Book`       | Declared type of the variable |
| `first`      | Reference variable |
| `new Book()` | Creates and initializes a `Book` object |
| `=`          | Stores the resulting reference in `first` |

The reference variable is not the object.

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

### What happens without new?

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

### Default field values

- Instance fields receive default values before constructor initialization:

| Field type | Default |
| --- | --- |
| Integer numeric types | 0 |
| Floating-point types | 0.0 |
| boolean | false |
| char | '\u0000' |
| Reference types | null |

- Local variables do not automatically receive usable default values.

```java
class Example {
    int count; // Defaults to 0

    void show() {
        int local;
        // System.out.println(local); // Compilation error
    }
}
```

### Constructors

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
- It has no return type—not even void.
- It can accept parameters.
- It can be overloaded.
- It is not inherited.

This is a method, not a constructor:

```java
void Book() {
}
```

### Default constructor versus no-argument constructor

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

A no-argument constructor that you write is not the compiler-generated default constructor.

### The this keyword

- this refers to the current object.

```java
Book(String title) {
    this.title = title;
}
```

Here:

- this.title means the object’s field.
- title means the constructor parameter.

Without this:

```java
title = title;
```

you assign the parameter to itself.

### Constructor overloading and chaining

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

this(...) invokes another constructor in the same class.

The benefit is centralized initialization: all paths eventually reach the constructor that assigns the fields.

For code compatible with older Java versions, put this(...) or super(...) first. Java 25 introduced restricted support for statements before these calls, so “it must always be the first statement” is no longer a universal rule. Oracle’s constructor update

### Primitive values versus objects

```java
int pages = 200;
Book book = new Book("Java Basics", 200);
```

An int variable holds a primitive value. A Book variable holds a reference.

Avoid memorizing “all primitives are on the stack.” A primitive field can be part of an object; a primitive array contains primitive elements. Storage depends on context and JVM implementation.

A useful conceptual model is:

- Method execution uses stack frames.
- Objects and arrays belong to the JVM heap model.
- Reference values connect variables and object fields to objects.

### Wrapper classes

Wrappers provide object types corresponding to primitives:

| Primitive | Wrapper |
| --- | --- |
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

```java
Integer boxed = 25; // Autoboxing
int value = boxed;  // Unboxing
```

Wrappers are useful with generics:

```java
ArrayList<Integer> numbers = new ArrayList<>();
```

You cannot use ArrayList<int>.

Wrappers are immutable. Also, unboxing null fails:

```java
Integer number = null;
int value = number; // NullPointerException
```

### The final keyword

For variables, final means the variable can be assigned only once.

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

A final reference does not automatically make the object immutable.

### Java is always pass-by-value.

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

change uses its copied reference to modify the shared object.

replace changes only its own parameter variable.

```java
Class = definition; object = instance; reference = connection.
```

this = this particular object.

Final reference = fixed connection, potentially changeable object.

Draw arrows whenever two variables might share an object.

For constructors, check name, parameters, initialization.

Never infer wrapper value equality from ==; use .equals() when comparing wrapper objects by value.

### Summary

A class groups state and behavior. Objects are instances, and reference variables refer to them. Constructors initialize objects; this identifies the current object or chains constructors. Wrappers represent primitive values as objects. final restricts reassignment. Garbage collection handles memory, while resource cleanup needs deliberate handling.

<a id="video-2"></a>

## Video 2 — Packages, static, and nested classes

### Packages

Packages organize types and help prevent name collisions.

```java
package learning.library;

public class Book {
}
```

The fully qualified name is:

learning.library.Book

```text
Another package can have its own Book.
```

Imports let you use short names:

```java
import learning.library.Book;
```

An import does not copy code or create an object.

Important details:

- java.lang is implicitly imported.
- Types in your own package generally need no import.
- import java.util.*; does not import subpackages.
- company and company.tools are separate packages; their names do not grant special access privileges.

### Static

In Java, the keyword static means something belongs to the class itself, rather than to any specific object (instance) created from that class.

Think of a class as a blueprint for a house:

Non-static things belong to each individual house you build (e.g., house #1's paint color, house #2's front door). Every house has its own.

Static things belong to the blueprint itself. There is only one shared copy, and every house shares it.

### The 3 Main Uses of static

### 1. Static Variables (Shared Data)

If you declare a variable as static, there is only one single copy of that variable in memory, no matter how many objects you create. All objects share that exact same variable.

```java
class Counter {
    static int count = 0; // Shared across all Counter objects

    Counter() {
        count++; // Every time a new object is made, the shared count goes up
    }
}

public class Main {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();

        System.out.println(Counter.count); // Prints 3! 
        // Notice we called it using the class name 'Counter', not an object name.
    }
}
```

### 2. Static Methods (Utility/Helper Methods)

A static method belongs to the class, which means you can call it without creating an object first using the new keyword.

Where you've seen it: Math.sqrt(25) or Integer.parseInt("123"). You never do Math m = new Math();. You just call Math.sqrt() directly.

Why main is static: public static void main(String[] args) is static because when you run a Java program, the JVM needs to call the main method before any objects of your class have been created yet.

```java
class MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        // We didn't do: MathUtils utils = new MathUtils();
        // We just called it directly on the class name!
        int sum = MathUtils.add(5, 10); 
        System.out.println(sum); // 15
    }
}
```

### 3. Static Blocks

Used to run a block of code once the very first time the class is loaded into memory (often used for complex static variable initialization).

### The Golden Rules of static (Interview Essentials)

Static can see static: A static method or variable can directly access other static variables and methods.

Static cannot see non-static directly: A static method cannot use non-static variables or call non-static methods directly. Why? Because non-static things belong to a specific object, and a static method doesn't know which object you are talking about!

No this or super: You cannot use the this keyword inside a static method because this refers to the current object instance, and static methods don't belong to any single instance.

### Understanding System.out.println

```java
System.out.println("Hello");
```

Break it down:

- System: a class in java.lang.
- out: a static field referring to a PrintStream.
- println(...): an instance method invoked on that stream.

### Nested classes

A nested class is declared inside another class.

```java
class Library {
    static class Catalog {
    }

    class Shelf {
    }
}
```

A static nested class has no implicit enclosing Library instance:

```java
Library.Catalog catalog = new Library.Catalog();
```

An inner class is associated with an enclosing instance:

```java
Library library = new Library();
Library.Shelf shelf = library.new Shelf();
```

Use an inner class when that association is meaningful. Otherwise, a static nested class often avoids unnecessary coupling.

### Tips, techniques, and mnemonics

- Static = shared by the class; instance = specific to an object.
- Ask: “Does this operation need one particular object’s state?”
- Access static members through the class name.
- Inner needs an outer; static nested does not.
- Singleton does not mean “one object across every server in a distributed application.”

**Practice: Build a Ticket class with a per-ticket ID and a shared creation counter.**

### Summary

Packages organize types. Static members belong to the class, while instance members belong to objects. Static methods need explicit references to access instance state. Nested classes group related types. A singleton controls instance creation, but shared mutable state still requires careful design.

<a id="video-3"></a>

## Video 3 — The four OOP principles

### 1. Detailed teaching

### Inheritance

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

Printer extends Device represents an is-a relationship.

A printer is a device.

super(name) invokes a superclass constructor. Constructors are not inherited, but superclass initialization is part of constructing the subclass object.

A superclass’s private fields remain part of the object’s state, but subclass code cannot directly access them.

### Forms of inheritance

| Form | Example | Java classes |
| --- | --- | --- |
| Single | Printer extends Device | Supported |
| Multilevel | LaserPrinter extends Printer extends Device | Supported |
| Hierarchical | Several subclasses extend Device | Supported |
| Multiple direct class inheritance | One class extends two classes | Not supported |

A class may implement multiple interfaces, which you will study in Video 5.

### Reference type versus runtime object type

```java
Device device = new Printer("Office printer");
```

The declared reference type is Device. The runtime object is a Printer.

The declared type determines which members are accessible through that expression:

```java
device.start(); // Allowed
// device.print(); // Not available through Device
```

The object has not lost its printer behavior. The reference type limits what the compiler permits directly.

### Polymorphism

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
Device device = new Printer();
device.start(); // Printer warming up
```

For an overridden instance method, the runtime object determines which implementation executes. Oracle’s polymorphism explanation

```java
public class CombinedDemo {

    // Parent Class
    static class Device {
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

    // Child Class
    static class Printer extends Device {
        Printer(String name) {
            super(name);
        }

        // 1. POLYMORPHISM: Overriding the parent's method
        @Override
        void start() {
            System.out.println(getName() + " (Printer) is warming up");
        }

        // 2. UNIQUE METHOD: Something only a Printer can do
        void print() {
            System.out.println(getName() + " is printing documents");
        }
    }

    public static void main(String[] args) {

        // ==========================================
        // SCENARIO 1: Pure Parent
        // ==========================================
        Device d = new Device("mouse");
        d.start(); // Allowed: Device has start()
        // d.print(); 
        // ❌ COMPILE ERROR! The reference type 'Device' doesn't know what print() is.

        // ==========================================
        // SCENARIO 2: Pure Child
        // ==========================================
        Printer p = new Printer("priii");
        p.start(); // Allowed (Runs Printer's version)
        p.print(); // Allowed: Reference type 'Printer' has print()

        // ==========================================
        // SCENARIO 3: Polymorphism (Parent Ref -> Child Obj)
        // ==========================================
        Device device = new Printer("laser-jet");

        // WHY DOES THIS RUN THE PRINTER'S VERSION? (Runtime Polymorphism)
        // - Compile-time: Compiler checks reference type (Device) -> sees start() exists -> allows it.
        // - Runtime: Java looks at the actual object on the heap (Printer) -> runs the child's overridden version.
        device.start(); // Prints: laser-jet (Printer) is warming up

        // WHY CAN'T WE CALL print() DIRECTLY HERE? (Reference Visibility)
        // device.print(); 
        // ❌ COMPILE ERROR! Even though the object is a Printer, the reference is a Device.
        // The compiler blocks you because generic Devices shouldn't have print().

        // HOW TO FIX IT? (Explicit Casting)
        // If you want to use child-specific methods through a parent reference, 
        // you must explicitly tell the compiler: "Trust me, treat this as a Printer."
        ((Printer) device).print(); // ✅ Works!
    }
}
```

Reference Type (Device — Left Side): Decides compile-time visibility. It tells the compiler what methods you are allowed to call. If it's not in Device, the compiler throws an error.

Object Type (Printer — Right Side): Decides runtime behavior. If the child class overrode a method (@Override), Java runs the child's version at runtime (Polymorphism).

### Overloading versus overriding

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
| --- | --- | --- |
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
Object value = "hello";
inspect(value); // Object version
```

Overload selection uses the expression’s compile-time type.

### Encapsulation

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

0 <= level <= 100

Adding getters and setters mechanically does not guarantee good encapsulation. Operations should protect meaningful rules.

### Abstraction

Abstraction exposes the operations a caller needs while hiding unnecessary implementation details.

```java
printer.print();
```

The caller need not understand how the printer queues pages, communicates with hardware, or manages buffers.

The distinction:

- Encapsulation: control access and preserve valid state.
- Abstraction: present a useful, simpler view of behavior.

They often work together.

### Tips, techniques, and mnemonics

- Is-a → consider inheritance. Has-a → consider composition.
- Overload = different inputs. Override = different implementation.
- Reference type checks access; runtime object selects overridden behavior.
- Four principles: A PIE — Abstraction, Polymorphism, Inheritance, Encapsulation.
- Use @Override whenever you intend to override a method.

<a id="video-4"></a>

## Video 4 — Access control, packages, and Object

### 1. Detailed teaching

### Access modifiers

For ordinary member access, remember:

| Modifier | Declaring class | Same package | Subclass in another package | Unrelated class in another package |
| --- | --- | --- | --- | --- |
| private | Yes | No | No | No |
| No modifier | Yes | Yes | No | No |
| protected | Yes | Yes | Yes, with restrictions | No |
| public | Yes | Yes | Yes | Yes |

This is the introductory view; nested types have additional private-access privileges, and accessibility of the enclosing type also matters. Oracle’s access-control guide

Package-private means no access modifier:

```java
class Helper {
}
```

Do not write default class Helper.

For ordinary top-level classes, choose public or package-private. Top-level classes cannot be private or protected.

### The subtle part of protected

In a different package, subclass access to an inherited protected instance member is restricted by the receiver type.

```java
// package first;
public class Parent {
    protected int value;
}
// package second;
public class Child extends Parent {
    void inspect(Child child, Parent parent) {
        System.out.println(this.value);  // Allowed
        System.out.println(child.value); // Allowed

        // System.out.println(parent.value); // Not allowed here
    }
}
```

Being a subclass does not grant unrestricted access to the protected state of every Parent object.

### Useful standard packages

| Package | Typical purpose |
| --- | --- |
| java.lang | Fundamental types such as String, Object, wrappers |
| java.util | Collections and utilities |
| java.io | Stream-oriented I/O |
| java.nio.file | Paths and file operations |
| java.net | Networking |
| java.time | Dates and times |
| java.awt | Desktop graphics and UI infrastructure |

Treat applet examples in older material as historical rather than a starting point for new applications.

### The Object class

Every Java class ultimately inherits from Object, directly or indirectly.

Several methods are especially important:

| Method | Purpose |
| --- | --- |
| toString() | Text representation |
| equals(Object) | Equality comparison |
| hashCode() | Hash value used by hash-based structures |
| getClass() | Runtime class information |
| clone() | Protected copying mechanism with a specific contract |

Object also supplies monitor methods such as wait(), notify(), and notifyAll(), which belong in a later concurrency lesson.

### == versus equals()

For references, == asks whether the references identify the same object.

```java
String first = new String("Java");
String second = new String("Java");

System.out.println(first == second);      // false
System.out.println(first.equals(second)); // true
```

equals() expresses the equality defined by the class.

The default implementation inherited from Object uses identity. Custom value-like classes often override it.

### Designing equality

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

Equal objects must have equal hash codes.

Equal hash codes do not necessarily mean equal objects.

A hash code is not guaranteed to be unique or to represent a memory address. Object’s equality, hashing, and cloning contracts are documented in its API reference.

### instanceof versus getClass()

```java
Object value = "hello";

System.out.println(value instanceof String);        // true
System.out.println(value.getClass() == String.class); // true
```

With inheritance:

- instanceof checks compatibility with a type.
- getClass() identifies the exact runtime class.
- null instanceof SomeType is false.
- Calling getClass() through null throws NullPointerException.

### 2. Extra content you need

### Do not mutate hash keys in ways that change equality.

Imagine putting an object into a HashSet, then changing the ID used by hashCode().

The object remains stored according to its old hash placement, while future lookups use its new hash. Operations such as contains() can then behave unexpectedly.

Prefer stable, immutable identity fields for keys.

For null-safe equality:

```java
Objects.equals(first, second);
```

For arrays:

```java
Arrays.equals(firstArray, secondArray);
```

Ordinary array .equals() does not compare all elements.

### 3. Tips, techniques, and mnemonics

- Access expands: private → package → protected → public.
- == = same object; .equals() = class-defined equality.
- Equal → same hash. Same hash ↛ necessarily equal.
- Generate equals() and hashCode() together, then review which fields define identity.
- Start fields as private and expose only operations that callers need.

**Practice: Put two separate Product objects with the same ID into a HashSet. With the implementation above, the set should contain one logical product.**

### 4. Summary

Access modifiers establish boundaries. Packages organize APIs. Object provides shared foundational behavior. Reference identity and logical equality differ. Equality and hashing must agree, especially when objects enter sets and maps.
