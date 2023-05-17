# Git Advanced
## Welcome
Welcome to the sequel tutorial to Git Basics! This is a companion to the PowerPoint presentation which will go over actually _performing_ all of the Git operations gone over in that presentation. Everything from branching to stashing—we'll go over that here. Have the GitHub page for your forked repository open, because you'll be making a lot of changes both locally and remotely!

---

# Section 1

## Branching
### Creating branches
Create a new branch using `git checkout`—for this example, we'll call our new branch “first”: `git checkout -b first`<br>
Now, you can make all the changes you want on this branch without affecting the master branch until your changes are ready! How cool is that? You can use `git branch` to list all the branches you have, at which point you should now see `first` added to that list, with an asterisk next to it, indicating that `first` is your currently-selected branch.<br>To push this branch onto GitHub, use `git push --set-upstream origin first`.

### Switching branches
For a future step, create another branch `git checkout -b second`. Since we still need to be on the `first` branch, use `git switch first` to switch back to it.

### Committing and merging
Make a change on the `first` branch—write some arbitrary text into the file _scratch.txt_. Commit that change, and `git switch master` to get back onto the master branch. Notice that _scratch.txt_ has updated in your working tree and no longer contains the text that you wrote, as you wrote it on the `first` branch.<br>Use `git merge first` to merge the changes back into master, and you've successfully merged a branch!

### Deleting branches
Now that you've merged the `first` branch into `master`, you can delete it to keep your branches clean by using `git branch -d first`.

### Rebasing
But now the branch called `second` that you created earlier is behind the master branch—it's missing some new changes that might be needed to work on it. Switch to that branch with `git switch second` and use `git rebase master` to pull new changes onto that branch. Notice that _scratch.txt_ has been updated, as well as the `git log`. Switch back to the master branch with `git switch master`.

---

## Undoing mistakes
### Reverting commits
Say we want to get _scratch.txt_ back to the way it was before you merged the branch `first` into `master`—maybe the changes weren't quite ready yet and now the master branch has some issues. You can revert the merge commit by using `git revert HEAD`, and should now see the file in your working tree updated to be blank again.

### Resetting the commit history
Make 3 different commits to the file _scratch.txt_ so that its contents are updated and the `git log` is now three longer. Push those commits with `git push`. Assume we only want to keep the first of those changes—use `git reset HEAD~2` to delete the last two commits. Ensure that the `git log` now only contains the first commit that you made, and the state of the file in your working tree reflects this. Then, use `git push origin --force` to update the commit history on remote. The last two commits have been reset without an extra commit needed to reset them, and without the possibility of a revert conflict!

### Getting rid of local changes
Make some changes without committing them. Say we don't want to keep these changes—either they don't work, or someone has just pushed to remote and we just want to take that change. Use `git restore scratch.txt` or `git reset --hard` to delete those changes.<br>
> **What about if I've already committed**?<br>
Refer to the previous step on resetting commit history to use `git reset <number-of-commits>` to remove local commits, then `git reset --hard` to set all your files back to those commits, and then you can pull to update your working tree back to normal.

---

# Section 2

## Stashing
> Note: anytime `[stash]` is written in brackets, it means that that argument is optional, and if not included, will default to the most recent stash. If you only have one stash, that's what it'll default to. Angle brackets (`<>`) imply a non-optional argument.
### Saving changes to be used later
Update _scratch.txt_ with any new content. To save these changes for later without committing them, stash all uncommitted changes and give the stash a good description by using `git stash save <description>`. Use `git stash list` to make sure the stash was saved, and verify that _scratch.txt_ has been restored in your working tree.

### Viewing the changes made in stashes
Use `git stash show [stash]` to show your most recently-stashed changes. This isn't super helpful, except telling you how many lines were added to what files—to see a more in-depth diff, use `git stash show -p [stash]`. You should see whatever changes you made in the stash that you made.

### Bringing stashes back
Use `git switch second` to switch to the branch you created earlier. To apply the changes from the stash you made onto this branch, so they don't affect the master branch, use `git stash apply [stash]`. Use `git stash list` to notice that the stash is still in your stash list due to the use of `apply` as opposed to `pop`, and `git status` to verify that changes have been made to your working tree.

### Deleting stashes
Say we no longer need a stash—use `git stash drop [stash]` to delete it, and `git stash list` to verify that it's no longer there. The last two steps could have been done in one command with `git stash pop [stash]`, applying and then deleting.

> **Cleanup**<br>
> Use `git switch master` and `git merge second` to merge that branch back into the master branch, then `git branch -d second` to remove it.

### Creating a new branch from stashed changes
Make some changes and stash them. To make a new branch from these changes, use `git stash branch <branch-name> [stash]`. Call our new branch `third`—`git stash branch third` in preparation for the miscellaneous Git commands. You will automatically be switched to this branch with your changes un-committed but on your working tree.

---

## Miscellaneous
### Cherry-picking
Commit your currently un-committed changes on the `third` branch. Switch back to the master branch. Say we want to grab one specific commit from the `third` branch to pull onto our current branch, but we don't want any other commits that may be on that branch—just that one. Use `git log third --oneline` to find out the short hash for the commit you want to cherry-pick, then use `git cherry-pick <commit-hash>` to pull it onto your current branch. Your working tree should be updated and your `git log` should show that it's now included on the master branch, too!

### Seeing who did what to a file
Use `git blame -L66,67 README.md` to see who wrote these lines you're reading right now and what the commit message was when they wrote it. The answer won't surprise you!

### Viewing changes of individual commits
Using the commit hash of that commit, use `git show [hash]` to show what was changed in that commit. In this instance, the hash should be `a5c4c9a`, so `git show a5c4c9a`.

### Viewing upstream changes without pulling
Make a change on GitHub to _scratch.txt_, and on your local repository, run `git fetch` to see that change. To see the exact changes made before you use `git merge` to update your working tree, you can use `git diff origin/master`. End by using `git merge` to update your working tree with those changes.

### Remembering what you've done
If you've made changes but you have yet to commit them, but you don't quite remember what you changed, you can use `git diff` to see what you've updated.

### The various flavors of `git log`
There are many different ways `git log` can be used—throughout this presentation, we've mostly used `git log` and `git log --oneline`, but you can specify a lot of different options. For example, `git log --pretty=format:"(%h) %an - %ar: \"%s\""` to specify _exactly_ what the format should look like, or `git log -p` to show the diff of each commit, `git log --graph` to show a branch graph, and `--oneline` can be used with the last of those two to make it more readable. The ways you can format `git log` are very powerful!

## The end
Use `git merge third` and `git branch -d third` to get right back to where you started with regard to branches. Congratulations!
