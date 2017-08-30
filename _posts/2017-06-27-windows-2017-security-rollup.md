---
title: Microsoft's June 2017 Security Rollup Woes
date: 2017-06-27 22:02:35.000000000 -05:00
---
Let me set the scene for you.

It's the morning of June 14th, 6:30am to be exact, and we're going to make a standards deployment. You know how I know? Because it's Wednesday. And Wednesday morning is the morning that we usually make standards deployments. Monday is just too early in the week. Tuesday, well, we just don't like Tuesdays, but Wednesday morning we make sweet, standards deployments.

![Business Time](http://reesespieces.io/content/images/2017/07/bus.jpg)

On the docket that morning were some changes that impacted how our dialog boxes behaved as well as **printing** throughout the entire system.

All of the scheduled changes had already gone through internal and automated testing. The necessary test records were applied to the ticket, we were ready to rock and roll.

My team and I pushed the deployment button, and not 15 minutes later, we received a call from one of our customer care representatives that printing was failing for a single customer. Then another, and another. To play it safe, we decided to simply roll back the change and ask questions later. Unfortunately.. it did not fix the problem.

##The Problem

We continue to investigate potential causes and eventually made the realization that the issue only occurred on Windows machines that had consumed the latest Windows updates. Specifically the June 2017 security roll up.

After spinning up a VM with Windows 7 and the latest updates, we're in business. We notice that printing from an iframe results in a blank page. [Fiddler](http://www.telerik.com/fiddler) and the developer tools reveal it's actually an HTTP status code of **404**.

See, when printing an iframe (or from within an iframe) IE apparently takes the contents of the iframe and downloads it locally. This downloaded iframe is then used as the source for the iframe and then it is printed from that copy. However, it appears that this security change was now referencing the iframe on the web server.

For example

```html
<iframe src="myframe.html" />
```

Became

```html
<iframe src="ABC123.html" />
```

Where ABC123.html is just some auto-generated name for the locally downloaded iframe. The problem with this is that obviously ABC123.html does not exist on our web server. So the iframe was loading a page that did not exist, and thus, printing a blank page.

## The Solution
Luckily [the community](https://answers.microsoft.com/en-us/ie/forum/ie11-windows_7/kb4022719-printing-issues/e431c6e1-5f27-4bef-93ce-d8d9ae23a477) banded together. It gave us confidence that this was not a localized issue and that other 3rd parties were experiencing the same pains. Progress!

Everyone in the community was pretty sold on the same workaround. Instead of using `window.print()` use `document.execCommand('print')`.  So we decided to implement this change in the core areas of the system that print from within an iframe (which about covers 99% of the system).

We deploy the change to our quality environment where its tested by our internal teams and customers a like. Everything passes testing, everyone seems pretty happy with the results, and we push the change to our Production environment the following week.

Everything is going smoothly, we're back up and running! Except for one area of the system... our check printing module.

Now, checks unfortunately fell within that 1%. The part of the system that wasn't covered by our change. Which we immediately thought wasn't a big deal, we can just adjust how checks are printed using the same method. How wrong we were..

Checks are a special flower within our system and have their own design. Completely separate from how other areas of the system (reports, labels, etc) print. I won't get into all of the technical details of how they were implemented, but it essentially meant that the only way to resolve the issue was to fix the underlying HTML. 

After a couple days of tweaking HTML and CSS, pixel by pixel. Comparing old checks to the new implementation, everything looked good for launch. The finish line was right in front of us.

##Conclusion

But of course, the morning of the final deployment.. Microsoft released a [hotfix](https://support.microsoft.com/en-us/help/4032782/a-blank-page-or-404-error-prints-when-you-try-to-print-a-frame-in-ie).

It's honestly great news. That's the best outcome we could have hoped for. A timely fix from the original offender. It just could've come a little sooner...

That's the game you have to play though. Software that you depend on breaks, which in turn, breaks your customers. You only have a couple options. You can do nothing and hope that the offender will release a hotfix in a timely manner, or you can make strides to develop a workaround.

We immediately took the latter approach, as it's the safest and quickest way to get our customers back up and running. Who knows when Microsoft would've released a fix. It could have never come for all we know.

As they say.. better late than never.

