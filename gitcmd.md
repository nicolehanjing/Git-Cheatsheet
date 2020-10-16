# Git: basics, commands & tricks

<br>

## Git basics
The most important function of Git is to allow teams to add (and merge) code at the same time to the same project. There are also many other things Git does really well: it allows us to revert changes, create new branches for adding new features, resolve merge conflict etc.
Basics

<br>

Git works like this: 
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
#### 2. Working with branches
To create a new branch and switch to it at the same time, you can run the command with the -b switch:
```
git checkout -b branch_name
```
This is short for:
```
// create a new branch
git branch branch_name

// switch from one branch to another
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

// search in code
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
// push a local branch for the first time
git push --set-upstream origin <branch>

// use the -f option flag to force it
git push -f orgin <branch>
```
<br>
#### Fetch changes
Fetch changes from upstream:
```
git fetch upstream
// fetch changes from both origin and upstream in the same shot:
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
<br>

## Tips & Tricks
Here I post some of my favorite git commands and tricks that I use almost daily. I believe most of them are well known, but if you happen to not know to any of those, they are really cool and can help to make your revision-control experience more useful and powerful.
### 1. Throw away all you uncommitted changes
Just as it says, this command will throw away all your uncommitted changes:
```
git reset --hard HEAD
```
<br>
### 2. Remove a file from git without removing it from the computer
Sometimes using “git add” command you might end up adding files that you didn’t want to add.
If you are not careful during a “git add”, you may end up adding files that you didn’t want to commit. You should remove the staged version of the file, and then add the file to .gitignore to avoid making the same mistake a second time:
```
git reset file_name
echo filename >> .gitignore
```
<br>
### 3. Format all the commits
Formating all the commits on just one line is really nice and usually you will prefer this than the longer format:
```
git log --oneline
```
git log -n (n being a number) will display that exact number of commits. I do this a lot. I don’t know why. 
Maybe is not that useful after all, but I like to keep the number small since I usually only check the last two or three commits.
```
git log n
```

<br>
### 4. Cherry-pick a commit to your current branch
Cherry pick is one of the coolest weapons you have when you mess up things and start adding commits to the wrong branch. It applies the single changes from a commit to your current branch.
So let's say you are working on branch B and then you checkout to branch A to fix something. You add a commit to branch A and push. Then you forget to checkout to the previous branch B and add another commit for branch B.
Now you should do git log and copy the commit hash "b3243adsa8". You checkout to bran B and do:
```
git cherry-pick b3243adsa8 
```
And boom: now your branch gets that commit. Copy/paste at is finest.

<br>
### 5. Checkout a single file from another branch
If you destroy a file and just wish you could have a fresh start, or need the changes you made in one file in another branch, this command lets you grab just one file from another branch:
```
git checkout some-other-branch -- hello.go
```
You can use the same trick to checkout one file from a specific commit:
```
git checkout 9123284712 -- hello.go
```
This is an effective trick if cherry-pick would pick up other files that you don't need.

<br>
### 6. Get rid of all untracked changes
If you create a new file that didn't previously exist in the git history, you've made an untracked change. To start tracking that file, you'd need to commit it to the repo
Sometimes, you change your mind halfway through a commit and really just want to start over without all the changes you've got. Well, git checkout . will get rid of all the tracked changes you've made, but your untracked changes will still be floating around. To remedy that, we've got git clean.
```
git clean -f -d
```
<br>
### 7. Your ~/.gitconfig file
The first time you tried to use the git command to commit a change to a repository, you might have been greeted with something like this:
```
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
to set your account's default identity.
```
What you might not have realized is that those commands are modifying the contents of ~/.gitconfig, which is where Git stores global configuration options. There are a vast array of things you can do via your ~/.gitconfig file, including defining aliases, turning particular command options on (or off!) on a permanent basis, and modifying aspects of how Git works.
Basically you just need to add lines to ~/.gitconfig:
```
[alias]
    st = status
    ci = commit -v
```
Or you can use the git config alias command:
```
git config --global alias.st status 
```

<br>
### 8. See staged differences
The git diff command shows you the difference between the last commit and the state of the current working directory. That's really useful and you might not use it as much as you should. The --cached option shows you just the differences that you've staged.
This provides a way to preview your own patch, to make sure everything is in order. Crazy useful. See below for the example:
```
git diff --cached
```

<br>
### 9. stash your changes and apply back
"git stash" takes all of the staged changes and stores them away somewhere. This is useful if you need to stash your changes so you can be on a clean branch or maybe because you want to go back and try something before you make a commit with these changes. Here's how you do a stash:
```
git stash
```
If you want to unstash those changes and bring them back into your working directory:
```
git stash pop
```
Or maybe you want to unstash your changes without popping them off the stack. In other words, you might want to apply these stashed changes multiple times. To do this:
```
git stash apply
```
For a list of stashes:
```
git stash list
```
And to apply a specific stash from that list (e.g., stash@{3}):
```
git stash apply stash@{3}
```

<br>
### 10. Print out a cool visualization of your log
This is pretty cool! To visualize all of your long-standing branches:
```
git log --pretty=oneline --graph --decorate --all
```
We can return to a previous version by first copying ID from git log and then:
```
git reset --hard ID
```

<br>
<br>
## Reference
[Pro git book](https://git-scm.com/book/en/v2)
There is an online book about git, authored by Scott Chacon and Ben Straub, which is, in my opinion, far better than the official git documentation. When I need to refresh or learn some git concept, I always go there by default.
