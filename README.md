# CSharp-Hacks
Quick Tips For C# Programming.

Examples are taken from the book [C# 6.0 Cookbook](https://www.amazon.com/6-0-Cookbook-Solutions-Developers-ebook/dp/B015YOJS6I)

Obviously, won't be covering all of it, but only those that are really interesting (to me)

### Structs VS Classes
---

Structs are allocated on the stack, and so accessing it is faster, as opposed to data on the heap.

Cleaning up memory on the stack is easier, as opposed to the heap (which requires garbage collection)

Structs fall short when they are passed by value to methods, as the entire data has to be copied over.

### Returning multiple items from a method
---

If you wish to return multiple items from a method, use `ref` or `out` as the parameters, so the method modifies them directly

`public void ChangeMultipleValues(ref int x, ref int y, ref int z)`

### Initialize Constant Field at runtime
---

Fields marked as `const` has to be initialized at compile time. To initialize it at runtime, mark it as `readonly` instead.

### Ensuring object disposal
---

Use the keywork `using` to properly dispose of resources

```
using(FileStream FS = new FileSteam("Test.txt", FileMode.Create))
{
   ...
}
```

### Reversing a Sorted List
---

Use LINQ. 

```
var query = from d in data
                orderby d.Key descending
                select d;
                
foreach (KeyValuePair<int, string> kvp in query)
{
   Console.WriteLine("{0}, {1}\n", kvp.Key, kvp.Value);
}
```             

### Checking for NULL
---

This

```
if (val == null)
{
   val.Trim().ToUpper();
}
```

Can be reduced to this

```
val?.Trim().ToUpper();
```

`?` can also be chained

`Person?.Address?.State?.Trim();`

### Testing every element in a collection
---

Use `TrueForAll` method

```
List<string> strings = new List<string>() {"one", "two", null};

string str = strings.TrueForAll(delegate(string val)
{
   val? retrun true : return false;
}
```

`TrueForAll` accepts a generic delegate `Predicate<T>` called `match`

`public bool TrueForAll(Predicate<T> match)`

`match` determines if `TrueForAll` should return true or false

### Handling Concurrency
---

`ConcurrencyDictionary<TKey, TValue>` for multiple read and writes

`ImmuatableDictionary<TKey, TValue>` for multiple reads

### Detecting sneaky overflows
---

Use the keyword `checked` to check if overflows have silently occured

An `OverflowException` is thrown when the checked code fails

```
long lhs = 34000;
long rhs = long.MaxValue;

try
{
   check((int)(lhs + rhs))
}
catch (OverflowException)
{
   ...
}
```

C# code can run in checked and unchecked mode. The default is unchecked

### Lambda
---

`<Type> <variable name> = s => <function on s>`

The variables inside the lambda expression can be defined outside the expression

```
public void CountUp()
{
   int count = 0;
   
   // count is defined outside the lambda expression
   Func<int> countUp = () = count++;
   
   for (int i = 0; i < 10; i++)
      countUp();
}
```

Expression Lambdas have no paramters

`Func<int> countUp = () => count++;`

Statement Lambdas take paramters, and are enclosed in curly brackets

```
Func<int, int> writeInt = i =>
{
   Console.WriteLine(i);
   return i;
}
```

### Parallel LINQ
---

When working with multiple results of LINQ query, we can process them in parallel

`chapters.Select(c => TimedEvaluateChapter(c, rnd)).ToList();`

`chapters.AsParallel().Select(c => EvaluateChapter(c)).ToList();`

Use sparingly only for large data, for parallelism has some overheads.

### More Verbose Error Handling
---

Use the `Data` field in `System.Exception`, which functions as a dictionary

```
try
{
...
{
catch(Exception e)
{
   e.Data["Cause"] = "File not found!";
}
```

### Building Objects Dynamically
---

Use `ExpandoObject()` to create objects on the fly

```
dynamic expando = new ExpandoObject();

// Adding field
expando.name = "John";

// Adding function
expando.IsValid = (Func<bool>) ( () =>
{

// Do validation checks

});

// Adding property
AddProperty(expando, "Language", "English");

// Adding event
AddEvent(expando, "LanguagedChanged", eventHandler);
```

### Locking a FileStream for concurrency
---

Use the `Lock` method in `FileStream`

```
FileStream fs = new FileStream(fileName, FileMode.Create, FileAccess.ReadWrite, FileShare.ReadWrite, 4096, useAsync: true);


// Locks the entire file
fs.Lock(0, fileStreamLength);

// Locks a portion of the file
fs.Lock(0, fileStreamLength - 10);
```

### Making Static not shareable between threads
---

Static fields by default are shared between threads. Use `ThreadStaticAttribute` to mark `static` fields as not shareable between threads

```
public class Foo
{
   [ThreadStaticAttribute()]
   public static string foo = "my string";
}
```

### Locks for concurrency
---

Create an object, and lock it

```
public static class Locking
{
   private static int num = 1;
   private static object syncObject = new object();

   public static void increment()
   {
      lock(syncObject)
      {
         num++;
      }
   }
   
   public static void decrement()
   {
      lock(syncObject)
      {
         num--;
      }
   }
}
```

DONT DO NESTED LOCKING. Each class should create and use its own internal lock.

### Thread Local Storage
---

Storing data to each thread for privacy. Use `AllocateDataSlot`, `AllocateNamedDataSlot` or `GetNamedDataSlot`

### Optimizing Database requests
---

For database request, use `async` and `await` keywords, so your main thread can do something else while the database is processing

### Running tasks in order
---

Use the keyword `Task.ContinueWith`
