---
date:
  created: 2025-01-07
categories:
  - CS
tags:
  - Python
authors:
  - why

---

# Git push
An exmaple of a git push:

<!-- more -->

```py title="git push"
git add.
git status
git branch
git checkout master
git commit -m "commit message"
git push origin master
```
# Git add remote repository
An exmaple of adding remote repository:
```py title="add repository"
git remove origin # remove remote repository
git remote -v # check remote repository
git remote add origin https://github.com/why/why.git # add remote repository
```

# Git cheet sheet
[Git cheet sheet](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)

# Git Synchronize your local repository with the remote repository on GitHub.com
An exmaple of Synchronizing your local repository:
```py title="Synchronizing local repository"
git fetch
#Download all the history of the remote trace branch
git merge
#Merge the remote trace branch into the current local branch
git push
#Upload all local branch commits to GitHub
git pull
#Update your current local working branch with all new commits from the corresponding remote branch of GitHub. is a combination of and git pullgit fetchgit merge
```