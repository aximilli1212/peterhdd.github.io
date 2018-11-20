---
layout: post
comments: true
title: Understanding Git Part 2
---

<p class="message"> 
Extra information about Git, that may help you in your daily life.
</p>

## Extras

| Command       | Description         | 
| :------------- |-------------| 
| `git branch nameOfBranch`    | creates another branch | 
| `git checkout nameOfBranch`    | switches to the other branch     | 
| `git branch` | returns the current branch you are in   |
| `git checkout master` | switches to master branch |
| `git merge nameOfBranch`   | merges the changes from a branch to master | 
| `git branch -d nameOfBranch`    | delete the branch     | 
| `git push` | pushes all the changes to github   |
| `git --help` | help page |
| `git help config` | help about config command   |
| `git config --global user.name "Name"` | configure git, it adds your name and email to the config file|
| `git show HEAD` | to see the most recent commit |
| `git remote update` | Brings remote ref up to date |
| `git status -uno` | Tells you if the branch you are tracking is ahead, behind or has diverged |
| `git diff` | It shows the difference between the index(staging area) and the working directory after you edit |
| `git rebase` | Rewinds the commits on your current branch, pulls in the commits from the other branch, and reapplies the rewinded commits back on top.[git rebase](https://jeffkreeftmeijer.com/git-rebase/) |

* `.gitignore` tells git which files (or patterns) it should ignore, it's usually used to avoid committing transient files from your working directory that aren't useful to other collaborators, such as compilation products, temporary files IDEs create, etc.

* `FETCH_HEAD` is a short-lived ref, to keep track of what has just been fetched from the remote repository.

* In Git a branch is just a label for a commit.

* Fast-forward merge is when the remote branch includes some changes and it is then merged with the local branch.

* Merge commit occurs when both remote and local branches have some changes and you execute git pull command, a new commit will be created with an auto generated commit message indicating that it was merged. A merge conflict can occur when there are changes on the same file and a git pull is executed.

* Git searches for the email and password inside the `config` file in repository or inside the `gitconfig` file in the home directory. To set the username and email in the `gitconfig` file then use the `--global` option.

* If you want to contribute in open source project:
1. First you need to fork the project 
2. Do some changes
3. Do a pull request (compares branches), the repository owner will accept the changes if they are correct and merges.

![graph](/images/graph.png)