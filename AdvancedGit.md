---
title: Github Websites
layout: template
filename: AdvancedGit.md
---
# Advanced GIT

#### [Back to Github Tutorial](index)

## Contents
- [Purpose of Page](#Purpose_of_Page)
- [Foxtrot Merges](#Foxtrot_Merges)

## Purpose of Page

This page is here to document GIT tools and paradigms which I've run into since college. There are good-practice topics as well as tools which you generally don't need on small, short-lived projects, and therefore I didn't run into them until I got to a professional environment.

## Foxtrot Merges

### My Frustrations (feel free to skip)

On bitbucket, there's an annoying button to disable "foxtrot merges", which someone enabled, and about 3/4 of my pushes were suddenly being rejected. There's someone at bitbucked who feels very strongly about so-called foxtrot merges, and he wrote [this incomprehensible page](https://blog.developer.atlassian.com/stop-foxtrots-now/) in which he completely fails to explain why. 

I'm too lazy to generate images and upload them, so here's some ascii art to try and sumarize his argument and why it breaks (letters are commits, lines are where they go):


His argument is that foxtrot merges put commits in the wrong order
```
  B-----\
 /       \
A -- C -- D
```
becomes
```
  B-----\
 /       \
A -- C -(B)-D
```
after the merge, putting B after C instead of before.

And he likes to draw the line like this to confuse you:
```
  B
 / \
A --\- C -(B)- D
     \----/
```
This, however, makes no sense, because who's to say that I didn't do my B commit after my C commit? The entire problem is actually that it's unclear what order B and C should go in:
```
  /----C?
 /       \
A - B?- -- D
```
This order woud mean everything is fine by his argument.

Also, all of these pictures are nice, but in practice it means that you just can't use the workflow where you do work on a branch and regularly pull from master to get other people's new additions (which is the work flow I, at least, learned in college) because any new change on master would necessarily create a foxtrot merge, and the guy at bitbucket does, somewhere in the middle of his rant, get around to this, and recommends using rebase all the time for everything. This is the right answer, but first we should go back and figure out the question.


### Why ~~foxtrots~~ pulls from master are bad

If you skipped my above rant, the key takeaway is that any pull from master (assuming there was actually new work on master) necessarily creates a "foxtrot" merge.

So, what's the problem?

The issue is in the commit history on your master branch. Basically, if a code change gets pulled into master and breaks something, you want to be able to look at the commit history on master and see the commit that actually broke the code at the top of the history tree.
To explain this, I'm going to use a bit of a timeline:

We'll start with code at commit `A`, then branch off and create commit `B`:
```
  B
 /
A
```
Next, master gets updated with commit `C`:
```
  B -
 /
A -- C
```
If you're following my compaints from above, I'll now add commit `B1` to show timeline is irrelevant:
```
  B --- B1 -
 /
A -- C -----
```
At this point, assume master (lower branch) is working, and we want to get commits B and B1 onto master. A pull from master would do this (this is the foxtrot merge because `C` ends up in front of `B1`):
```
  B --- B1 - C
 /          /
A -- C ----/--
```
And the subequent merge into master would result in this commit history on master (where `D` is the merge itself):
```
A - B - B1 - C - D
```
Now, if B1 broke the code, when you look through the history, it appears hidden behind `C`, which actually happened a while ago and definitely didn't break the code. As a result, you're likely to spend a lot of time looking through commit `C` and finding nothing instead of finding your problem in commit `B1`.


### how to avoid them

So, assuming this is actually bad, or at least that you'd rather avoid it, how do we do that?

The answer is rebase, a lot, actually all the time.

If you're like me and like to stick with the few things you actually know, this sounds scary: rebase sounds like something you resort to when all the normal things didn't work and now you don't know what to do. So, it's important to realize what a rebase is, and how it compares to a merge:

**merge:**
```
  X -- B         X - B 
 /    /    >>   /
A -- B         A (- B)
```
**rebase:**
```
   /-----\
  X   B - X             X
 /   /        >>       /
A - B             A - B
```
What I'm trying to show is that both merges and rebases are ways to combine two pieces of code where were changed independently.
The only difference is the resulting order in the GIT history.
A merge puts the commits being brought in _after_ the things that are already there, and a rebase puts the commits being brought in _before_ whatever is there.
This probably sounds like you want a merge: things that are being brought in should go after exising things.
However, when you are working on a branch which you want to merge into master (put things on branch after things already in master),
you actually want to put all of the working code from master _before_ any of the commits on your branch.
That way, once you merge your branch to master, all of your _new to master_ commits end up _after_ the previously tested master commits:

**merge:**
```
  X -- B           X
 /    / \    >>   / \
A -- B - D       A   B-D
```
**rebase:**
```
   /-----\
  X - B - X            X
 /   /     \   >>     / \
A - B ----- D      A-B   D
```
Note: whether `B` came before or after `X` in time is irellevant because `B` is working code on master, and `X` is new code which could break things.

Basically, because you want to treat the "existing" code on your branch as new code and the "new" code which you are pulling from master as existing/tested code, when you are bringing in code from master to your branch, you want a rebase instead of a merge.

### explanation attempt 3

Another way to think of this is to think about the full commit history with many commits:

**merge-based**
```
  W -- X -- Y ---- Z          W   Y
 /    /           / \   >>   / \ / \ 
A -- B - C - D - E - F      A   B   C-D-E-F
```
Here, the code from the feature branch gets distributed throughout the code from master, and the history on master can change all the way back to the earliest commit with an active branch coming off of it.

**rebase**
```
   /-----\     /------\
  W   B - W - Y   E - W-Y                  W-Y
 /   /           /       \   >>           /   \
A - B - C - D - E ------- F      A-B-C-D-E     F
```
This way any branch, no matter how large, exists in an isolated location in the final git history, and all changes which are merged into master append to the end of the commit history instead of modifying it throughout.
