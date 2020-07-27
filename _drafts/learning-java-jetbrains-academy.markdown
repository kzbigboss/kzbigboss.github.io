---
layout: "post"
title: "Learning: Java JetBrains Academy"
date: "2020-07-25 12:00"
---

## What is this?
Notes from studying the Java Developer path via JetBrains Academy

### Reading from standard in to capture text keyboard entries

```java
import java.util.Scanner // Scanner can read from System.in

// instantiate a new Scanner object
Scanner scanner = new Scanner(System.in);

// note: Scanner will default entries from System.in as a String
String input1 = scanner.next() // grab next set of String between cursor and whitespaces

String input2 = scanner.nextLine() // grab next set of String cursor and line break
```

### Underscores in numbers
You can use the underscore to separate groups of digits within a number:

```java
int million = 1_000_000;
```

### Reading from standard in to capture numerical keyboard entries
```java
import java.util.Scanner // Scanner can read from System.in

// instantiate a new Scanner object
Scanner scanner = new Scanner(System.in);

String input1 = scanner.nextInt() // store as int

String input2 = scanner.nextLong() // store as L
```

### Prefix/postfix forms of increments/decrements
Both the increment and decrement operators have prefix and postfix modes:
- prefix (eg: ++n) alters the value before it is used.
- postfox (eg: n--) alters the value after it is used.

### Java String methods
- `length()`: return number of characters in the string
- `charAt(int index)`: return character at index, 0 based
- `isEmpty()`: boolean describing if String is populated
- `toUpperCase()`: returns new String in uppercase
- `toLowerCase()`: returns new String in lowercase
- `startsWith(prefix)`: returns `true` if String starts with prefix
- `endsWith(suffix)`: returns `true` if String ends with suffix
- `contains(...)`: returns `true` if string contains given string or character
- `substring(beginIndex, endIndex)`: return substring between range `begin` and `end - 1`
- `replace(old, new)`: returns a new string by replacing `old` with `new`
- `trim()`: returns copy of string by omitting leading + trailing whitespace
- More at official Java docs
  - https://docs.oracle.com/javase/8/docs/api/java/lang/String.html

## Thoughts on learning experience
- "Solve in IDE" doesn't explain how a solution can be improved
  - Turns out that the answer key is also checking white space between methods and parameters.  It will mark solution as "correct but could be improved" if you mash things together.  For example:
    - `System.out.println(phrase.replace("a","b"));`
    - `System.out.println(phrase.replace("a", "b"));`
    - no whitespace between target and replacement in replace() method.
    - IntelliJ actually masked the whitespace when it draws on the screen the names of the parameters you're working with.  I couldn't easily see whitespace between the parameters names.
- Some of the "JetBrains Academy Team" selected answers make use of methods that were not mentioned in the theory readings.
  - eg: `replaceAll()`, `equalsIgnoreCase()`
