---
title: Prefer readonly to const
date: 2016-04-10 14:56:00.000000000 -05:00
---
I was recently reading [Effective C#](http://www.amazon.com/Effective-Covers-4-0-Specific-Development/dp/0321658701/ "Effective C#") by Bill Wagnar.  In the book, the author makes the statement that we should **prefer readonly to constants**.  I wasn't immediately able to piece together why we should, I mean, what's even the difference between the two? To answer this, let's first discuss how they're the same.

#####Both are static
Now obviously ```static readonly ``` is static because it is decorated by the static keyword.  One thing you may have overlooked however though, is that the ```const``` keyword is **implicitly declared as static**.

Static, put simply, means that whatever member it is decorating, belongs to the class itself rather than the object. The static member must be referenced by type and not by an instance. For example, the following code would not compile:

```language-csharp
public class TheClass
{
    public static void TotallyStaticMethod() { ... }
}
```

```language-csharp
var someClass = new TheClass();
someClass.TotallyStaticMethod(); // cannot be accessed with an instance reference;
```

The above would generate a compiler error, specifically: *Member 'TheClass.TotallyStaticMethod()' cannot be accessed with an instance reference; qualify it with a type name instead.*

#####Both are immutable
Once declared ```readonly``` or ```const```, the value cannot change.  Now, there is a slight difference as to when the values become truly immutable.  The value of a ```const``` must be initialized when it is declared. No sooner (though that'd be impressive) and no later.

On the other hand, ```readonly``` may be initialized during its declaration or in the constructor of the class that it was declared.  This is useful for facilitating dependency injection through the constructor or even configuration values.

The biggest difference between the two?

#####Const is evaluated at compile time
What does this mean exactly? When you set the value of a constant, the compiler will actually take your variable assignment and bake it directly into the IL.

To see this in action, let's say we have the following code snippet.

```language-csharp
namespace AnotherAssembly
{
    public class Library
    {
        public static readonly string ReadOnlyValue = "first readonly";
        public const string ConstValue = "first const"; 
    }
}
```

If you were to compile the above code, reference the DLL in another project, and inspect the DLL with ILSPY (or your disassembler of choice) you would see the following output:

![](/content/images/2016/04/readonlyconstil-1.png)

This clearly shows that our ```readonly``` variable is a reference type, referencing the ReadOnlyValue in the Library class which lives in AnotherAssembly.  On the other hand, the const variable is being directly loaded with the value "first const".

This means that if you were to change the value of a ```const``` in an assembly, the assemblies that depend on that ```const``` will have the old value until they are rebuilt.  This could cause a lot of headaches down the road, which is why it's always best to reserve ```const``` for values that you know will never change.

So while it is true that ```const``` will be slightly more performant than ```readonly```, it's going to be a negligible amount. Always keep in mind that premature optimization is the root of all evil!

####Summary
1. ```const``` is evaluated at compile time, ```readonly``` is evaluated at runtime.
2. Prefer ```const``` for values that you know will never change, for any reason.
3. When in doubt, use ```readonly``` (the performance gain is negligible).
