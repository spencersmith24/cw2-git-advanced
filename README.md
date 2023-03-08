# Git Advanced
## Welcome
Welcome to the sequel tutorial to Git Basics! This is a companion to the PowerPoint presentation which will go over actually _performing_ all of the Git operations gone over in that presentation. Everything from branching to stashing—we'll go over that here. Have the GitHub page for your forked repository open, because you'll be making a lot of changes both locally and remotely!

---

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
But now the branch called `second` that you created earlier is behind the master branch—it's missing some new changes that might be needed to work on it. Switch to that branch with `git switch second` and use `git rebase master` to pull new changes onto that branch. Notice that _scratch.txt_ has been updated, as well as the `git log`.

---

## Undoing mistakes
### Reverting commits
Say we want to get _scratch.txt_ back to the way it was before you merged the branch `first` into `master`—maybe the changes weren't quite ready yet and now the master branch has some issues. You can revert the merge commit by using `git revert HEAD`, and should now see the file in your working tree updated to be blank again.

### Resetting the commit history
Make a 3 different commits to the file _scratch.txt_ so that its contents are updated and the `git log` is now three longer. Push those commits with `git push`. Assume we only want to keep the first of those changes—use `git reset HEAD~2` to delete the last two commits. Ensure that the `git log` now only contains the first commit that you made, and the state of the file in your working tree reflects this. Then, use `git push origin --force` to update the commit history on remote. The last two commits have been reset without an extra commit needed to reset them, and without the possibility of a revert conflict!

### Getting rid of local changes
Make some changes without committing them. Say we don't want to keep these changes—either they don't work, or someone has just pushed to remote and we just want to take that change. Use `git restore scratch.txt` or `git reset --hard` to delete those changes.<br>
**What about if I've already committed**?<br>
Refer to the previous step on resetting commit history to use `git reset <number-of-commits>` to remove local commits, then `git reset --hard` to set all your files back to those commits, and then you can pull to update your working tree back to normal.

---

## Stashing
### Saving changes to be used later
Update _scratch.txt_ with any new content. To save these changes for later without committing them, stash all uncommitted changes and give the stash a good description by using `git stash save <description>`. Use `git stash list` to make sure the stash was saved, and verify that _scratch.txt_ has been restored in your working tree.