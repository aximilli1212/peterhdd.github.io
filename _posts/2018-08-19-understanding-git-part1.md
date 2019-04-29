---
layout: post
comments: true
title: Getting Started With Git
description: In this git tutorial, i will compare cli with git gui and explain git, after that you can get started with git. Also check understanding git part 2.
ad: <iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=12&l=ur1&category=software&banner=0BSSVHTA3XB4Y210FD82&f=ifr&linkID=fd41d430ec3b71cc5b75d15f7227d096&t=petercoding20-20&tracking_id=petercoding20-20" width="300" height="250" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
ids: 1
---

<p class="message"> 
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. Git can be used to track old versions of files and to check all the different changes.
</p>

## Getting Started
---

### Installing Git

First you need to install Git to be able to use it. Since not everyone prefers to use the terminal, then you are able to install a Git Gui and a Git Bash from [git-scm](https://git-scm.com/downloads).
 After finishing the installation, if you are using windows operating system then you can right click and click Git Gui to open the graphical user interface.

 In the below tutorial, I will give comparison between the Git Gui and Git Bash.

## Initializing a Repository and Using it
---

First you need to navigate to the location where you want to create a new repository, then do the following:

#### Git Bash:

```bash
cd Desktop/gitExample
git init
```
#### Git GUI:

![git gui new repo](/images/gitguinewrepo.PNG)

This will create a new hidden file called `.git` inside the folder `gitExample`, thus making it a local repository with a branch called `master`.

### Adding files to the repository

First create a file inside the folder `newfile.txt` and write some text inside of it. Then you need to add this file to the staging area, to be able to commit it later on. You can also use `git status` that will inform you in which branch you currently reside in and if you have any untracked files.

#### Git Bash:
```bash
git add newfile.txt #tracks the file and adds it to the staging area
git commit -m "initial commit" #adds tracked files to repo
```
   


If you did a mistake you can execute `git reset newfile.txt` and it will remove the staged file from the local repository. If you want to add multiple files then do the following `git add <file-name-1> <file-name-2> <file-name-3>` or if you want to add a file then do `git add 'fileName/'`.

#### Git GUI:

![add a new file](/images/gitaddfile.PNG)

Here you need to click on *Stage Changed* which will add the files to the stage area and then write a message and click *Commit*.

### Pushing files to the repository

After you commit the files, you are now ready to push them! You need to push the files to the server so your team members, or the community if you are pushing to GitHub, can access the code.

But, first you need to add the remote repository. Remote refers to any repository that is not in your local machine. Example, if you want to use Github you need to first create an account there and then create a repository to be able to push and pull. After adding a remote, you will be able to push the file to the repository.

#### Git Bash
```bash
git remote add origin https://github.com/<userName>/<repoName>.git 
git push -u origin master
```
**Note**: `origin` is an alias to the URL of the remote repository, which means you do not have to write the url everytime you want to do a pull or push operation. Also `master` is your local branch.

If you execute `git branch -a` then you will get the following output:

    master
    remotes/origin/master

`remotes/origin/master` is a remote tracking branch which is located inside `.git/refs`. Its purpose is to keep track of the current state of a remote branch, `git status` will also give you information about the remote tracking branch.

#### Git GUI

![git push](/images/gitpush.PNG)

Here you need to click on *Push* and then add the remote url in the *Arbitrary Location*.

### Pull the Changes

Now if any changes were added to the file in the remote repository, then you need to pull first and then push your local changes to the repository. `git pull` does a `git fetch` followed by a `git merge`.

#### Git Bash
 ```bash 
git pull origin master
```

#### Git GUI

![git merge](/images/gitmerge.PNG)

Here you need to first add the  remote url after clicking *Remote/Add*, then click *Fetch From* and *Merge*. Also refer to this answer [pull in git gui](https://stackoverflow.com/questions/22666828/no-pull-in-git-gui).

---

## Cloning a Repository

If there is a repository in github, then you want to use locally in your computer then you need to clone it(copy).

#### Git Bash
 ```bash 
git clone https://github.com/<userName>/<repositoryName>
```

Now if you execute `git branch -a`, you will get the following output:

    * master
    remotes/origin/HEAD -> origin/master
    remotes/origin/master

`master` is the current branch you are reside in and it is the local branch, the second line is a symbolic branch referenced by the remote repository. The third line is the remote tracking branch.

#### Git GUI

![git clone](/images/gitclone.PNG)

To clone a repository, simply click on *Clone new repository*, the Source location field should contain the remote url and the target directory should contain a folder.

## Recommended Books
----
<a target="_blank"  href="https://www.amazon.com/gp/product/B00DMJQ7IK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00DMJQ7IK&linkCode=as2&tag=petercoding20-20&linkId=fbb131ed8a9871bf9ef06fb59ed90320"><img border="0" src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=B00DMJQ7IK&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=petercoding20-20" ></a><img src="//ir-na.amazon-adsystem.com/e/ir?t=petercoding20-20&l=am2&o=1&a=B00DMJQ7IK" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
<a target="_blank"  href="https://www.amazon.com/gp/product/1520786506/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1520786506&linkCode=as2&tag=petercoding20-20&linkId=f5ee5914da6a3f790a87713b72e2c024"><img border="0" src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=1520786506&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=petercoding20-20" ></a><img src="//ir-na.amazon-adsystem.com/e/ir?t=petercoding20-20&l=am2&o=1&a=1520786506" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

*I hope you enjoyed reading this git tutorial, please feel free to leave any comments or feedback on this post!*