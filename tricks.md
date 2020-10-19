# Tips & Tricks
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