# GIT

Path: [[01_Tools]]
See also: [[bash]], [[ssh]], [[vim]], [[linting]] (for pre-commit hooks)
Platforms: [[github]], [[gitlab]]

#tools


Todo: 🔥🔥
* https://git-scm.com/docs/git-restore
* https://git-scm.com/docs/git-rebase#_recovering_from_upstream_rebase

# Typical everyday use

* `git pull` - 	fetch latest changes, merge them, and rebase HEAD to the latest commit
* `git diff` - show differences to unstaged files
* `git add .` - stage everytyhing
* `git add folder_name_or_file_name` - stage only some files in particular
* `git commit -m "Message"` - commit
* `git push` - push to origin ('Origin' is a silly name they use for a remote repo)

# Analyzing stuff

* `git reflog` - some sort of most complete history? #todo
* `git status` - see uncommited files + comparison to origin
* `git diff` - see unstaged changes
    * If the text keeps appearing, and you don't need that, quit the [[vim]] session using `:q!`
* `git diff HEAD` - see both staged and unstaged changes
* `git log -n 5 --online` - show a quick summary of 5 latest commits
* `git show [branch]:[file]` - show file changes. Tons of output formatting. On itself, describes head. [1](https://git-scm.com/docs/git-show)
* `git blame [file]` - describes changes for a file. Note that filenames go without quotation marks.
* `git diff commit1 commit2` - compare two commits.
* `git log -p [file/dir]` - full history for this particular file, with diffs.
* `git add [file]` - add one file only.

# Branching

* `git branch` - show the list of branches + indicate the current one
* `git switch branch_name` - move to a target branch (in the past, used `checkout` command).
* `git branch branch_name` - creates a new branch.
* `git switch -c branch_name` - an alias for creating a new branch at current HEAD, and checking it out. May be a good idea after a pull, to do all sorting in a safe branch, without endangering Master.
* `git branch -m old_name new_name` - rename branch

The concept of a **detached HEAD**: when HEAD points to an old commit. In this situation you cannot commit anything, as there's no branch to commit to (committing can only be done to the end of a branch). You can however create a new branch form this point in the past, and commit to it. Typically, you do: `git checkout HEAD~1` (or whatever number of commits that you want to go back), and then once you travel back in time, create a branch. Alternatively, `git checkout <commit_id>`, where commit ids can be taken from `git log`.

To return to the present after traveling to the past with `git checkout HEAD~1`, type `git switch -`.

To go back, just do `git switch master` (or some other branch) - and it will reset HEAD to the latest commit.

To clone a remote branch that doesn’t yet exist locally, do this:
```bash
git fetch --prune
git branch -v -a
git switch remote_branch_name
```
The `remote_branch_name` is just a name, without any `origin.` before it, as if the remote branch already existed locally. Switching to it seems to trigger the pull. The `--prune` key for `fetch` is unrelated to the operation itself, but is a nice feature: it matches our local list of remote branches to the actual list of remote branches, and removes those records that refer to branches that no longer exist. Without `--prune`, the list of “theoretically existing remote branches” can grow indefinitely long. The local branches are not affected; it doesn't remove local branches that actually exist. It only updates our local "understanding" of which branches exist or don't exist remotely.

To change which branch is considered remote-upstream for your local branch (for example, if you renamed them branches in some weird way, and now need to link everything correctly once again):
`git branch branch_name --set-upstream-to=origin/your_new_remote_branch_name`
If this sequence produces an error, sometimes you need to instead push and create a remote branch at the same time. For this, you do:
`git push -u origin branch_name`
(which is also shorter, and so in practice, probably, preferable?)

Another DIY alternative is just to delete a branch at either local or remote end, and then pull (or push, respectively) to make local and remote repos match again.

* https://stackoverflow.com/questions/34519665/how-can-i-move-head-back-to-a-previous-location-detached-head-undo-commits
* https://www.atlassian.com/git/tutorials/using-branches/git-checkout
* https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch

# Merging

* `git switch main ; git merge hotfix` - merge branch `hotfix` into main
* `git diff main..feature` - compare branches before merging (actually this code, with pointses!)

If there's a conflict, one has to … 🔥🔥 _How to resolve conflicts? If you don't have an IDE, especially?.._

⚠️ Note that unlike for `rebase` (below), merging branch1 into branch2 with one simple command doesn't exist! You can technically do it by providing an `--into-name <branch2>` command, but it's not just two branch names in a row! And anyways it's advisable to always first switch to the branch you are merging to. If you type  (_incorrectly!_) "merge branch1 branch2" you will attempt to merge BOTH branches onto the current branch!

If you pulled a branch and commited locally, but someone else had pushed a commit to the origin branch, you can no longer push, and you can no longer pull with a simple `git pull`. Instead you should do `git fetch` followed by `git merge`. If you are lucky and there are no conflicts, that's it basically (just push back immediately); if there are coflicts - resolve them first, then commit and push.

**Triggering conflicts**: If you want to a manual careful merge, you can try to avoid auto-merging using `git merge --no-commit --no-ff source_branch`. 🔥It's unclear however if it's enough to trigger a neat resolution sequence in the IDE.

Apparently these 2 commands mark every file edited in both branches as a conflict.
```bash
git merge --no-commit source_branch
git checkout --conflict merge .  # This . apparently being critical
```
This approach doesn't work for "clean merges" when no file is edited in both branches (for clean merges it still does a fast-forward), but that's probably ok.

**Squashing** (that I keep calling "squishing" for some reason): just `git merge hotfix --squash`. That's if feature branches are already on `origin`. If not, squash-rebasing (described deep below) may be better (but I haven't tried it).

# Rebase

`git rebase [target_branch] <source_branch>` - recommits new commits from the source branch (or the current branch, if the source_branch is not specified), to the end of the target branch; then moves HEAD to the end. As if you were taking a fresh set of commit and changed the "base" on which they are grafted. Supposedly the typical workflow for rebase is the following: you're working on feature A; people have developed and merged feature B in master. So now you want to catch up. You do:
```git
git pull my_feature
git pull main
git checkout my_feature
git rebase main
```
Now your feature branch is ahead of master on your commits. And when it's time to do `git push` you will only push your commits.

The pluses and minuses of rebase (compared to Merge):
* Good: it makes history linear, and thus clear, as there's no branching and merging. Say, if a month ago you created a side-project file in a side branch, and nobody touched it since, but kept working on the main thing, there's no need in merging this file in; you can as well just recommit it on top of master (rebase).
* Good: allows code clean-up (via interactive rebase and commit squashing)
* Bad: unlike for merging, commits are not inhereted, but are committed anew, and if branching was conceptually important, the history of it will be lost. Even worse, if b1 is rebased onto b2, and then b2 is merged back to b1, then all commits in b1 will get duplicated (they will be first  recommitted to b2 as new commits during rebasing, but then merged back).
    * "Always merge" may be a messy option, but it's the safest one for unexperienced teams ([ref](https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase))
    * For example, it means that new commits in master should never be introduced into a development branch using rebase, as it would duplicate them. Rebase is only tolerable if commits are moved from "more temporary" branches towards "less temporary", or it will result in a horrible mess.
* Bad: If you have a conflict, you'll see contradicting commits (one adding something, the other one reverting this something back). Which will be quite hard to clean up. (But you can always do `git regabse --abort` and merge instead)
* Bad: never use rebase on public branches, as rebasing rewrites history, and if somebody is synchronized with this branch, they'll get a weird conflict of histories. The only way to use it with remote branches is with force-pushing at the end (rewriting the remote branch), which is a bit of an extreme measure (and often forbidden anyways). [1](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)

Other interesting uses:
* `git rebase --onto [target_branch] [list of branches]` - allows advanced tree manipulation that I don't understand for now: [1](https://git-scm.com/docs/git-rebase) 🔥 
* `git pull --rebase` - in practice, this command integrates unpulled (relatively recent) commits form the origin, placing them before (sic!) recent commits in the local branch. Equivalent to virtually rebasing to the origin, and then pulling the result to the local branch. The end-result is exactly as if you pulled origin before making your commits, so it may be considered a fix for "omg I forgot to pull before committing". In this case rebase also rewrites the history (your old commits disappear, and are recommitted a new at the HEAD), but this way it is safe: it can be pushed back to origin without any issues, as rebasing happened locally, and origin haven't seen your commits yet. [1](http://thelazylog.com/git-rebase-or-git-pull/), [2](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)

Footnotes:
* https://womanonrails.com/git-rebase

# Destructive commands

* `git branch branch_name --force` - to leave weird states like "Detached head with uncommited changes after a failed rebase"
* `git restore .` - cancel all unstaged changes in existing files (but all new files still remain). Basically a nuclear Ctrl-Z without a possibility of Ctrl-Y. Be careful!
* `git clean` - from the current dir, removes all files that are not under version control (all untracked files)
* `git reset --hard [commit_id]` - reset to commit (last one by default), mercilessly discarding and erasing everything that happened since (all changes, committed and uncommited).
* `git push --force` - hard push to origin, overwriting it: A _super_ dangerous a wrong thing to do; forbidden on all normal repos.
* `git fetch origin main; git reset --hard origin/main` - **hard pull from origin** erasing any local changes.
* `git branch -d branch_name` - deletes branch
* `git reset --hard HEAD~3` - destroy and delete last 3 commits, reset HEAD three positions lower. Note: If you want to save their content just in case, before running this, do `git branch [branch_name]`. Then when you destroy commits in the original branch, they will still be present in this new branch. Note also that rewriting history like that is a very bad idea if those commits were already shared.

Footnotes:
* https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files

# Fixing mistakes

* `git reset [file]` - when a file was staged, but not yet committed, unstage it.
* `git stash` - push all uncommited files to a buffer
* `git stash pop` - pop files from a buffer. An archetypical use of "oops, wrong branch" includes stashing, changing branch, and unstashing back.
* `git reset --soft HEAD^` - returns git to the stage before last commit. Here `HEAD^` signifies last commit, as is it synonimous to `HEAD~1`. You can also use any other commit as the target. All files that were changed since that commit become staged
* `git reset` - default mode of reset is `mixed`, which is not destructive (as in `hard`), but unlike `soft`, doesn't automatically stage files that were changed since that commit. Which means that it may be easier to lose them. If commit isn't specified, it just resets the last commit.
* `git commit --amend` - equivalent to soft reset to `HEAD~` (the parent of current `HEAD`), following by a new commit. Essentially, rewrites last commit with updated content
    * To squash several commits (before they are pushed to the repo of course), reset by `HEAD~2` (or whatever a good number it), and recommit them all together with a good message. Be careful not go beyond the latest pushed commit though, or you'll get a weird conflict on your hands...
* `git rebase -i HEAD~3` - squash last 3 commits into one, interactively. **Do it only for commits that weren't pushed yet!!** or you'll get a conflict with a remote repo, unless you're morally prepared to fix everything locally and force-push to origin, rewriting it (which is typically forbidden anyway). "Interactively" here means that a list of commits is generated, and you leave `pick` for those you want to leave; replace it with `drop` for those you want to delete, and `squash` for those that needs to be squashed (just make sure there's a 'pick' before it, for somewhere to squash to). This workflow is a reason why we shouldn't push to public repo too often (don't push micro-commits); wait until a reasonable piece of work is done, **clean the commits**, then push. [1](https://git-scm.com/docs/git-rebase#_interactive_mode), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa)
* `git restore [file]` - restore a file in the current tree from another tree or commit. Doesn't update the branch. In the past was homonymous with branch changing (both used the `checkout` keyword).
* `git revert HEAD~3` - make a new commit that reverts changes in fourth last (3d with zero indexing) commit.

If a branch is screwed-up beyond recognition, you have two options:
1. Move back in time (by resetting a few commits back) in mixed mode, start a new branch, then only stage those changes that you like, commit them, reset the rest. If you have pushed already, and you can't push branches, create a new branch at the commit that you want to reset to, then locally reset on your branch, stash the changes, `git fetch` and switch to the newly created alt-branch, pop that stash, and select which files to commit.
2. Pick a point in the past, branch from their, then cherry-pick commits from your other branch into the new branch. The process of cherry-picking is described below. If you have pushed already, just branch online than fetch and cherry-pick locally.

Cherry-picking: check the history of commits, either using `git log` or looking at the commits graph online ([[github]] calls it "Network" for some reason). Every commit comes with a hashcode, or commit id that looks something like `1a23bc45`. 
* `git cherry-pick <commit_id>` copies and applies an individual commit from another branch. If there's a merge conflict, you'll get stopped before the merge, but with all the changes brought over, and staged. The same commit message is applied as in the original commit.
* `git cherry-pick <commit_id> -e` - same, but asks you to change commit message
* `git cherry-pick <commit_id> -n` - bring the changes over, but without committing them, so that you could commit them manually later. When fixing stuff, especially when there are several commits to bring over, that's the best option actually, as it allows to accumulate several changes, then commit them all at once. Something like cherry-picking and squashing at the same time!

Footnotes:
* Some recommendations on cherrypicking (but not too detailed tbh) https://docs.gitlab.com/ee/topics/git/cherry_picking.html

# Other work patterns

* https://blog.carbonfive.com/2017/08/28/always-squash-and-rebase-your-git-commits/ - before merging a feature into main, manually squash commits with `git rebase -i HEAD~[NUMBER OF COMMITS]` or `git rebase -i [SHA]`, then pull main, rebase branch on main (with `checkout branch_name; rebase main`), then resolve conflicts, merge into main, and push. The weird thing about this workflow is that after squashing the local branch becomes incompatible with remote branch, so one has to either force push, or duplicate branch locally. Which may get annoying if merging gets delayed by extra work, for whatever reason.

# Setting things up

For specific platforms, see [[github]], [[gitlab]]

* `git remote -v` - to see remote connections that are set up currently
* `git remote set-url origin CONNECTION` - to set it up. The specifics of how the `CONNECTION` string looks like depends on whether you use [[github]], [[gitlab]] or something else, and also on whether you connect via [[ssh]] or https. But it's some combo of tokens, usernames etc.

To set your identify for commits:
```bash
git config --global user.name "Name Name"
git config --global user.email "a@a.a"
```

# Open questions

* `--no-ff` for `git merge`?
* `git pull -f` - what does this f mean?
* `pick`
* `fixup` - condenses several commits into one?
* why `commit -am` (for committing all) is even an option? What's the point of committing all?
* pull requests?

# References

* Funny short cheatsheet "Dangit" (without swears) or this one with: https://ohshitgit.com/