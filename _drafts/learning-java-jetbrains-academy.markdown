---
layout: "post"
title: "Learning: Java JetBrains Academy"
date: "2020-07-25 12:00"
---

<!-- TOC -->

- [What is this?](#what-is-this)
  - [Reading from standard in to capture text keyboard entries](#reading-from-standard-in-to-capture-text-keyboard-entries)
  - [Underscores in numbers](#underscores-in-numbers)
  - [Reading from standard in to capture numerical keyboard entries](#reading-from-standard-in-to-capture-numerical-keyboard-entries)
  - [Prefix/postfix forms of increments/decrements](#prefixpostfix-forms-of-incrementsdecrements)
  - [Java String methods](#java-string-methods)
  - [Conditional Statement: Theory Notes](#conditional-statement-theory-notes)
  - [Conditional Statement: Queen](#conditional-statement-queen)
    - [Problem Statement](#problem-statement)
    - [Example cases](#example-cases)
    - [Original Working Solution](#original-working-solution)
    - [Functional Working Solution](#functional-working-solution)
    - [Highlighted Solution](#highlighted-solution)
    - [Review Thoughts](#review-thoughts)
- [Thoughts on learning experience](#thoughts-on-learning-experience)

<!-- /TOC -->

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

### Conditional Statement: Theory Notes
- Think about each logical statement as a **branch** that creates a **decision tree**.
  - **Control flow statements** is another phrased used to describe the order in which conditional statements are executed.
  - Example graphic:
  ![javalearning_decisiontree](images/2020/07/javalearning-decisiontree.png)

### Conditional Statement: Queen

#### Problem Statement

> You are given coordinates of two queens on a chess board. Find out whether or not they hit each other.
>
> Input data format:
>
> Four integer numbers x1,y1,x2,y2x
>
> Output data format:
>
> Type "YES" (uppercase) if they hit each other or "NO" if they don't.
>
> You may need a method that calculates the absolute value of the number, so here it is:
>
> Math.abs(n)

#### Example cases
> **Sample Input 1:**
>
> 1 1 3 3
>
> **Sample Output 1:**
>
> YES
>
> **Sample Input 2:**
>
> 1 1 4 3
>
> **Sample Output 2:**
>
> NO
>
> **Sample Input 3:**
>
> 2 2 5 2
>
> **Sample Output 3:**
>
> YES

#### Original Working Solution
```java
import java.util.Scanner;

class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // parse chess piece locations
        int x1 = scanner.nextInt();
        int y1 = scanner.nextInt();
        int x2 = scanner.nextInt();
        int y2 = scanner.nextInt();

        // determine if pieces intersect
        int slope = Math.abs(y2 - y1) / Math.abs(x2 - x1);
        if (x1 == x2 || y1 == y2 || slope == 1) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
    }
}

// failing test #6: 4 4 2 7
// integer division causing abs( (7-4)/(2-4) ) to round to 1, false positive
```

#### Functional Working Solution
```java
import java.util.Scanner;

class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // parse chess piece locations
        double x1 = scanner.nextInt();
        double y1 = scanner.nextInt();
        double x2 = scanner.nextInt();
        double y2 = scanner.nextInt();

        // determine if pieces intersect
        double slope = Math.abs(y2 - y1) / Math.abs(x2 - x1);
        if (x1 == x2 || y1 == y2 || slope == 1) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
    }
}

// test #6 now working: 4 4 2 7
```

#### Highlighted Solution
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int num1 = scanner.nextInt();
        int num2 = scanner.nextInt();
        int num3 = scanner.nextInt();
        int num4 = scanner.nextInt();

        if (num1 == num3) {
            System.out.println("YES");
        } else if (num2 == num4) {
            System.out.println("YES");
        } else if (Math.abs(num1 - num3) == Math.abs(num2 - num4)) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
    }
}
```

#### Review Thoughts
Looks like each condition where a queen could "touch" is evaluated as a separate conditional.  Also, the way the slope is evaluated avoids the need for division.  I had to switch to doubles because integer division was causing some false positives when results would round to 1.

### Ternary Operator
Condensed way of handling conditional function.

```java
result = condition ? trueCase : elseCase;   // basic structure

int max = a > b ? a : b;                    // print max of a and b
```

### Measurements of Data
- Binary: basis of computer storage
  - On v Off
- Smallest unit of information is a bit (b, lowercase)
- 8 bits = 1 byte (B, uppercase)
- Large units of information
  - Two different basis of measurements at odds with each other
    - 1) Decimal-based system where bytes are measured in the powers of ten
      - kilobyte (10^3), megabyte (10^6), gigabyte (10^9)
    - 2) Binary-based system where bytes are measured in the powers of two
      - kilobyte (2^10), megabyte (2^20), gigabyte (2^20)
      - Used to describe computer memory in binary
    - To handle the two different systems, distinct prefixes were introduced
      - kilo, mega, giga for decimal-based measurements
      - kibi, mebi, gibi for binary-based measurements

### Sizes and ranges
- Numbers
  - Integers
    - `byte`: size 8 bits (1 byte), range from -128 to 127
    - `short`: size 16 bits (2 bytes), range -32768 to 32767
    - `int`: size 32 bits (4 bytes), range -(2^31) to (2^31)-1
    - `long`: size 64 bits (8 bytes), range -(2^63) to (2^63)-1
    - Most comment integer types are `int` and `long`
  - Fractional / Floating-point
    - `double`: size 64 bits, ~14-16 decimal digits
    - `float`: size 32 bits, ~6-7 decimal digits
  - Special marks
    - Denote float with f: `2.71828f`
    - Denote long with L: `1_000_000L`

    ### The largest element of a sequence
    Given the sequence of integer numbers (which ends with the number 0). Find the largest element of the sequence.

    The number 0 itself is not included in the sequence but serves only as a sign of the sequence’s end.

    > Sample Input 1:
    >
    > 1
    >
    > 7
    >
    > 9
    >
    > 0
    >
    > Sample Output 1:
    >
    > 9
    ### My Solution
    ```java
    import java.util.Scanner;

    class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            int max = 0;
            int input;

            while ((input = scanner.nextInt()) != 0) {
                if (input > max) {
                    max = input;
                }
            }

            System.out.println(max);
        }
    }
    ```

    ### Highlighted Solution
    ```java
    import java.util.Scanner;

    public class Main {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            int number = scanner.nextInt();
            int max = number;

            while (number != 0) {
                if (max < number) {
                    max = number;
                }
                number = scanner.nextInt();
            }

            System.out.println(max);
        }
    }
    ```

    #### Review Thoughts
    I was not thrilled with the idea of setting `max` to zero as any negative numbers would never be set to the max.  The highlighted solutions handles this gracefully by initializing `max`'s value as the first int obtained.  Unfortunately the learning platform accepted my answer as correct.  I would have preferred to see them fail my solution and hint that I need to think about negative numbers.

## Thoughts on learning experience
- "Solve in IDE" doesn't explain how a solution can be improved
  - Turns out that the answer key is also checking white space between methods and parameters.  It will mark solution as "correct but could be improved" if you mash things together.  For example:
    - `System.out.println(phrase.replace("a","b"));`
    - `System.out.println(phrase.replace("a", "b"));`
    - no whitespace between target and replacement in replace() method.
    - IntelliJ actually masked the whitespace when it draws on the screen the names of the parameters you're working with.  I couldn't easily see whitespace between the parameters names.
- Some of the "JetBrains Academy Team" selected answers make use of methods that were not mentioned in the theory readings.
  - eg: `replaceAll()`, `equalsIgnoreCase()`
- IDE will suggest a tool top (eg: "add clarifying parenthesis") but then solution will mark them as "unnecessary".
