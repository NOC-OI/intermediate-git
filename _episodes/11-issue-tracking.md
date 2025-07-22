---
title: "Large Files"
teaching: 0
exercises: 0
questions:
- "How do we track issues with code in GitHub?"
objectives:
- "Understand how to track issues with code in GitHub."
keypoints:

---

Sharing code openly promotes collaboration, transparency, and innovation by allowing others to review, use, and 
improve the code. 
It fosters knowledge exchange, accelerates scientific progress, and enhances the reproducibility of research. 
Additionally, open sharing encourages community contributions and can lead to better-maintained, 
more reliable software.

Adding a [license](../learners/licensing.md) and other metadata to our code (covered in the previous episode) are the first steps towards 
sharing the code publicly.
There are several other important steps to consider which we will cover here.


## Sharing code to encourage collaboration

### Making the code public

By default repositories created on GitHub are private and only their creator can see them. 
Since we added an open source license to our repository we probably want to make sure people can actually 
access it. 

To make your repository public, if it is not already, go to your repository on GitHub and click on 
the `Settings` link near the top right corner. 
Then scroll down to the bottom of the page and the "Danger Zone" settings. Click on "Change Visibility" and you 
should see a message saying "Change to public".
If it says "Change to private" then the repository is already public. 
You will then be asked to confirm that you indeed want to make the repository public and agree 
to the warning that the code will now be publicly visible. 
As a security measure, you will then have to put in your GitHub password.

### Transferring to an organisation

Currently our repository is under the GitHub "namespace" of our individual user. 
This is OK for individual projects where we are the sole or at least the main code author,
but for bigger and more complex projects it is common to use a GitHub organisation named after our project. 
If we are a member of an organisation and have the appropriate
permissions then we can transfer a repository from our personal namespace to the organisation's. 
This can be done with another option in the "Danger Zone" settings, the
"Transfer ownership" button. 
Pressing this will then prompt us as to which organisation we want to transfer the repository to. 


## Working with collaborators

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

The code from the testing chapter earlier has a bug with an extra bracket in eva_data_analysis.py (and if you have fixed that a missing import of summarise_categorical in the test).
Let's go ahead and create a new issue in our GitHub repository to describe this problem. 
We can find the issue tracker on the "Issues" tab in the top left of the GitHub 
page for the repository. 
Click on this and then click on the green "New Issue" button on the right hand side of the screen. 
We can then enter a title and description of our issue.

A good issue description should include:

 - What the problem is, including any error messages that are displayed.
 - What version of the software it occurred with.
 - Any relevant information about the system running it, for example the operating system being used.
 - Versions of any dependent libraries.
 - How to reproduce it.

After the issue is created it will be assigned a sequential ID number.

:::  challenge

### Write an issue to describe our bug

Create a new issue in your repository's issue tracker by doing the following:

 - Go to the GitHub webpage for your code
 - Click on the Issues tab
 - Click on the "New issue" button
 - Enter a title and description for the issue
 - Click the "Submit Issue" button to create the issue.
:::

### Discussing an issue

Once the issue is created, further discussion can take place with additional comments. 
These can include code snippets and file attachments such as screenshots or logfiles.
We can also reference other issues by writing a # symbol and the number of the other issue. 
This is sometimes used to identify related issues or if an issue is a duplicate.

### Closing an issue

Once an issue is solved then it can be closed. 
This can be done either by pressing the "Close" button in the GitHub web interface or by making a commit which includes the word
"fixes", "fixed", "close", "closed" or "closes" followed by a # symbol and the issue number.
