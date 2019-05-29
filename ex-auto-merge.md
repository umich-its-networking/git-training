---
title: Auto Merge
layout: page
---

This exercise illustrates how git will automatically merge two branches that have diverged, if the changed sections of code do not overlap.

## Setup

You can use the repository from the [Git Basics]({{ "ex-git-basics" | relative_url }}) exercise (or configure repositories on GitLab and locally as described in the first steps of that exercise).

Or you can create a new, empty repository.

```terminal
$ mkdir auto-merge
$ cd auto-merge/
$ git init
Initialized empty Git repository in [home]/auto-merge/.git/
```

The steps in this exercise won't push to a remote repository, so there's no need to set up a project in GitLab.

## Add a new file

Create a new file called `lorem.txt` with these contents:

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
$ git add lorem.txt
$ git commit -m 'Add lorem text'
[master (root-commit) 191519d] Add lorem text
 1 file changed, 6 insertions(+)
  create mode 100644 lorem.txt
```

## Create another branch

Use the `checkout` command to create a new local branch that matches `master`.

```terminal
$ git checkout -b auto
Switched to a new branch 'auto'
```

## Make and commit a change at the top ot the file

Edit `lorem.txt` to add a line at the top of the file:

```
123

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
[auto 390c0ff] Add to top
 1 file changed, 2 insertions(+)
```

## Make a change on the master branch

Checkout your `master` branch.

```terminal
$ git checkout master
Switched to branch 'master'
```

Note that `lorem.txt` doesn't have the changes you just committed to the `auto` branch.

Change `lorem.txt` to add a line at the end of the file.

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.

456
```

Commit the change.

```terminal
$ git commit -a -m 'Add to bottom'
[master 991bf0f] Add to bottom
 1 file changed, 2 insertions(+)
```

## Merge the auto branch into master

Merge `auto` into `master`.

```terminal
$ git merge auto
```

This will open your `$EDITOR`, it will be populated with text like this:

```
Merge branch 'auto'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

It is usually fine to just use the default message.

Once you save the file and exit your editor, the git output will continue...

```terminal
Auto-merging lorem.txt
Merge made by the 'recursive' strategy.
 lorem.txt | 2 ++
  1 file changed, 2 insertions(+)
```

Note that the merge says, "Auto-merging lorem.txt". It can't do a fast-forward merge the branches have diverged. But since the diffs didn't overlap, it was able to do an auto-merge.


Your `lorem.txt` file now contains the changes from both branches.

```
123

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.

456
```

## Summary

This same basic process is at work whether you're merging branches from different repositories or the same repository. Git will auto merge if the changes on each branch are not too close together. To see what happens when the changes are too close to each other or are overlapping, continue to the [Merge Conflict]({{ "ex-merge-conflict" | relative_url }}) exercise.
