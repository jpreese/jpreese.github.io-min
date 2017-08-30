---
title: 'The Rules Pattern: How to Drop Your Guard'
date: 2017-06-21 15:45:26.000000000 -05:00
---
In your travels as a programmer, you will more than likely come across a body of code that looks a little something like the following:

```language-csharp
public bool CheckSystem(Computer computer)
{
    if (computer.Ghz < 3)
    {
        return false;
    }

    if (computer.Ram < 4)
    {
        return false;
    }

    if (computer.DiskSpace < 10)
    {
        return false;
    }

    return true;
}
```

Here we have a method called ```CheckSystem``` which tries to validate whether or not a model of a computer meets all of the system requirements. If the model fails to meet all of the specified requirements, the method will return false. It attempts to validate the model by using a number of different conditionals, one after the other. These types of conditionals are called [Guard Clauses](http://wiki.c2.com/?GuardClause).

For a method that may only have a few conditions to check, guard clauses are completely acceptable. There's no need to over complicate things. However, if you find yourself with a large number of conditions to validate and/or you feel that the conditions will change over time, you may want to consider an alternative approach. Enter the **Rules Pattern**.

The Rules pattern is not a pattern that you'll see in the design patterns book by the [Gang of Four](http://wiki.c2.com/?GangOfFours), but could be considered an implementation of the [Command](https://en.wikipedia.org/wiki/Command_pattern) pattern. 

For the purpose of this blog post, we'll be implementing a set of rules to check if a computer meets all of the minimum requirements to run a given piece of software. Here's how it works.

First create an interface that all of your *rules* will inherit from.

```language-csharp
public interface ISystemRequirementsRule
{
    bool CheckRequirements(Computer computer);
};
```

The interface will only expose one method which will be used to validate the condition for the rule.

You will then need to create all of your rules. As stated previously, each of your rules will inherit from the same interface. Each rule will be one of your guard clauses.

```language-csharp
class DiskSpaceRule : ISystemRequirementsRule
{
    public bool CheckRequirements(Computer computer)
    {
        var ruleResult = computer.DiskSpace > 10;
        return ruleResult;
    }
}
```

This rule validates that the disk space of the computer exceeds 10. Ten what? You can associate whatever unit you want, doesn't matter in this case! Create as many of these rules as required to ensure all of your requirements are checked.

Next, you'll need to create a class whose responsibility is to run through and validate every rule that you have created. There are a couple approaches that I would recommend.

```language-csharp
public class SystemRequirementsChecker
{
    var _rules = new List<ISystemRequirementsRule>();

    public SystemRequirementsChecker()
    {
        _rules.Add(new DiskSpaceRule());
        _rules.Add(new RamRule());
    }

    public decimal CheckSystem(Computer computer)
    {
        foreach (var rule in _rules)
        {
            if(!rule.CheckRequirements(computer))
            {
                return false;
            }        
        }

        return true;
    }
}
```

This approach simply holds all of the available rules in a collection. When you want to validate all of your rules, simply call the ```CheckSystem``` method. This method will then iterate through all of the rules that you have defined in the constructor.

If you want an approach that fully embraced the [Open/Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle) then I would recommend something similar to the following:

```language-csharp
public class SystemRequirementsChecker
{
    private readonly IEnumerable<ISystemRequirementsRule> _rules;

    public SystemRequirementsChecker()
    {
        _rules = GetRules();
    }

    public bool CheckSystem(Computer computer)
    {
        return _rules.All(r => r.CheckRequirements(computer));
    }

    private IEnumerable<ISystemRequirementsRule> GetRules()
    {
        var currentAssembly = GetType().GetTypeInfo().Assembly;
        var requirementRules = currentAssembly
                .DefinedTypes
                .Where(type => type.ImplementedInterfaces.Any(i => i == typeof(ISystemRequirementsRule)))
                .Select(type => (ISystemRequirementsRule)Activator.CreateInstance(type))
                .ToList();

        return requirementRules;
    }
}
```

This approach uses reflection to find all classes that inherit from the ```ISystemRequirementsRule``` interface (all of the rules that you will have written). It then takes all of these rules, instantiates them via ```CreateInstance```, and returns them as a list so that the ```CheckSystem``` can iterate through them and validate each and every rule.

The reflection approach allows you to create a new rule with the expected interface, rebuild the application, and that's it. Your new rule will be enforced inside of the ```SystemRequirementsChecker```. No need to touch any of the pre-existing source code!

The former approach, utilizing a list and instantiating each rule in the constructor would require you to create the class and then modify the ```SystemRequirementsChecker``` to include your new rule before it knew about its existence.

I'd consider either approach acceptable, it depends on your situation.

So there you have it, the **Rules Pattern**. A useful pattern that you typically don't run across when studying design patterns. Hope it helps!
