---
title: "Identifying breaking commits"
teaching: 30
exercises: 0
questions:
- "How can I use git to track down problems in code?"
objectives:
- "Learn to identify when and in what commit problems were introduced"
keypoints:
- "Learnt to use git blame to identify when a problem line was introduced"
- "Learnt to use binary searches to identify lines which first introduce a problem"

---

## Episode setup
First we need to pull down some code from a remote repository, we will need an example with some broken code 
which can be found in the `broken` branch of our example repository. 
~~~
$ cd ~/Desktop
~~~
{: .language-bash}
and clone the code
~~~
$ git clone git@github.com:NOC-OI/intermediate-git-test-repo.git
~~~
{: .language-bash}
and change into the fresh repository and switch to the `broken` branch.
~~~
$ cd intermediate-git-test-repo
$ git switch broken
~~~
{: .language-bash}
Let's take a look at the contents of this repository
~~~
$ ls
~~~
{: .language-bash}
We see a small number of files; let's have a look inside `plot_bouys.py`.
~~~
$ nano plot.py
~~~
{: .language-bash}
Let's try to run the code
~~~
$ python plot_bouys.py
~~~
{: .language-bash}
This clearly has a problem, as expect. Let's look at the log history to see if we can spot it.
~~~
$ git log --oneline
~~~
{: .language-bash}
If we looked at this for a while, can could probably spot the commit that might be causing the issue, the commit labelled "changing function to plot_data".
In reality however, finding the problem wouldn't be this simple. In general, we might not know what file the problem is in, or where in that file.
We may have hundreds of files with hundreds of lines each, and no idea where to start looking. Let's start by looking at the initial commit.
~~~
$ git checkout 2890
~~~
{: .language-bash}
And see if the `plot_buoys.py` script runs here.
~~~
$ python `plot_buoys.py`
~~~
{: .language-bash}

The file runs with no problems from an earlier commit, somewhere since this commit something went wrong. In this section, we will explore ways in which we can investigate the sources of errors.
Let's move back to the head of the `broken` branch.
~~~
$ git checkout broken
~~~
{: .language-bash}

## `git blame`

If we know where the problem is in the file, we might ask ourselves what (and who) introduced this problem. What commit introduced this line. Let's try this with
~~~
$ git blame plot_buoys.py
~~~
{: .language-bash}
We see that most lines were created in the same two commits, but some were modified in other commits. There are a lot of lines here,
let's focus on the range of lines 57 to 61 (the part not in a function)
~~~
$ git blame -L 57,61 plot.py
~~~
{: .language-bash}
That's better. Let's take a closer look at the commit on line 61.
~~~
$ git show 4445
~~~
{: .language-bash}
That's interesting. We have found a change to that line, but not the one which 
altered the function name. Let's try going back a bit in the history with `git checkout` and do this again.

~~~
$ git checkout HEAD~1
$ git blame -L 57,61 plot.py
~~~
{: .language-bash}

This still hasn't found the commit which renamed the function, let's try going back further.

~~~
$ git checkout HEAD~1
$ git blame -L 57,61 plot.py
~~~

We can see that the problematic line was brought in during commit `eecf`.
Multiple commits after something breaks can make `git blame` a little harder to use.

> ## Challenge: Using git blame across files
> We can ask `git blame` to attempt to track movement between files
> ~~~
> $ git blame -C -L 30,50 hello.sh
> ~~~
> {: .language-bash}
> `git blame` is a very useful tool if you know the line that causes the
> issues in the first place, but you want to look at the commit message
> of that generated the line to check where it came from. Now we can see
> the lines were actually introduced in another commit, let's take a
> look at that commit now
> ~~~
> $ git show 8f67
> ~~~
> {: .language-bash}
{: .challenge}


## Binary searching with Git
We could checkout each commit one at a time, and check each one, but this is very time consuming. We'd have to check out each commit one at a time, like this
~~~
$ git checkout HEAD~7
$ git checkout HEAD~6
$ git checkout HEAD~5
...
$ git checkout HEAD~3
$ git checkout HEAD~2
$ git checkout HEAD~1
~~~
{: .language-bash}
We can do better than this if we choose a half way point between the
bad and good commit, check if that is good or bad, and keep choosing a
half way point until we find the commit that causes the code to go
from good to bad. Git can actually help us do this with the `git
bisect` command. Let's try it, first let's make sure we have reset
`HEAD` to the most recent commit on the `broken` branch.
~~~
$ git checkout broken
$ git bisect start
~~~
{: .language-bash}
We mark the current commit as bad
~~~
$ git bisect bad HEAD
~~~
{: .language-bash}
Then we can mark the commit from the merge as good
~~~
$ git bisect good 116c
~~~
{: .language-bash}
Git will now drop us at a commit half way between the good and the bad commits, which should be commit 2890. We can verify this with
~~~
$ git log --oneline broken
~~~
{: .language-bash}
We see some commits marked as bad and good, and git has placed us in the middle commit. Now we can test this commit
~~~
$ python plot_buoys.py
~~~
{: .language-bash}
It works! The code wasn't broken at this point. Let's mark this commit as good
~~~
$ git bisect good
~~~
{: .language-bash}
Great, git has moved us again. Let's check where we are this time, it should be commit d022
~~~
$ git log --oneline broken
~~~
{: .language-bash}
The markers for good and bad have moved, because we've given bisect more information, and `HEAD` has been placed between them. 
~~~
$ python plot_buoys.py
~~~
{: .language-bash}
This failed, let's mark this as a bad commit
~~~
$ git bisect bad
~~~
{: .language-bash}
We found a bad commit, let's take a look at where we are now:
~~~
$ git log --oneline master
~~~
{: .language-bash}
Git has marked the good and bad commits, but it doesn't know yet if the previous commit might have been the first bad one. It needs us to check that. Let's go ahead and do that
~~~
$ python plot_buoys.py
~~~
{: .language-bash}
This is also a bad commit, let's mark it
~~~
$ git bisect bad
~~~
{: .language-bash}

We've now only got one commit left so Git automatically identifies the commit which broke things as `eecf`. Had we marked our good commit one commit earlier then
we could have used `git bisect good` when we came across the first good commit.

Finally, git has found the commit we were looking for and told us where it is. Let's see where we are
~~~
$ git log --oneline master
~~~
{: .language-bash}
Git has marked the relevant commits as bad, but it hasn't moved us to the first bad commit. It left us in this pending state. Let's take a look at the content of the breaking commit
~~~
$ git show eecf
~~~
{: .language-bash}
Git is telling us that the problem was introduced by a change that
happened on line 55 of `plot_buoys.py` where `plot_buoy_data()` was changed to
`plot_data()`. For us, this was probably a problem that is easy enough to
resolve without using bisect, but for a large complex code base when
we don't know where to start, bisect can instantly point us to the
change which first caused the problem. Let's exit the bisect state and
go back to master with
~~~
$ git bisect reset
~~~
{: .language-bash}
This worked great, and we can go through large numbers of commits with
this technique, but there was a lot of typing. Can Git do a better
job? It turns out that it can. Let's look at the return value from Python
~~~
$ python plot_buoys.py
$ echo $?
~~~
{: .language-bash}
The variable `$?` is a special variable containing the return value of
the function. In this case it is non-zero, indicating an error. Let's
look at the historic commit
~~~
$ git log --oneline
$ git checkout 2890
~~~
{: .language-bash}
And test the code
~~~
$ python plot_buoys.py
$ echo $?
~~~
{: .language-bash}
In this case the script returns 0, indicating success. This is a
common convention in Unix scripts, and you can write your own scripts
that follow this convention. Git can use this convention to decide if
a commit is good or bad. Let's try it
~~~
$ git bisect start HEAD 2890
~~~
{: .language-bash}
Once again, git drops us in the middle of a commit. This time, instead
of running `python plot_buoys.py`, we tell Git to run it for us
~~~
$ git bisect run 'python plot_buoys.py'
~~~
{: .language-bash}
Git does all the boring work for us. Every time it runs the command we gave
and gets a zero return value, it marks the commit as good, every time it sees
a non-zero value, it marks the commit as bad. It then tells us the first commit
if finds which changes the state of the repository from "good" to "bad".
Now that we're done, we exit again with
~~~
$ git bisect reset
~~~
{: .language-bash}

>## One caveat
> This is a very powerful debugging tool, but it relies on all your
> code being in a runnable state, such that Git can automatically
> identify when this state changes. It works best when used with a
> branching and merging strategy, to ensure there are no breaking
> commits on the main branch.
{: .callout}

{% include links.md %}


