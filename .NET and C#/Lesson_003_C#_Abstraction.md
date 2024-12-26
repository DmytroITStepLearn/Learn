# Lesson 3. Abstraction in C#

Abstraction is a 4th principle in OOP. It allows us to make _inheritance_ more secure and avoid making mistakes when writing implementation in derrived classes. Abstraction is more complex principle, however in this lesson we will cover the main concept of using abstraction in C#

## Abstract class

So, imagine you have created a class  **Sender** that has to send information to users. Then you create a class **EmailSender** that sends the info by email. Here's an example:

```C#
class Sender
{
    public virtual void Send(string data)
    {
        // do nothing
    }
}

class EmailSender : Sender
{
    private SmtpClient _smtpClient;

    public EmailSender()
    {
        _smtpClient = new SmtpClient();
    }

    public override void Send(string data)
    {
        // send data by email
        // just a code for example, do not try at home :)
        _smtpClient.Send(new MailMessage("from@gmail.com", "to@gmail.com", "title", data));
    }
}
```

Everything works fine but then you decided to add another class **PushMessageSender** that sends message to users' phones. But you forgot to add an implementation to method _Send()_

```C#
class PushMessageSender : Sender
{

}
```

No error will be thrown but when you run the code you will see that the push message never sends. It's because we use the implementation from class **Sender** and did not add implementation to a class **PushMessageSender**
To solve this problem we will need to create **abstract class**. When a class is abstract it means the derrived classes **must** implement its **abstract methods**

```C#
abstract class Sender // abstract class
{
    // abstract method
    public abstract void Send(string data);
}

class PushMessageSender : Sender // error
{

}
```

We will get error because the class **PushMessageSender** does not implement the abstract method _Send()_. It makes code more secure because now we cannot forget about the implementation of methods.
Also, what if we need to create code that sends messages depending on our choice (by email or by push messages)? We can write it like this:

```C#
Sender sender = new EmailSender();
sender.Send("Hello world");
```

Now if we want to change the implementation we just need to change **EmailSender** to **PushMessageSender**, and we do not have to worry about method _Send()_ because this method is abstract and must have implementation. Great! But what if we accidentely wrote code this way:

 ```C#
Sender sender = new Sender();
sender.Send("Hello world");
```

For class **Sender** there is no implementation for method _Send()_. But no worries, because if the class is abstract, it means we cannot create an object of this type. It can be ``Sender sender``, but it can never be ``new Sender()``. It also provides security for our code.

Methods and properties can be abstract, but only is class is marked with keywork _abstract_. Abstract class can still have fields, constructors and everything that other classes can have, but cannot be initialized with keyword _new_. 

## Interface

Sometimes we need out abstract class to contain only abstract methods or properties and for this reason we can create an **interface** instead. Imagine a smartphone and an old dial phone. They both can _call_ and _decline call_, but they do that with different implementations. Let's see an example how we can create those classes with interface:

```C#
interface ITelephone
{
    void Call();
    void DeclineCall();
}

class Smartphone : ITelephone
{
    public void Call()
    {
        
    }

    public void DeclineCall()
    {
        
    }
}

class DialPhone : ITelephone
{
    public void Call()
    {

    }

    public void DeclineCall()
    {

    }
}
```

Here we must have added methods _Call()_ and _DeclineCall()_ in our classes because they implement the **interface**. And we also do not use keyword **override** anymore, we don't have to add this when implementing interface.

So, to create an interface we have to use keywork **interface**. Also methods and properties in interface do not have body (because they are abstract by default) and they do not have access modifiers (it is **public** by default). We also add letter I at the beginning of the name of interface.

```C#
interface IMyInterface
{
    int MyProperty { get; set; }
    void MyMethod();
}
```

We can also use interface as a type when creating an object, but cannot initialize objec with this type, just like with _abstract classes_.

```C#
ITelephone telephone = new Smartphone();

telephone.Call();
telephone.DeclineCall();
```

Interfaces can only contain methods and properties without implementation, if you need to have abstraction and methods with realization or fields or constructors, then you need to use an abstract class for this.

> Actually interfaces can have realization of methods, fields and so on, but it became possible after new .NET updates and it breaks the logic of abstrction a bit, so I recommend using old-fashined way to work with interfaces

Also interfaces allow you to implement two, three or more interfaces for one class. For example, you have interface with methods _SaveData()_ and _LoadData()_ and you have classes _FileManager_ and _DatabaseManager_ that can do this operations. But then, you have to create a new class _CloudManager_ that can save and load data, but also _migrate data_. For that reason you add a new method to your interface _MigrateData()_, but then other classes must implement it, and the implementation will be empty for _FileManager_ because it does not support this logic.

```C#
interface IDataManager
{
    void SaveData(string data);
    string LoadData();
    void MigrateData(); // new method 
}

class FileManager : IDataManager
{
    public string LoadData()
    {
        // load data from file
        return "data from file";
    }

    public void SaveData(string data)
    {
        // save data to file
    }

    public void MigrateData()
    {
        throw new NotSupportedException("Cannot migrate in file");
    }
}

class DatabaseManager : IDataManager
{
    public string LoadData()
    {
        // load data from database
        return "data from database";
    }

    public void SaveData(string data)
    {
        // save data to database
    }

    public void MigrateData()
    {
        // migrate data in database
    }
}

class CloudManager : IDataManager
{
    public string LoadData()
    {
        // load data from cloud
        return "data from cloud";
    }

    public void SaveData(string data)
    {
        // save data in cloud
    }

    public void MigrateData()
    {
        // migrate data in cloud
    }
}
``` 

Now the method _MigrateData()_ in class _FileManager_ will throw exception if we call this method and it's bad practice. Instead, we should devide our interface into two and use them where they belong

```C#
interface IDataManager
{
    void SaveData(string data);
    string LoadData();
}

interface ICanMigrateData
{
    void MigrateData(); // new method 
}

class FileManager : IDataManager
{
    public string LoadData()
    {
        // load data from file
        return "data from file";
    }

    public void SaveData(string data)
    {
        // save data to file
    }
}

class DatabaseManager : IDataManager, ICanMigrateData
{
    public string LoadData()
    {
        // load data from database
        return "data from database";
    }

    public void SaveData(string data)
    {
        // save data to database
    }

    public void MigrateData()
    {
        // migrate data in database
    }
}

class CloudManager : IDataManager, ICanMigrateData
{
    public string LoadData()
    {
        // load data from cloud
        return "data from cloud";
    }

    public void SaveData(string data)
    {
        // save data in cloud
    }

    public void MigrateData()
    {
        // migrate data in cloud
    }
}
```

Now the code looks much clearer. We do not have to worry that we will get an exception because a class cannot fully implement interface. We can also inherit a class and implement some interfaces at the same time. 

```C#
abstract class Product
{
    public string Name { get; set; }
    public double Price { get; set; }
}

interface IHaveDiscount
{
    double GetDiscount();
}

interface IHaveWarranty
{
    public int DaysToReturnByWarranty { get; set; }
}

class Phone : Product, IHaveDiscount, IHaveWarranty
{
    public Phone()
    {
        Name = "Phone";
        Price = 999;
        DaysToReturnByWarranty = 60;
    }
    public int DaysToReturnByWarranty { get; set; }

    public double GetDiscount()
    {
        return Price * 0.8;
    }
}

class HeadPhones : Product, IHaveWarranty
{
    public HeadPhones()
    {
        Name = "Headphones";
        Price = 199;
        DaysToReturnByWarranty = 14;
    }
    public int DaysToReturnByWarranty { get; set; }
}

class Doll : Product
{
    public Doll()
    {
        Name = "Barbie";
        Price = 10;
    }
}
```

This way we can now display the info about our products with the following code:

```C#
Phone phone = new Phone();
HeadPhones headPhones = new HeadPhones();
Doll doll = new Doll();

Product[] products = [phone, headPhones, doll];

for(int i = 0; i < products.Length; i++)
{
    Console.Write($"{i+1}) {products[i].Name} ");
    if (products[i] is IHaveDiscount)
    {
        Console.Write("$" + (products[i] as IHaveDiscount).GetDiscount());
    }
    else
    {
        Console.Write("$" + products[i].Price);
    }

    if (products[i] is IHaveWarranty) {
        Console.WriteLine($"\nYou can return this product in {
            (products[i] as IHaveWarranty).DaysToReturnByWarranty} days");
    }
}
/*
1) Phone $799,2
You can return this product in 60 days
2) Headphones $199
You can return this product in 14 days
3) Barbie $10
*/
```

Now if we add new product class, the code for displaying products will not change and will work fine. 

## Static members

Static members does not really belong to abstraction, but they have something in common: we also cannot initialize them. Do you remember the class _Math_? We use this class to perform meth operation. Let's find a squere root of 144 for example:

```C#
int number = 144;
double result = Math.Sqrt(number);

Console.WriteLine($"Squere root of 144 = {result}");
//Squere root of 144 =  12
```

Here we didn't create an object of a class _Math_, we just use this method. Some methods do not really need an object to work, therefore we can create a method that does not need an object. We can do it with **static methods**. This methods does not need an object and do not use properties or fields or other methods in a class.

```C#
string title = HtmlGenerator.ToHeading("Hello world!");
string list = HtmlGenerator.ToList(["first", "second", "third"]);

Console.WriteLine(title);
Console.WriteLine(list);
/*
<h1>Hello world!</h1>
<ul>
        <li>first</li>
        <li>second</li>
        <li>third</li>
</ul>
*/

class HtmlGenerator
{
    public static string ToHeading(string data)
    {
        return $"<h1>{data}</h1>";
    }

    public static string ToList(string[] data)
    {
        string html = "<ul>\n";
        for(int i = 0; i < data.Length; i++)
        {
            html += $"\t<li>{data[i]}</li>\n";
        }
        html += "</ul>";

        return html;
    }
}
```

We have created a class HtmlGenerator to cover our text in html tags. We do not need an object or other class members to do that. We just get data, work with this data and return it. To create a method static we just add a modifier **static**, and to call this method we use _class name_ instead of object.

Not only methods can be static, but also properties, fields and constructors (but only one and without parameters). If a class is marked with a keyword **static**, then every member of a class must be static too. 

Also static members can use only other static members. That's why we can call our methods from _Main()_ method only if they are static as well. 