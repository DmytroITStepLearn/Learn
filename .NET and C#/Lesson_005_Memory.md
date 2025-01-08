# Lesson 5. Memory

As programmers we always have to care about memory to make out project betterin performance. To understand how to work with memory we have to understand how it works in .NET behind the scene. First of all, data types in .NET can be devided into **value types** and **reference types**. They can be stored in our memory in **Stack** or **Heap**

_Stack_ is a part of memory that can store and return data very fast, but the amount of memory in Stack is limited.

_Heap_ has much more memory size to store data, but the speed is less efficient. 

### Value types
Value types are data types that directly store data in a fixed amount of memory. They are simple types that we commonly use to store numbers, date and so on. Also _structs_ are value types too

```C#
// Value types  
int number = 100;  
char symbol = '*';  
double percent = 0.5;  
DateTime date = DateTime.UtcNow;
```

Advantages
- They are efficient, as they are stored in _Stack_ and use less memory and have faster access time

Disadvantages
- They are limited in size, as they cannot exceed the size of the stack



### Reference types

Reference types are data types that store references to data in a variable amount of memory. Reference types are: classes, arrays, Object.

```C#
// Reference types  
object obj = new object();  
int[] array = new int[3];  
Program program = new Program();  
string str = "Hello"; // also reference type
```


Advantages
- They have dynamic memory allocation, as they can grow or shrink as needed

Disadvantages
- They have indirect access, as they require an extra step to access the data through the reference

> Even though **string** is a reference type, it has a behavior of value type because of its operators

---

There are also some differences between using value types and reference types


### Data allocation

**Value types**  are stored in _Stack_, **Reference types** are stored in _Heap_. However we can change this behavior. We can store value types in _Heap_, but it's a bad thing and we must avoid it

```C#
object obj = 100; // value type in Heap
```

It is called **boxing** when we put a value type data to object (reference type). The opposite is **unboxing** when we take value type out of the object. This operation is dangerous so be careful with it.

```C#
int number = (int)obj; // unboxing
```

This is a bad practice because we store low-memory data in _Heap_ and lose performance

### Copy value

In **value types** we copy the values, but in **reference types** we copy the _reference_ to the object in memory, so in the example below ``user1`` is pointing to the data in memory where ``user2`` is also pointing -- they share the same memory.

```C#
int number1 = 100;  
int number2 = number1;  
number2 = 200;  
  
Console.WriteLine($"number1: {number1}\nnumber2: {number2}");  
// number1: 100  
// number2: 200  
  
User user1 = new User();  
user1.FirstName = "John";  
User user2 = user1;  
user2.FirstName = "Alice";  
  
Console.WriteLine($"user1: {user1.FirstName}\nuser2: {user2.FirstName}");  
// user1: Alice  
// user2: Alice
```

### Equality comparison

**Value types** are compared by value, when **reference types** are compared by reference

```C#
int number1 = 100;
int number2 = 100;

Console.WriteLine(number1 == number2);
// true

User user1 = new User();
user1.FirstName = "John";
User user2 = new User();
user2.FirstName = "John";

Console.WriteLine(user1 == user2);
// false

User user3 = user1;
Console.WriteLine(user1 == user3);
// true
```