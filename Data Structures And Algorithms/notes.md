# Bit operations
## shifts 
```java 
int a = 4; 
a >> 1; // 2 
// shift bits of a one position to the left it's equivelnt to dividing a by 2

int b = 3; 
b << 1 
// shifts bits of b one positin to the right it's equivelent to multiply by 2
```

## Ops 
### XOR 
```java
a ^ a = 0;
0 ^ a = a;
```

## common techniques
1 - eliminates the last one to the right. useful to count num of 1's bits in a string known as hamming weight
```java 
n = 0100011100001001 

n & (n - 1) 
n - (n & -n)

``` 

2- convert char to lower case / to upper case

```java 
// you can replace the space by asci representaion which is 32
'A' | ' '  // to lower case
'B' | ' ' 
'b' | ' ' // still the same 


'b' & '_' // convert to upper case
'B' & '_' // stille the same

```

3- getting the decimal value of binary 
```java

int res = 0; 

res = res << 1 | one_digit_of_binary_num;
````

# Language Specific 

### Casting Primitive types
```java
(char) 12 // will cast 12(integer) to the coressponding character.
```

# HashMap 

## frequency map 

```java 
// store each element of array and number of occurences 
// getOrDefault checks if nums presents or not if presents adds one else set zero and add one. 
map.put(nums[i], map.getOrDefault((nums[i], 0) + 1));
````

# Language API

## String 
1- convert array of chars to string
```java
String.valueOf(chars);
```
2- convert StringBuilder object to regular String 
```java
sb.toString();
``` 

3- convert string to Integer
```java
Integer.parseInt("123a"); // => 123
```

4 - substring
```java
string.substring(startIndex, endIndex);

```

4- convert String to Chars 
```java 
Strint s = "Hello There"; 
s.toCharArray(); 
``` 

5- evaluate arithmeatic experssions 
```java
String x = "15"; 
int add = x.charAt(1) - '2'; // => 3 
```
6 - add all lists of hashmap values to a list 
```java 
List<List<String>> res = new ArrayList<>();
res.addAll(map.values()); // list of map vlaue lists
```

# LinkedList 

- create dummy node to save the pointer of the new constructed list 
- user curr as a pointer on the new list. 
- frequently used when you reconstruct new list.
```java
ListNode dummy = new ListNode(); 
ListNode curr = dummy;
```


# Trees 

## Traversal 
- preorder use stack push, right first.
- postorder use stack push, left first.
- inorder use stack,
- levelorder use Queue.
- depth is the num of nodes from root to the farthest leaf node.

## BST 
- inorder traversal return sorted list.
- sorted list can be converted to BST, starting from mid of the array nad recursively mid, left, right.
- root of a BST is smaller than the leftmost node in the right subtree and bigger than the rightmost node in the left subtree.
- 


# Sliding Window
