# Git Cheat Sheet

## Table of Contents

 - [Initial setup of Git on a new system](#initial-setup-of-git-on-a-new-system)
 - [Getting started](#getting-started)
 - [Using multiple accounts on same computer](#using-multiple-accounts-on-same-computer)
 - [Basic Git work flow](#basic-git-work-flow)
   - [Add changes to staging area](#add-changes-to-staging-area)
   - [Committing staged changes to local repository](#committing-staged-changes-to-local-repository)
   - [Synchronizing local and remote repository](#synchronizing-local-and-remote-repository)
 - [Fast-forward merge](#fast-forward-merge)
 - [Three-way merge](#three-way-merge)
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
git clone https://github.com/FahYen/repo.git
```
 - A new folder called `repo` is made
 - It is initialized as a Git repository
 - A remote named `origin` is created, pointing to the URL you cloned from
 - All of the repository's files and commits are downloaded there
 - The default branch is checked out

### By default you're on main branch of the repository, you want to start a new branch for your work, do
```bash
git branch feature_branch   # To create new local branch
git checkout feature_branch # To switch branch
```



## Using multiple accounts on same computer
### Assuming you already set up SSH key for first GitHub account, create a new SSH key for different account
```bash
ssh-keygen -t rsa -b 4096 -C "enter-your-email-address-linked-with-Github-account-here"
```
### The output will look like this:
```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/home/kallen/.ssh/id_rsa):
```
### Instead of saving to `id_rsa` (default personal account) change file name.
### For example, to change to name it in file `id_rsa_kallen`:
```bash
Enter file in which to save the key (/home/kallen/.ssh/id_rsa): /home/kallen/.ssh/id_rsa_kallen
```
### Afterwards you get the typical prompt to enter a passphrase.
```bash
Enter passphrase (empty for no passphrase)
```
### You will be prompted to enter the passphrase again.
```bash
Enter same passphrase again (empty for no passphrase):
```
### Now two files (public and private keys) have been created in the location you specified. Output public key with
```bash
cat .ssh/id_rsa_kallen.pub
```
### Manually copy the output and add the key in your desired GitHub account. Go to profile picture >> Settings >> SSH and GPG keys
### Add key to SSH
```bash
$ ssh-add ~/.ssh/id_rsa_kallen
Enter passphrase for /home/kallen/.ssh/id_rsa_kallen:
Identity added: /home/kallen/.ssh/id_rsa_kallen (kallen@gmail.com)
```
### If successfully added, a line stating Identity Added will appear
### To specify which account you want to push from, create a config file in .ssh folder
```bash
touch ~/.ssh/config
vim config
```
### Paste the following:
```bash
# Default GitHub
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa
```
### This is the default for pushing to the primary GitHub account. Copy and modify this default snippet so you have changed the Host and IdentityFile
### For example, Host is changed to github-kallen.com and IdentityFile is ~/.ssh/id\_rsa\_kallen:
```bash
Host github-kallen.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_kallen
```
### When you want to push from this account make sure git remote is set with this Host
### For example, setting origin to FahYen/EngineeringNotes.git with previous config identity for kallen:
```bash
git remote add origin git@github-kallen.com:FahYen/EngineeringNotes.git
```
### If you have configured git to use a global email and user name, set the email for this repository so the correct acount shows
```bash
git config user.email "set-desired-GitHub-email-here"
```
### Now when you push to this repository, the correct account will display



## Basic Git Work Flow
![Basic rebase workflow](Illustrations/rebase.png "Basic rebase workflow")
### Add changes to staging area
Git add specifies what changes are staged for the next commit.
#### Add all changes in current working directory except files specified by .gitignore
```bash
git add .
```
#### .gitignore example
.gitignore can be in any directory in the repository, each one applies to its directory and all subdirectories below it. The glob pattern take the working directory as the location of .gitignore.
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
#### git add specifics
```bash
# Add src directory in current working directory
git add src/
# Equivalaent to
git add ./src/
# Add with absolute path, file location must be within repository
git add /home/user/project/src/file.js
# Add all python files in .src/
git add src/*.py
```



### Committing Staged Changes to Local Repository
Create a "commit" or a snapshot of repository of everything that's staged.
```bash
git commit -m "note-about-your-changes"
```



### Synchronizing Local and Remote Repository
```bash
git push REMOTE-NAME BRANCH-NAME
```
- By default, origin is the remote-name of the URL you cloned.
#### To add a remote repository to current branch
```bash
git remote add REMOTE-NAME REMOTE-REPO-URL
```
- A convention is to name the original repository `upstream` and your remote fork repository `origin`
- `git remote` only manages remote repository connections, not branches within the remote repositories

#### To create a branch in the remote repository, pushes your local branch to it, AND tracks it with current local branch
```bash
git push --set-upstream REMOTE-NAME REMOTE-BRANCH-NAME # Can be different names
```
- Tracking means, `git push` and `git pull` will default to the tracking branch without need for specification.

#### Resolve non-fast-foward errors
This happens when remote repository branch diverged
```bash
# To update local compressed information of a remote branch for merging
git fetch origin/main
# Three way merge
git merge origin/main
# Push merged local branch to remote branch
git push
```



## Fast-Forward Merge
### Fast-Forward Merge without divergence, say you have


	      A---B---C feature_branch
	     /
    D---E main

### Then you can just run
```bash
git checkout main
git merge feature_branch
```
### Then the graph becomes

          A---B---C feature_branch & main
	     /        
    D---E
### Fast-Forward Merge with divergence, say you have

          A---B---C feature_branch
	     /
    D---E---F---G main

### To achieve Fast-Forware Merge, singular path of commit history
```bash
git checkout feature_branch
git rebase main
```
### `git rebase` reapplies the commtis on top of another base tip, G
                  A---B---C feature_branch
	             /
    D---E---F---G main
### Then you can apply [Fast-Forware Merge without divergence](#fast-forward-merge-with-divergence-say-you-have) as usual



## Three-Way Merge
### Say you have following history

	       A---B---C feature_branch
	     /
    D---E---F---G main
### Three-Way Merge takes two branch tips, C and G, and a common base commit, E, and create a new merge commit that combines all three commits.
```bash
git checkout main
git merge feature_branch
```
	      A---B---C feature_branch
	     /         \
    D---E---F---G---H main
## When to Choose Fast-Forward Merge vs Three-Way Merge

### Fast-Forward Merge
- When the feature branch is directly ahead of the main branch with no divergent commits.
- When you prefer a clean, linear commit history without merge commits.
- Ideal for small, short-lived branches or single-topic changes.

**Benefits:**
- Simpler history and easier to follow a single line of development.

**Drawbacks:**
- Loses explicit record of the branch integration point.
- Harder to trace feature context if many changes are merged this way.

### Three-Way Merge
- When branches have diverged and both contain unique commits.
- When you want to preserve the context and history of a feature branch.
- Ideal for long-lived or collaborative branches and complex integrations.

**Benefits:**
- Clearly records the merge commit and context of when and how branches were integrated.
- Easier to revert or isolate feature work grouped under a merge commit.

**Drawbacks:**
- Introduces extra merge commits, making the commit graph non-linear.
- Can clutter history with many merge commits if overused.