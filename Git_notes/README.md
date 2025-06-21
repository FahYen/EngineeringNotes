# Git cheatsheet

## Table of Contents

 - [Initial setup of Git on a new system](#initial-setup-of-git-on-a-new-system)
 - [Getting started](#getting-started)
 - [Basic Git work flow](#basic-git-work-flow)

## Initial setup of Git on a new system
### Configure user information

```bash
git config --global user.name "[Firstname Lastname]"
git config --global user.email "[Valid-email]"
```

### Setup ssh key, first check if there is an existing ssh key
```bash
ls ~/.ssh
```
### If not, generate key type Ed25519
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
### Print public key and manually copy to your GitHub profile's ssh key
```bash
cat ~/.ssh/id_ed25519.pub
```



## Getting started
### Copy the SSH or HTTPS link and do
```bash
git clone https://github.com/FahYen/EngineeringNotes.git
```

### By default you're on main branch of the repository, you want to start a new branch for your work, do
```bash
git checkout feature_branch
```





## Basic Git work flow

![Basic rebase workflow](Illustrations/rebase.png "Basic rebase workflow")
## 'git add'
Git add specifies what changes are staged for the next commit.
### Add all changes in current working directory except files specified by .gitignore
```bash
git add .
```
### .gitignore example
.gitignore can be in any directory in the repository, each one applies to its directory and all subdirectories below it. The glob pattern assume the working directory to be the location of .gitignore.
```bash
# Ignore everything
*

# But not these files...
!.gitignore
!README.md

# But ./src/ itself
!src/
# And everything in ./src/ recursively
!src/**
```
### git add specifics
```bash
# Add src directory in current working directory
git add src/
# Equivalaent to
git add ./src/
# Add with absolute path
git add /home/user/project/src/file.js
# Add all python files in .src/
git add src/*.py
```
## 'git commit'
Create a "commit" or a snapshot of repository of everything that's staged.
## 'git push'
Apply local commits to the remote repository
```bash
git push
# Equivalent to
git push orgin
# You can also do
git push upstream
# if you have an upstream, if you were forking another repository
```
```bash
git checkout main
git pull
git checkout feature_branch
git rebase main
git checkout main
git merge feature_branch
git push
```