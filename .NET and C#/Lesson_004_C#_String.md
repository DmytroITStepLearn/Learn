# Lesson 4. String 

String is used in programming everywhere to work with text data. We use data type **string** for storing data about clients, products, and almost all models in a project and also for validation, logging, information for users and so on
String is a class and also an array. We can consider **string** as both class and array because we can create an object of class String and we can use it like an array to get access to each symbol in the text
We can use many methods in class String to work with text data and transform it in a format we want. That is not necessarily to remember all these methods, but it is important to know what we can do with text data. Also we need to know that when we work with **string** the methods we use does not effect the object. The methods just create a bew object of type string with changes, but the original object remains unchanged

```C#
string originalString = "Hello world!";
string changedString = originalString.ToUpper();

Console.WriteLine(originalString);
Console.WriteLine(changedString);
// Hello world!
// HELLO WORLD!
``` 

As we can see here, the ``originalString`` object did not change, but we created a new object ``changedString`` with all symbols in upper case. Such classes that have methods that do not change the object but create a new one are colled _immutable_

So, let's see a list of useful methods in class String and obserwe how they work

```C#
string str1 = "Hello";
string str2 = " world";
```

> By the way: the operator ``+=`` changes the object even though methods does not change the original string object. That's because the operator ``+=`` transforms into operator ``=`` in the end. This operator can change the object
> ```C#
> str1 += "Hello";   
> str1 = str1 + "Hello"; // same here
> ```


| Method | Description | Example |
|--------|-------------|---------|
|+= (operator)|Adds text to the end|``str1 += str2; //Hello world``|
|Substring(index, length)|Gets a part of a text|``str1.Substring(1, 3); //ell``|
|Replace(old, new)|Replaces the symbols in a text by new symbols|``str1.Replace("He", "Yi"); //Yillo``|
|ToLower()|Gets all symbols in lower case|str1.ToLower(); //hello |
|ToUpper()|Gets all symbols in upper case|str1.ToUpper(); //HELLO|
|== (operator)|Compares to strings|``str1 == str2; //false``|
|Contains(text)|Checks if string object can a specified text inside|``str1.Contains("Hel"); //true``|
|StartsWith(text)|Checks if string object starts with specified text|``str1.StartsWith("W"); //false``|
|EndsWith(text)|Checks if string object ends with specified text|``str1.EndsWith("lo"); //true``|
|IndexOf(text)|Finds the index where the specified text first appears|``str1.IndexOf("l"); //2``|
|Split()|Devides text in string object by space|``"How are you?".Split(); //["How", "are", "you"]``|
|Insert(index, text)|Adds specified text at the specified index|``str1.Insert(2, "lll"); //Helllllo``|
|Trim()|Removes spaces frm start and end|"   text    ".Trim(); //text|

There are other interesting methods in class String that you can discover and learns. Class String also has static methods that can be useful

| Method | Description | Example |
|--------|-------------|---------|
|Join(text, array)|Makes a string with elements of the array and a text between them|``string.Join("; ", [1, 2, 3]); //1; 2; 3``|
|IsNullOrEmpty()|Checks is string object is null or has no symbols|``str1.IsNullOrEmpty(); //false``|
|IsNullOrWhiteSpace()|Checks if string object is null or have only spaces|``"   ".IsNullOrWhiteSpace(); //true``|

Remember these methods when you work with **string**, they can help in many cases when you have to transform a string or validate the text data.

## String formatting

There are some ways we can show text data in a different formats. We usually use _interpolation_ in order to add our data inside text. The syntax of _interpolation_ is very simple: you just have to add dollar sign `$` in front of the string and surround the data with `{}`

```C#
int value = 120;
string info = $"The value is {value}";
Console.WriteLine(info);
// The value is 120
```

Also we have to format integer data to display it. We always have to dsiplay prices in a different formats. For example, the number 60000 should be displayed as _$60,000.00_. For that matter we can use `ToString(format)` method to display numbers in a desired format. You can find the table of formats in [Microsoft Docs](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings). Also as long as there are many different regional formats we can use _CultureInfo_ class to specify the format. We can use `en` format to work with dollars

```C#
using System.Globalization;

int price = 60000;
string formattedPrice = price.ToString("C2", CultureInfo.CreateSpecificCulture("en"));
Console.WriteLine(formattedPrice);
// $60,000.00
```

> Hint: you can use new easier syntax to work with formats, but without specifing the culture info
> ```C#
>  Console.WriteLine($"Price: {60000:C2}");
> ```


## StringBuilder

Sometimes we have to perform multiple operations when we add some new text to the string object. This is called _concatenation_ when we add two strings. This operation can be expensive for memory if we have to perform _concatenation_ for many times. To save up memory we can use _StringBuilder_ class. This class does not lead to problems wth meory when we add strings

```C#
// With string
long memory = GC.GetTotalMemory(false);

string text = "";
int countOfOperations = 10000;

for(int i = 0; i < countOfOperations; i++)
{
    text += "hello";
}

long memoryAfterOperation = GC.GetTotalMemory(false);

Console.WriteLine($"Time for operation: {memoryAfterOperation - memory} bytes");
//Time for operation: 1361608 bytes
```

```C#
// With StringBuilder
using System.Text;

long memory = GC.GetTotalMemory(false);

StringBuilder text = new StringBuilder();
int countOfOperations = 10000;

for(int i = 0; i < countOfOperations; i++)
{
    text.Append("hello");
}

long memoryAfterOperation = GC.GetTotalMemory(false);

Console.WriteLine($"Time for operation: {memoryAfterOperation - memory} bytes");
//Time for operation: 116840 bytes
```

As we can see, when we use _string_ we get 1 361 608 bytes in memory, but when we use _StrinBuilder_, the amount goes down to only 116 840 bytes, almost 10x times better.