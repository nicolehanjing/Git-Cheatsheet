# Git: basics, commands & tricks

<br>

## Git basics
The most important function of Git is to allow teams to add (and merge) code at the same time to the same project. There are also many other things Git does really well: it allows us to revert changes, create new branches for adding new features, resolve merge conflict etc.
Basics

<br>

**Git works like this:**
- Git stores project in a repository. 
- Commits are made to the project and they tell Git that you are satisfied with the new or changed code you created. 
- New code/changes are committed on branches. Most of the work is committed on other branches and then merged with master branch. All this is stored in the same directory as the project but in a sub-folder called .git. 
- To share the code with your colleagues you push the changes to the repository, and to get the new code from your colleagues you pull changes from the repository.
- Git is the tool, GitHub/Bitbucket/Gitlab is the service for projects that use Git.

### Two basic rules when using Git:
> Rule 1: Create a Git repository for every new project
>
> Rule 2: Create a new branch for every new feature

<br>
<br>

## My day-to-day workflow with Git
If you don’t already have Git on your computer, you can go to [Git official guide](https://git-scm.com/) and follow the instructions. 
Run git help to print a list of the most common commands.

There are lots of Git commands but you will not use all of them. Here are some common commands that I use pretty much every day:

<br>

### 1. Initial setup
#### Initialize a new Git repository: Git init
Everything you code is tracked in the repository. To create an empty git repo or reinitialize an existing one:
```shell
git init
```

#### Fork a repo
Click the "Fork" button at the top-right of any repository's GitHub page.
#### Clone a repo
Clone the foo repo into a new directory called foo:
```
git clone https://github.com/<username>/foo.git foo
```

#### Setup remotes
First, let's see a list of the repositories (remotes) whose branches you track:
```
git remote -v
```
If you haven't setup upstream. Now is the time:
```
git remote add upstream https://github.com/<upstream_username>/<repo_name>.git
git fetch upstream
```
<br>

### 2. Working with branches
To create a new branch and switch to it at the same time, you can run the command with the -b switch:
```
git checkout -b branch_name
```

This is short for:
```
# create a new branch
git branch branch_name
# switch from one branch to another
git checkout branch_name
```
<br>

#### Git branch
To list all the branches and see on what branch you currently are:
```
git branch
```

<br>

### Git log
This command will list the version history for the current branch:
```
git log
# search in code
git log -S "window.alert"
```

<br>

### 3. Every-day workflow
#### Git add
This command adds one or all changed files to the staging area.
To just add a specific file to staging:
```
git add helloworld.go
```
To stage new, modified or deleted files:
```
git add -A
```
To stage new and modified files:
```
git add .
```
To stage modified and deleted files:
```
git add -u
```
Unstage changes/ restore files
Maybe you accidentally staged some files that you don't want to commit.
```
git restore helloworld.go
git restore .
```
<br>

#### Git commit
This command records the file in the version history. The -m means that a commit message follows. This message is a custom one and you should use it to let your colleagues or yourself sometime in the future know what was added in that commit.
```
git commit -m "your text"
```
To modify the most recent commit message:
```
git commit --amend -m "your text"
```
It lets you combine staged changes with the previous commit instead of creating an entirely new commit. It can also be used to simply edit the previous commit message without changing its snapshot. But, amending does not just alter the most recent commit, it replaces it entirely, meaning the amended commit will be a new entity with its own ref.
To add empty commit (to trigger something maybe):
```
git commit --allow-empty -m “Trigger notification”
```
<br>

#### Undo commits
The following command will undo your most recent commit and put those changes back into staging, so you don't lose any work:
```
git reset --soft HEAD~1
```
The next one will completely delete the commit and throw away any changes:
```
git reset --hard HEAD~1
```
<br>

#### Squash commits
If you have 3 commits, but you haven't pushed anything yet and you want to put everything into one commit so your boss doesn't have to read a bunch of garbage during code review.
```
git rebase -i HEAD~3
```
An interactive text file is displayed. You'll see the word "pick" to the left of each commit. Leave the one at the top alone and replace all the others with "s" for squash, save and close the file. This will display another interactive window where you can update your commit messages into one new commit message. 

<br>

#### Git status
This command will list files in green or red colors. Green files have been added to the stage but not committed yet. Files marked as red are the ones not yet added to the stage.
```
git status
```

Unstaged changes since last commit
```
git diff
```

<br>
<br>

### 4. Push & Pull
#### Git push
This command sends the committed changes to the remote repository:
```
# push a local branch for the first time
git push --set-upstream origin <branch>

# use the -f option flag to force it
git push -f orgin <branch>
```
<br>

#### Fetch changes
Fetch changes from upstream:
```
git fetch upstream
# fetch changes from both origin and upstream in the same shot:
git fetch --multiple origin upstream
``` 
<br>

#### Rebase
I haven't used merge command for a long time. It has created merge bubbles that have overwritten my or other's code. I would prefer to use rebasing.
Rebasing is a way of rewriting history. In place of merge, what this does is stacks your commits on top of commits that are already pushed up. In this case, you want to stack your commits on top of origin/feature_x:
```
git fetch origin
git rebase origin/feature_x
```
If you already have a local branch set to track feature_x then just do:
```
git rebase feature_x
```
Would you like to fetch, merge and then stack your changes on top, all in one shot? You can! If you have tracking setup on the current branch, just do:
```
git pull --rebase
```
Another great use of rebasing is just rewriting commit messages. To get an interactive text editor for the most recent commit, do:
```
git rebase -i HEAD~1
```
Now, you can replace "pick" with "r" and just change the commit message.

<br>

#### Git pull
Pulling is just doing a fetch followed by a merge. If you know your branch is clean (e.g., master branch), go ahead and get the latest changes. There will be no merge conflicts as long as your branch is clean.
```
git pull origin/feature_x
```

<br>
<br>

### 5. Delete branches
#### Delete local branch
```
git branch -d <local_branchname>
```
Use the -D option flag to force it.
#### Delete a remote branch on origin:
```
git push origin --delete <remote_branchname>
```

<br>

## Reference
[Pro git book](https://git-scm.com/book/en/v2)
There is an online book about git, authored by Scott Chacon and Ben Straub, which is, in my opinion, far better than the official git documentation. When I need to refresh or learn some git concept, I always go there by default.