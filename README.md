# CSharp-Hacks
Quick Tips For C# Programming.

Examples are taken from the book [C# 6.0 Cookbook](https://www.amazon.com/6-0-Cookbook-Solutions-Developers-ebook/dp/B015YOJS6I)

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

