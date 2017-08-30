---
title: IIS Debugging Roulette
date: 2016-04-13 20:40:23.000000000 -05:00
---
It's a typical day in the office. You've spent your day writing top notch unit tests and elegant code. You sit back, marvel at what you have accomplished and decide, being that good developer that you are, that maybe you should do some functional testing.

You navigate to the Debug pane, click your trusty "Attach to Process" and this familiar window pops up:

![](/content/images/2016/04/process-1.png)

"Here we go again" you say to yourself, dreading what is to come next. You click on the first w3wp.exe process and attach. Nope.. doesn't look like that worked. You repeat this until you finally find the correct process to attach to so you can test your code.  You hate having to do this every time you want to debug, but you accept it as a necessary evil of debugging an IIS process. It's just how it has to be done, right?

####IIS Manager to the Rescue
You may be relieved to hear that this isn't how it has to be, there is hope! If you navigate your IIS Manager, you should see an option called **Worker Processes** underneath the **IIS** section.

![](/content/images/2016/04/worker.png)

After choosing this option, you will be presented with the Worker Process feature, which describes all of the w3wp.exe processes in greater detail than the debugger window.  It is here that you can confidently link the application pool that you're trying to attach to with the correct process identifier. No more gambling!
