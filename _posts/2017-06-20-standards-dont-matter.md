---
title: Coding Standards Should Not Matter
date: 2017-06-20 18:08:56.000000000 -05:00
---
Yeah that's right. That behemoth of a document that you call your standards document just does not matter. Now that's a pretty bold statement, so let me explain.

I believe that a standards document should be about how to style code. It should be about where to place curly braces, it should answer the great spaces vs. tabs debate (even though there is obviously only ever one clear choice), and similar concerns. What a standards document should not contain is substance, best practices. So what is a standard? A best practice?

It's very likely that the definition of a standard or a best practice vary from individual to individual, so to get everyone on the same page, these are how I define standards and best practices.

* **Standard:** A rule put in place by an authority for the purpose of consistency.  
* **Best Practice:** A proven approach, generally the **best** way to solve a problem.

The key would in the standards definition is *consistency*. It should not matter at all how the code is formatted as long as the team agrees to an approach and follows it. Ensuring that the coding style of the repository remains consistent.

Consistency is extremely important. Code should look like a team wrote it, and not any one individual. Everyone on the team is going to be sharing the same code base and the style should not vary from developer to developer. You definitely do not want developers as their first order of business when starting a new task to be to reformat the code to their own personal preference.

A best practice is more along the lines of do not wrap your entire class in a try-catch and swallow the exception to avoid generating errors or [keep your collection setters private](/hide-collection-setter/). While as important as a standard, if not more so, these types of statements should not exist within a standards document.

So why shouldn't a standards document contain both?

First and foremost, they are different concerns. We generally like to split up concerns into easily digestible bits and not combine them.  It is not going to matter all that much if your team puts curly braces on the same lines as your conditionals. It is going to matter if you start piling all of your concerns into a single class.

However, from my experience, a standards document is rarely *just* standards. A standards document will generally combine standards and best practices. 

Though I do not believe the solution is to split up these two concerns into different documents. No, I believe a standards **document** should not exist. Emphasis on document. We should have standards. Unfortunately the medium we choose to present our standards is generally the incorrect one. 

So whats the better approach? Let your code be your standards document. 

I believe that coding standards are better enforced through tooling and the code base itself. Tools such as [ReSharper](https://www.jetbrains.com/resharper/) and [StyleCop](https://stylecop.codeplex.com/) allow teams to define their standards and then have them be automatically enforced through the development environment. No need to spend time writing out paragraphs in English, comparing and contrasting different styling approaches (Do vs. Don't). No need to worry about document formatting. Simply define your rules and push it to the teams who you intend to consume the styling that was agreed upon.

Like the old saying goes, the best standards document is no standards document at all.
