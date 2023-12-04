# File: combined_all.md


# File: Week 01.md

# CS 61B Week 01

## 1.1 Intro, Hello World Java

### Introduction

**What is 61B about?**

* Writing codes that runs efficiently. (Algorithms and data structures.)
* Writing code efficiently. 

### First Java Program

#### Hello World

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

* All codes must be in a class.
* Curly braces and semi-colons.
* Variables have declared types and must be declared before use.
* Functions must have a return type or declared as void function.
* The compiler ensures type consistency.

#### Compilation

The common way to run a Java program is to run it through two programs: `javac` and `java`.

```
$ javac HelloWorld.java
$ java HelloWorld
Hello World!
```

#### Defining Functions

Since all Java code is part of a class, we must define functions so that they belong to some class. 
Functions that are part of a class are commonly called "methods". 

For instance, here's a piece of code in Java which defining a function that could compare two integers and return the larger one.

```java
public class LargerDemo {
    public static int larger(int x, int y) {
        if (x > y) {
            return x;
        }
        return y;
    }

    public static void main(String[] args) {
        System.out.println(larger(8, 10));
    }
}
```

#### Code Style

In this course, we will aim to make our code readable. Here are some tips to achieve that goal:
* Consistent style
* Comments (use `//` for line comments and  `/*` and `*/` for block comments)
* Avoidance of repetitive code
* Descriptive naming (variables, functions, classes)

Moreover, we could use [Javadoc](https://en.wikipedia.org/wiki/Javadoc) to automatically generate documents.

## 1.2 Defining and Using Classes

### Defining Classes

* Every method is associated with some class.
* To run a class, we must define a main method.

```java
public class Dog {
    public static void makeNoise() {
        System.out.println("Bark!");
    }
}
```

The code above can't be run directly because there's no main method.

```java
public class DogLauncher {
    public static void main(String[] args) {
        Dog.makeNoise();
    }
}
```

However, the method could be called from another class. 

```
$ java DogLauncher
Bark!
```

### Object Instantiation

* Classes can contain not just methods, but also data, such as the `size` of each dog.
* Classes can be instantiated as objects. For example, we will create a single Dog class, and then create instances of this Dog.

Here's a Dog class which provdes a blueprint of Dog objects.

```java
public class Dog {
    // Instance variable
    public int weightInPounds; 

    // Constructor
    public Dog(int startingWeight) { 
        weightInPounds = startingWeight;
    }

    // Non-static method (Instance method)
    public void makeNoise() {
        if (weightInPounds < 10) {
            System.out.println("yipyipyip!");
        } else if (weightInPounds < 30) {
            System.out.println("bark. bark.");
        } else {
            System.out.println("woof!");
        }
    }
}
```

```java 
public class DogLauncher {
    public static void main(String[] args) {
        Dog smallDog; // Declaration
        new Dog(20); // Instantiation
        smallDog = new Dog(5); // Instantiation and Assignment
        Dog hugeDog = new Dog(150); // Declaration. Instantiation, and Assignment
        smallDog.makeNoise(); // Invocaton
        hugeDog.makeNoise(); // The dot notation 
    }
}
```

### Array of Objects

To create an array of objects:
* First use the new keyword to create the array.
* Then use new again for each object that you want to put in the array.

```java
Dog[] dogs = new Dog[2];
dogs[0] = new Dog(8);
dogs[1] = new Dog(20);
dogs[0].makeNoise();
```

### Staitc vs. Non-static

Key difference between static and non-static methods:
* Static method are invoked using the class name.
* Instance methods are invoked using an instance name.
* Static method can't access instance variables.
, but are more simple.

Sometimes, a class may contain both static and non-static methods.

```java
public static Dog maxDog(Dog d1, Dog d2) {
	if (d1.weightInPounds > d2.weightInPounds) {
   		return d1;
	}
	return d2;
}

public Dog maxDog(Dog d2) {
    if (this.weightInPounds > d2.weightInPounds) {
        return this;
    }
    return d2;
}
```

We could declare static variables which are properties shared by all instances of the class.

```java 
public static String binomen = "Cans famliiaris";
```

* A variable or method defined in a class is also called a **member** of that class. 
* Static members are accessed using **class name**.
* Non-static members cannot be invoked using class name.
* Static methods must access instance variables via **a specific instance**.

### public static void main(String[] args)

* `public`: So far, all of our methods start with this keyword.
* `static`: It is a static method, not associated with any particular instance.
* `void`: It has no return type.
* `main`: This is the name of the method.
* `String[] args`: This is a parameter that is passed to the main method.

For example, this program prints out the first command line argument.

```java
public class ArgsDemo {
    public static void main(String[] args) {
        System.out.println(args[0]);
    }
}
```

```
$ java ArgsDemo these are command line arguments
these
```

### Using Libraries

In CS 61B, we will use libraries include:
* The built-in Java libraries such as Math, String, Integer, List, Map.
* The Princeton standard library such as StdDraw, StdAudio, In.


# File: Week 02.md

# CS 61B Week 02

## References, Recursion, and Lists

### Primitive Types

#### Mystery of Walrus

The code below will both change the weight of Walrus a and b.

```java
Walrus a = new Walrus(1000, 8.3);
Walrus b;
b = a;
b.weight = 5;
System.out.println(a);
System.out.println(b);
```

However, the code below will only change integer x to 2.

```java
int x = 5;
int y;
y = x;
x = 2;
System.out.println("x is: " + x);
System.out.println("y is: " + y);
```

#### Bits

Information are stored in memory of computers, which is a sequence of ones and zeros.
The identical sequence may have different meanings, since each Java type has a different way to interpret bits. 
For instance, '01001000' may represent integer 72 or character 'H'.

Java has 8 **primitive types**: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`, `char`. 
Computers set aside exactly enough bits to hold a thing of a certain type when you declare a variable of that type. 
For example, declaring an int sets aside a box of 32 bit, and a double sets aside a box of 64 bits. 

* Java creates an internal table that maps each variable name to a location.
* Java does not write anything into the boxes, and does not allow access to an uninitialiized variable.

### Reference Types

* Every type not included in the primitive types is a **reference type**, such as `Array`.
* Refernce type store address of the object, primitive type stored bytes at the location of the variable in memory 
* ``` Dog a = new Dog(10, 10) != Dog b = new Dog(10, 10) ``` They have diffrent address  
* a.equal(b) = true this method compare the objects themself  
* a = 5 == b = 5 => _ture_   
#### Object Instantiation

When we instantiate an object, Java first allocates a box of bits for each instance variable of the class and fills them with a default value. The constructer usually fill them with other values.

Typically, object will have overheads in addition to the memory used by its instance variables.

#### Reference Variable Declaration

When we declare a variable of any reference type:

* Java allocates a box of size 64 bits.
* These bits can be either set to Null or the address of a specific instance of that class (returned by `new`).

#### Pass By Value and Pass By Refernce 
* **Pass By Value** we pass a copy of the **Value** so when we change it locally it change without affect the actual argument 
* **Pass By Refernce** We pass  copy of **Refernce** not value so when we change it locally it's change the actual **Object** 

```java
 int x = 5;  // Memory box with value 5
 int[] arr = new int[]{1, 2, 4, 5}; // memory box with refernce 0x1111
 doSomething(x, arr); 
 
 public void doSomething(int y /*Pass by Value */, int[] a /*pass by ref*/){  
    int y = 15; // memory box with value 13 does not affect value of X 
    a[2] = 14;  // it's affect the actual value of the refrence 0x1111
    }
``` 

### The Golden Rule of Equals

When you write `y = x`, you are telling the Java interpreter to **copy the bits** from x into y.

Just as with primitive types, the equals sign copies th bits stored in the reference variable, which is the address of a specific instance. Moreover, passing parameters will obey the rule.

### Arrays

Arrays are also objects, which are instantiated using the `new` keyword.

* Creates a 64 bit box for storing an array address. (Declaration)
* Creates a new array object. (Instantiation)
* Puts the address of this new object into the 64 bit box. (Assignment)

```java
int[] x; // Declaration
new int[] {0, 1, 2, 3}; // Instantiation
int[] x = new int[] {0, 1, 2, 3}; // Declaration, Instantiation, Assignment
```

### IntLists

Here's a very basic linked list:

```java
public class IntList {
    public int first;
    public IntList rest;        

    public IntList(int f, IntList r) {
        first = f;
        rest = r;
    }
}
```

There are two ways to create a list of the numbers 5, 10, and 15: build it forward or backward.

```java
IntList L = new IntList(5, null);
L.rest = new IntList(10, null);
L.rest.rest = new IntList(15, null);
```

```java
IntList L = new IntList(15, null);
L = new IntList(10, L);
L = new IntList(5, L);
```

We will add methods to the IntList class to let it perform basic tasks.

#### size() and iterativeSize()

We could either use recursive or iterative methods to get the size of the IntList.

```java
public int size() {
    if(this.rest == null) {
        return 1;
    } else {
        return 1 + rest.size();
    }
}

public int iterativeSize() {
    IntList p = this;
    int totalSize = 0;
    while (p != null) {
        totalSize += 1;
        p = p .rest;
    }
    return totalSize;
}
```

#### get()

Write a method `get(int i)` that returns the ith item of the list.

```java
public int get(int i) {
    if (i == 0) {
        return this.first;
    } else {
        return this.rest.get(i - 1);
    }
}
```

## SLLists, Nested Classes, Sentinel Nodes
* 
The IntList class we've already created is called a 'naked' data structure which is hard to use.

### Rebranding

```java
public class IntNode {
    public int item;
    public IntNode next;

    public IntNode(int i, IntNode n) {
        item = i;
        next = n;
    }
}
```

Knowing that `IntNodes` are hard to work with, we're going to create a separate class called `SLList` that the user will interact with.

```java
public class SLList {
    public IntNode first;

    public SLList(int x) {
        first = new IntNode(x, null);
    }
}
```

Thus, we could create a list by using `new SLList(10);`, which is easier to instantiate, instead of using `new IntNode(10, null);`.

### Private

However, our `SLList` can be bypassed and the naked data structure can be accessed. In order to solve this problem, we could make the `first` from public to private. Thus, it could be only accessed within the same class.

```java
public class SLList {
    private IntNode first;
...
```

* Hide implementation details from users of your class.
* Safe for you to change private methods.
* Nothing to do with protection against hackers or other evil entities.

### Nested Classes

* Java provides a simple way to combine two classes within one file.
* Having a nested class has no meaningful effect on code performance, and is simply a tool for keeping code organized.
* When `IntNode` is declared as `static class`, it could not access any instance methods or variables of `SLList`.

```java
public class SLList {
       private static class IntNode {
            public int item;
            public IntNode next;
            public IntNode(int i, IntNode n) {
                item = i;
                next = n;
            }
       }

       private IntNode first; 

       public SLList(int x) {
           first = new IntNode(x, null);
       } 
...
```

### Methods

#### addFirst()

```java
/** Adds x to the front  of the list. */
public void addFirst(int x) {
    first = new IntNode(x, first);
}
```

#### getFirst()

```java
/** Returns the first item in the list. */
public int getFirst() {
    return first.item;
}
```

#### addLast()

```java
public void addLast(int x) {
    IntNode p = first;
    while (p.next != null) {
        p = p.next;
    }
    p.next = new IntNode(x, null);
}
```
#### size()

```java
/** Returns the size of the list that starts at IntNode p. */
private static int size(IntNode p) {
    if (p.next == null) {
        return 1;
    }
    return p.next.size() + 1;
}

public int size() {
    return size(first);
}
```

### Caching
Obviously, the `size()` method is unefficient, so we will add a integer variable to track the size of the list.

```java
private int size;

public SLList (int x) {
    size = 1;
    ...
}

public void addFirst(int x) {
    size += 1;
    ...
}

public void addLast (int x) {
    size += 1;
    ...
}

public int size() {
    return size;
}
```

### Empty List

Here is a simple way to create an empty list, but it has subtle bugs: The program will crash when you add an item to the last of the list.

```java
public SLList() {
    first = null;
    size = 0;
}
```

In order to fix the bug, we could either fix the `addLast()` method, which is not simple, or add a sentinel node.

### Sentinel Node

We could create a special node that is always there, which is called a "sentinel node".

```java 
/** The first item, if it exists, is at sentinel.next. */
private IntNode sentinel;

public SLList() {
    sentinel = new IntNode(63, null);
    size = 0;
}

public SLList(int x) {
    sentinel = new IntNode(63, null);
    sentinel.next = new IntNode(x, null);
    size = 1;
}

public void addFirst(int x) {
    sentinel.next = new IntNode(x, sentinel.next);
    size = size + 1;
}

public int getFirst() {
    return sentinel.item;
}

public void addLast(inx x) {
    IntNode p = sentinel;
    while (p.next != null) {
        p = p.next;
    }
    p.next = new IntNode(x, null);
    size = size + 1;
}
```

### Invariants

A `SLList` with a sentinel node has at least the following invariants:

* The `sentinel` reference always points to a sentinel node.
* The front item (if it exists), is always at `sentinel.next.item`.
* The `size` variable is always the total number of items that have been added.

## DLLists, Arrays

Although SLList provides a efficient way for us to manipulate the list, it's really slow when we want to add an item to the end of the list.

### addLast()

Similar to `size()`, we could cache the last pointer; however, remove it will be still really slow because we will iterate to the second last item.

We could add a pointer to each node which points to the previous node, but the problem is that the `last` pointer of the `SLList` class may points to the sentinel node.

Thus, we could add a second sentinel at the end of the list, and point it with `sentBack`.

Alternatively, we may just use a single sentinel and make the list circular.

### Generic List

We could improve our IntList to make it available for other types.

```java
public class DLList<BleepBlorp> {
    private IntNode sentinel;
    private int size;

    public class IntNode {
        public IntNode prev;
        public BleepBlorp item;
        public IntNode next;
        ...
    }
    ...
}
```

We put the desired type inside of angle brackets during declaration, and also use empty angle brackets during instantiation.

```java
DLList<String> d2 = new DLList<>("hello");
d2.addLast("world");
```

### Array Overview

### Definition and Creation

Array is a special kind of object which consists of a numbered sequence of memory boxes. An array consists of:

* A fixed integer `length`.

* A sequence of N memory boxes where `N=length`, and all boxes hold the same type of value, which are numbered from 0 to `length-1`.

Like classes, arrays are instantiated with `new`:

```java
y = new int[3];
x = new int[]{1, 2, 3, 4, 5};
int[] w = {9, 10, 11, 12, 13};
```

#### Arraycopy

Two ways to copy arrays:
* Item by item using a loop.
* Using arraycopy: Source array, start position in source, target array, start position in target, number to copy.

```java
System.arraycopy(b, 0, x, 3, 2);
```

#### 2D Array

We could create a 2-dimensional array with the following codes. 

```java
int[][] matrix;
matrix = new int[4][4];
matrix = new int[4][];
matrix[0] = new int[]{1};

int[][] pascalAgain = new int[][]{{1}, {1, 1},{1, 2, 1}, {1, 3, 3, 1}};
```

#### Arrays and Classes

Both arrays and classes can be used to organize a bunch of memory boxes. In both cases, the number of memory boxes is fixed, i.e. the length of an array cannot be changed, just as class fields cannot be added or removed.

The key differences between memory boxes in arrays and classes:

* Array boxes are numbered and accessed using [] notation, and class boxes are named and accessed using dot notation.
* Array boxes must all be the same type. Class boxes can be different types.



# File: Week 03.md

# CS 61B Week 03

## ALists, Resizing, vs. SLists

### Limitation of DLists

Suppose we added `get(int i)`, which returns the ith item of the list. While we have a quite long DList, this operation will be significantly slow.

By instead, we could use Array to build a list without links.

### Random Access in Arrays

Retrieval from any position of an array is very fast, which is independent of the size of it.

### Naive AList Code

```java
public class AList {
    private int[] items;
    private int size;

    public AList() {
        items = new int[100];
        size = 0;
    }

    public void addLast(int x) {
        items[size] = x;
        size += 1;
    }

    public int getLast() {
        return items[size - 1];
    }

    public int get(int i) {
        return items[i];
    }

    public int size() {
        return size;
    }
}
```

Here are some invariants of this piece of code:

*  The position of the next item to be inserted is always `size`.
* `size` is always the number of items in the AList.
* The last item in the list is always in position `size - 1`.

### Delete Operation

```java
public int removeLast() {
    int returnItem = items[size - 1];
    items[size - 1] = 0;
    size -= 1;
    return returnItem;
}
```

### Naive Resizing Arrays

The limitation of the above data structure is that the size of array is fixed.

To solve that problem, we could simply build a new array that is big enough to accomodate the new data. For example, we can imagine adding the new item as follows:

```java
public void addLast(int x) {
  if (size == items.length) {
    int[] a = new int[size + 1];
    System.arraycopy(items, 0, a, 0, size);
    items = a;  	
  }
  items[size] = x;
  size += 1;
}
```
The problem is that this method has terrible performance when you call `addLast` a lot of times. The time required is exponential instead of linear for SLList.

Geometric resizing is much faster: Just how much better will have to wait. (This is how the Python list is implemented.)

```java
public void addLast(int x) {
  if (size == items.length) {
	resize(size * 2);
  }
  items[size] = x;
  size += 1;
}
```

### Memory Performance

Our AList is almost done, but we have one major issue. Suppose we insert 1,000,000,000 items, then later remove 990,000,000 items. In this case, we'll be using only 10,000,000 of our memory boxes, leaving 99% completely unused.

To fix this issue, we can also downsize our array when it starts looking empty. Specifically, we define a "usage ratio" R which is equal to the size of the list divided by the length of the `items` array. For example, in the figure below, the usage ratio is 0.04.

### Generic Array

When creating an array of references to Item:

* `(Item []) new Object[cap];`
* Causes a compiler warning, which you should ignore.

The another change to our code is that we will delete an item by setting it to `null` instead of `0`, which could be collected by Java Garbage Collector.

## Testing

When you are writing programs, it's normal to have errors, and instead of using the autograder, we could write tests for ourselves.

We will write a test method `void testSort()` first to test a `sort()` method before we really implement the method.

### Ad Hoc Testing

We could easily create a test, such as the code below, which will return nothing or the first mismatch of the provided arrays.

We should not use `==` while comparing two array objects because this operation will simply compare the memory address pointed by the two variables.

```java
public class TestSort {
    /** Tests the sort method of the Sort class. */
    public static void testSort() {
        String[] input = {"i", "have", "an", "egg"};
        String[] expected = {"an", "egg", "have", "i"};
        Sort.sort(input);
        for (int i = 0; i < input.length; i += 1) {
            if (!input[i].equals(expected[i])) {
                System.out.println("Mismatch in position " + i + ", expected: " + expected + ", but got: " + input[i] + ".");
                break;
            }
        }
    }

    public static void main(String[] args) {
        testSort();
    }
}
```

### JUnit

With the JUnit library, our test method could be simplified to the following codes:

```java
public static void testSort() {
    String[] input = {"i", "have", "an", "egg"};
    String[] expected = {"an", "egg", "have", "i"};
    Sort.sort(input);
    org.junit.Assert.assertArrayEquals(expected, input);
}
```

### Selection Sort

Basic steps of selection sort:

* Find the smallest item.
* Move it to the front.
* Selection sort the remaining N-1 items (without touching the front item).

#### findSmallest()

Since `<` can't compare strings, we could use the `compareTo` method.

```java
public class Sort {
    /** Sorts strings destructively. */
    public static void sort(String[] x) { 
           // find the smallest item
           // move it to the front
           // selection sort the rest (using recursion?)
    }

    /** Returns the smallest string in x. */
    public static String findSmallest(String[] x) {
        String smallest = x[0];
        for (int i = 0; i < x.length; i += 1) {
            int cmp = x[i].compareTo(smallest);
            if (cmp < 0) {
                smallest = x[i];
            }
        }
        return smallest;
    }
}
```

Test the method above:

```java
public static void testFindSmallest() {
    String[] input = {"i", "have", "an", "egg"};
    String expected = "an";

    String actual = Sort.findSmallest(input);
    org.junit.Assert.assertEquals(expected, actual);        

    String[] input2 = {"there", "are", "many", "pigs"};
    String expected2 = "are";

    String actual2 = Sort.findSmallest(input2);
    org.junit.Assert.assertEquals(expected2, actual2);
}
```

#### Swap

```java
public static void swap(String[] x, int a, int b) {
    String temp = x[a];
    x[a] = x[b];
    x[b] = temp;
}
```

```java
public static void testSwap() {
    String[] input = {"i", "have", "an", "egg"};
    int a = 0;
    int b = 2;
    String[] expected = {"an", "have", "i", "egg"};

    Sort.swap(input, a, b);
    org.junit.Assert.assertArrayEquals(expected, input);
}
```

### Recursive Array Helper Method

Since Java doesn't support array slicing (`x[1:2]`), we should add a helper method to recursively sort the whole array.

```java
public static void sort(String[] x) {
    sort(x, 0);
}

public static void sort(String[] x, int start) {
    smallestIndex = findSmallest(x);
    swap(x, start, smallestIndex);
    sort(x, start + 1);
}
```

However, the `findSmallest` method doesn't work well since it will start from 0 instead of the start point. We could easily fix it.

```java
public static int findSmallest(String[] x, int start) {
    int smallestIndex = start;
    for (int i = start; i < x.length; i += 1) {
        int cmp = x[i].compareTo(x[smallestIndex]);
        if (cmp < 0) {
            smallestIndex = i;
        }
    }
    return smallestIndex;
}
```

### Enhanced JUnit Test

To allow IntelliJ to automatically run the tests, we could modify our test structure as the following rules:

* Import the library `org.junit.Test`.
* Precede each method with `@Test` (no semi-colon).
* Change each test method to be non-static.
* Remove our main method from the `TestSort` class.


## Inheritance, Implements 

- 

### Method Overloading
- when there is many methods with the same name and return type but diffrent signature(parameters).int, float. 

Suppose we have the method below, which will return the longest string in a `SLList`.

```java
public static String longest(SLList<String> list) {
    int maxDex = 0;
    for (int i = 0; i < list.size(); i += 1) {
        String longestString = list.get(maxDex);
        String thisString = list.get(i);
        if (thisString.length() > longestString.length()) {
            maxDex = i;
        }
    }
    return list.get(maxDex);
}
```

However, since `SLList` and `AList` have exactly the same structure, we could write two `longest` methods to make it compatible with both classes, which is a feature called "method overloading" in Java.

```java
public static String longest(SLList<String> list)
public static String longest(AList<String> list)
```

The disadvantage of method overloading is that it makes the source code quite longer than usual and add more codes to maintain.

### Hypernyms, Hyponyms, and Interface Inheritance

In Java, in order to express the hierarchy, we need to do two things:

* Define a type for the general list hypernym -- we will choose the name List61B.
* Specify that SLList and AList are hyponyms of that type.

First, we will create a `List61B` interface, which is a contract that specifies a list of method a list must able to do:

```java
public interface List61B<Item> {
    public void addFirst(Item x);
    public void add Last(Item y);
    public Item getFirst();
    public Item getLast();
    public Item removeLast();
    public Item get(int i);
    public void insert(Item x, int position);
    public int size();
}
```

Next, we will modify the definition of both classs to make a promise that both of them implement all the method defined in the interface `List61B`.

```
public class SLList<Item> implements List61B<Item>{...}
public class AList<Item> implements List61B<Item>{...}
```

Now we can edit our `longest` method to take in a `List61B`. Because `AList` and `SLList` share an "is-a" relationship.

### Method Overriding

Method overriding means that you implement a method as the same structure as it is defined in a interface, while overloaded methods could have different parameters. In this course, we will add `@Override` tag above each overrided methods. Although this tag is unnecessary, it's useful in debugging.

Different from the Golden Rules of Equal, if X is a subclass of Y, the memory box of X may contain Y. For instance, this piece of code will works well:

```java
public static void main(String[] args) {
    List61B<String> someList = new SLList<String>();
    someList.addFirst("elk");
}
```

### Implementation Inheritance

Typically, we can't add specific implementation to a interface. However, We could add a `default` key word for a method in a interface to allow subclasses to inhert it. Although `SLList` or `AList` does not implement the `print` method in their classes, it will still work.

```
default public void print() {
    for (int i = 0; i < size(); i += 1) {
        System.out.print(get(i) + " ");
    }
    System.out.println();
}
```

In order to re-implement the default method in a subclass, we must use the `Override` tag, or the default one will be invoked.

#### Dynamic Method Selection

In Java, variables have two phases of types: static type and dynamic type. In the code below, `lst` has a static type of `List61B` and a dynamic type `SLList`. When Java runs a method that is overriden, it searches for the appropriate method signature in it's **dynamic type** and runs it.

```java
List61B<String> lst = new SLList<String>();
```

#### Method Selection Algorithm

Suppose we have a function `foo.bar(x1)`, where `foo` has the static type `TPrime`, and `x1` has the static type `T1`.

##### Compile

When compiling the code, compiler verifies that `TPrime` has at least one method that could handle `T1`, and will record the most **specific** one.

##### Run

When running the code, if `foo`'s dynamic type **override** the `bar` method in `TPrime`, use the overridden method. Otherwise, use the recorded method.

### Interface Inheritance vs Implementation Inheritance

* Interface inheritance (what): Simply tells what the subclasses should be able to do.
* Implementation inheritance (how): Tells the subclasses how they should behave.


## Interface & Classes  
- Interface implemented by classes, they describe the ability that can apply to many classes 
- Interface like abstract classes, they do not implemnts the methods they specify 
- one class can implemnts many interface. 
- class can extend only one class, class can extended by many classes.


# File: Week 04.md

# CS 61B Week 04

## Extends, Casting, Higher Order Functions
- 
- Although interfaces provide us a way to define a hierarchical relationship, the `extends` key word let us define a hierarchical relationship between classes. 


If we want to define a new class which have all methods in the `SLList` but new ones such as `RotatingRight`.

```java
public class RotatingSLList<Item> extends SLList<Item>
```

With `extends` key word, the subclass withh inhert all these components:

* All instance and static variables
* All methods
* All nested classes

## VengefulSLList

We could build a `VengefulSLList` class to make a list that could remeber the deleted items. `super` key word could be used to call the corresponding method in the super class.

```java
public class VengefulSLList<Item> extends SLList<Item> {
    SLList<Item> deletedItems;

    public VengefulSLList() {
        deletedItems = new SLList<Item>();
    }

    @Override
    public Item removeLast() {
        Item x = super.removeLast();
        deletedItems.addLast(x);
        return x;
    }

    /** Prints deleted items. */
    public void printLostItems() {
        deletedItems.print();
    }
}
```

### Constructors

While constructors are not inherited, Java requires that all constructors must start with a call to one of its superclass's constructors. If you don't call it explicitly, Java will automatically call it for you. If we forget to specify which contructor to use, Java will call the default one without parameters.

### The Object Class

Every class in Java is a descendant of the Object class, or extends the Object class. Even classes that do not have an explicit extends in their class still implicitly extend the Object class.

### Encapsulation

* A model is a set of methods working together to perform some task or set of related tasks.
* A model is said to be encapsulated if its implementation is highly hidden: It can be accessed merely through the documented interfaces.

### Type Checking and Casting

Compilers will check types of objects based on its staitc type. For instance, the following code will result in a compile-time error since the compiler thinks that `SLList` does not have the `printLostItem` method and `vsl2` can't contain the `SLList` object.

```java
VengefulSLList<Integer> vsl = new VengefulSLList<Integer>(9);
SLList<Integer> sl = vsl;
sl.printLostItems();
VengefulSLList<Integer> vsl2 = sl;
```

#### Expressions

As we seen above, expression with `new` key word has compile-time types.

```java
SLList<Integer> sl = new VengefulSLList<Integer>();
```

Above, the compile-time type of the right-hand side of the expression is `VengefulSLList`. The compiler checks to make sure that `VengefulSLList` "is-a" `SLList`, and allows this assignment.

#### Method

The type of a method's return value is the method's compile-time type. Since the return type of `maxDog` is `Dog`, any call to `maxDog` will have compile-time type `Dog`.

```java
Poodle frank = new Poodle("Frank", 5);
Poodle frankJr = new Poodle("Frank Jr.", 15);

Dog largerDog = maxDog(frank, frankJr);
Poodle largerPoodle = maxDog(frank, frankJr); //does not compile! RHS has compile-time type Dog
```

#### Casting

We could specify the type of an expression or a method to let Java compiler ignore type check. That might be dangerous and may cause run-time errors.

```java
Poodle largerPoodle = (Poodle) maxDog(frank, frankJr); // compiles! Right hand side has compile-time type Poodle after casting
```


### High Order Functions

In Python, we could define a function that will take another function as a parameter.

```python
def tenX(x):
    return 10 * x

def do_twice(f, x):
    return f(f(x))
```

In Java, we could do so by declaring an interface.

```java
public interface IntUnaryFunction{
    int apply(int x) {}
}

public class TenX implements IntUnaryFunction{
    public int apply(int x) {
        return 10 * x;
    }

    
}
```

```java
public static int do_twice(IntUnaryFunction f, int x) {
    return f.apply(f.apply(x));
}

System.out.println(do_twice(new TenX(), 2));
```

## Subtype Polymorphism vs. HoFs

### Max Function

Suppose we will write a method `max` that will take an array of objects and return the maximum one.

```java
public static Object max(Object[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i += 1) {
        if (items[i] > items[maxDex]) {
            maxDex = i;
        }
    }
    return items[maxDex];
}

public static void main(String[] args) {
    Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
    Dog maxDog = (Dog) max(dogs);
    maxDog.bark();
}
```

The problem is that the `Dog` object can't work with the `>` operater.
To fix this problem, we may change the function slightly.

```java
public static Dog maxDog(Dog[] dogs) {
    if (dogs == null || dogs.length == 0) {
        return null;
    }
    Dog maxDog = dogs[0];
    for (Dog d : dogs) {
        if (d.size > maxDog.size) {
            maxDog = d;
        }
    }
    return maxDog;
}
```

However, another problem is that we couldn't generalize this function to other type of objects.

### compareTo

We can create an interface that guarantees that any implementing class, like `Dog`, contains a comparison method, which we'll call `compareTo`.

Let's write our own interface.

```java
public interface OurComparable {
    public int compareTo(Object o);
}
```

* Return a negative number if `this` < o.
* Return 0 if `this` equals o.
* Return a positive number if `this` > o.

Now, we could let our `Dog` class implements the `OurComparable` interface.

```java
public class Dog implements OurComparable {
    private String name;
    private int size;

    public Dog(String n, int s) {
        name = n;
        size = s;
    }

    public void bark() {
        System.out.println(name + " says: bark");
    }

    public int compareTo(Object o) {
        Dog uddaDog = (Dog) o;
        return this.size - uddaDog.size;
    }
}
```

Then, we could generalize the `max` function by taking in `OurComparable`  objects.

```java
public static OurComparable max(OurComparable[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i += 1) {
        int cmp = items[i].compareTo(items[maxDex]);
        if (cmp > 0) {
            maxDex = i;
        }
    }
    return items[maxDex];
}
```

### Comparables

Although `OurComparable` interface seems solved the issue, it's awkward to use and there's no existing classes implement `OurComparable`.
The solution is that we could use the existed interface `Comparable`.

```java
public interface Comparable<T> {
    public int compareTo(T obj);
}
```

Notice that `Comparable<T>` means that it takes a generic type. This will help us avoid having to cast an object to a specific type.

```java
public class Dog implements Comparable<Dog> {
    ...
    public int compareTo(Dog uddaDog) {
        return this.size - uddaDog.size;
    }
}
```

### Comparator

We could only implement one `compareTo` method for each class. However, if we want to add more orders of comparasion, we could implement `Comparator` interface.

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```
This shows that the `Comparator` interface requires that any implementing class implements the `compare` method.

To compare two dogs based on their names, we can simply defer to `String`'s already defined `compareTo` method.

```java
import java.util.Comparator;

public class Dog implements Comparable<Dog> {
    ...
    public int compareTo(Dog uddaDog) {
        return this.size - uddaDog.size;
    }

    private static class NameComparator implements Comparator<Dog> {
        public int compare(Dog a, Dog b) {
            return a.name.compareTo(b.name);
        }
    }

    public static Comparator<Dog> getNameComparator() {
        return new NameComparator();
    }
}
```

```java
Comparator<Dog> nc = Dog.getNameComparator();
```

## Exceptions, Iterators, Object Methods

### Lists

Java provides us a built-in `List` interface and several implementations. 

```java
java.util.List<Integer> L = new java.util.ArrayList<>();
```

We could import the packages to make the code more simple.

```java
import java.util.List;
import java.util.ArrayList;

public class Example {
    public static void main(String[] args) {
        List<Integer> L = new ArrayList<>();
        L.add(5);
        L.add(10);
        System.out.println(L);
    }
}
```

### Sets

Sets are a collection of unique elements, which means that you can only have one copy of each element. There is also no sense of order.

```java
import java.util.Set;
import java.util.HashSet;

Set<String> s = new HashSet<>();
s.add("Tokyo");
s.add("Lagos");
System.out.println(S.contains("Tokyo"));
```

### ArraySet

Our goal is to make our own set with `ArrayList` we built before, which has the following methods.

* `add(value)`: add the value to the set if not already present
* `contains(value)`: check to see if ArraySet contains the key
* `size()`: return number of values

Here's our code.

```java
public class ArraySet<T>  {
    private T[] items;
    private int size; // the next item to be added will be at position size

    public ArraySet() {
        items = (T[]) new Object[100];
        size = 0;
    }

    /* Returns true if this map contains a mapping for the specified key.
     */
    public boolean contains(T x) {
        for (int i = 0; i < size; i += 1) {
            if (items[i].equals(x)) {
                return true;
            }
        }
        return false;
    }

    /* Associates the specified value with the specified key in this map. */
    public void add(T x) {
        if (contains(x)) {
            return;
        }
        items[size] = x;
        size += 1;
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return size;
    }
}
```

### Exceptions

When we add `null` to our `ArraySet`, the program will crash since we are calling `null.equals(x)` and will throw a `NullPointerException`.

However, we could manually throw an exception `IllegalArgumentException` by updating the `add` method.

```java
public void add(T x) {
    if (x == null) {
        throw new IllegalArgumentException("can't add null");
    }
    if (contains(x)) {
        return;
    }
    items[size] = x;
    size += 1;
}
```

### Iterations

We could use enhanced loop in Java's `HashSet`:

```java
Set<String> s = new HashSet<>();
s.add("Tokyo");
s.add("Lagos");
for (String city : s) {
    System.out.println(city);
}
```

The code above is equal to the following ugly implementaion.

```java
Set<String> s = new HashSet<>();
...
Iterator<String> seer = s.iterator();
while (seer.hasNext()) {
    String city = seer.next();
    ...
}
```

If we want to do the same (ugly version of enhanced loop) with our own `ArraySet`, we have to implement the `Iterator` interface.

```java
public interface Iterator<T> {
    boolean hasNext();
    T next();
}
```

```java
private class ArraySetIterator implements Iterator<T> {
    private int wizPos;

    public ArraySetIterator() {
        wizPos = 0;
    }

    public boolean hasNext() {
        return wizPos < size;
    }

    public T next() {
        T returnItem = items[wizPos];
        wizPos += 1;
        return returnItem;
    }
}
```

However, if we want to let our class support enhanced for loop, we have to implement `Iterable` interface.

```java
import java.util.Iterator;

public class ArraySet<T> implements Iterable<T> {
    private T[] items;
    private int size; // the next item to be added will be at position size

    public ArraySet() {
        items = (T[]) new Object[100];
        size = 0;
    }

    /* Returns true if this map contains a mapping for the specified key.
     */
    public boolean contains(T x) {
        for (int i = 0; i < size; i += 1) {
            if (items[i].equals(x)) {
                return true;
            }
        }
        return false;
    }

    /* Associates the specified value with the specified key in this map.
       Throws an IllegalArgumentException if the key is null. */
    public void add(T x) {
        if (x == null) {
            throw new IllegalArgumentException("can't add null");
        }
        if (contains(x)) {
            return;
        }
        items[size] = x;
        size += 1;
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return size;
    }

    /** returns an iterator (a.k.a. seer) into ME */
    public Iterator<T> iterator() {
        return new ArraySetIterator();
    }

    private class ArraySetIterator implements Iterator<T> {
        private int wizPos;

        public ArraySetIterator() {
            wizPos = 0;
        }

        public boolean hasNext() {
            return wizPos < size;
        }

        public T next() {
            T returnItem = items[wizPos];
            wizPos += 1;
            return returnItem;
        }
    }

    public static void main(String[] args) {
        ArraySet<Integer> aset = new ArraySet<>();
        aset.add(5);
        aset.add(23);
        aset.add(42);

        //iteration
        for (int i : aset) {
            System.out.println(i);
        }
    }

}
```

### toString

If we want to print out the whole class, we have to override `toString` method, or we will only get the name and the address of the object.

```java
public String toString() {
    String returnString = "{";
    for (int i = 0; i < size; i += 1) {
        returnString += keys[i];
        returnString += ", ";
    }
    returnString += "}";
    return returnString;
}
```

Here's an enhanced version utilizing the `StringBuilder` which is more efficient.

```java
public String toString() {
    StringBuilder returnSB = new StringBuilder("{");
    for (int i = 0; i < size - 1; i += 1) {
        returnSB.append(items[i].toString());
        returnSB.append(", ");
    }
    returnSB.append(items[size - 1]);
    returnSB.append("}");
    return returnSB.toString();
}
```
### equals

In order to check whether a given object is equal to another object, we have to implement the `equals` method, since `==` will only compare the memory address.

```java
public boolean equals(Object other) {
    if (this == other) {
        return true;
    }
    if (other == null) {
        return false;
    }
    if (other.getClass() != this.getClass()) {
        return false;
    }
    ArraySet<T> o = (ArraySet<T>) other;
    if (o.size() != this.size()) {
        return false;
    }
    for (T item : this) {
        if (!o.contains(item)) {
            return false;
        }
    }
    return true;
}
```


# File: Week 05.md

# CS 61B Week 05 

## Command ling programming, Git 
- Command line Args is the way that provide input to program in the command line. 
- args is an array of values.

```java 

public class ArgSum { 
  public static void main(String[] args) { 
    int index = 0; 
    int sum  = 0; 

    while(index < args.length) { 
      sum += Integer.parseInt(args[index]); 
      index += 1; 
    }

    System.out.println(sum);
  }
}
```


### Git 
- C code is compiled into binary which is does'nt require intetrpereter. 
- ``` char **argv ``` is equal to String[] args. 
- Git depends on Maps, Hashs, File I/0, Graphs. 
- Evry time you make change git store a **Copy** of the entire reop in a file called .git.

#### Redundancy 
- We only store files that changed. 
- each commit is a map. foreach filename it maps that filename to  a version number. 
- we use the last commits versions. 
- if we revert to the same version of file we doesn't Store the new file. 
##### avoid redundancy with Hashing 
- Use time and date as the version number!  
- No not the time or the date it's SHA-1 hashing algorithm 
- SHA-1 is a deterministic function of the file content, tow identical files will have the same git-SHA1 number. 
- 160 bits long, is the hash of the (File size, a zero, file content).
- git hash-object 'file-name' give the hash of the file from the CL.
- git computes SHA1 for the file, create folder in .git/objects with name of first 2 digits, sore the content in the file with name 'rest of hash' in a copressed format.
- Objects 
- Collision maybe will be there two files with the same hash but it's almost imposible. 
- inject malicious code into file would result in change in the git-sha1 hash. 

##### Serializing and storing 
- implements serializable to store arbitary objects. 

#### Branching 
- 
## Asymptotics1

### Writing Efficient Programs

In the past few weeks, our focus is on saving our time: Java syntax, debugging tools, and other stuffs. However, in this week, we will focus on designing efficient programs. 

Let's take a typical question as an example: Determine if a sorted array contains any duplicates.
Given sorted array `A`, are there indices `i` and `j` where `A[i] = A[j]`?

Silly algorithm: Consider every possible pair, returning true if any match.
Are (-3, -1) the same? Are (-3, 2) the same? ...

Better algorithm: For each number A[i], look at A[i+1], and return true the first time you see a match. If you run out of items, return false.

### Intuitive Runtime Characterizations

Our goal is to somehow characterize the runtimes of the functions below.

Characterization should be simple and mathematically rigorous.

Characterization should demonstrate superiority of `dup2` over `dup1`.

```java
public static boolean dup1(int[] A) {
  for (int i = 0; i < A.length; i += 1) {
    for (int j = i + 1; j < A.length; j += 1) {
      if (A[i] == A[j]) {
         return true;
      }
    }
  }
  return false;
}

public static boolean dup2(int[] A) {
  for (int i = 0; i < A.length - 1; i += 1) {
    if (A[i] == A[i + 1]) { 
      return true; 
    }
  }
  return false;
}
```

#### Technique 1

Measure in Seconds using the following tools:

*  Physical stopwatch.
* Unix has a built in time command that measures execution time. 
- ``` time proj ```
* Princeton Standard library has a Stopwatch class.

Advantage: Easy to measure and interpret.

Disadvantage: Result varies with machine and compilers.

#### Technique 2A

Count possible operations for an array of size `N = 10,000`.

For example, the `<` operates for 2 to 50,015,001 times.

Advantage: Machine independent. Input dependence captured in model. 

Disadvantage: Tedious to compute. Array size was arbitrary. Doesn’t tell you actual time.

#### Technique 2B

Count possible operations in terms of input array size N.

For example, the `<` operater in `dup1` runs for 2 to (N^2+3N+2)/2 times.

Advantage: Machine independent. Input dependence captured in model. Tells you how algorithm scales.

Disadvantage: Even more tedious to compute. Doesn’t tell you actual time.

### Comparing Algorithms

Although we have these techniques, we have to compare the efficiency of two algorithms: `dup1` and `dup2`, which cost 2 to (N^2+3N+2)/2 and 0 to N.

Since parabolas grow faster than straight lines, `dup2` is better.

In most case, we only consider asymptotic behavior (what happens for very large N), which means we will ignore the case that for small datasets, `dup1` might faster than `dup2`.

#### Simplified Solution

* Consider only the worst case where the difference of efficience occurs.
* Restrict attention to one operation.
* Eliminate low order terms. (N^2+3N+2)/2 -> (N^2)/2
* Eliminate multiplicative constants. (N^2)/2 -> N^2

Through the four steps, the order of growth of `dup1` is N^2, while that of `dup2` is N.

#### Simplified Analysis Process

Rather than building an entire table, we could:

* Choose representative operation to count. (cost model)
* Figure out the order of growth of that function by making exact count or using intuition and inspection.

### Big Theta

Big Theta represents the order of growth of a function.

### Big O

Whereas Big Theta means "equals", Big O means "less than or equal". (The order of growth of a function is less than or equal to Big O)

**Asymptotics** allow us to evaluate the performance of our programs using math. We ignore all constants and only care about the value with reference to the input (usually defined as N)

* **Big O** - The upper bound in terms of the input. In other words, if a function has big O in f(x), we say that it could grow at most as fast as f(x), but it could grow more slowly.  
**is the functions f(x) is grow smaller than g(x) f(x) belongs to O(g(n)** 
* **Big Ω** - The lower bound in terms of the input. In other words, if a function has big Ω in f(x), we say that it could grow at least as slowly as f(x), but it could grow more quickly.  **is the functions f(x) is grow faster than g(x). f(x) belongs to Ω(g(n)** 
* **Big Θ** - The tightest bound, which only exists when the tightest upper bound and the tightest lower bound converge to the same value.  **is the fuction grow at the same rate as f(g).** 

**Fun sums**
	1 + 2 + 3 + . . . + N  = Θ(N2)          
	1 + 2 + 4 + . . . + N  = Θ(N)



# File: Week 06.md

# CS 61B Week 06

## Disjoint Sets

### Dynamic Connectivity and the Disjoint Sets Problem

#### Introduction

Our data structure will implement these operations:

* connect(x, y): Connects x and y.
* isConnected(x, y): Returns true if x and y are connected. Connections can be transitive, which means that they don’t need to be direct.

For simpilicity, we will declare that all items are integers and independent from each other.

```java
ds = DisjointSets(7)
ds.connect(0, 1)
ds.connect(1, 2)
ds.connect(0, 4)
ds.connect(3, 5)
ds.isConnected(2, 4) // true
ds.isConnected(3, 0) // false

ds.connect(4, 2)
ds.connect(4, 6)
ds.connect(3, 6)
ds.isConnected(3, 0) //true
```

#### Disjoint Sets Interface

```java
public interface DisjointSets {
	/** Connects two items P and Q. */
	void connect(int p, int q);
 
	/** Checks to see if two items are connected. */
	boolean isConnected(int p, int q);
}
```

We will implement this interface to achieve these goals:          

* Number of elements N can be huge.
* Number of method calls M can be huge.
*  Calls to methods may be interspersed

Naive Approach: Record all the connections in a data structure, and do some iteration to see if one thing can be reached from each other.
```
List<Set<Integer>> 
```

Better Approach: Ignore how things are connected, and only record sets that each belongs to.

### Quick Find

To find whether two items are connected, here are two ways:

1. List of sets of integers.

Very inituitive way, but quite slow for large N.

2. List of integers where ith entry gives set number.

connect(p, q): Change entries that equal id[p] to id[q]  

`{0, 1, 2, 4}, {3, 5}, {6}`

![quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.2.1.png)

![quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.2.2.png )     

```java
public class QuickFindDS implements DisjointSets {
   private int[] id;
   
   public QuickFindDS(int N) {
      id = new int[N];
      for (int i = 0; i < N; i++) {
         id[i] = i;
      }
	}

	public boolean isConnected(int p, int q) {
      return id[p] == id[q];
	}
 
	public void connect(int p, int q) {
      int pid = id[p];
      int qid = id[q];
      for (int i = 0; i < id.length; i++) {
         if (id[i] == pid) {
            id[i] = qid;
         }     
      }
    }
} 
```

However, connecting is still slow.

### Quick Union   

Instead of using random number to represent the index of sets, we could let each entry to be its parent, which results in a tree-like shape.

To connect two items  
* Find the root of the two items.
* simply change the root of one item to the root of another item.

However, this method is still slow since the tree might be quite tall and the cost of the worst case is proportional to the height.  

`{0, 1, 2, 4}, {3, 5}, {6}`

![quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.3.1.png)


`connect(5, 2)`

![quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.3.2.png)


```java        
public QuikUnionDS(int num) { 
        parent = new int[num]; 
        for(int i =0 ; i < parent.length; i+= 1) { 
            parent[i] = i; 
        }
    }

    private static int find(int p) { 
         while(parent[p] > 0) { 
           p = parent[p];
         }
         return p;
    }

    public void connect(int p, int q) { 
        int i = find(p); 
        int j = find(q); 
        parent[i] = j; 
    }

    public boolean isConnected(int p, int q) { 
        return (find(p) == find(q));        
    }


```

### Weighted Quick Union

We could modify Quick Union to avoid tall trees: Track tree size and link root of smaller tree to the larger one. 
assing root of smaller tree to the root of larger tree. 
root tree take negative value to indicate the size of the tree.

Thus, the `connect` and `isConnected` operation will never be slower than `log N`, which is fast enough for most programs.

Although we could track the height instead of weight, we will find out that the performance is similar.  
to  maximize the height of the tree by one, you double the size of the tree. 
![weighted-quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.4.1.png) 

![weighted-quick-union](https://joshhug.gitbooks.io/hug61b/content/chap9/9.4.2.png)



## Asymptotics II

In this section, we will discuss more difficult examples related to run-time analysis.

### Function dup1

```java
public boolean dup1(int[] A) {
	int N = A.length;
	for (int i = 0; i < N; i += 1)
	for (int j = i + 1; j < N; j += 1)
		if (A[i] == A[j])
			return true;   
	return false;
}
```

The worst case is that we have to go through every entry (the outer loop runs N times).

The number of comparisons is: `C = 1+2+3+...+(N−3)+(N−2)+(N−1) = N(N−1)/2`

Thus, since `==` is a constant time operation, the overall runtime in the worst case is Theta(N^2).

### printParty

```java
public static void printParty(int N) {
   for (int i = 1; i <= N; i = i * 2) {
      for (int j = 0; j < i; j += 1) {
         System.out.println("hello");
         int ZUG = 1 + 1;
      }
   }
}
```
Let's create a visualization to find out the runtime cost of the function above.

![printParty](https://joshhug.gitbooks.io/hug61b/content/assets/loops2_4.png "printParty")

We could conclude that `C(N) = 1 + 2 + 4 + ... + N = 2N - 1 (if N is a power of 2)`, which is in the linear family.

### Recursion (f3)

```java
public static int f3(int n) {
   if (n <= 1) 
      return 1;
   return f3(n-1) + f3(n-1);
}
```

Here's a visualization of the function above.

![recursion](https://joshhug.gitbooks.io/hug61b/content/assets/asymptotics2_tree.png "recursion")

We could conclude that the runtime cost of the function is `C(1)=1 C(2) = 1 + 2C(2)=1+2 C(3) = 1 + 2 + 4C(3)=1+2+4 C(N) = 1 + 2 + 4 + ... +C(N) = 1+2+4+...+ ??? = 2^(N−1)`, which is in the `2^N` family.

### Binary Search

Binary search is a practical way to find an item in a sorted list.
To do a binary search, we start in the middle of the list, and check if that's our desired element.

* Start in the middle of the list and check if that's our desired element. 
* If the desired element is larger, eliminate the first half of the list and return to step one.
* If the desired element is smaller, eliminate the second half of the list and return to step one.

```java
static int binarySearch(String[] sorted, String x, int lo, int hi)
    if (lo > hi) return -1;
    int m = lo + (hi - lo) / 2;
    int cmp = x.compareTo(sorted[m]);
    if (cmp < 0) return binarySearch(sorted, x, lo, m - 1);
    else if (cmp > 0) return binarySearch(sorted, x, m + 1, hi);
    else return m;
}
```
Intuitively, the runtime cost of binary search is `log_2 N`, since we can figure out that the count seems to increase by one only when `N` hits a power of 2.

We can be even more precise: `C(N) = ⌊log_2 (N)⌋+1`. Because `⌊f(N)⌋ = Θ(f(N))`, `Θ(⌊log_2 (N)⌋) = Θ(log N)`.

Log time is faster than linear time and even as better as constant time, which makes binary search an efficient algorithm.

### Merge Sort

Selection sort works off two basic steps:

* Find the smallest item among the unsorted items, move it to the front, and fix it in place.
* Sort the remaining unsorted items using selection sort.

If we analyze selection sort, we see that it's `Theta(N^2)`. To improve it, we could divide the array into two halves, sort them, and merge them, in which merging only costs `Theta(N)`.

This is the essence of merge sort:
* If the list is size 1, return. Otherwise:
* Mergesort the left half
* Mergesort the right half
* Merge the results

Mergesort has worst case runtime: `C = Theta(NlogN)`, since it has `logN` levels:
* The top level takes ~N.
* Next level takes ~N/2 + ~N/2 = ~N.
* One more level down: ~N/4 + ~N/4 + ~N/4 + ~N/4 = ~N.

`Theta(NlogN)` is better than `Theta(N^2)`, so that merge sort is better than selection sort.

## ADTs, Sets, Maps, BSTs

### Abstract Data Type

An Abstract Data Type (ADT) is defined only by its operations, not by its implementation. For instance, we say that `ArrayDeque` and `LinkedListDeque` are implementations of the `Deque` ADT. Here are a few commonly used ADTs.

**Stacks**: Structures that support last-in first-out retrieval of elements
* push(int x): puts x on the top of the stack
* int pop(): takes the element on the top of the stack

**Lists**: an ordered set of elements
* add(int i): adds an element
* int get(int i): gets element at index i 

**GrabBag** 
* insert(int x): Inserts x into the grab bag.
* int remove(): Removes a random item from the bag.
* int sample(): Samples a random item from the bag (without removing!)
* int size(): Number of items in the bag.


**Queues**: Structures that support first-in first-out retrieval of elements
* enqueue(int x): puts x on the end of the queue
* int dequeue(): takes the element at the front of the queue


**Sets**: an unordered set of unique elements (no repeats)
* add(int i): adds an element
* contains(int i): returns a boolean for whether or not the set contains the value

**Maps**: set of key/value pairs
* put(K key, V value): puts a key value pair into the map
* V get(K key): gets the value corresponding to the key

### Binary Search Trees

Linked List is unefficient in searching for items since it will cost linear time even if the list is sorted.

We can optimize by adding pointers to the middle of each recursive half of the linked list. The structure is called a **binary tree**.

![BST](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-02-28%20at%2012.59.39%20AM.png "BST")

#### Properties of trees

Trees are composed of:
* nodes
* edges that connect those nodes. (Only one path between any two nodes.)

In some trees, we select a **root** node which is a node that has no parents.

Tree also has **leaves**, which are nodes with no children.

Binary Trees: In addition to the above requirements, also hold the binary property constraint that each node has either 0, 1, or 2 children.

Binary Search Trees: For each node X in a binary tree:

* Every key in the left subtree is less than X’s key.
* Every key in the right subtree is greater than X’s key.

Here's a simple BST class:

```java
private class BST<Key> {
    private Key key;
    private BST left;
    private BST right;

    public BST(Key key, BST left, BST Right) {
        this.key = key;
        this.left = left;
        this.right = right;
    }

    public BST(Key key) {
        this.key = key;
    }
}
```

#### Search

* Start from root note.
* If searched item is larger, move to the child at right.
* Otherwise, move to the child at left.
* Repeat recursively until we find the item or we get to the leaf of the tree, which means that the tree does not contain the item we want to find.

```java
static BST find(BST T, Key sk) {
   if (T == null)
      return null;
   if (sk.equals(T.key))
      return T;
   else if (sk ≺ T.key)
      return find(T.left, sk);
   else
      return find(T.right, sk);
}
```

If our tree is relatively "bushy", the find operation will run in `log N` time because the height of the tree is `log N`.

### Insert

We always insert at a leaf node.

* Search the item in the tree. If we find it, then we don't do anything.
* We can just add the new element to either the left or right of the leaf, preserving the BST property.

```java
static BST insert(BST T, Key ik) {
  if (T == null)
    return new BST(ik);
  if (ik ≺ T.key)
    T.left = insert(T.left, ik);
  else if (ik ≻ T.key)
    T.right = insert(T.right, ik);
  return T;
}
```

### Delete

#### No children

If the node has no children, it is a leaf, and we can just delete its parent pointer and the node will eventually be swept away by the garbage collector.

#### 1 child

If the node only has one child, we know that the child maintains the BST property with the parent of the node because the property is recursive to the right and left subtrees.

Therefore, we can just reassign the parent's child pointer to the node's child and the node will eventually be garbage collected.

![Delete 1 child](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-02-28%20at%2010.35.56%20AM.png "Delete 1 child")

#### 2 children

If the deleted node has two chldren, we choose a new node to replace the deleted one.

We know that the new node must be:
* larger than everything in left subtree.
* smaller than everything right subtree.

We can just take the right-most node in the left subtree or the left-most node in the right subtree. The method is called Hibbard deletion.

![Delete 2 children](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-02-28%20at%2010.43.45%20AM.png "Delete 2 children")

### BSTs as Sets and Maps

We can use BST to implement `Set` ADT, which is better than using ArraySet, since the worst-case runtime cost `log N` is faster than `N`.

We can also implement `Map` with BST by having each node hold (key,value) pairs. We will compare each element's key to determine where to place it within our tree.     


# File: Week 07.md

# CS 61B Week 07

## B-Trees (2-3, 2-3-4 Trees)

Height of BST
* Worst case: `Theta(N)` == `O(N)`
* Best case: `Theta(log N)`.

where N is number of nodes.

![Height of BST ](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-05%20at%2012.56.54%20PM.png "Height of BST ")

O is just an upper bound, rather than the worst case. However, many people use O as a shorthand for worst case.

### BST Performance

* depth: the number of links between a node and the root.
* height: the deepest depth of a tree, which determines the worst case of runtime.
* average depth: average of the total depths in the tree, which determines the average-case runtime.

### BST insertion order

If we insert nodes in random order, it will actually end up being relatively bushy, in which the average depth and height are expected to be `Theta(log N)`.  

**Bushy** means that the average depth is expected to be `Theta(logN)`. 
**Spindly** means that the average depth is expected to be `Theta(N)`.

However, in the real world situations, data are unlikely inserted randomly.

### B-Trees

![2-3-4 Tree](https://media.geeksforgeeks.org/wp-content/uploads/20200506235136/output253.png "2-3-4 Tree")


The tree showed above is called **B-Tree** or **2-3-4 Tree**. 2-3-4 and 2-3 refer to the number of children each node can have. 

The process of adding a node to a 2-3-4 tree is:

* Similar to BST, we compare new item with each node in the tree and insert it into the leaf node.
* If the node has 4 items, pop up the middle left item and re-arrange the children accordingly.
* If the parent node having 4 nodes, pop up the middle left node again, rearranging the children accordingly.
* Repeat this process until the parent node can accommodate or you get to the root.

### B-tree Runtime Analysis

The worst case runtime of searching a B-tree:
* Each node had the maximum number of elements
* Traverse all the way to bottom

The amount of operations will be `L log N`. Since `L` is constant, the runtime is `O(log N)`. B-tree is complex, but it could handle insertion operations in any order. 

### B-tree invariants 
- All leaves must be the same distance from the root. 
- A non-leaf node with k items mut has exactly k+1 children.

## Red Black Trees (Lecture 18)

We will create a new tree structure similar to B-trees, since B-trees are really difficult to implement.

### Rotating Trees

rotateLeft(G): Let x be the right child of G. Make G the new left child of x.

rotateRight(G): Let x be the left child of G. Make G the new right child of x.

Below is a graphical description of what happens in a left rotation on the node G.

![Rotating Left](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-06%20at%2010.25.18%20PM.png)

We could also rotate a non-root node.

![None Root Node](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-06%20at%2010.37.17%20PM.png)

### Red Black Trees

We could use a red link to convert a 3-node to BST tree. We choose arbitrarily to make the left element a child of the right one. The structure is called left-leaning red-black trees (LLRB).

![Red Link](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-06%20at%2010.56.51%20PM.png)

#### Properties

* Left-Leaning Red-Black trees have a 1-1 correspondence with 2-3 trees.
* No node has 2 red links.
* There are no red right-links.
* Every path from root to leaf has same number of black links.
* Height is no more than 2x height of corresponding 2-3 tree.

#### Inserting into LLRB

* While inserting, use a red link.
* If there is a right leaning "3-node", rotate left the appropriate node to fix.
* If there are two consecutive left links, rotate right the appropriate node to fix.
* If there are any nodes with two red children, color flip the node to emulate the split operation.

#### Runtime

Because a LLRB tree has a 1-1 correspondence with a 2-3 tree and will always remain within 2x the height of its 2-3 tree, the runtimes of the operations will take `log N` time.

# Hashing (Lecture  19)

### Limits of Search Tree Based Sets

* Require items to be comparable.
* Have excellent performance, but could be better.

### Using Data as an Index

We could create an array of booleans indexed by data.
* Initially all values are `false`.
* When an item is added, set the index to `true`.

```java
DataIndexedIntegerSet diis;
diis = new DataaIndexedIntegerSet();
diis.add(0); // set 0 to true
diis.add(5); // set 5 to true
```

#### Implementation

```java
public class DataIndexedIntegerSet {
    private boolean[] present;

    public DataIndexedIntegerSet() {
        present = new boolean[2000000000];
    }

    public void add(int x) {
        present[i] = true;
    }

    public boolean contains(int x) {
        return present[i];
    }
}
```

#### Performance

* `contains(x)`: Theta(1);
* `add(x)`: Theta(1);
* Extremely wasteful of memory.
* Need ways to generalize beyond integers.

### Inserting Words

#### First Attempt

Suppose we want to insert "cat" to the data strucutre, we could use the first letter as index. (a = 1, b = 2, ... , z = 26)

However, other words may start with 'c' causing collision or special characters.

#### Avoiding Collisions

We could se all digits by multiplying each by a power of 27.
* a = 1, b = 2, ... , z = 26
* The index of "cat" is (3 x 27^2)  + (1 x 27^1) + (20 x 27^0) = 2234.
* The index of "bee" is (2 x 27^2)  + (5 x 27^1) + (5 x 27^0) = 1598.


```java 

public static int letterSum(String str, int i) { 
    int ithChar = str.charAt(i); 
    if(ithChar > 'z' || ithChar < 'a') {   
        throw new IllegalArgumentException("Invalid character");
    }
    return (ithChar - 'a') + 1;
}

public static int englishToInt(String str) {
    int sum = 0;
    for(int i = 0; i < str.length(); i++) {
        sum = sum * 27; 
        sum = sum + letterSum(str, i);
    }
    return sum;
}
```
As long as we pick a base >= 26, this algorithm is guaranteed to give each lowercase English word a unqiue number. Thus, we will never have a collision.

```java
// englishToInt() is a helper method which could turn string to index.

public class DataIndexedEnglishWordSet {
    private boolean[] present;

    public DataIndexedEnglishWordSet() {
        present = new boolean[2000000000];
    }

    public void add(String s) {
        present[englishToInt(s)] = true;
    }

    public boolean contains(int i) {
        resent present[englishToInt(s)];
    }
}
```

### Inserting Strings

We need to change our base to 126 if we want to insert strings other than English word.

The most basic character set used by computers is ASCII format.
* Each possible charaacter is assigned a value between 0 and 127.
* Characters 33 - 126 are "printable".
* For example, `char c = 'D'` is equivalent to `char c = 68`.

```java
public static int asciiToInt(String s) {
    int intRep = 0;
    for (int i = 0; i < s.length(); i += 1) {           
        intRep = intRep * 126;
        intRep = intRep + s.charAt(i);
    }
    return intRep;
}
```

However, if we want to represent character sets for other languages like Chinese, we could use unicode. 
For example, `char c = "鳌"` is equivalent to `char c = 40140`. 
The largest possible value for Chinese characters is 40959, so we need to use this as our base, but the index might be really large.

#### Integer Overflow

Because Java has a maximum integer (2,147,483,647), we will easily run into overflow for short strings and create collisions.

For example, `asciiToInt(omens)` will return `-1,867,853,901` instead of `28,196,917,171`.

From the smallest to the largest possible integers, there are a total of 4,294,967,296 integers in Java, which means that collision is inevitable, and we need to find a way to avoid it.

Pigeonhole princeple tells us that if there are more than 4,294,967,296 possible items, multiple items will share the same hash code. (The official term of the number we are computing.)

* Resolve hash code collisions. (collision handling)
* Compute a hash code for arbitrary objects. (computing a hash code)

### Collision Handling

We know that a few items will share a same hash code due to integer overflow. 
However, we could store that bucket of these items into the position in the array instead of store `true` or `false`. 
We could implement the bucket with LinkedList, BST, and other data structures.

Each bucket in our array is initially empty. Bucket h is a separate chain of all items that have a hash code h. When an item x gets added at h: 

* If bucket h is empty, create a new list containing x and store it at h.
* If bucket h is already a list, add x to the list at h if it's not already present.

Worst case runtime will be proportional to length of longest list (Q).

* `contains(x)`: Theta(Q);
* `add(x)`: Theta(Q); 

### Saving Memory

We can use modulus of hash code to reduce bucket count, but lists will be longer. 
If the hash code of "abomamora" is 111239443 and we have only 10 buckets, we will put that string into bucket 3, since 111239443 % 10 = 3.

What we've just created is called a hash table.

* Data is converted by a hash function into an integer representation called a hash code.
* The hash code is then reduced to a valid index, usually using the modulus operator.
* data -> hash function -> hash code -> reduce -> index

### Dynamic Growth

We could dynamicly adjust the number of buckets to make the data structure more efficient. 
Suppose we have M buckets and N items, the load factor is `N/M`, and we need to keep that factor low. 
When the load factor reaches certain threshold, we double M:

* Create a new HashTable with 2M buckets.
* Iterate through all the items in the old HashTable, and add them into this new HashTable.
* Since the modulus changes, items may belong to different buckets.

If all elements are evenly distributed, the runtime will be `Theta(N/M)`. Because `N/M` is lower than the threshold, `Theta(N/M)` is equal to `Theta(1)`.

Item will be evenly distributed if we have a hash code algorithm that gives fairly random values for different items.

### Hash Code Algorithm

* We could use the base strategy as we developed before.
* We could use a small prime base number, such as 31, since we don't need to avoid collisions.
* Base 126 means that any string that ends in the same last 32 characters has the same hashcode.


# File: Week 08/Heaps and PQs.md

# Heaps and PQs

## Priority Queue Interface

The abstract data type "priority queue" could be used to find the smallest or largest element quickly instead of searching through the whole tree. There can be memory benefits to using this data structure.

```java
/** (Min) Priority Queue: Allowing tracking and removal of 
  * the smallest item in a priority queue. */
public interface MinPQ<Item> {
    /** Adds the item to the priority queue. */
    public void add(Item x);
    /** Returns the smallest item in the priority queue. */
    public Item getSmallest();
    /** Removes the smallest item from the priority queue. */
    public Item removeSmallest();
    /** Returns the size of the priority queue. */
    public int size();
}
```

## Tree Representation

There are many approaches to represent trees.

### Approach 1a, 1b, and 1c

Create mappings between nodes and their children.

```java
public class Tree1A<Key> {
  Key k;
  Tree1A left;
  Tree1A middle;
  Tree1A right;
  ...
}
```

![Tree1A](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-09%20at%209.54.04%20PM.png "Tree1A")

```java
public class Tree1B<Key> {
  Key k;
  Tree1B[] children;
  ...
}
```

![Tree1B](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-09%20at%2010.03.15%20PM.png "Tree1B")

```java
public class Tree1C<Key> {
  Key k;
  Tree1C favoredChild;
  Tree1C sibling;
  ...
}
```

![Tree1C](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-09%20at%2010.08.44%20PM.png "Tree1C")

### Approach 2

Store the keys array as well as a parents array.

```java
public class Tree2<Key> {
  Key[] keys;
  int[] parents;
  ...
}
```


### Approach 3

Assume that our tree is complete.

```java
public class Tree3<Key> {
  Key[] keys;
  ...
}
```

### The Three Approaches

![ADTs implementations](../image/PQtry.png)

## Implementation

### Heap Structure

Binary min-heap has the following properties:

* Min-heap: Every node is less than or equal to both of its children.
* Complete: Missing items only at the bottom level (if any), all nodes are as far left as possible.

![Heap](https://joshhug.gitbooks.io/hug61b/content/assets/heap-13.2.1.png "Heap")

The `Tree3` could be used to implement the heap structure. Leave one empty spot at the beginning to simplify computation. 


### Priority Invariants 

![PQ specs](../image/PQin.png)

### Ones indexing 
* `leftChild(k)` = k * 2
* `rightChild(k)` = k * 2 + 1
* `parent(k)` = k / 2

### Heap Operations

* `add`: Add to the end of heap temporarily. Swim up the hierarchy to the proper place. (Swimming involves swapping nodes if child < parent)
* `getSmallest`: Return the root of the heap.
* `removeSmallest`: Swap the last item in the heap into the root. Sink down the hierarchy to the proper place.

Here's the code of swim operation:

```java
public void swim(int k) {
    if (keys[parent(k)] ≻ keys[k]) {
       swap(k, parent(k));
       swim(parent(k));              
    }
}
```

### Runtime

Ordered Array

* `add`: ` Θ(N)`
* `getSmallest`: ` Θ(1)`
* `removeSmallest`: ` Θ(N)`

Bushy BST

* `add`: ` Θ(log N)`
* `getSmallest`: ` Θ(log N)`
* `removeSmallest`: ` Θ(log N)`

HashTable

* `add`: ` Θ(1)`
* `getSmallest`: ` Θ(N)`
* `removeSmallest`: ` Θ(N)`

Heap Structure

* `add`: ` Θ(log N)`
* `getSmallest`: ` Θ(1)`
* `removeSmallest`: ` Θ(log N)`

![ADTs implementations](../image/ADT.png)

## The Search Problem 
The problem we are presented: Given a stream of data, retrieve information of interest. 

All of the data structures we have discussed so far have been to solve the search problem 

Abstraction often happens in layers. Abstract Data Types can often contain two abstract ideas boiling down to one implementation. 

![ADTs implementations](../image/ADTs.png)


# File: Week 08/Prefix Operations and Tries.md

# Prefix Operations and Tries

## Introduction

Tries is an ordered tree data structure used to store keys which can be broken into "characters" and share prefixes with other keys.

* Every node stores only one letter.
* Nodes can be shared by multiple keys.
* The last character of the key is marked.

To check if the trie contains a key, walk down the tree from the root along the correct nodes. If the character does not exist or the final node is not marked, the key does not exist.

For example, the following trie contains `["sam", "sad", "sap", "same", "a", "awls"]`.

![Tries](../image/trie1.png)

* `contains("a)`: true, the final node is marked
* `contains("sa")`: false, the final node is not marked
* `contains("saq")`: false, fell off tree

## Implementation

### DataIndexedCharMap

The `DataIndexedCharMap` class is efficient implementation for a map which takes in character keys. The value `R` represents the number of possible characters (128 for ASCII).

```java
public class DataIndexedCharMap<V> {
    private V[] items;
    public DataIndexedCharMap(int R) {
        items = (V[]) new Object[R];
    }
    public void put(char c, V val) {
        items[c] = val;
    }
    public V get(char c) {
        return items[c];
    }
}
```

When building a trie, the `DataIndexedCharMap` class provides a map to all of a nodes' children. However, the `DataIndexedCharMap` object of a node with relatively few children will have mostly null links, which is wasting excess memories.

Since each link corresponds to a character if and only if that character exists, the character variable of each node can be removed. The value of the character is its position in the parent.

```java
public class TrieSet {
   private static final int R = 128;
   private Node root;

   private static class Node {
      // private char ch;
      private boolean isKey;
      private DataIndexedCharMap next;

      //private Node(char c, boolean blue, int R) {
      //  ch = c; 
      private Node(boolean blue, int R) {
          isKey = blue;
          next = new DataIndexedCharMap<Node>(R);
      }
   }
}
```

### Child Tracking

The problem of the implementation above is that it wastes excess memories with lots of null spots that don't contain any children. However, alternative ways to track children is available.
* Hash-Table based Trie: Initialize the default value and resize the array only when necessary with the load factor.
* BST based Trie: Create children pointers when necessary and store them in a BST. Runtime is not efficient.


DataIndexedCharMap
* Space: 128 links per node
* Runtime: `Θ(1)`

BST
* Space: C links per node
* Runtime: `O(log R)`

Hash Table
* Space: C links per node
* Runtime: `O(R)`

(C is the number of children. R is the size of the alphabet.)

## Performance

If a Trie has N keys, the runtime for our Map/Set operations are as follows:

* add: `Θ(1)`
* contains: `Θ(1)`

The runtime is independent from the number of the keys, since the operations only traverse the length of one key in the worst case.

## Trie String Operations

### Prefix Matching

Tries could efficiently support specific string operations like prefix matching.

The following pseudocode of `collect` operation could return all keys in a trie.

```
collect():
    Create an empty list of results x
    For character c in root.next.keys():
        Call colHelp(c, x, root.next.get(c))
    Return x

colHelp(String s, List<String> x, Node n):
    if n.isKey:
        x.add(s)
    For character c in n.next.keys():
        Call colHelp(s + c, x, n.next.get(c))
```

The following pseudocode of `keyWithPrefix` operation could return all keys with specific prefix. (The `collect` operation begins from the node at the end of the prefix.)

```
keysWithPrefix(String s):
    Find the end of the prefix, alpha
    Create an empty list x
    For character in alpha.next.keys():
        Call colHelp("sa" + c, x, alpha.next.get(c))
    Return x
```

### Autocomplete

The search suggestion of Google is a kind of autocomplete, which could be implemented with a trie.

* Values of nodes will represent the weight of the string.
* Trie could store billions of strings efficiently since they share nodes.
* When a user types a query, `keysWithPrefix` operation will return several strings with the highest value.



# File: Week 08/Range Searching and Multi-Dimensional Data.md

# Range Searching and Multi-Dimensional Data

## Introduction

Multi-Dimensional data strucutres could perform operations on a set of Body objects in space.

* 2D Range Finding: Count the objects in a specific region.
* Nearest Neighbors: Find the closest object to another object.

## HashTable

If the objects are stored in a HashTable, the problems above could be solved inefficiently.

* Runtime: `Θ(N)`, since the bucket that each object resides in is effectively random.

## Uniform Partitioning

The HashTable could be enhanced if each object is stored in a specific bucket based on its position. The space could be divided with a grid, and each portion is corresponded to a bucket in the HashTable, which is called "spatial hashing".

Instead of using the `hashCode()` method, each object provide a `getX()` and `getY()` method so that it can compute its own bucket number.

This method could reduce the amount of buckets to be searched, so that it is faster than without spatial partitioning on average.

* Runtime: `Θ(N)`, since the amount of grids is a constant.

## X-Based Tree or Y-Based Tree

Search Trees could explicitly track the order of items. To build a tree of objects in a two-dimensional space, the items should be comparable.

* X-Based Tree: Compare objects only based on their x-coordinate.
* Y-Based Tree: Compare objects only based on their y-coordinate.

Here's an example of X-Based Tree: 
![X-Based Tree](../image/k-d1.png)

If a range finding operation is performed in the green rectangle, the right child of the root node is ignored. Skipping searching through parts of the search tree is called "pruning".

* Search in the optimized dimension: `log N`, due to the pruning operation.
* Search in the non-optimized dimension: `N`, which could be optimized with the QuadTree.

## QuadTree

The QuadTree is a tree data structure which could partition a two-dimensional space by recursively subdividing it into four quadrants.

![QuadTree](../image/k-d2.png) 

![QuadTree](../image/k-d3.png)) 


* Each node has exactly four children: The northwest, northeast, southeast, and southwest region.
* The node B is inserted as the NE child of node A, since B resides in the northeast quadrant of A.

![QuadTree](../image/k-d4.png)

If a range finding operation is performed in the green rectangle, from any node the quadrants which the green rectangle does not lie within could be ignored and pruned away.

Quad-Trees are effective in 2-D spaces, but K-D trees could perform the operations in higher dimension spaces.

## K-D Trees

The K-D Tree could extend the hierarchical partitioning idea to multi-dimensions, which works by rotating through all the dimensions by each level.

For the 2-D case, it partitions like an X-based Tree on the first level, then like a Y-based Tree on the next, then as an X-based Tree on third level, etc. 

* Each node has two children, since each level is partitioned into "greater" and "less than".
* Items equal in one dimension should always be stored in the "greater" part of its parent node.

![K-D Tree](../image/k-d5.png)



### Nearest Neighbor 

![K-D Tree](../image/k-d6.png)

* Start at the root and compute its distance to the query point, and save that as the minimum distance.
* For each subspace the node partitioned into, try to find a better point by computing the shortest distance between the query point and the edge of the subspace.
* Compute recursively for each subspace which might contains a possibly better point.
* Return the point with the minimum distance.


# File: Week 09/Graph Traversals and Implementations.md

# Graph Traversals and Implementations

## BFS (Breadth First Search)

This algorithm of exploring a vertex's immediate children before moving on to its grandchildren is known as Breadth First Traversal.

* Initialize the a queue with the starting vertex and mark that vertex.
* Repeat until the queue is empty:
* * Remove vertex v from the front of the queue.
* * For each unmarked neighbor n of v:
* * * Mark n.
* * * Add n to the end of the queue.
* * * Set edgeTo[n] = v.
* * * Set distTo[n] = distTo[v] + 1.

## Graph API

To Implement our graph algorithms like BreadthFirstPaths and DepthFirstPaths:
* An API for Graphs.
* An underlying data structure to represent the graphs.

The API uses integers to represent vertices. `Map<Label, Integer>` is required to lookup vertices by labels.

```java
public class Graph {
  public Graph(int V):               // Create empty graph with v vertices
  public void addEdge(int v, int w): // add an edge v-w
  Iterable<Integer> adj(int v):      // vertices adjacent to v
  int V():                           // number of vertices
  int E():                           // number of edges
...
```

* Number of vertices must be specified in advance.
* Does not support weights on nodes or edges.
* Has no method for getting the degree for a vertex.

### degree(Graph G, int v)

```java
public static int degree(Graph G, int v) {
    int degree = 0;
    for (int w: G.adj(v)) {
        degree += 1;
    }
    return degree;
}
```

### print(Graph G)

```java
public static void print(Graph G) {
	for (int v = 0; v < G.V(); v += 1) {
 	    for (int w : G.adj(v)) {
    	    System.out.println(v + “-” + w);
    	}
    }
}
```

## Graph Representations

The choice of data structure to represent the graph should have implications on both runtime and memory usage.

![Graph Representations](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-27%20at%202.03.44%20AM.png)

### Adjacency Matrix

Use a 2D array. There is an edge connecting vertex `s` to `t` if that corresponding cell is 1, which represents `true`.

![Adjacency Matrix](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-27%20at%201.58.11%20AM.png)

### Edge Sets

Use a single set to create a collection of all edges.

![Edge Sets](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-27%20at%202.03.36%20AM.png)

### Adjacency Lists

Use an array of lists, indexed by vertex number. Each index contains the index of its adjancent vertices.

![Adjacency Lists](https://joshhug.gitbooks.io/hug61b/content/assets/Screen%20Shot%202019-03-27%20at%202.05.55%20AM.png)

#### Implementation

```java
public class Graph {
	private final int V;
    private List<Integer>[] adj;
	
	public Graph(int V) {
        this.V = V;
        adj = (List<Integer>[]) new ArrayList[V];
        for (int v = 0; v < V; v++) {
            adj[v] = new ArrayList<Integer>();
        }
	} 

	public void addEdge(int v, int w) {
        adj[v].add(w);
        adj[w].add(v);
	}

	public Iterable<Integer> adj(int v) {
        return adj[v];
	}
}
```

### Runtime

| Representation | addEdge(s, t)| for(w : adj(v)) | print() | hasEdge(s, t) | space used |
| --- |---|----| --- | --- | --- |
| Adjacency Matrix | `Θ(1)` | `Θ(V)` | `Θ(V^2)` | `Θ(1)` | `Θ(V^2)` |
| Edge Sets | `Θ(1)` | `Θ(E)` | `Θ(E)` | `Θ(E)` | `Θ(E)` |
| Adjacency Lists | `Θ(1)` | `Θ(1) to Θ(V)` | `Θ(V+E)` | `Θ(degree(v))` | `Θ(V+E)` |

## Implementation

Create a Paths API, which takes a Graph object and query the graph-processing for information.

```java
public class Paths {
    public Paths(Graph G, int s)    // Find all paths from G
    boolean hasPathTo(int v)        // is there a path from s to v?
    Iterable<Integer> pathTo(int v) // path from s to v (if any)
}
```

### Depth First Search

```java
public class DepthFirstPaths {
    private boolean[] marked;
    private int[] edgeTo;
    private int s;

    public DepthFirstPaths(Graph G, int s) {
        ...             // Data structure initialization
        dfs(G, s);      // Recursively find vertices connected to s
    }

    private void dfs(Graph G, int v) {
        marked[v] = true;
        for (int w: G.adj(v)) {
            if (!marked[w]) {
                edgeTo[w] = v;
                dfs(G, w);
            }
        }
    }

    public Iterable < Integer > pathTo(int v) {
        if (!hasPathTo(v)) return null;
        List <Integer> path = new ArrayList < > ();
        for (int x = v; x != s; x = edgeTo[x]) {
            path.add(x);
        }
        path.add(s);
        Collections.reverse(path);
        return path;
    }

    public boolean hasPathTo(int v) {
        return marked[v];
    }
}
```

#### Runtime

* The DepthFirstPaths constructor: `O(V+E)`.
* * Each vertex is visited at most once (`O(V)`).
* * Each edge is considered at most twice (`O(E)`).

### Breadth First Search

```java
public class BreadthFirstPaths {
    private boolean[] marked;
    private int[] edgeTo;
    ...

    private void bfs(Graph G, int s) {
        Queue <Integer> fringe = new Queue <Integer> ();
        fringe.enqueue(s);
        marked[s] = true;
        while (!fringe.isEmpty()) {
            int v = fringe.dequeue();
            for (int w: G.adj(v)) {
                if (!marked[w]) {
                    fringe.enqueue(w);
                    marked[w] = true;
                    edgeTo[w] = v;
                }
            }
        }
    }
}
```

## Graph Problems

### Adjacency List Based Graphs

| Problem                | Description                                        | Solution             | Efficiency (adj. list)            |
|------------------------|----------------------------------------------------|----------------------|-----------------------------------|
| s-t paths              | Find a path from s to every reachable vertex.     | DepthFirstPaths      | `O(V+E)` time, `Θ(V)` space.       |
| s-t shortest paths     | Find a shortest path from s to every reachable vertex.| BreadthFirstPaths  | `O(V+E)` time, `Θ(V)` space.       |


### Adjacency Matrix Based Graphs

| Problem                | Description                                        | Solution             | Efficiency (adj. list)            |
|------------------------|----------------------------------------------------|----------------------|-----------------------------------|
| s-t paths              | Find a path from s to every reachable vertex.     | DepthFirstPaths      | `O(V^2)` time, `Θ(V)` space.       |
| s-t shortest paths     | Find a shortest path from s to every reachable vertex.| BreadthFirstPaths  | `O(V^2)` time, `Θ(V)` space.       |



# File: Week 09/Shortest Paths.md

# Shortest Paths

## BFS v.s. DFS for Path Finding

* BFS returns path with shortest number of edges.
* Time efficiency is similar for both algorithms.
* Space Efficiency is quite different.
* * DFS is worse for spindly graphs. (Call stack gets very deep.)
* * BFS is worse for bushy graphs. (Queue gets very large.)
* Track the `distTo` and `edgeTo` arrays requires `Θ(V)` memory.

## Shortest Path Tree

* For every vertex v (which is not s) in the graph, find the shortest path from s to v.
* Combine all the edges that the algorithm found above.

## Dijkstra's Algorithm

Instead of BFS, an algorithm which takes into account edge distances (edge weights) is created.

* Create a priority queue.
* Add s to the priority queue with priority 0. 
* Add all other vertices to the priority queue with priority infinity.
* While the priority queue is not empty, and relax all of the edges going out from the vertex.

As long as the edges are all non-negative, Dijkstra's is guaranteed to be optimal.

### Relax

* For the popped vertex v, iterate through its edges.
* For the edge (v,w), compare the `currentBestDistToV + weight(v,w)` and `currentBestDistToW`.
* If the former is smaller, set the `currentBestDistToW = currentBestDistToV + weight(v,w)`, and set `edgeTo[w] = v`.
* Never relax edges that point to already visited vertices.

### Code

```python
def dijkstras(source):
    PQ.add(source, 0)
    For all other vertices, v, PQ.add(v, infinity)
    while PQ is not empty:
        p = PQ.removeSmallest()
        relax(all edges from p)

def relax(edge p,q):
   if q is visited (or q is not in PQ):
       return

   if distTo[p] + weight(edge) < distTo[q]:
       distTo[q] = distTo[p] + w
       edgeTo[q] = p
       PQ.changePriority(q, distTo[q])
```

### Proof

* At start, `distTo[source] = 0`.
* After relaxing all edges from source, assume v1 is the closet vertex to the source.
* Suppose there's another path `(s,va,vb,...,v1)`, which is shorter than `(s,v1)`.
* Since `(s,va)` is longer than `(s,v1)`, that path doesn't exist.
* Thus, v1 is the closet vertex to the source, and that argument is still valid for all the edges of v1.

### Runtime

Dijkstra's Algorithm is based on the PQ.

Overall runtime: `O(V log(V) + V log(V) + E log(V))`

Connected graph: `O(E log V)` (E > V)

* `add`: V, each costing `O(log V)` time.
* `removeSmallest`: V, each costing `O(log V)` time.
* `changePriority`: E, each costing `O(log V)` time.


# File: Week 09/Tree and Graph Traversals.md

# Tree and Graph Traversals

## Tree Definition (Review)

Tree
* A set of nodes (or vertices).
* A set of edges that connect those nodes.
* There is exactly one path between any two nodes.

Rooted tree
* The tree has a designated root.
* Every node except the root has exactly one parent.
* A node can have 0 or more children.

## Trees Traversals

Tree traversal refers to the process of visiting each node in a tree data structure, exactly once. Traversals are classified by the order in which the nodes are visited.

![Example Tree](../image/tree1.png)

### Level Order Traversal

Iterate the tree by levels, from left to right.
* First level: D
* Second level: B F
* Third leve: A C E G

```
D B F A C E G
```

### Depth-First traversal

#### Pre-order Traversal

* Visit root node.
* Visit all the nodes in the left subtree.
* Visit all the nodes in the right subtree.

```java
preOrder(BSTNode x) {
    if (x == null) return;
    print(x.key)
    preOrder(x.left)
    preOrder(x.right)
}
```

```
D B A C F E G
```

#### In-order Traversal

* Visit all the nodes in the left subtree.
* Visit the root node.
* Visit all the nodes in the right subtree.

```java
inOrder(BSTNode x) {
    if (x == null) return;    
    inOrder(x.left)
    print(x.key)
    inOrder(x.right)
}
```

```
A B C D E F G
```

#### Post-order Traversal

* Visit all the nodes in the left subtree.
* Visit all the nodes in the right subtree.
* Visit the root node.

```java
postOrder(BSTNode x) {
    if (x == null) return;    
    postOrder(x.left)
    postOrder(x.right)
    print(x.key)   
}
```

```
A C B E G F D
```

## Graphs Definition

### Graph
* A set of nodes (or vertices).
* A set of zero of more edges, each of which connects two nodes.
* All trees are also graphs, but not all graphs are trees.

### Simple Graphs and Multigraphs
* Only one edge between each of two nodes.
* No edge from a node to itself.
* The course will only focus on simple graphs.

### Undirected and Directed Graphs
* Undirected graphs contain bidirectional edges.
* Directed graphs contain directed edges.

### Acyclic and Cyclic Graphs
* Cyclic graphs contain at least one graph cycle.
* Acyclic graphs do not have graph cycles.

### More Definitions
* Verticies with an edge between are adjacent.
* Vertices or edges may have lables or weights.
* A sequence of vertices connectted by edges is a path.
* A path of which first and last verticies are the same is a cycle.
* Two vertices are connected when there is a path between them.
* If all vertices are connected, the graph is connected. 

![Graph properties](../image/graph1.png)

## Graph Problems

### s-t Path

Find whether there's a path between vertices s and t.

To solve this problem, an algorithm similar to the tree traversal is created. 

```java
public boolean connected(s, t)

mark s

if (s == t):
    return true;

for child in unmarked_neighbors(s):
    if isconnected(child, t):
        return true;

return false;
```

The algorithm marks the verticies it has already visited to avoid infinite loop.

### DFS (Depth First Search)

This algorithm of exploring a neighbor’s entire subgraph before moving on to the next neighbor is known as Depth First Traversal.

This kind of traversal could help us to find a path from s to every other reachable vertex, visiting each vertex at most once.

To implement this traversal algorithm, two arrays, `marked` and `edgeTo`, are required to record whether an vertex is marked and the parent of the vertex. 

* Mark v.
* For each unmarked adjancent vertex w:
* * Set `edgeTo[w] = v`.
* * Run these steps for w.

### Graph Traversals

* DFS Preorder: Action is before DFS calls to neighbors. (The algorithm created above.)
* DFS Postorder: Action is after DFS calls to neighbors.
* BFS (Breadth First Search) Order: Act in order of distance from s. (Analogous to "level order.") 

![Graph/Tree Traversal](../image/graph2.png)


# File: Week 10/Minimum Spanning Trees.md

# Minimum Spanning Trees

## Definition

Given an undirected graph, a spanning tree T is a subgraph of G, which has the following properties. And it is called "spanning" since all vertices are included.
* Is connected.
* Is acyclic.
* Includes all of the vertices.


![MST](../image/MST1.png)

A minimum spanning tree is a spanning tree of minimum total weight.
A shortest paths tree depends on the start vertex, while there is no source for a minimum spanning tree. 



## Cut Property

* A cut is an assignment of a graph’s nodes to two non-empty sets.
* A crossing edge is an edge which connects a node from one set to a node from the other set.
* Given any cut, minimum weight crossing edge is in the MST.

![Cut Property](../image/MST2.png)

## Generic MST Finding Algorithm

* Start with no edges in the MST.
* Find a cut that has no crossing edges in the MST. 
* Add smallest crossing edge to the MST.
* Repeat until V-1 edges.

## Prim’s Algorithm

* Start from some arbitrary start node.
* Repeatedly add shortest edge that has one node inside the MST under construction.
* Repeat until V-1 edges. 
## Before
![Prim's Algorithm](../image/MST4.png) 

## After 
![Prim's Algorithm](../image/MST5.png)

### Implementation

Prim’s and Dijkstra’s algorithms are similar, however:
* Dijkstra's considers "distance from the source".
* Prim's considers "distance from the tree."

```java
public class PrimMST {
    public PrimMST(EdgeWeightedGraph G) {
        edgeTo = new Edge[G.V()];
        distTo = new double[G.V()];
        marked = new boolean[G.V()];
        fringe = new SpecialPQ < Double > (G.V());

        distTo[s] = 0.0;
        setDistancesToInfinityExceptS(s);
        insertAllVertices(fringe);

        /* Get vertices in order of distance from tree. */
        while (!fringe.isEmpty()) {
            int v = fringe.delMin();
            scan(G, v);
        }
    }

    private void scan(EdgeWeightedGraph G, int v) {
        marked[v] = true;
        for (Edge e: G.adj(v)) {
            int w = e.other(v);
            if (marked[w]) {
                continue;
            }
            if (e.weight() < distTo[w]) {
                distTo[w] = e.weight();
                edgeTo[w] = e;
                pq.decreasePriority(w, distTo[w]);
            }
        }
    }
}
```

### Runtime

Priority Queue operation count, assuming binary heap based PQ.
* Insertion: V, each costing `O(log V)` time.
* Delete-min: V, each costing `O(log V)` time.
* Decrease priority: `O(E)`, each costing `O(log V)` time.

Overall runtime: `O(V * log(V) + V * log(V) + E * logV)`.
Assuming E > V, this is just `O(E log V)`.  

![Analysis Summary](../image/MST8.png)



## Kruskal’s Algorithm

* Consider edges in increasing order of weight.
* Add edge to MST unless doing so creates a cycle.
* Repeat until V-1 edges. 


### Before 
![Kruskal's Algorithm](../image/MST6.png) 

### After 
![Kruskal's Algorithm](../image/MST7.png)

### Implementation

Adding an edge to MST means that add the edge to the ArrayList and connect the two vertices in the `WeightedQuickUnion`. If the two vertices are already connected, the edge should be ingored since adding it will create a cycle.

```java
public class KruskalMST {
    private List < Edge > mst = new ArrayList < Edge > ();

    public KruskalMST(EdgeWeightedGraph G) {
        MinPQ < Edge > pq = new MinPQ < Edge > ();
        for (Edge e: G.edges()) {
            pq.insert(e);
        }
        WeightedQuickUnionPC uf =
            new WeightedQuickUnionPC(G.V());
        while (!pq.isEmpty() && mst.size() < G.V() - 1) {
            Edge e = pq.delMin();
            int v = e.from();
            int w = e.to();
            if (!uf.connected(v, w)) {
                uf.union(v, w);
                mst.add(e);
            }
        }
    }
}
```

### Runtime

* Insertion: E, each costing `O(log E)` time.
* Delete-min: `O(E)`, each costing `O(log E)` time.
* Union: `O(V)`, each costing `O(log V)` time.
* IsConnected: `O(E)`, each costing `O(log V)` time.
Overall runtime: `O(E + V log * V + E log * V)`.
Assuming E > V, this is just `O(E log V)`. 

![Kruskal Analysis](../image/MST9.png) 


### Running Time Analysis Comparison 

![Summary](../image/MST10.png)


# File: Week 10/Reductions and Decomposition.md

# Reductions and Decomposition

## Topological Sort

* Topological Sort is an ordering of a graph's vertices such that for every directed edge u→v, u comes beforev in the ordering.
* Topological sorts only apply to directed, acyclic (no cycles) graphs (DAG).
* Topological sort is called a linearization of the graph, since all the vertices in the graph could be redrawn in one line.

### Topological Sort Algorithm

* Perform a DFS traversal from every vertex in the graph, not clearing markings in between traversals.
* Record DFS postorder along the way.
* Topological ordering is the reverse of the postorder.

The total runtime of DFS is `O(V + E)`.

```
topological(DAG):
    initialize marked array
    initialize postOrder list
    for all vertices in DAG:
        if vertex is not marked:
            dfs(vertex, marked, postOrder)
    return postOrder reversed

dfs(vertex, marked, postOrder):
    marked[vertex] = true
    for neighbor of vertex:
        dfs(neighbor, marked, postOrder)
    postOrder.add(vertex)
```

## Shortest Path Algorithm for DAGs.
* Visit vertices in topological order.
* On each visit, relax all outgoing edges.



# File: Week 11/Basic Sorts.md

# Basic Sorts

## Definition of Sorting

An ordering relation < for keys a, b, and c has the following properties:

*   Law of Trichotomy: Exactly one of a < b, a = b, b < a is true.
    
*   Law of Transitivity: If a < b, and b < c, then a < c.

A sort is a permutation (re-arrangement) of a sequence of elements that puts the keys into non-decreasing order relative to a given ordering relation.
In Java, ordering relations are typically given in the form of `compareTo` or `compare` methods.

```java
import java.util.Comparator;

public class LengthComparator implements Comparator<String> {
	public int compare(String x, String b) {
		return x.length() - b.length();
	}
}
```

## Performance of Algorithms

The **Time Complexity** of an algorithm is the characterization of its runtime efficiency. Dijkstra’s has time complexity `O(E log V)`.

The **Space Complexity** of an algorithm is the characterization of its **extra** memory usage. Dijkstra’s has space complexity `Θ(V)`.

## Selection Sort

### Steps of Selection Sort
* Find the smallest item.
* Swap the item to the front and fix it.
* Repeat for unfixed items until all items are fixed.

### Runtime
`Θ(N^2)` time if an array is used to store the items.

## Heap Sort

### Naive

Maintain a max-oriented heap to get the maximum item fast.
* Insert all items into a max heap, and discard input array. Create output array.
* Delete largest item from the max heap.
* Put largest item at the end of the unused part of the output array.

#### Runtime

*  Getting items into the heap `O(N log N)` time.
*  Selecting largest item: `Θ(1)` time.
*  Removing largest item: `O(log N)` for each removal.
The overall runtime is `O(N log N)`.

### In-Place

Rather than inserting into a new array of length N + 1, use a process known as “bottom-up heapification” to convert the array into a heap. This approach could avoid the need for extra copy of all data.

#### Runtime

- The Time Complexity of In-Place Heap Sort is the same as the Naive Heap Sort.
- The Space Complexity of In-Place Heap Sort is `Θ(1)`.

## Merge Sort

-   Split items into 2 roughly even pieces.
-   Mergesort each half recursively.
-   Merge the two sorted halves to form the final result.

#### Runtime

- The Time Complexity of Merge Sort is `Θ(N log N)`.
- The Space Complexity of Merge Sort is `Θ(N)`.

## Insertion Sort

-   Starting with an empty output sequence.
-   Add each item from input, inserting into output at right point.
-   Perform these steps in place by swapping with previous items.

Example:
```
P O T A T O (0 swap)
O P T A T O (1 swap)
O P T A T O (0 swap)
A O P T T O (3 swaps)
A O P T T O (0 swap)
A O O P T T (3 swaps)
```

On arrays with a small number of inversions, insertion sort is extremely fast.
For small arrays (N < 15 or so), insertion sort is fastest.

#### Runtime

- The best case for insertion sort is `Θ(N)`.
- The worst case of insertion sort is `Θ(N ^ 2)`.


# File: Week 11/Quick Sort.md

# Quick Sort

## Partitoning

To partition an array a[] on element x = a[i] is to rearrange a[] so that:
- x moves to position j. (probably the same as i)
- All entries to the left of x are <= x.
- All entries to the right of x are >= x.

## Partiton Sort (Quick Sort)

Quicksorting N items:
-   Partition on leftmost item.
-   Quicksort left half.
-   Quicksort right half.
For most common situations, Quick Sort is empirically the fastest sort.

### Runtime

Best Case: Pivot Always Lands in the Middle.

Runtime for the best case: `Θ(N log N)`.

Worst Case: Pivot Always Lands at Beginning of Array.

Runtime for the worst case: `Θ(N ^ 2)`.

The average runtime is `Θ(N log N)`.

### Performance

The performance of Quick Sort depend critically on:
-   How the pivot is selected.
-   How the partition is performed around that pivot.
-   Other optimizations.

The rare worst case will happen when:
-   Bad ordering: Array already in sorted order or almost sorted order.
-   Bad elements: Array with all duplicates.

Methods to avoid the worst case:
-   Always use the median as the pivot.
-   Randomly swap two indices occasionally.
-   Shuffle before sorting.
-   Partition from the center of the array.


# File: Week 12/More Quick Sort, Sorting Summary.md

# More Quick Sort, Sorting Summary

## Worst Case of Quicksort

There are four philosophies to avoid the worst case of Quicksort.

### Randomness

The worst cases do happen in practice:
* Bad ordering: Array already in sorted order.
* Bad elements: Array with all duplicates. 

Dealing with bad ordering:
-   Strategy #1: Pick pivots randomly.
-   Strategy #2: Shuffle before sorting.

### Smarter Pivot Selection

#### Constant Time

For any pivot selection procedure that is deterministic and costs constant time, there is a family of dangerous inputs that an adversary could easily generate.

#### Linear Time

Exact median Quicksort is safe: Worst case `Θ(N log N)`, but it is slower than Mergesort.

The method to find the exact median is called the Quick Select.

### Introspection

If Quicksort exceeds some critical value (such as 10 ln N), switch to Mergesort.

### Preprocess

Could analyze array to see if Quicksort will be slow. No obvious way to do this, though.

## Quicksort v.s. Mergesort

### Quicksort Flavors

Using this scheme (L3S) yields a Quicksort algorithm that is slower than the Mergesort.

* Pivot selection: Always use leftmost.
* Partition algorithm: Make an array copy then do three scans for red, white, and blue items (white scan trivially finishes in one compare).
* Shuffle before starting to avoid worst case.


### Tony Hoare’s In-place Partitioning Scheme

Using this partitioning scheme (LTHS) yields a very fast Quicksort.

* Left pointer loves small items.
* Right pointer loves large items.
* Big idea: Walk towards each other, swapping anything they don’t like.
* End result is that things on left are “small” and things on the right are “large”.

More recent pivot/partitioning schemes do somewhat better. The best known Quicksort uses a two-pivot scheme.

## Quick Select

Computing the exact median would be great for picking an item to partition around, which could avoid the worst case of Quicksort. However, computing that exact median is inefficient.

However, partitioning can be used to find the exact median, which results in the median identification algorithm.

### Performance

On average, Quick Select will take `Θ(N)` time.

The worst case runtime is `Θ(N ^ 2)` when the array is in sorted order.

However, the Quicksort algorithm with Quick Select to find the exact median is quite slow.

## Stability, Adaptiveness, Optimization

### Stability

A sort is said to be stable if order of equivalent items is preserved. 

For example, if an array is sorted by name and then sorted by grades, the relative order of name is preserved if the sorting algorithm is stable.

* Stable: Mergesort and Insertion Sort
* Unstable: Heapsort and Quicksort

### Optimization

* Switch to insertion sort when a subproblem reaches size 15 or lower.
* Make sort adaptive.
* Exploit restrictions on set of keys.
* For Quicksort, make the algorithm introspective, switching to a different sorting method if recursion goes too deep.

### Arrays.sort

In Java, `Arrays.sort(someArray)` uses:

* Mergesort (specifically the TimSort variant) if `someArray` consists of Objects.
* Quicksort if `someArray` consists of primitives.


# File: Week 12/Sorting and Algorithmic Bounds.md

# Sorting and Algorithmic Bounds

## Theoretical Bounds on Sorting

Let the ultimate comparison sort (TUCS) be the asymptotically fastest possible comparison sorting algorithm, and let R(N) be its worst case runtime.
* Worst case run-time of TUCS, R(N) is `O(N log N)`. (Not worse than Mergesort.)
* Worst case run-time of TUCS, R(N) is `Ω(N)`. (Have to at least look at every item.)

A stronger statement of the lower bound is `Ω(N log N)`, which means that there's no sorting algorithm that could have a worst case runtime better than `Θ(N log N)`.

### The Game of Puppy, Cat, Dog

Suppose there is a puppy, a cat, and a dog, each in an opaque soundproof box labeled A, B, and C, and a scale is given to figure out which is which.

A decision tree for N = 3 could be generated with 3 levels at most. For N items, the decision tree will have at most `Ω(log(N!))` levels.

The puppy, cat, dog could reduce to sorting, since a solution to the sorting problem also provides a solution to puppy, cat, dog. Thus, any lower bound on difficulty of puppy, cat, dog must also apply to sorting, which means that it should takes `Ω(log(N!))` comparisons in the worst case.

Because `log(N!) ∈ Ω(N log N)`,  `log(N!)` grows at least as quickly as `N log N`.

Since TUCS is `Ω(lg N!)` and `lg N!` is `Ω(N log N)`, TUCS is `Ω(N log N)`.

Any comparison based sort requires at least order `N log N `comparisons in its worst case.

### Proof of log(N!) ∈ Ω(N log N)

*   `N! ≥ (N/2)N/2`.
*   Taking the log of both sides, `log(N!) ≥ log(N/2) ^ (N/2)`.
*   Bringing down the exponent, `log(N!) ≥ N/2 log(N/2)`.
*   Discarding unnecessary constants, `log(N!) ∈ Ω(N log N)`.


# File: Week 13/Radix Sorts.md

# Radix Sorts

## Counting Sort

Sorting requires `Ω(N log N)` compares in the worst case. 

However, there are different algorithms that could avoid comparisons.

### Sleep Sort

For each integer x in array A, start a new program that:
* Sleeps for x seconds.
* Prints x.

### Counting Sort

General idea:
*   Create a new array.
*   Copy item with key i into ith entry of new array.
The algorithm could sort N items in `Θ(N)` worst case time.

#### Complex Cases

Alphabet case: Keys belong to a finite ordered alphabet.
* Count number of occurrences of each item.
* Iterate through list, using count array to decide where to put everything.

#### Performance

For sorting an array of the 100 largest cities by population, Quicksort is better than Counting Sort since Counting Sort requires building an array of size 37,832,892 (population of Tokyo).

Runtime for counting sort on N keys with alphabet of size R: `Θ(N + R)`.

* Create an array of size R to store counts: `Θ(R)`.
* Counting number of each item: `Θ(N)`.
* Calculating target positions of each item: `Θ(R)`.
* Creating an array of size N to store ordered data: `Θ(N)`.
* Copying items from original array to ordered array: Do N times:
* * Check target position: `Θ(1)`.
* * Update target position: `Θ(1)`.
* Copying items from ordered array back to original array: `Θ(N)`.

Memory usage: `Θ(N + R)`. (N for ordered array, while R for counts and starting points.)
If `N ≥ R`, then the algorithm will have a reasonable performance.

## LSD Radix Sort

Not all keys belong to finite alphabets, such as Strings. However, Strings consist of characters from a finite alphabet.

General idea of LSD Radix Sort:

* Sort each digit independently from rightmost digit towards left.
* Pick appropriate letters to represent non-constant terms.
* When keys are of different lengths, can treat empty spaces as less than all other characters.

### Performance

Runtime of LSD Radix Sort: `Θ(WN + WR)`.
Runtime depends on length of longest key.
* N: Number of items.
* R: size of alphabet.
* W: Width of each item in # digits.

## MSD Radix Sort

Just like LSD, but sort from leftmost digit towards the right.

Sort each subproblem separately to avoid conflicts.

### Performance

* Best Case: Finish in one counting sort pass, looking only at the top digit. `Θ(N + R)`
* Worst Case: Look at every character, degenerating to LSD sort. `Θ(WN + WR)`


# File: Week 13/Sorting and Data Structures Conclusion.md

# Sorting and Data Structures Conclusion

## Radix Sort vs. Comparison Sorting

Merge Sort requires `Θ(N log N)` compares.

Merge Sort’s runtime on strings of length W:
* `Θ(N log N)` if each comparison takes constant time. (Strings are all different in top character.)
* `Θ(WN log N)` if each comparison takes `Θ(W)` time. (   Strings are all equal.)

### LSD vs. Merge Sort

Treating alphabet size as constant, LSD Sort has runtime `Θ(WN)`. 

Merge Sort has runtime between `Θ(N log N)` and `Θ(WN log N)`.

* Intuitively if `W < log N`, or if the Strings are similar, LSD is faster.
* If the Strings are very different, Merge Sort is faster.

### Cost Model

An alternate approach is to pick a cost model: the number of characters examined.
* Radix Sort: Calling charAt in order to count occurrences of each character.
* Merge Sort: Calling charAt in order to compare two Strings.

Suppose there are 100 strings of 1000 characters each.

Estimate the total number of characters examined by MSD Radix Sort if all strings are equal. In the worst case (all strings equal), every character is examined exactly once. 

Estimate the total number of characters examined by Merge Sort if all strings are equal. Merging N strings of 1000 characters requires `N/2 * 2000 = 1000N` examinations. Examine approximately `1000N log2 N` total characters.

* MSD radix sort will examine `~1000N` characters (For N = 100: 100,000).
* Merge sort will examine `~1000N log2(N)` characters (For N = 100: 660,000).

### Empirical Study

The cost model isn’t representative of everything that is happening.

Java’s Just-In-Time Compiler secretly optimizes your code when it runs. 

Example: Performing calculations whose results are unused.

Comparing algorithms that have the same order of growth is challenging.
* Have to perform computational experiments.
* In modern programming environments, experiments can be tricky due to optimizations like the JIT in Java.

## Sorting Summary

Three basic flavors: Comparison, Alphabet, and Radix based.

Each can be useful in different circumstances, but the important part was the analysis and the deep thought.

* Comparison based: Selection (Heapsort), Merge, Partition, Insertion.
* Alphabet based: Counting.
* Radix based: MSD, LSD.


# File: Week 14/Compression, Complexity, and P=NP.md

# Compression, Complexity, and P=NP?

## Model 1 vs. Model 2 for Compression

A Flaw in Compression Model 1: Source code for decompression algorithm itself might be highly complex.

### Question 1: Comprehensible Compression

Is there a "comprehensible" compression takes as input a target bitstream B, and outputs useful, readable Java code?

### Question 2: Optimal Compression

Is there an optimal compression takes as input a target bitstream B, and outputs the shortest possible Java program that outputs this bitstream?

## Kolmogorov Complexity

The Java-Kolmogorov complexity K_J (B) is the length of the shortest Java program (in bytes) that generates B.

Fact #1: Kolmogorov Complexity is effectively independent of language. For any bit stream, the Java-Kolmogorov Complexity is no more than a constant factor larger than the Python-Kolmogorov Complexity.

Could write a Python interpreter in Java and then runs the Python program.

It means that most bitstreams are fundamentally incompressible no matter what programming language is used for the compression algorithm.

## Space/Time Bounded Compression

An optimal compression algorithm takes as input a target bitstream B, and outputs the shortest possible Java program that outputs this bitstream.

No “optimal compression” algorithm exists.

### Space Bounded Compression

Takes two inputs, a target bitstream B and a size S, and outputs a Java program of `length ≤ S` that outputs B.

Does not exist, since it could be used to find K_J (B) by binary searching on S.

### Space/Time Bounded Compression

Takes three inputs, a target bitstream B, a size S, a maximum number of lines of bytecode executed T, and outputs a Java program of `length ≤ S` that outputs B in fewer than T executed lines of bytecode.

For each possible program p of length S or less:
* If p compiles, run program p until either:
* p terminates.
* We output B.
* T lines of bytecode are executed.
Runtime: `O(T x 2 ^ S)`.

### Efficient Space/Time Bounded Compression

* Need to make a more precise definition of what we mean by “efficient”.
* Closely related to an important puzzle in computer science: Does P = NP?

## P = NP?

An efficient solution to these three problems (3SAT, Independent Set, and Longest Path) would also give an efficient space/time bounded compression algorithm.

Space/time bounded compression reduces to 3SAT, Independent Set, and Longest Path.

### Definition

Two important classes of yes/no problems:
P: Efficiently solvable problems.
NP: Problems with solutions that are efficiently verifiable.

Examples of problems in P:
* Is this array sorted?
* Does this array have duplicates?

Examples of problems in NP:
* Is there a solution to this 3SAT problem?
* In graph G, does there exist a path from s to t of weight > k?

Any decision problem for which a yes answer can be efficiently verified can be transformed into a 3SAT problem. This transformation is also "efficient" (polynomial time).


# File: Week 14/Compression.md


# Compression

Compression Model #1: Algorithms Operating on Bits.
* Bitstream -> Algorithms Operating on Bits -> Compressed bits C
* Compressed bits C -> Decompression Algorithm -> Bitstream

A lossless compression algorithm require that no information is lost.

## Prefix Free Codes

English text is usually represented by sequences of characters, each 8 bits long. Use fewer than 8 bits for each letter: Have to decide which bit sequences should go with which letters.

Morse code creates ambiguity. Avoid ambiguity by making code prefix free. A prefix-free code is one in which no codeword is a prefix of any other. 

Some prefix-free codes are better for some texts than others. It’d be useful to have a procedure that calculates the optimized code for a given text.

## Huffman Coding

### General Idea

Calculate relative frequencies.
* Assign each symbol to a node with weight = relative frequency.
* Take the two smallest nodes and merge them into a super node with weight equal to sum of weights.
* Repeat until everything is part of a tree.

### Data Structures

For encoding (bitstream to compressed bitstream):
* Array of BitSequence[], to retrieve, can use character as index.
* HashMap<Character, BitSequence>. (Lookup in a hashmap consists of: Compute hashCode, mod by number of buckets, look in a linked list.)

Compared to HashMaps, Arrays are faster, but use more memory if some characters in the alphabet are unused.

For decoding (compressed bitstream back to bitstream):
* Tries. (Need to look up longest matching prefix.)

### Practice

Two possible philosophies for using Huffman Compression:
1.  Build one corpus per input type.
2.  For every possible input file, create a unique code just for that file. Send the code along with the compressed file.

* Approach 1 will result in suboptimal encoding.
* Approach 2 requires to use extra space for the codeword table in the compressed bitstream. (Use in real world.)

Given a file X.txt that prepares to compress into X.huf:
* Consider each b-bit symbol of X.txt, counting occurrences of each of the 2b possibilities, where b is the size of each symbol in bits.
* Use Huffman code construction algorithm to create a decoding trie and encoding map. Store this trie at the beginning of X.huf.
* Use encoding map to write codeword for each symbol of input into X.huf.

To decompress X.huf:
* Read in the decoding trie.
* Repeatedly use the decoding trie’s `longestPrefixOf` operation until all bits in X.hug have been converted back to their uncompressed form.

## Compression Theory

The big idea in Huffman Coding is representing common symbols with small numbers of bits.

General idea: Exploit redundancy and existing order inside the sequence.

Different compression algorithms achieve different compression ratios on different files.

There's no compression algorithm that can compress any bitstream by 50%.

* Argument 1: If true, it will be able to compress any bitstream down to a single bit. Interpreter would have to be able to do the following task for any output sequence.
* Argument 2: There are far fewer short bitstreams than long ones.

### Compression Model

Universal compression is impossible, but the following example implies that comparing compression algorithms could still be quite difficult. 

Suppose there is a special purpose compression algorithm that simply hardcodes small bit sequences into large ones. Example, represent GameOfThronesSeason6-Razor1911-Rip-Episode1.mp4 as 010. To avoid this sort of trickery, the compression model should include the bits needed to encode the decompression algorithm itself. 

Compression Model #2: Self-Extracting Bits

As a model for the decompression process, the algorithm and the compressed bitstream could be treated as a single sequence of bits. (Can think of the algorithm and compressed bitstream as an input to an interpreter.)


