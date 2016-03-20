# Github Tutorial
We are going to review the basics of github now that you've had a chance to use it so you can catch up on anything you've missed, then we will go more in depth on some of the features which might help you in your final project, especially related to coding with other people.

### Basics
##### Cloning a repository
1. go to the repository page on github and copy the web address
2. `git clone <html>` 

##### Getting code from a repository
1. `git pull`

##### Dealing with code
1. `git add <file>` tells github to track that file
  * `git add -A` adds all untracked files not in .gitignore
2. `git commit` commits the added changes to your local git repository
  * This only effects your computer, it doesn't push anything to the github website
  * `git commit -am` simultaneously adds changes to previously tracked files and commits them (it will not add a new file)
3. `git push` pushes your changes to github online

### Merge Conflicts
GitHub "merges" files by matching up lines from each file, which allows it to merge files automatically as long as two people haven't edited the same line of code.  When this happens, GitHub has two options, and it has no way of knowing which one is correct, so it tells you there is a merge conflict, and lets you decide which code to keep.

Merge conflicts look like this (in the file with the conflict):
``` python
<<<<<<< HEAD
    code here 
    #this is where your edits to the code will be
=======
    different code here 
    #this is where other peopl's edits to the same lines will be
>>>>>>> 08345707847058r3439
```
`HEAD` is your local changes, the equals signs are where it switches from your changes to the other changes, and the giant number is the name of the specific commit you are trying to merge with.
To deal with this, just open the file and find the conflicts, then delete everything you don't want.
Then just add, commit, and push your changes.

### Branches
Sometimes you want to try things out, but you don't know if they're going to work, and you don't want to break the things you already have.
Or you and your partner are doing different things on the same code, and you don't want to keep dealing with merge conflicts everytime you sit down to do work.
This is what branches are for

##### Quick Overview
Branches allow you to make changes to code while preserving a copy of the original.
master is the branch you have been using, and it is the branch which should always hold the original working code.
When you are trying things that might break everythin, you should make a branch, get your code working, then merge that branch back into master.  Alternatively, if everything you try fails miserably, you can just delete that branch and go back to the old code.

##### Making a branch
* `git branch <branchname>` creates a local copy of a new branch, however it does not switch you to that branch

##### switching between branches
* `git checkout <branchname>` switches you to that branch
* `git branch --list` lists all branches

##### push and pull revisited
* `git pull <from> <branch>` is the full command of git pull
  * typically it assumes `<from>` to be `origin` and `<to>` to be your current branch
    * this means pull from origin's copy of your current branch (on GitHub) to your local copy of that branch
  * you have also used `git pull upstream master` to get code from paul's repo, which is "upstream" of origin
  * When using branches you can pull from any branch to any other branch
    * a common one is `git pull origin master` which is used to pull the master branch into whatever branch you are currently working on.
    * if you are working on a branch and someone else changes code on master, you probably want to stay up to date with master, so you would pull those changes into your branch.
* `git push` works the same way, where you can push from any branch to any other branch, although the prefered way of getting code to master is with pull requests (below)

##### pull requests
You generally want to make sure all code on the master branch works, so rather than just trusting that anyone who pushes code to master knows what they're doing, it is good to use pull requests to allow other people in the project to check over the new code before incorporating it into master.
* You have been creating pull requests to submit your code, but you haven't seen the other side of them.
* When you look at a pull request, you can see all of the differences bewtween the file on master and the new file, and you can make comments.
* Other people can also switch to your branch and run your code
* Once people have decided the branch looks good, they can merge it into master from the GitHub website

###### pull request merge conflicts
* Merge conflicts are caused by the same thing in pull requests as in other merges, but pull requests are mostly handled from the website, where you don't have access to the files.
* To fix this, you just move to the master branch and pull from the branch you want to merge in `git pull origin <yourbranch>`, then fix the merge conflicts the same way as before.
* If you pull from master on your new branch before submitting the pull request,, then you will handle the merge conflicts on the branch, which is generally better than handling them on master.

### SSH keys
So, you know how you have to type in your username and password every time you commit something to github?
There's a nice way around that, called ssh keys.
[here] (https://help.github.com/articles/checking-for-existing-ssh-keys/) is the beginning of github's tutorial for setting them up.
