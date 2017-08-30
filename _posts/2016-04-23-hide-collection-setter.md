---
title: Keep Your Collection Setters Private
date: 2016-04-23 09:01:24.000000000 -05:00
---
When exposing properties of a class, you may find yourself immediately exposing a public getter and a public setter.  This is a very common approach, especially in anemic models.  However, this approach can be problematic when applied to collections.  Allowing consumers of your collection to freely modify the entire collection is very destructive. In this post, I will go over two scenarios in which a public setter on your collection can get you into trouble.

#####You cannot iterate over a null collection

Lets compare two approaches to empty a collection.  One approach that we could take is to call the .Clear() method like so:

```language-csharp
myCollection.Clear();
```

Another approach, assuming we had a public setter, could be done by setting the collection to null:

```language-csharp
myCollection = null;
```

This is the first problem area for allowing public setters on your collections.  By allowing your consumers to directly modify the collection, it's no longer possible to control how they will interact with your collection.  Setting the entire collection to ```null``` for example, can potentially cause problems later in your programs life cycle.

Iterating over your collection is one of these pain points.  The following code will behave differently if the collection is set to null, or if it is simply empty.

```language-csharp
for(var item in collection)
{
    // do stuff with collection items
}
```

If the collection was set to null, you will run into a lovely ```NullReferenceException```, like the one below.

*An unhandled exception of type 'System.NullReferenceException' occurred in YourProgramHere.exe*

This is because all collections expose a GetEnumerator() method, which foreach leverages to iterate over your collection.  If the collection is null, its GetEnumerator() method cannot be called.  However, if the collection was simply empty via .Clear(), the run time error would not occur.

#####Events will not be triggered

Another problem that can arise is that it be very hard, if not impossible, to know when your collection has changed. Consider the following ```MyCollection``` class.

```language-csharp
public class MyCollection
{
    public ObservableCollection<int> Numbers { get; set; }

    public MyCollection()
    {
        Numbers = new ObservableCollection<int>();
        Numbers.CollectionChanged += collectionChanged;
    }

    private void collectionChanged(object sender, NotifyCollectionChangedEventArgs args)
    {
        Console.WriteLine("Hey! Listen!");
    }
}
```

This class defines a collection that will output some text when the collection changes. Now while this example only shows one collection, it could be entirely possible that this collection has many subscribers that wish to be notified when its contents change.

Using this collection, the only time the ```collectionChanged``` event will fire is when you leverage its .Add(), .Remove(), etc methods.  Setting the collection to null will indeed empty the collection, but its subscribers will be unaware of anything that has happened.

In summary, when you expose a public setter on a collection, you are exposing much more behavior than you need to.  It gives a lot of unnecessary control to your consumers, which could potentially put your collection into an undesirable state.
