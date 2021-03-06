---
layout: post
comments: true
title: Understanding Git Part 1
description: In this git tutorial, i will compare cli with git gui and explain more about git. Also in this tutorial, I will explain what is git clone, fetch, pull and push.
ad: <iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=12&l=ur1&category=software&banner=0BSSVHTA3XB4Y210FD82&f=ifr&linkID=fd41d430ec3b71cc5b75d15f7227d096&t=petercoding20-20&tracking_id=petercoding20-20" width="300" height="250" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
ids: 1
categories: Git
---

<p class="message"> 
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. Git can be used to track old versions of files and to check all the different changes.
</p>

## Getting Started
---

## Installing Git

First you need to install Git to be able to use it. Since not everyone prefers to use the terminal, then you are able to install a Git Gui and a Git Bash from [git-scm](https://git-scm.com/downloads).
After finishing the installation, if you are using windows operating system then you can right click and click Git Gui to open the graphical user interface.

In the below tutorial, I will give comparison between the Git Gui and Git Bash.
<!-- inside posts -->
<!-- <style>
  .example_responsive { width: 300px; height: 250px; }
</style>
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle example_responsive"
     style="display:block"
     data-ad-client="ca-pub-8689548599050263"
     data-ad-slot="2590272657"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script> -->

## Initializing a Repository and Using it
---

First you need to navigate to the location where you want to create a new repository, then do the following:

## Git Bash:

```bash
cd Desktop/gitExample
git init
```
## Git GUI:

![git gui new repo](/assets/images/gitguinewrepo.jpg)

This will create a new hidden file called `.git` inside the folder `gitExample`, thus making it a local repository with a branch called `master`.

## Adding files to the repository

First create a file inside the folder `newfile.txt` and write some text inside of it. Then you need to add this file to the staging area(index), to be able to commit it later on. You can also use `git status` that will inform you in which branch you currently reside in and if you have any untracked files.

## Git Bash:
```bash
git add newfile.txt #tracks the file and adds it to the staging area
git commit -m "initial commit" #adds tracked files to the local repo
```
**Note:** If you want to add all files in the folder to the staging area then use `git add -A` (the option -A)

So as you can see `git add` adds the files to the staging area or index and then `git commit` adds the files to the local repository.

If you did a mistake you can execute `git reset newfile.txt` and it will remove the staged file from the local repository. If you want to add multiple files then do the following `git add <file-name-1> <file-name-2> <file-name-3>` or if you want to add a file then do `git add 'fileName/'`.

## Git GUI:

![add a new file](/assets/images/gitaddfile.jpg)

Here you need to click on *Stage Changed* which will add the files to the stage area and then write a message and click *Commit*.

## Pushing files to the repository

After you commit the files, you are now ready to push them! You need to push the files to the server so your team members, or the community if you are pushing to GitHub, can access the code.

But, first you need to add the remote repository. Remote refers to any repository that is not in your local machine. Example, if you want to use Github you need to first create an account there and then create a repository to be able to push and pull. After adding a remote, you will be able to push the file to the repository.

## Git Bash
```bash
git remote add origin https://github.com/<userName>/<repoName>.git 
git push -u origin master
```
**Note**: `origin` is an alias to the URL of the remote repository, which means you do not have to write the url everytime you want to do a pull or push operation. Also `master` is your local branch.

If you execute `git branch -a` then you will get the following output:

    master
    remotes/origin/master

`remotes/origin/master` is a remote tracking branch which is located inside `.git/refs`. Its purpose is to keep track of the current state of a remote branch, `git status` will also give you information about the remote tracking branch.

## Git GUI

![git push](/assets/images/gitpush.jpg)

Here you need to click on *Push* and then add the remote url in the *Arbitrary Location*.

## Pull the Changes

Now if any changes were added to the file in the remote repository, then you need to pull first and then push your local changes to the repository. `git pull` does a `git fetch origin master` followed by a `git merge origin/master`. Basically `git fetch` will fetch all the commits from the remote repository but it will not merge them with your local branch. After doing `git fetch` you can execute `git status` to see if your branch is up to date with the remote branch. You can also execute `git log --all` to see the latest commits and the authors or `git show origin/master` to see the latest changes in the commits done.

**Note**:
- `origin`: is the remote branch.
- `master`: is the local branch.
- `origin/master`: is a representation or a pointer to the remote branch, basically it is a local copy of the branch `master` in the remote `origin`.
- `git fetch origin master` : fetches changes from origin to master (local branch) but does not merge, it creates a local copy called `origin/master`
- `git merge origin/master` : merges the changes that were fetched.

## Git Bash
 ```bash 
git pull origin master
```

## Git GUI

![git merge](/assets/images/gitmerge.jpg)

Here you need to first add the  remote url after clicking *Remote/Add*, then click *Fetch From* and *Merge*. Also refer to this answer [pull in git gui](https://stackoverflow.com/questions/22666828/no-pull-in-git-gui).

---

## Cloning a Repository

If there is a repository in github, then you want to use locally in your computer then you need to clone it(copy).

## Git Bash
 ```bash 
git clone https://github.com/<userName>/<repositoryName>
```

Now if you execute `git branch -a`, you will get the following output:

    * master
    remotes/origin/HEAD -> origin/master
    remotes/origin/master

`master` is the current branch you are reside in and it is the local branch, the second line is a symbolic branch referenced by the remote repository. The third line is the remote tracking branch.

## Git GUI

![git clone](/assets/images/gitclone.jpg)

To clone a repository, simply click on *Clone new repository*, the Source location field should contain the remote url and the target directory should contain a folder.

## Merge Conflicts

So, let's say someone in the team added their changes to the staging area, and then commited to their local repository and pushed those changes to the remote repository. But both you and the other developer are working on the same file, and if you have done some changes on the file but didnt push you will get some problems.. so let's see what will happen.

First, you do:

```
git fetch origin master
```

As I said before this will fetch the changes done but it will not merge. Now if you do `git status`, you will get the following:

<img class="lazy-loading" data-sizes="auto" src="/assets/images/gitstatus.png" width="400" data-src="/assets/images/gitstatus.png" alt="git status started" data-srcset="/assets/images/gitstatus.png 300w,
    /assets/images/gitstatus.png 600w,
    /assets/images/gitstatus.png 900w">

So, as you can see I modified the file *understading-git-part1*, but the changes are not staged. But before staging, first we executed `git fetch origin master`, now we have to do `git merge origin/master` to add the changes to our local repository. So if we execute that we get the following:

```
error: Your local changes to the following files would be overwritten by merge:
	_posts/2018-08-19-understanding-git-part1.md
Please commit your changes or stash them before you merge.
Aborting
```
Unfortunately `git` is not able to merge the changes, because I have changed the content of this file and the remote repository contains some changes also.

So, now if I want my changes that are not staged to be in the remote repository and I want to be able to merge the changes in the remote repository to my local repository, I have to do the following:

```bash
git add <file_name>
git commit -m "my changes"
```

Now my changes are inside the local repository, if I execute:

```bash
git merge origin/master ## or git pull origin master
```
I will have a merge conflict, I can fix the conflict by opening an IDE or editor like vscode, merging the changes from the repository to my file, keeping my local changes and then I can do `git push origin master` to send my file with changes to the repository.

------

Second way to solve the following error:

```
error: Your local changes to the following files would be overwritten by merge.
```

is to execute `git reset --hard` and then `git pull origin master`, but note **git reset will override all your local changes**, so basically all your work will be gone, you can do `git reset --hard` when you just want the changes in the repository and don't want your local changes. Or instead of reset you can do `git checkout <file>` which will disregard local changes but before doing that make sure that file is not staged via `git reset HEAD <file>`, this command will remove that file from your staging area.

----- 

Third way is to `stash` your changes, so basically this will save your uncommited changes in a `stash`, removing all your changes from the repository but keeping them hidden. So after you do `git fetch` and `git merge` and get the error, you can then do:
```bash
 git stash
 git pull
 git stash pop
 ```

 After doing `git pull` the changes will be added to your local repository and then when you do `git stash pop`, git will add your saved changes back to the working tree which may lead to a merge conflict, if you click save on vscode, you will get the following:

 <img class="lazy-loading" data-sizes="auto" src="/assets/images/mergeconflict.png" width="400" data-src="/assets/images/mergeconflict.png" alt="git status started" data-srcset="/assets/images/mergeconflict.png 300w,
    /assets/images/mergeconflict.png 600w,
    /assets/images/mergeconflict.png 900w">

Then you have to click *compare*, add the changes, and commit/push. Usually `git stash` is used when you want to hide your changes but don't want to commit, if you actually want to commit then it's better to just use the first way specified in this article. Also in `git stash` you can either `pop` or `apply`. `git stash pop` will throw away the stash after applying it, whereas `git stash apply` leaves it in the stash list for possible later reuse. You can also delete the applied stash later by executing `git stash drop`.



## Recommended Books
----
<a target="_blank"  href="https://www.amazon.com/gp/product/B00DMJQ7IK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00DMJQ7IK&linkCode=as2&tag=petercoding20-20&linkId=fbb131ed8a9871bf9ef06fb59ed90320"><img border="0" src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=B00DMJQ7IK&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=petercoding20-20" ></a><img src="//ir-na.amazon-adsystem.com/e/ir?t=petercoding20-20&l=am2&o=1&a=B00DMJQ7IK" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
<a target="_blank"  href="https://www.amazon.com/gp/product/1520786506/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1520786506&linkCode=as2&tag=petercoding20-20&linkId=f5ee5914da6a3f790a87713b72e2c024"><img border="0" src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=1520786506&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=petercoding20-20" ></a><img src="//ir-na.amazon-adsystem.com/e/ir?t=petercoding20-20&l=am2&o=1&a=1520786506" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

*I hope you enjoyed reading this git tutorial about how to push/pull, please feel free to leave any comments or feedback on this post!!*
