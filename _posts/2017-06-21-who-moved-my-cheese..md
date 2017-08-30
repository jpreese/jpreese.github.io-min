---
title: Who Moved My Cheese?!
date: 2017-06-21 12:37:57.000000000 -05:00
---
Earlier this morning, I was reminded of an incident that I had with a customer a couple years ago. It went a little something like this:

I was working on a web application. It consisted of a tabular grid that displayed information based on search criteria. Very similar to anything you would see on any sort of e-commerce website that allowed you to filter products by price range, name, etc. The customer wanted to enhance this specific grid. They wanted a new filter that would give them the ability to hide all records that were considered inactive. A fairly reasonable request.

Okay great, requirements established. I began on my merry way. 

Now, this sort of request is relatively trivial in our environment. We need to add a new UI control to the filter section and then wire it up to the backing stored procedure. The whole process probably took around two hours from opening the development environment to pushing it out into our production environment. Once deployed to production, we inform the customer that their feature request is now live, they sign off on the change, and are as happy as could be.

![Alt text](http://i1.kym-cdn.com/photos/images/original/001/243/635/8e0.png)

...I receive a notification that the customer is having difficulty filtering results within the grid, because a filter that used to be there no longer is. 

Now, this struck me as odd, because I actually added a filter. So I opened up the application myself to see if I could validate the customers claim. I was not able to do so. Everything looked exactly as I would expect, and nothing changed from when the customer had previously signed off on it.

In an effort to get the potential miscommunication resolved swiftly, I hopped on a conference call with the customer to see if we could get to the bottom of this. We had already done some back and forth on the ticket itself, so the customer (lets call him Mark) and I had already established a little bit of a rapport. The conversation went something along the lines of:

> <sub>
**John:** Hey Mark, It's John. So show me the screen you're looking at. I'm seeing the filter on me end.  
**Mark:** See here? The filter isn't on the top left.  
**John:** Yeah we added a new filter yesterday, so the filter that you're looking for is right next to it.  
**Mark:** Oh, I see it now. Yeah, that's not going to work. We would have to retrain all of our employees.  
**John:** Why is that?   
**Mark:** Our documentation on how to use this screen in our facility includes screenshots of the filters. It explicitly states which filters to use and where they are on the screen. We would have to print out entirely new documentation for our facility and re-train all of our employees.
</sub>

This immediately got me thinking about a story I had read some time ago called [Who Moved My Cheese?](https://en.wikipedia.org/wiki/Who\_Moved\_My\_Cheese) I'll let you read the Wikipedia entry if you are so inclined, but it essentially speaks to how people have a hard time adjusting to change. Now, for me, this came out of nowhere. Never would I have guessed that the customer had such rigid documentation of how to use our software. 

The software that I develop and maintain changes many times per days. Sometimes we push over a hundred changes to the system in a given day. It's fluid, changing constantly. So to think that a user would assume our layout had no potential for change and would revolve their whole training philosophy around something that had the potential to change is a little boggling. On top of all of this, it was the customer who requested the change, just a different department.

I ended up getting the situation resolved by getting the other department involved and explaining the situation. The change was going to stick, there really wasn't a way around it. 

So here we had a feature request directly from the customer, the change approved in our quality environment, and then verified on our production environment. Only to be followed up with an urgent message from the same customer stating that their workforce was slowed because the training was no longer valid after the change was made.

The whole situation really made me take a step back and realize that you can never be completely confident in any change you make to an existing system.
