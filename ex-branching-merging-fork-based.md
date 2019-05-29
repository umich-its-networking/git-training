---
title: Fork-based Branching and Merging
layout: page
---

This exercise is set up for two people to do together. Find a partner, or proceed alone and play both roles. It is very similar to the [Collaborative Branching and Merging]({{ "ex-branching-merging" | relative_url }}) exercise, but use separate repositories in GitLab, one the fork of the original.

The players:
  - **Maintainer**
  - **Contributor**

Each of the sections below is labeled for the player who should perform the steps in that section.


## MAINTAINER: Create a repository

Create a project in GitLab
  - Use the project creation form: [{{ site.gitlaburl }}projects/new]({{ site.gitlaburl }}projects/new)
  - Call the project `advanced`
  - For this exercise, create it under your own username, not any group that you may have access to (not everyone will have access to groups)
  - Set the Visibility Level to "Public"

Add a `README.md` file to the repository. You can do this with the GitLab interface (there's an `Add Readme` button) or by following the steps in the [Git Basics]({{ "ex-git-basics" | relative_url }}) exercise. Use the following content:

```
# Git Advanced Workflow

Hello World
```

## CONTRIBUTOR: Fork the repository

Fork the maintainer's repository with the GitLab interface. See the [How to fork a project]({{ site.gitlaburl }}help/gitlab-basics/fork-project.md) documentation. After doing this you should have you own copy of the reposity at {{ site.gitlaburl }}[username]/advanced.git

## BOTH: Clone your respective repository

The maintainer should clone his/her repository, and the contributor should clone the repository from his/her fork.

```terminal
$ git clone git@{{ site.gitlabhost }}:[username]/advanced.git
Cloning into 'advanced'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
```

Each player should have a clone of the repository now, both should contain the `README.md` file created by the maintainer in the step above.

## CONTRIBUTOR: Add a remote for the Maintainer's repository

Add a remote to your local working copy that points to the maintainer's GitLab repository.

```terminal
$ git remote add upstream git@{{ site.gitlabhost }}:[maintainer]/advanced.git
```

You now have to remotes configured.

```terminal
$ git remote -v
origin    git@{{ site.gitlabhost }}:[maintainer]/advanced.git (fetch)
origin    git@{{ site.gitlabhost }}:[maintainer]/advanced.git (push)
upstream    git@{{ site.gitlabhost }}:[contributor]/advanced.git (fetch)
upstream    git@{{ site.gitlabhost }}:[contributor]/advanced.git (push)
```

Use `git fetch` to update your local repository's history of the new remote.

```terminal
$ git fetch upstream
From {{ site.gitlabhost }}:[username]/advanced
 * [new branch]      master     -﹥ upstream/master
```

## MAINTAINER: Make a change to your repository

Make a small change to your README.md file so the content looks like this:

```
# Git Advanced Workflow

Hello World
Hello [Contributor]
```
Commit the change and push it to your remote.

```terminal
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file﹥..." to update what will be committed)
  (use "git checkout -- <file﹥..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'Add greeting to contributor'
[master 50dad14] Added to README
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 269 bytes | 269.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To {{ site.gitlabhost }}:[username]/advanced.git
   3e99c8c..50dad14  master -﹥ master
```

## CONTRIBUTOR: Sync your repository to get maintainer's changes

In order to get the maintainer's changes into your repository, you need to fetch them from their remote and merge them into your `master` branch.

```terminal
$ git fetch upstream
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From {{ site.gitlabhost }}:[username]/advanced
   3e99c8c..50dad14  master     -﹥ upstream/master
$ git merge upstream/master
Updating 3e99c8c..50dad14
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ cat README.md
# Git Advanced Workflow

Hello World
Hello [Contributor]
```

It is a good practice to stay in sync with upstream remotes.

## CONTRIBUTOR: Create a branch with a change, and push

Create a branch called `add-greeting` in your local repo.

```terminal
$ git checkout -b add-greeting
Switched to a new branch 'add-greeting'
```

Edit README.md so it looks like this:

```
# Git Advanced Workflow

Hello World
Hello [Contributor]
Hello [Maintainer]
```

Commit the change.

```terminal
$ git commit -a -m 'Add greeting to maintainer'
[add-greeting 355165e] Add greeting to maintainer
 1 file changed, 1 insertion(+)
```

Push the `add-greeting` branch to your `origin` remote. The `-u` flag tells git to "track" this remote branch for the current local branch. You only need to use this flag for the first push. If you do a plain `git push` with this local branch in the future, git will know that you want to push to `add-greeting` on the `origin` remote.

```terminal
$ git push -u origin add-greeting
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (9/9), 780 bytes | 390.00 KiB/s, done.
Total 9 (delta 1), reused 0 (delta 0)
remote:
remote: To create a merge request for add-greeting, visit:
remote:   {{ site.gitlaburl }}contributor/advanced/merge_requests/new?merge_request%5Bsource_branch%5D=add-greeting
remote:
To {{ site.gitlabhost }}:its-inf-net/advanced.git
 * [new branch]      add-greeting -﹥ add-greeting
Branch 'add-greeting' set up to track remote branch 'add-greeting' from 'origin'.
```

Those lines in the output that begin with "remote: " are interesting. They are messages from the remote repository. GitLab has a feature that sets this message when you push a branch to your repository. In this case it is giving you a link to follow in order to start a Merge Request.

*A note on terminology:* GitLab calls this a "Merge Request"; GitHub calls its version of this process a "Pull Request". They are basically the same thing, but each platform has its own features and other things that it does with the process. It is also worth noting that this is not a native git feature, but one that platforms like GitLab, GitHub and BitBucket offer to streamline the workflow.

Copy and paste that URL into your browser and follow the process to submit a pull request. You should make sure that you are requesting that this branch be merged into the the `master` branch on the maintainer's repository. If you followed the forking steps above, this should happen automatically because GitLab assumes that since you are a downstream contributor, you will want your changes merged upstream to the maintainer's repository.

It is helpful to give the Merge Request a good title and to add a description if more context or clarification is needed.

Once you submit the form, you will be redirected to the newly created Merge Request.

## BOTH: Take a look at the Merge Request page

Note that the merge request is part of the maintainer's project, but that anyone can participate in the discussion on this page,

Trying writing a few comments in the discussion section.

## MAINTAINER: Review the Merge Request

Go to the "Changes" section. You will see a diff of the contributor's changes. Make a comment in this diff by hovering over the last line in the diff, and clicking the little speech bubble that appears. Add the comment, "This looks good, but I prefer the greeting 'aloha'. Please update."

## CONTRIBUTOR: Respond to the review and make the requested change

Note that the maintainer's comment appears in the Changes section and in the threaded Discussion on the Merge Request.

Respond to the comment, saying you'll make the change.

Edit the `README.md` file in your local repository so that it looks like this:

```
# Git Advanced Workflow

Hello World
Hello [Contributor]
Aloha [Maintainer]
```

Commit the change to the `add-greeting` branch, and push to your remote. (You should still have `add-greeting` checked out locally, so you shouldn't have to make any changes before running these commands.)

```terminal
$ git commit -a -m 'Fix greeting'
[add-greeting c07ffa7] Fix greeting
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: View merge request for add-greeting:
remote:   {{ site.gitlaburl }}[username]/advanced/merge_requests/1
remote:
To {{ site.gitlabhost }}:its-inf-net/advanced.git
   355165e..c07ffa7  add-greeting -﹥ add-greeting
```

You get the helpful URL again from the remote, but note that this time it says "View merge request for add-greeting", and that the URL is to the Merge Request you created last time. Since you still have the Merge Request page open, just go back to your browser and refresh.

## BOTH: Take another look at the Merge Request page

See how just pushing to the branch updated the Merge Request. GitLab sees the changes, and notes them in the discussion and the diff now reflects the current state of `README.md` in the contributor's `add-greeting` branch.

## MAINTAINER: Merge the request

Click the green "Merge" button on the Merge Request page. The `master` branch in the maintainer's GitLab repository will now include the two commits that the contributor made

Update your local repository. Since you're still on your `master` branch, you don't need to make any changes before running this command.

```terminal
$ git pull
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 7 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (7/7), done.
From {{ site.gitlabhost }}:[username]/advanced
   ebc61d8..2cfabd4  master     -﹥ origin/master
Updating ebc61d8..2cfabd4
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

## CONTRIBUTOR: Sync your local repository

These commands will checkout the local `master` branch and sync it with the maintainer's `master` branch, then push those changes to your remote.

```terminal
$ git checkout master
Switched to branch 'master'
$ git fetch upstream
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), done.
From {{ site.gitlabhost }}:[username]/advanced
   ebc61d8..2cfabd4  master     -﹥ upstream/master
$ git merge upstream/master
Updating ebc61d8..2cfabd4
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
$ git push origin master
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 280 bytes | 280.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To {{ site.gitlabhost }}:its-inf-net/advanced.git
   3e99c8c..2cfabd4  master -﹥ master
```

It is a good practice to remove the `add-greeting` branch from the local and remote repositories. Remember that branching is cheap and easy with git, so there's no reason to keep them around or reuse them.

```terminal
$ git branch -d add-greeting
Deleted branch add-greeting (was c07ffa7).
```

That command will remove the local repository. There are ways to remove a remote branch with the command line, but they are very non-intuitive. It's easiest and best to use the web interface to remove those if you can. (You may have noticed the GitLab Merge Request had a checkbox to remove the branch when the request was merged, this is very handy.)

## BOTH: Wrap up

Now that the contributor's `add-greeting` branch has been merged and deleted, we're back to a state, in terms of branches, that is the same as when we started.

This may seem like a lot of work, but this workflow is very powerful and it can be useful to follow for even small changes especially if collaborators are working asynchronously and on many projects at once.
