---
layout: episode
title: Interrupted work
teaching: 10
exercises: 0
questions:
  - How can Git help us to deal with interrupted work and context switching?
objectives:
  - Learn to switch context or abort work without panicking.
keypoints:
  - There is almost never reason to clone a fresh copy to complete a task that you have in mind.
---

## Frequent situation: interrupted work

We are saying that you should make nice, prefect code.  But the real world is much more choatic.

- You are in the middle of a Jackson-Pollock-style debugging spree with 27 modified files
  and debugging prints everywhere.
- Your supervisor comes in and wants you to fix/commit something right now.
- What to do?

Git provides lots of ways to switch tasks without ruining everything.


---


## Option 1: Stashing

The **stash** is the first and easiest place to temporararily "stash"
things.

- `git stash` will put working directory and staging area changes
  away.  Your code will be same as last commit.

- `git stash pop` will return to the state you were before.  Can give it a list

- `git stash list` will list the current stashes.

- `git stash save NAME` is like the first, but will give it a name.
  Useful if it might last a while.

- `git stash save [-p] [filename]` will stash certain files files
  and/or by patches.

- `git stash drop` will drop the most recent stash (or whichever stash
  you give).

- The stashes form a stack, so you can stash several batches of modifications.


### Exercise: stashes

- Make a change

- Check status/diff, stash the change, check status/diff again.

- Make a separate, unrelated change which doesn't touch the same
  lines.  Commit this change.

- Pop off the stash you saved, check status/diff.

- Optional: Do the same but stash twice.  Also check `git stash list`.
  Can you pop the stashes in the opposite order?

- Advanced: What happens if stashes conflict with other changes?  make
  a change and stash it.  Modify the same line or one right above or
  below.  Pop the stash back.  Resolve the conflict.  Note there is no
  extra commit.

- Advanced: what does `git graph` show when you have something
  stashed?


---

## Option 2: Create branches

You can use branches almost like you have already been doing if you
need to save some work.  You need to do something else for a bit?
Sounds like a good time to make a feature branch.

You basically know how to do this:

```shell
$ git checkout -b temporary  # create a branch and switch to it
$ git add <paths>            # stage changes
$ git commit                 # commit them
$ git checkout master        # back to master
                             # do your work...
$ git checkout temporary     # continue where you left off.
```

Later you can merge it to master or rebase it on top of master and resume work.


### Exercises (optional)

You already know how to do this...

- Optional: Go through the process above.  Start a change, create new
  branch and store your changes.  Go back to master and fix something
  else.  Resume your work and merge the new branch.

- Discuss how to resume your former work.  Can you git rid of branch?
  Continue using it?  etc.


---

## Aborting a conflicting merge

- Imagine it is Friday evening, you try to merge but have conflicts all over the place.
- You do not feel like resolving it now and want to undo the half-finished merge.
- Or it is a conflict that you cannot resolve and only your colleague knows which version is the one to keep.

What to do?

- There is no reason to delete the whole repository.
- You can undo the broken merge by resetting the repository to `HEAD` (last committed state).

```shell
$ git merge --abort
```

The repository looks then exactly as it was before the merge.


## Storing various junk you don't need but don't want to get rid of.

It's often that you do something and don't need it, but you don't want
to lose it right away.  You can use either of the above strategies to
stash/branch it away: branches is probably better.  Note that if you
try to use a branch after a long time, conflicts might get really bad
- but at least you have the data still.