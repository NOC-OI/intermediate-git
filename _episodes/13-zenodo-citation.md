---
title: "Publishing and Citing Code"
teaching: 0
exercises: 0
questions:
- "How do I ensure my code is citable?"
objectives:
- "Understand how to archive code to Zenodo and create a digital object identifier (DOI) for a software project (and include that info in CITATION.cff)."
keypoints:

---

Sharing code openly promotes collaboration, transparency, and innovation by allowing others to review, use, and 
improve the code. 
It fosters knowledge exchange, accelerates scientific progress, and enhances the reproducibility of research. 
Additionally, open sharing encourages community contributions and can lead to better-maintained, 
more reliable software.

Adding a license and other metadata to our code (covered in the previous episode) are the first steps towards 
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

### Archiving code to Zenodo and obtaining a DOI

[Zenodo](https://zenodo.org/) is a data archive run by CERN. 
Anybody can upload datasets up to 50GB to it and receive a Digital Object Identifier (DOI). 
Zenodo's definition of a dataset is quite broad and can include code - which gives us a way to obtain a DOI for our 
software. 

Let us now look into how we can archive a GitHub repository to Zenodo. 
Note that, instead of using the real Zenodo website, we will practice with [Zenodo Sandbox](https://sandbox.zenodo.org/).

> ### Zenodo Sandbox
> [Zenodo Sandbox](https://sandbox.zenodo.org/) is a testing environment for Zenodo, a repository for research outputs, 
> allowing users to safely experiment with its features without affecting the live system.
> It is a clone of Zenodo, created for testing purposes, that works exactly the same way as Zenodo you can use it 
> for learning, training, experimenting, and preparing uploads without impacting the primary Zenodo repository until
> you are ready to publish and release your code (or other research outputs) officially.
> It will also not create real DOIs for a number of test repositories we use for this course and saturate the DOI space
> (remember that a DOI, once created, is meant to exist forever).
{: .callout}

We can archive our GitHub repository to Zenodo (Sandbox) by doing the following:

 1. Go to the [Zenodo Sandbox login page](https://sandbox.zenodo.org/login) and choose to login with GitHub.
 2. Authorise Zenodo Sandbox to connect to GitHub. 
 3. Go to the [GitHub page](https://sandbox.zenodo.org/account/settings/github/) in your Zenodo Sandbox account. 
This can be found in the pull down menu with your user name in the top right corner of the screen.
 4. You will now have a list of all of your GitHub repositories. Next to each will be an "On" button. 
If you have created a new repository you might need to press the "Sync" button to update the list of repositories 
Zenodo Sandbox knows about.
 5. Press the "On" button for the repository you want to archive. If this was successful you will be told to refresh the page.
 6. The repository should now appear in the list of "Enabled" repositories at the top of the screen, but
it does not yet have a DOI. To get one we have to make a "release" on GitHub. Click on the repository and 
then press the green button to create a release. 
This will take you to GitHub's release page where you will be asked to give a title and description of the release. 
You will also have to create a "tag" for your release - a way of having a friendly name for the version of some code in 
Git instead of using a long hash code. 
Often we will create a sequential version number for each release of the software and have the tag name match this, 
for example v1.0 or just 1.0.
 7. If we now refresh the Zenodo Sandbox page for this repository we will see that it has been assigned a DOI.

The DOI does not just link to GitHub, Zenodo will have taken a copy (a snapshot) of our repository at the point 
where we tagged the release. 
This means that even if we delete it from GitHub or even if GitHub were ever to go away or remove it, 
there will still be a copy on Zenodo. 
Zenodo will allow people to download the entire repository (more accurately, its state at the time it was tagged for release) as a single `zip` file. 

Zenodo will have actually created two DOIs for you. One represents the latest version of the software and 
will always represent the latest if you make more releases. 
The other is specific to the release you made and will always point to that version. 
We can see both of these by clicking on the DOI link in the Zenodo page for the repository.

One of the things which is displayed on this page is a badge image that you can copy the link for and add to the 
README file in your GitHub repository so that people can find the Zenodo version of the repository. 
If you click on the DOI image in the Details section of the Zenodo page then you will be shown instructions for 
obtaining a link to the DOI badge in various formats including Markdown.
Here is the badge for this repository and the corresponding Markdown: 

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.11869450.svg)](https://doi.org/10.5281/zenodo.11869450)

```text
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.11869450.svg)](https://doi.org/10.5281/zenodo.11869450)
```

> ### Archive your repository to Zenodo (Sandbox)
> Note: for this exercise, as demonstrated earlier, you should use the [Sandbox Zenodo](https://sandbox.zenodo.org/) (a version of 
> Zenodo for testing and playing with before minting a real DOI).
> For real software releases, you should use Zenodo.
>
> * Create an account on Zenodo Sandbox that is linked to your GitHub account.
> * Use Zenodo Sandbox to create a release for your repository and obtain a DOI for it.
> * Get the link to the DOI badge for your repository and add a link to this image to your README file in 
> Markdown format. Check that this is the DOI for the latest version and not the DOI for a specific version, 
> if not you will be updating this every time you make a release.
{: .challenge}

> ## Problems with GitHub and Zenodo integration
> The integration between GitHub and Zenodo does not interact well with some browser privacy features and extensions. 
> Firefox can be particularly problematic with this and 
> might open new tabs to login to GitHub and then give an error saying: `Your browser did something unexpected. 
> Please try again. If the error continues, try disabling all browser extensions.`
> If this happens try disabling the extra privacy features/extensions or using another browser such as Chrome.
{: .callout}


### Adding a DOI and ORCID to the citation file

Now that we have our DOI it is good practice to include this information
in our citation file. 
Earlier we created a `CITATION.cff` file with information about how to cite our code.
There are a few fields we can add now which are related to the DOI; one of these is the `version` file which covers 
the version number of the software.
We can add a DOI to the file in the `identifiers` section with a type of `doi` and `value` of the Zenodo URL.
Optionally we can also add a `date-released` field indicating the date we released this software.
Here is an updated version of our `CITATION.cff` from the previous episode with a version number, DOI and release date added.

```yaml
# This CITATION.cff file was generated with cffinit.
# Visit https://bit.ly/cffinit to generate yours today!

cff-version: 1.2.0
title: Spacewalks
message: >-
  If you use this software, please cite it using the
  metadata from this file.
type: software
authors:
  - given-names: Jaffa
    name-particle: Sarah
  - given-names: Aleksandra
    family-names: Nenadic
  - given-names: Kamilla
    family-names: Kopec-Harding
repository-code: >-
  https://github.com/YOUR-REPOSITORY-URL/spacewalks.git
abstract: >-
  A Python script to analyse NASA extravehicular activity
  data
keywords:
  - NASA
  - Extravehicular activity
version: 1.0.1
identifiers:
  - type: doi
    value: 10.5281/zenodo.1234
date-released: 2024-06-01
```

:::  challenge

### Add a DOI to your citation file

Add the DOI you were allocated in the previous exercise to your `CITATION.cff` file and then commit and 
push the updated version to your GitHub repository. 
If you used the `commit` field in your `CITATION.cff` file before to point to a given version of the code - you can 
now remove it as using the DOI field is better for this job.

:::::::::::::::::::::::::::::::::::::::::::::::


::: callout

### Going further with publishing code

We now have our code published online, licensed as open source, archived with Zenodo, accessible via a DOI and with a citation file to encourage people to cite it. 
What else might we want to do in order to improve how findable, accessible or reusable it is?
One further step we could take is to publish the code with a peer reviewed journal. Some traditional journals will accept software submissions, although these are usually
as a supplementary material for a paper. There also journals which specialise in research software such as the [Journal of Open Research Software](https://openresearchsoftware.metajnl.com/),
[The Journal of Open Source Software](https://joss.theoj.org/) or [SoftwareX](https://www.sciencedirect.com/journal/softwarex). With these venues, the submission will be the software
itself and not a paper, although a short abstract or description of the software is often required.
:::
