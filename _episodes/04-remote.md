---
title: "Remote Repositories"
teaching: 0
exercises: 0
questions:
- "How do I connect my code to other versions of the it?"
- "How can I work in remote teams and with remotely hosted code?"
objectives:
- "Learn about remote repositories."
- "Learn to work with multiple remotes"
keypoints:
- "The `git remote` command allows us to create, view and delete connections to other repositories."
- "Remote connections are like bookmarks to other repositories."
- "Other git commands (`git fetch`, `git push`, `git pull`) use these bookmarks to carry out their syncing responsibilities."
- "We've been introduced to remotes and working with multiple remotes"
---

https://www.atlassian.com/git/tutorials/syncing

Git's distributed collaboration model, which gives every developer their own copy of the repository, complete with its own local history and branch structure. Users typically need to share a series of commits rather than a single "changeset". Instead of committing a "changeset" from a working copy to the central repository, Git lets you share entire branches between repositories.

## Git remote

The git remote command lets you create, view, and delete connections to other repositories. Remote connections are more like bookmarks rather than direct links into other repositories. Instead of providing real-time access to another repository, they serve as convenient names that can be used to reference a not-so-convenient URL.

![Remote Schematic](../fig/06-remote.png)

For example, the diagram above shows two remote connections from your repo into the central repo and another developer’s repo. Instead of referencing them by their full URLs, you can pass the origin and john shortcuts to other Git commands.

The `git remote` command is essentially an interface for managing a list of remote entries that are stored in the repository's `./.git/config` file. The following commands are used to view the current state of the remote list.

Git is designed to give each developer an entirely isolated development environment. This means that information is not automatically passed back and forth between repositories. Instead, developers need to manually pull upstream commits into their local repository or manually push their local commits back up to the central repository. The `git remote` command is really just an easier way to pass URLs to these "sharing" commands.

## View Remote Configuration

To list the remote connections of your repository to other repositories you can use the `git remote` command:

~~~
git remote
~~~
{: .language-bash}

If you test this in our training repository, you should get only one connection, `origin`:
~~~
origin
~~~
{: .output}

When you clone a repository with `git clone`, `git` automatically creates a remote connection called `origin` pointing back to the cloned repository. This is useful for developers creating a local copy of a central repository, since it provides an easy way to pull upstream changes or publish local commits. This behaviour is also why most Git-based projects call their central repository origin.

We can ask `git` for a more verbose (`-v`) answer which gives us the URLs for the connections:

~~~
git remote -v
~~~
{: .language-bash}

For our training repository this should return:

~~~
origin	https://github.com/user_name/advanced-git-training.git (fetch)
origin	https://github.com/user_name/advanced-git-training.git (push)
~~~
{: .output}

As expected these point to the original repository we cloned.

## Create and Modify Connections

The `git remote` command also lets you manage connections with other repositories. The following commands will modify the repo's `./.git/config` file. The result of the following commands can also be achieved by directly editing the `./.git/config` file with a text editor.

Create a new connection to a remote repository. After adding a remote, you’ll be able to use `＜name＞` as a convenient shortcut for `＜url＞` in other Git commands.

~~~
git remote add <name> <url>
~~~
{: .language-bash}


Remove the connection:

~~~
git remote rm <name>
~~~
{: .language-bash}

Rename a connection:
~~~
git remote rename <old-name> <new-name>
~~~
{: .language-bash}

To get high-level information about the remote `＜name＞`:
~~~
git show <name>
~~~
{: .language-bash}

Exercise: Add a connection to your neighbour's repository. Having this kind of access to individual developers’ repositories makes it possible to collaborate outside of the central repository. This can be very useful for small teams working on a large project.

~~~
git remote add john http://dev.example.com/john.git
~~~
{: .language-bash}


## Starting a branch from the main repository state:

Remember that when you create a new branch without specifying a starting point, then the starting point will be the current state and branch. In order to avoid confusion, ALWAYS branch from the stable version. Here is how you would branch from your own origin/main branch:

~~~
git fetch origin main
git checkout -b <branch> origin/main
~~~
{: .language-bash}

You must fetch first so that you have the most recent state of the repository.

If there is another "true" version/state of the project, then this connection may be set as upstream (or something else). `Upstream` is a common name for the stable repository, then the sequence will be:

~~~
git fetch upstream main
git checkout -b <branch> upstream/main
~~~
{: .language-bash}

Now we can set the MPIA version of our repository as the upstream for our local copy.

Exercies: set the https://github.com/mpi-astronomy/advanced-git-training as the upstream locally.

~~~
git remote add upstream https://github.com/mpi-astronomy/advanced-git-training.git
git fetch upstream
git checkout -b develop upstream/develop
~~~
{: .language-bash}

Now examine the state of your repository with `git branch`, `git remote -v`, `git remote show upstream`



## Multiple remotes
For this section we'll need some code. We'll use a popular collection of git utility scripts called "gitflow". We've got a copy prepared for the lesson at https://github.com/sa2c/example-gitflow. The first thing we want to do is create a copy of this repository for us to work on. This create a fork by clicking the fork button on the top left of the page.
You'll be redirected after a short wait to your own personal
repository which is a copy of one at `sa2c/example-gitflow`. We will need to
clone the code from your fork.
First we change directory to the desktop, with
~~~
$ cd ~/Desktop
~~~
{: .language-bash}
Next, we find the URL of the forked repository under "Clone or download" on its github.com page. We clone clone using our own personal fork, which should look something like this:
~~~
$ git clone git@github.com:<username>/example-gitflow.git ~/example-gitflow-fork
~~~
{: .language-bash}
Where `<username>` is your github username.
~~~
$ cd example-gitflow-fork
~~~
{: .language-bash}
Let's check the remotes we have with the command
~~~
$ git remote -v
~~~
{: .language-bash}
We see a single remote, named `origin`. This is set up for us by `git
clone` when we create a new repository. It points to the place we
downloaded the code for, in this case this is our fork of the code.

Often, we may want to be able to pull changes directly from the repository we forked. For example if some other developers have made added some commits there.

In order to push to multiple repositories, we need to add them as additional remotes
~~~
$ git remote add upstream git@github.com:sa2c/example-gitflow
~~~
{: .language-bash}
We now see two repositories, `origin` and `upstream`. We can add the
`-vv` and `-a` flags to the `git branch` command to see all branches
~~~
$ git branch -vv -a
~~~
{: .language-bash}
we can now pull from upstream with
~~~
$ git pull upstream master
~~~
{: .language-bash}
This will pull from `upstream/master` into the current branch
(`master`), we can then push any changes we've pulled down to own
repository (`origin`), using.
~~~
$ git push origin master 
~~~
{: .language-bash}
This was not very exciting, because there are no new changes in the
master branch of upstream. But there is in fact a `hello-gitters`
branch which contains a small change based off `master`, which we can
pull instead. Let's first fetch all the latest changes
~~~
$ git fetch -a
~~~
{: .language-bash}
And take a look at `upstream/hello-gitters`
~~~
$ git log --oneline upstream/hello-gitters -5
~~~
{: .language-bash}
This is based off `master`, so we should have no difficulty pulling it into `master`. Let's check what it contains
~~~
$ git diff upstream/hello-gitters master
~~~
{: .language-bash}
Now that we're happy we want to merge it, we can pull with
~~~
$ git pull upstream hello-gitters
~~~
{: .language-bash}
We could also choose to use `git merge upstream/hello-gitters`. We now
push these changes to our repository with
~~~
$ git push
~~~
{: .language-bash}

We can configure as many remotes as we like. If you work closely with friends or colleagues, it could be common for you to want to pull interesting changes from their remotes, incorporate those into your current branches, and push those changes to your remote.

## Checking out remote branches
What about branches other than `master`? Can we check those out and
start work on them. Let's try it
~~~
$ git branch -vv -a
~~~
{: .language-bash}
There's a branch called `develop`. We can check this out in a local branch, with
~~~
$ git checkout --track upstream/develop
~~~
{: .language-bash}
Let's have a look at the result
~~~
$ git branch -vv -a
~~~
{: .language-bash}
We can see that we are now on a local branch `develop`, which is
configured to track the `develop` branch in `upstream`. Running `git
push` and `git pull` in this branch will automatically push to the
upstream branch. We can verify this with
~~~
$ git pull -v
~~~

>## Exercise 1: Create a feature branch
> You will be assigned a number by the instructor/helper. 
> Create a feature branch based on upstream main. Then create a file in the `trainees` folder called `hello_NNN.txt` using the number you just got (replace NNN with your number, e.g. 007).
> Then push your featurebranch out to GitHub.
>
> > ## Solution
> > ~~~
> > git fetch upstream main
> > git checkout -b myforkfeature upstream/main
> > touch ./triainees/hello_NNN.txt
> > git add ./triainees/hello_NNN.txt
> > git commit -m "adding my textfile"
> > git push origin myforkfeature
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

{% include links.md %}
