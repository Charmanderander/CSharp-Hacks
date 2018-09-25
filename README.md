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

