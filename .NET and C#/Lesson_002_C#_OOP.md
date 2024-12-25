# Lesson 2. OOP in C#

C# is an object-oriented programming language. The four basic principles of object-oriented programming are:

- Encapsulation
- Inheritance
- Polymorphism
- Abstraction

Let's discuss each principle now, but before we should see the syntax of classes in C#

## Classes

A class is a type that can contains **properties**, **fields**, **methods**, and **constructors**.

```C#
class MyClass // class
{
    private int _field; // field
    public int Property { get; set; } // property

    public MyClass() // constructor
    {
    }

    public void Method() // method
    {

    }
}
```

- field: a field has a modifier **private** and it usually is named with underscore _name
- property: a property has a modifier **public** and it also has **{ get; set; }**. It is a _sugar syntax_ for getters and setters. In other programming languages it is usually represented by methods set and get
- constructor: a constructor is a method that is invoked when the object is initialized. Constructor can also contain parameters that must be passed before using the object
- method: a method can be invoked using the object and use its parameters (properties, fields)

> Also classes can have destructors, but they are not usually used

Also constructors can be multiple, and you decide which one to use

```C#
MyClass myClass;

myClass = new MyClass(10); // initialize object

class MyClass {

    private int _field;

    // constructor with no parameters
    public MyClass()
    {
    }

    // constructor with parameter field
    public MyClass(int field)
    {
        _field = field;
    }
}
```

> Hint: to create constructor faster just type **ctor** + **Tab**
> Also for property: **prop** + **Tab**

## Encapsulation

Encapsulation allows to hide some information from other programmers. Also when a class contains lots of methods and properties and some of them should be used only in this class, we can hide them by using encapsulation so other programmers can use only those parts of the class they really need. Here's an example: 

```C#
Bunk bunk = new Bunk();
double moneyInFuture = bunk.GetSaving(1000, 12);

Console.WriteLine(moneyInFuture);

public class Bunk
{
    private double _percent;

    public Bunk()
    {
        _percent = 5;
    }

    public double GetSaving(double amount, int months)
    {
        double year = (double)(months / 12);
        return amount * Math.Pow(1 + _percent / 100, year);
    }
}
```

Here we do not allow other programmers to change _percent field in this class to prevent errors. Therefore we use modifier **private** to protect us. However we want programmers in out team to use method GetSaving(), so we added modifier **public**. Now let's talk about modifiers:

- public: everything with public modifier can be used anywhere in the project
- private: everything with private modifier can be used only inside the class
- protected: everything with protected modifier can be used inside a class and in its **derrived classes**. We will talk about them in _Inheritance_ section
- internal: everything with internal modifier can be used anywhere in current project, but not in other projects

> If you do not write the modifier, the **private** modifier is used by default. But in classes the default modifier is **internal**


## Inheritance

Inheritance allows us to write less code and contain different types as one **base type**. Imagine you have class Cat, Dog, and Bird. Each class has a property _Name_ and a method _Sound()_, but Cat also has a property _CountOfLives_ (we all know that cats have 9 lives) and Bird has a method _Fly()_ unlike Cat or Dog. We can create these classes and add property _Name_ and method _Sound()_ in each one and then add other properties, but this would be a lot of code. Instead we can use _Inheritance_.
This allows us to create a **base class** (like Animal) and add property _Name_ and method _Sound()_ in it. Then Cat, Dog, and Bird will inherit this class and automatically have these data

```C#
class Animal
{
    public string Name { get; set; }

    public void Sound()
    {
        Console.WriteLine("Animal sound");
    }
}

class Cat : Animal
{
    public Cat()
    {
        Name = "Cat Sue";
    }

    public int CountOfLives { get; set; }
}

class Dog : Animal
{
    public Dog()
    {
        Name = "Dog Ben";
    }
}

class Bird : Animal
{
    public Bird()
    {
        Name = "Bird Lily";
    }

    public void Fly()
    {
        Console.WriteLine("Birds fly");
    }
}
```

As you can see, we don't duplicate the code, it is in class Animal and it inherits to other classes. What if we want to use this methods and properties for the objects? We can add this objects in an array and then use it. As long as the classes inherit from class Animal - we can add them to an array

```C#
Cat cat = new Cat();
Dog dog = new Dog();
Bird bird = new Bird();

Animal[] animals = [cat, dog, bird];

for(int i = 0; i < animals.Length; i++)
{
    Console.WriteLine(animals[i].Name);
    animals[i].Sound();
}
/*
Cat Sue
Animal sound
Dog Ben
Animal sound
Bird Lily
Animal sound
*/
```

It works, but not good enough. The method _Sound()_ works with the implementation in class Animal and does not depend on the classes Cat, Dog or Bird. We can fix it by changing our classes a little.

```C#
class Animal
{
    protected string sound;
    public string Name { get; set; }

    public void Sound()
    {
        Console.WriteLine(sound);
    }
}

class Cat : Animal
{
    public Cat()
    {
        Name = "Cat Sue";
        sound = "Meow";
    }

    public int CountOfLives { get; set; }
}

class Dog : Animal
{
    public Dog()
    {
        Name = "Dog Ben";
        sound = "Bark";
    }
}

class Bird : Animal
{
    public Bird()
    {
        Name = "Bird Lily";
        sound = "Tweet Tweet";
    }

    public void Fly()
    {
        Console.WriteLine("Birds fly");
    }
}
```

Now it will works correctly. There is a better way to do that, we will learn how to do that in _Polymorphism_ section. Also here we used modifier **protected** so that class Animal and its derrived classes have access to _sound_, but there is no access outside the classes.
Now I want to display _CountOfLives_ if the object is of type Cat. We cannot just display it in a for loop because we do not have access to it. The array is of type Animal, and class Animal does not have a property _CountOfLives_, but we know that the type Cat has its property and it is covered by class animal. Let's see an example to understand how it works:

```C#
Cat cat = new Cat();
cat.CountOfLives = 9; // we have access
```
```C#
Animal cat = new Cat();
cat.CountOfLives = 9; //!!! we do not have access
```

The thing is that the type Animal does not have this property, but we know that **in fact** we have a class Cat here. the type Animal hides the type Cat. We can write ``Animal cat = new Animal``  and it hides the Cat implementation. However, we cannot write like this:

```C#
Cat cat = new Animal(); // error
```

> There is a simple rule: Every Cat is Animal, but not every Animal is Cat.

So, if I want to display property _CountOfLives_ if the type is Cat, how to do that? There is a syntax for it:

```C#
for (int i = 0; i < animals.Length; i++)
{
    if (animals[i] is Cat)
    {
        Console.WriteLine((animals[i] as Cat).CountOfLives);
    }

    Console.WriteLine(animals[i].Name);
    animals[i].Sound();
}
```

The keyword **is** used to check what type is **in fact** is stored. If animals[i] is in fact a Cat - true, otherwise - false
Another one is a keyword **as**. If we know for sure that the object is of type Cat, we will get a copy of this object of type Cat with the same data and we will be able to use property _CountOfLives_. But if the object is not of the type we will get null.

Now let's add a constructor in a class Animal that takes one parameter _name_. This will cause errors in inherited classes. Because we must invoke the constructor, it also means we must invoke constructors of inherited classes. It is the main rule of inheritance: we must invoke constructor of a base class.

```C#
class Animal
{
    public Animal(string name)
    {
        Name = name;
    }

    protected string sound;
    public string Name { get; set; }

    public void Sound()
    {
        Console.WriteLine(sound);
    }
}

class Cat : Animal
{
    public Cat() : base("Cat Sue")
    {
        sound = "Meow";
    }

    public int CountOfLives { get; set; }
}
```

We write **: base** and pass the parameter to call the constructor from class Animal

Also in C# we cannot have multiple inheritance. It means our class cannot inherit two or more classes. We cannot write ``class Cat : Animal, Dog`` because it will throw an error.


## Polymorphism

Polymorphism allows us to change the implementation of a method or a property in derrived classes. In this case we can be sure to have access to some parts of a class but have different implementation depending on the type. Let's see the example:

```C#
class Product
{
    public string Name { get; set; }
    public double Price { get; set; }

    public virtual double GetPriceWithDiscount()
    {
        return Price;
    }
}

class Laptop : Product
{
    public override double GetPriceWithDiscount()
    {
        return Price * 0.9; // discount 10%
    }
}
```

Here we have a class Product and a derrived class Laptop. In class Laptop we have changed the implementation of a method _GetPriceWithDiscount()_ and apply discount of 10 percents to a price of a laptop. Now even if the type Laptop is hiden by type Product, the implementation will be as the implementation in a class Laptop.

```C#
Product product = new Laptop();
product.Price = 100;

Console.WriteLine(product.GetPriceWithDiscount());
// 90
```

Here we used keywords **virtual** to set the method as 'possible to change implementation' and **override** to change the implementiation.

> If we don't use these keywords the method will not change the implementation but will hide the method implementation of a base class. It is not the same!

If a method or a property is marked **virtual** it does not mean that we must _override_ this method. We can just skip it and a class will use the implementation from base class.

### Class Object

In C# we have a class Object and every class is inherited from this class by default (even classes that we create). This class have some virtual methods that we can override then.

```C#
class Human
{
    public string Name { get; set; }
    public DateTime DateOfBirth { get; set; }

    // string representation of Human
    public override string ToString()
    {
        return $"Human {Name} {DateOfBirth}";
    }

    // compare two Human objects
    public override bool Equals(object? obj)
    {
        if (!(obj is Human)) return false;

        Human human = obj as Human;
        return human.Name == Name && human.DateOfBirth == DateOfBirth;
    }

    // get unique number for object
    public override int GetHashCode()
    {
        return (int)(DateOfBirth.ToBinary() + Name.GetHashCode());
    }
}
```

We can override methods ToString(), Equals(), GetHashCode() to use in some scenarios.

## Abstraction

This topic will be discussed in the next lesson.