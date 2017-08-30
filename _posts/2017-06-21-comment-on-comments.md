---
title: A Comment on Comments
date: 2017-06-21 13:41:09.000000000 -05:00
---
Full disclosure, I really don't like comments.

Like any feature of a language, I believe they have their purpose. After all, they were put in the language in the first place. I've just seen a lot of incorrect uses of comments, and the more I run into them, the more I feel like they don't add a whole lot of value.

Lets dive into some of the reasons why.

######Comments bloat classes
I see auto generated comments everywhere. They plague a large portion of our code base. They have good intentions, but often end up making the file larger and harder to understand. Especially when the developer pays no mind to the content that was generated.

```language-csharp
public class Cube
{
    /**
    * The height of the cube.
    */
    public Height { get; set; }

    /**
    * The width of the cube.
    */
    public Width { get; set; }

    /**
    * The depth of the cube.
    */
    public Depth { get; set; }
}
```

We've turned a simple model of a cube which could be a few lines long, into a bloated model with more lines of comments than of actual useful code. Everything that the comments are describing are self evident. The comment headers just aren't needed. In all honesty, if I see something similar to the above, I just go ahead and delete the comments.

######Comments are code duplication
One of my favorite light bulb moments. We as developers like to avoid code duplication, reuse is king. If you take a step back and think about comments in this regard, that's exactly what they are.

Code explains what is going to be done. Comments explain what is going to be done. They just do so in different languages. Just like with code duplication, if you change a line of code, you best remember to also change the comment lest you want to be left with:

```language-csharp
/**
* ...
<summary>Returns true when the test succeeds; false otherwise.</summary>
*/
public TestResult RunTest() 
{
```

You can probably guess that this method used to be a ```bool```, and it was decided that the returned result needed to be a little more verbose. So the developer went ahead and created a ```TestResult``` object to store some more information about the result of the test. However, they forgot to update the comment, and now it's out of sync

######Comments can be a sign of a bigger problem
Even if you believe you are using comments as intended, to explain potentially confusing code, a better solution is to just make the code less confusing.

```language-csharp
if(!(x > 10 && y < 5 && z == -1)) // ensure the object is not out of bounds
```

```language-csharp
bool isOutOfBounds = (x > 10 && y < 5 && z == 0)
if(isOutOfBounds) {
```

You could even go a step further and assign each int its own ```const``` variable so the user isn't guessing what exactly 10, 5, and 0 even mean.

Finally.. I've seen my fair share of auto generated comments, and you know what?

######Auto generated comments make me cry

```language-csharp
/**
* Processes the async
*/
public async Task<Result> ProcessAsync()
```

Oh.. I had no idea.

Anyway, after all this comment bashing, lets talk about a couple cases that I believe to be a strong candidates for comment use.

*Public APIs* are a great use of comments. You are going to have a large body of people consuming your code, calling your methods. It can be greatly helpful if everything is well documented, explaining exactly how everything is supposed to work.

Another valid use for comments is explaining *why* code is present. I like to think that code explains *how* it works, where comments explain why it works. Code can only make so much sense, regardless of how you name your variables, how well you format it, etc. Sometimes, there exists a scenario that just needs a little more explanation.

```language-csharp
/* Reflection may seem like an odd choice here, but after testing the use case
* scenarios it proved to be much faster than splitting everything up into database
* calls */
```

Like I said in the beginning. Comments were put into languages for a reason, they have their place. Please, just use them sparingly.
