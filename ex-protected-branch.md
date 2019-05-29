---
title: Protected Branch
layout: page
---

This exercise is set up for two people to do together. Find a partner, or proceed alone and play both roles. If working with a partner, you should both play the role of the submitter and then review your partner's merge request.  It is very similar to the [Collaborative Branching and Merging]({{ "ex-branching-merging" | relative_url }}) exercise, but the workflow is modified slightly when the master branch is protected. A protected master branch prevents anyone (including the owner/maintainer) from pushing directly to it from a local repository.  Instead, one must submit a merge request to be reviewed by a colleague.

The players:
  - **Submitter**
  - **Reviewer**

Each of the sections below is labeled for the player who should perform the steps in that section.


## SUBMITTER: Create a repository

Create a project in GitLab
  - **Use the project creation form: [{{ site.gitlaburl }}projects/new]({{ site.gitlaburl }}projects/new)**
      - Call the project `advanced-protected`
      - For this exercise, create it under your own username, not any group that you may have access to (not everyone will have access to groups)
      - Set the Visibility Level to "Public"

Invite the reviewer to your project.
  - Settings &rarr; Members
  - Add contributor's username under "Select members to invite"
  - Give them the MAINTAINER role under "Choose a role permission" so you will both be allowed to merge

Add a `README.md` file to the repository. You can do this with the GitLab interface (there's an `Add Readme` button) or by following the steps in the [Git Basics]({{ "ex-git-basics" | relative_url }}) exercise. Use the following content:

```
# Git Advanced Protected Branch Workflow

Hello World
```

## SUBMITTER: Protect your master branch

Modify protected branch settings.
  - Settings &rarr; Repository
  - Expand the `Protected Branches` section
  - In the protected branches section change the `Allowed to push` setting for the master branch to `No one`

## SUBMITTER: Clone the repository

```terminal
$ git clone git@{{ site.gitlabhost }}:[submitter]/advanced-protected.git
Cloning into 'advanced-protected'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
```

Change to the directory of the cloned repository.

```terminal
$ cd advanced-protected
```

You should have a clone of the repository now that contains the `README.md` file created in the step above.

## SUBMITTER: Make a change to your repository

Make a small change to your README.md file so the content looks like this:

```
# Git Advanced Protected Branch Workflow

Hello World
Hello [Reviewer]
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
$ git commit -a -m 'Add greeting to reviewer'
[master 50dad14] Added to README
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 312 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: GitLab: You are not allowed to push code to protected branches on this project.
To {{ site.gitlabhost }}:[submitter]/advanced-protected.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to '{{ site.gitlabhost }}:[submitter]/advanced-protected.git'
```

Notice how the push command fails and says that you are not allowed to push code to protected branches. This is expected and means that you simply need to create a branch and then push the branch along with a merge request so that changes can be reviewed before they are added to master.

## SUBMITTER: Create a branch with a change, and push

Create a branch called `add-greeting-for-reviewer` in your local repo.

```terminal
$ git checkout -b add-greeting-for-reviewer
Switched to a new branch 'add-greeting-for-reviewer'
```

Since you have already committed your changes in your local repository, your new branch will also include that commit.

Push the `add-greeting-for-reviewer` branch to your `origin` remote. The `-u` flag tells git to "track" this remote branch for the current local branch. You only need to use this flag for the first push. If you do a plain `git push` with this local branch in the future, git will know that you want to push to `add-greeting-for-reviewer` on the `origin` remote.

```terminal
$ git push -u origin add-greeting-for-reviewer
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (9/9), 780 bytes | 390.00 KiB/s, done.
Total 9 (delta 1), reused 0 (delta 0)
remote:
remote: To create a merge request for add-greeting-for-reviewer, visit:
remote:   {{ site.gitlaburl }}[submitter]/advanced-protected/merge_requests/new?merge_request%5Bsource_branch%5D=add-greeting-for-reviewer
remote:
To {{ site.gitlabhost }}:[submitter]/advanced-protected.git
 * [new branch]      add-greeting-for-reviewer -﹥ add-greeting-for-reviewer
Branch 'add-greeting-for-reviewer' set up to track remote branch 'add-greeting-for-reviewer' from 'origin'.
```

Those lines in the output that begin with "remote: " are interesting. They are messages from the remote repository. GitLab has a feature that sets this message when you push a branch to your repository. In this case it is giving you a link to follow in order to start a Merge Request.

*A note on terminology:* GitLab calls this a `Merge Request`; GitHub calls its version of this process a `Pull Request`. They are basically the same thing, but each platform has its own features and other things that it does with the process. It is also worth noting that this is not a native git feature, but one that platforms like GitLab, GitHub and BitBucket offer to streamline the workflow.

Copy and paste that URL into your browser and follow the process to submit a Merge Request.

It is helpful to give the Merge Request a good title and to add a description if more context or clarification is needed.

Once you submit the form, you will be redirected to the newly created Merge Request.

## BOTH: Take a look at the Merge Request page

Trying writing a few comments in the discussion section.

## REVIEWER: Review the Merge Request

Go to the "Changes" section. You will see a diff of the contributor's changes. Make a comment in this diff by hovering over the last line in the diff, and clicking the little speech bubble that appears. Add the comment, "This looks good, but I prefer the greeting 'aloha'."

## SUBMITTER: Respond to the review and make the requested change

Note that the submitter's comment appears in the Changes section and in the threaded Discussion on the Merge Request.

Respond to the comment, saying you'll make the change.

Edit the `README.md` file in your local repository so that it looks like this:

```
# Git Advanced Protected Branch Workflow

Hello World
Aloha [Reviewer]
```

Commit the change to the `add-greeting-for-reviewer` branch, and push. (You should still have `add-greeting-for-reviewer` checked out locally, so you shouldn't have to make any changes before running these commands.)

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
remote: View merge request for add-greeting-for-reviewer:
remote:   {{ site.gitlaburl }}[submitter]/advanced-protected/merge_requests/1
remote:
To {{ site.gitlabhost }}:[submitter]/advanced-protected.git
   355165e..c07ffa7  add-greeting-for-reviewer -﹥ add-greeting-for-reviewer
```

You get the helpful URL again from the remote, but note that this time it says "View merge request for add-greeting", and that the URL is to the Merge Request you created last time. Since you still have the Merge Request page open, just go back to your browser and refresh.

## BOTH: Take another look at the Merge Request page

See how just pushing to the branch updated the Merge Request. GitLab sees the changes, and notes them in the discussion and the diff now reflects the current state of `README.md` in the submitter's `add-greeting-for-reviewer` branch.

## REVIEWER: Merge the request

Click the green "Merge" button on the Merge Request page. The `master` branch in the submitter's GitLab repository will now include the two commits that the submitter made

## SUBMITTER: Sync your local repository

These commands will checkout the local `master` branch and sync it with the remote `master` branch.

```terminal
$ git checkout master
Switched to branch 'master'
$ git pull
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), done.
From {{ site.gitlabhost }}:[submitter]/advanced-protected
   ebc61d8..2cfabd4  master     -﹥ origin/master
Updating ebc61d8..2cfabd4
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

It is a good practice to remove the `add-greeting-for-reviewer` branch from the local and remote repositories. Remember that branching is cheap and easy with git, so there's no reason to keep them around or reuse them.

```terminal
$ git branch -d add-greeting-for-reviewer
Deleted branch add-greeting-for-reviewer (was c07ffa7).
```

That command will remove the branch from the local repository. There are ways to remove a remote branch with the command line, but they are very non-intuitive. It's easiest and best to use the web interface to remove those if you can. (You may have noticed the GitLab Merge Request had a checkbox to remove the branch when the request was merged, this is very handy.)

## BOTH: Wrap up

Now that the submitter's `add-greeting-for-reviewer` branch has been merged and deleted, we're back to a state, in terms of branches, that is the same as when we started.

This workflow is very powerful and it can help to keep the master branch clean and make it easy to implement a code review process whenever changes are made.
