---
title: "Forks"
teaching: 5
exercises: 5
questions:
- "What are forks?"
objectives:
- "Understand how forks are created."
keypoints:
- "A fork is a server-side copy of a repository"
- "A fork can be created on Github through the `Fork` button in the top right"
---

A fork of a repository is a new repository which shares code and history with the repository it was forked from - it is a server-side copy (clone) of the original repository. This is particularly useful when you want to work on an open source project where you don't have write permissions to the repository.

## Creating a fork and a local copy of the fork

In order to try out the commands in this lesson we need to set up a repository on GitHub:

- Go to [https://github.com/NOC-OI/intermediate-git-test-repo](https://github.com/NOC-OI/intermediate-git-test-repo)
- Click on the `Fork` button on the top right and follow the instructions. Make sure the 'Copy the main branch only' option is not selected. When this is process is done, you will be directed to your copy of the repository on GitHub.
- Click the green `Code` button. Copy the `SSH` path to the repository to your local machine. Do not download a ZIP file.
- Create a local copy. The command will be similar to this but with your user name:
~~~
git clone git@github.com:<user-name>/intermediate-git-test-repo.git
~~~
{: .language-bash}

>## Exercise 1: Create a fork
> Follow the above instructions to create a fork of the intermediate-git-test-repo and a local copy of it.
{: .challenge}

>## Exercise 2: Download data and ensure code runs
> To run this code we also need some data files. You can download these from [https://noc-oi.github.io/python-intermediate-esces/data/data.zip](https://noc-oi.github.io/python-intermediate-esces/data/data.zip).
> Create a sub directory called `data` in your code repository and extract the zip file into that directory. Ensure the code runs by running it with Python.
>
>> ## Solution
>> ~~~
>> curl -O https://noc-oi.github.io/python-intermediate-esces/data/data.zip
>> mkdir data
>> cd data
>> unzip ../data.zip
>> cd ..
>> python plot_buoys.py
>> ~~~
>> {: .language-bash}
> {: .solution}
{: .challenge}

We'll discuss forking further in the Remotes and Branching Models chapters.

{% include links.md %}
