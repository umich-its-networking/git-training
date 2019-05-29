---
title: Merge Conflict
layout: page
---

This exercise illustrates how git will help you handle merging two branches that have diverged, if the changed sections of code overlap.

## Setup

You can use the repository from the [Git Basics]({{ "ex-git-basics" | relative_url }}) exercise (or configure repositories on GitLab and locally as described in the first steps of that exercise).

Or you can create a new, empty repository.

```terminal
$ mkdir ex-merge-conflict
$ cd ex-merge-conflict/
$ git init
Initialized empty Git repository in [home]/ex-merge-conflict/.git/
```

The steps in this exercise won't push to a remote repository, so there's no need to set up a project in GitLab.

## Add a new file

Create a new file called `ipsum.txt` with these contents:

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

Add, commit to `master`.

```terminal
$ git add ipsum.txt
$ git commit -m 'Add lorem text'
[master (root-commit) e4ec3cb] Add lorem text
 1 file changed, 6 insertions(+)
  create mode 100644 ipsum.txt
```

## Create another branch

Use the `checkout` command to create a new local branch that matches `master`.

```terminal
$ git checkout -b add-text-above
Switched to a new branch 'add-text-above'
```

## Make and commit a change at the top ot the file

Edit `ipsum.txt` to add a line at the top of the file:

```
abc

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

Commit these changes.

```terminal
$ git commit -a -m 'Add to top'
[add-text-above 2a7579a] Add to top
 1 file changed, 2 insertions(+)
```

## Make a change on the master branch

Checkout your `master` branch.

```terminal
$ git checkout master
Switched to branch 'master'
```

Note that `ipsum.txt` doesn't have the changes you just commited to the `add-text-above` branch.

Change `ipsum.txt` to add a line at the top of the file.

```
xyz

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

Commit the change.

```terminal
$ git commit -a -m 'Add xyz to top'
[master db13884] Add xyz to top
 1 file changed, 2 insertions(+)
```

## Merge the add-text-above branch into master

Merge `add-text-above` into `master`.

```terminal
$ git merge add-text-above
Auto-merging ipsum.txt
CONFLICT (content): Merge conflict in ipsum.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git couldn't automatically merge our two branches because the changes we made in each branch overlapped with eachother. So we need to resolve the conflict manually. Let's look at the current status of the working copy.

```terminal
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file﹥..." to mark resolution)

        both modified:   ipsum.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

This message gives us a lot of information about the fact that we're in the middle of resolving a merge conflict. Note in the "Unmerged paths" section `ipsum.txt` is listed as "both modified".

When the repository is in this state, git modifies the flies with conflicts so that you can handle the conflicts in your editor. Try editing `ipsum.txt`, those contents should look like this:

```
<<<<<<< HEAD
xzy
=======
abc
>>>>>>> add-text-above

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

Git has modified the file to show changes from both branches. You'll have to clean things up and delete the lines that start with `<<<<<<<`, `=======`, and `>>>>>>>`. This is showing that the text "xyz" is what's in the current branch (indicated by "HEAD"), and the text "abc" is what's in the `add-text-above` branch.

To resolve the conflict, edit the file look like you want it to. For actual code this will require a judgement call: do you want to keep both changes? Just keep one? If so, which one? Should it actually be something totally different in order to make the two changes work together?

For this example, just pick "abc" and delete the other merge helper text so that `ipsum.txt` looks like this:

```
abc

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

Stage `ipsum.txt` and check the status.

```terminal
$ git add ipsum.txt
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

        modified:   ipsum.txt

```

Note that it tells you that you're still in a merge, and how to complete the process.

Run git commit without a commit message

```terminal
$ git commit
```

This will open your $EDITOR, it will be populated with text like this:


```
Merge branch 'add-text-above'

# Conflicts:
#       ipsum.txt
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#       modified:   ipsum.txt
#
```

It is usually fine to just use the default message. Note that the comments in this text give you a lot of information about the merge.

Once you save the file and exit your editor, the git output will continue...

```terminal
[master 52a2d9f] Merge branch 'add-text-above'
```

The merge is now completed.

```terminal
$ git status
On branch master
nothing to commit, working tree clean
```

## Summary

If you're confused about what state you're in during a merge, use the `git status` command. It will usually give you helpful information and suggest commands to move the process along.

Bear in mind that in addition to the direct conflict created by two branches trying to change the same part of the code, you may also need to consider the logical changes that each branch is making to the code and how to resolve those as well.
