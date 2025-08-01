---
title: "Issue Tracking"
teaching: 10
exercises: 10
questions:
- "How do we track issues with code in GitHub?"
objectives:
- "Understand how to track issues with code in GitHub."
keypoints:
- "Github includes an issue tracker for each repository where people can describe and discuss issues with code."
- "Issues can be opened, commented on and closed from the Github web interface."
- "Issues can also be closed in a commit message using 'fixes', 'fixed', 'close', 'closed' or 'closes' followed by a # symbol and the issue number."
---

## Introduction

The strength of online collaboration platforms such as GitHub does not just lie in the ability to share code. 
They also allow us to track problems with that code, 
for multiple developers to work on it independently and bring their changes together and to review those 
changes before they are accepted.

### Tracking issues with code

A key feature of GitHub (as opposed to Git itself) is the issue tracker. 
This provides us with a place to keep track of any problems or bugs in the code and to discuss them
with other developers. 
Sometimes advanced users will also use issue trackers of public projects to report problems they are having 
(and sometimes this is misused by users seeking help using documented features of the program). 

The broken branch of the code from the identifying breaking commits chapter earlier has a bug with a mismatched function name 
in plot_buoys.py.

Let's go ahead and create a new issue in our forked GitHub repository to describe this problem. 
We can find the issue tracker on the "Issues" tab in the top left of the GitHub 
page for the repository. 

> # Issue tracker missing in Github
> Sometimes when forking a Github repository the issue tracker is disabled.
> If you do not see an "Issues" tab in your fork of the repository then
> you can re-enable it by:
>
> * Clicking on the Setting button (cog icon in the middle near the top)
> * Scroll down to the Features section
> * Tick the Issues box
> 
> An Issues tab should now appear on the toolbar near the top of the screen. 
> 
> {: .callout}


Click on this and then click on the green "New Issue" button on the right hand side of the screen. 
We can then enter a title and description of our issue.

A good issue description should include:

 - What the problem is, including any error messages that are displayed.
 - What version of the software it occurred with.
 - Any relevant information about the system running it, for example the operating system being used.
 - Versions of any dependent libraries.
 - How to reproduce it.

After the issue is created it will be assigned a sequential ID number.

>## Write an issue to describe our bug
>
> Create a new issue in your repository's issue tracker by doing the following:
>
> - Go to the GitHub webpage for your code
> - Click on the Issues tab
> - Click on the "New issue" button
> - Enter a title and description for the issue
> - Click the "Submit Issue" button to create the issue.
{: .challenge}

### Discussing an issue

Once the issue is created, further discussion can take place with additional comments. 
These can include code snippets and file attachments such as screenshots or logfiles.
We can also reference other issues by writing a # symbol and the number of the other issue. 
This is sometimes used to identify related issues or if an issue is a duplicate.

### Closing an issue

Once an issue is solved then it can be closed. 
This can be done either by pressing the "Close" button in the GitHub web interface or by making a commit which includes the word
"fixes", "fixed", "close", "closed" or "closes" followed by a # symbol and the issue number.

> ## Challenge: Fix and close an issue
> Fix the issue of the function name mismatch on the broken branch. 
> You could do this either by renaming the function or changing the call.
> Commit your changes and add the appropriate text to your commit message to close the issue.
> Push your changes to your forked repository on Github. Check the issue tracker and ensure it has closed.
>> ## Solution
>> ~~~
>> def plot_buoy_data(figure_name):
>> ~~~
>> {: .language-python}
>> 
>> Becomes
>> ~~~
>> def plot_data(figure_name):
>> ~~~
>> {: .language-python}
>> 
>> or
>> ~~~
>>     plot_data("bouys_plot.png")
>> ~~~
>> {: .language-python}
>> 
>> Becomes
>> ~~~
>>     plot__buoy_data("bouys_plot.png")
>> ~~~
>> {: .language-python}
>> 
>> ~~~
>> git commit -m "Correcting function name mismatch, Fixes #1"
>> git push
>> ~~~
>> {: .language-bash}
>>
> {: .solution}
{: .challenge}

