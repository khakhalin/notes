# GIT

Path: [[01_Tools]]
See also: [[bash]], [[ssh]]

#tools


Todo:
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
* `git diff HEAD` - see both staged and unstaged changes
* `git log -n 5 --online` - show a quick summary of 5 latest commits
* `git show [branch]:[file]` - show file changes. Tons of output formatting. On itself, describes head. [1](https://git-scm.com/docs/git-show)
* `git blame [file]` - describes changes for a file. Note that filenames go without quotation marks.
* `git diff commit1 commit2` - compare two commits.
* `git log -p [file/dir]` - full history for this particular file, with diffs.
* `git add [file]` - add one file only.

# Fixing mistakes, carefully
* `git reset [file]` - when a file was staged, but not yet committed, unstage it.
* `git stash` - push all uncommited files to a buffer
* `git stash pop` - pop files from a buffer. An archetypical use of "oops, wrong branch" includes stashing, changing branch, and unstashing back.
* `git reset --soft HEAD^` - returns git to the stage before last commit (here `HEAD^` signifies last commit, as is it synonimous to `HEAD~1`. You can also use any other commit as	 the target). All files that were change since that commit become staged (_I think. The manual says "files to be committed" - does it mean "staged"?_)
* `git reset` - default mode of reset is `mixed`, which is not destructive (as in `hard`), but unlike `soft`, doesn't automatically stage files that were changed since that commit. Which means that it's easier to lose them. If commit isn't specified, it just resets the last commit.
* `git commit --amend` - equivalent to soft rest to `HEAD~` (the parent of current `HEAD`), following by a new commit. Essentially, rewrites a commit (_right?_)
* `git rebase -i HEAD~3` - squash last 3 commits into one, interactively. **Do it only for commits that weren't pushed yet** (or you'll get a conflict with a remote repo), unless you're morally prepared to fix everything locally and force-push to origin, rewriting it. "Interactively" here means that a list of commits is generated, and you leave `pick` for those you want to leave; replace it with `drop` for those you want to delete, and `squash` for those that needs to be squashed (just make sure there's a 'pick' before it, for somewhere to squash to). This workflow is a reason why we shouldn't push to public repo too often (don't push micro-commits); wait until a reasonable piece of work is done, **clean the commits**, then push. [1](https://git-scm.com/docs/git-rebase#_interactive_mode), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa)
* `git restore [file]` - restore a file in the current tree from another tree or commit. Doesn't update the branch. In the past was homonymous with branch changing (both used the `checkout` keyword).
* `git revert HEAD~3` - make a new commit that reverts changes in fourth last (3d with zero indexing) commit.

# Branching
* `git branch` - show the current branch
* `git switch branch_name` - move to a target branch (in the past, used `checkout` command).
* `git branch branch_name` - creates a new branch.
* `git switch -c branch_name` - an alias for creating a new branch at current HEAD, and checking it out. May be a good idea after a pull, to do all sorting in a safe branch, without endangering Master. [1](https://blog.carbonfive.com/2017/08/28/always-squash-and-rebase-your-git-commits/)
* `git branch -m old_name new_name` - rename branch

The concept of a **detached HEAD**: when HEAD points to an old commit. In this situation you cannot commit anything, as there's no branch to commit to (committing can only be done to the end of a branch). If you create a new branch there in the past however, you can commit, and then you can merge these changes if you need to. [2](https://www.atlassian.com/git/tutorials/using-branches/git-checkout)

In the past people used to recommend `git checkout branch_name` instead of `switch`, but afaik this use is now considered obsolete, as `checkout` was used for two different unrelated operations, and it is too confusing. [3](https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch)

To clone a remote branch that doesn’t yet exist locally, do this:

```bash
git fetch --prune
git branch -v -a
git switch remote_branch_name
```
The `remote_branch_name` is just a name, without any `origin.` before it, as if the remote branch already existed locally. Switching to it seems to trigger the pull. The `--prune` key for `fetch` is unrelated to the operation itself, but is a nice feature: it matches our local list of remote branches to the actual list of remote branches, and removes those records that refer to branches that no longer exist. Without `--prune`, the list of “theoretically existing remote branches” can grow indefinitely long. The local branches are not affected; it doesn't remove local branches that actually exist. It only updates our local "understanding" of which branches exist or don't exist remotely.

To change which branch is considered remote-upstream for your local branch (for example, if you renamed them branches in some weird way, and now need to link everything correctly once again):
`git branch branch_name --set-upstream-to your_new_remote/branch_name`

Another DIY alternative is just to delete a branch at either local or remote end, and then pull (or push, respectively) to make local and remote repos match again.

# Merging
* `git switch master ; git merge hotfix` - merge branch `hotfix` into master
* `git diff master..feature` - compare branches before mering (actually this code, with pointses!)

If there's a conflict, one has to cherrypick 🔥 
A basic manual on cherry-picking  in a terminal:  https://docs.gitlab.com/ee/topics/git/cherry_picking.html

# Rebasing (a dangerous practice) 🔥
`git rebase [target_branch] <source_branch>` - recommits new commits from the source branch (or the current branch, if the source_branch is not specified), to the end of the target branch; then moves HEAD to the end. As if you were taking a fresh set of commit and changed the "base" on which they are grafted. Supposedly the typical workflow for rebase is the following: you're working on feature A; people have developed and merged feature B in master. So now you want to catch up. You do:
```git
git pull my_feature
git pull master
git checkout my_feature
git rebase master
git push
```

* The pluses and minuses of rebase (compared to Merge):
    * Good: it makes history linear, and thus clear, as there's no branching and merging. Say, if a month ago you created a side-project file in a side branch, and nobody touched it since, but kept working on the main thing, there's no need in merging this file in; you can as well just recommit it on top of master (rebase).
    * Good: allows code clean-up (via interactive rebase and commit squishing)
    * Bad: unlike for merging, commits are not inhereted, but are committed anew, and if branching was conceptually important, the history of it will be lost. Even worse, if b1 is rebased onto b2, and then b2 is merged back to b1, then all commits in b1 will get duplicated (they will be first  recommitted to b2 as new commits during rebasing, but then merged back).
        * "Always merge" may be a messy option, but it's the safest one for unexperienced teams ([ref](https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase))
        * For example, it means that new commits in master should never be introduced into a development branch using rebase, as it would duplicate them. Rebase is only tolerable if commits are moved from "more temporary" branches towards "less temporary", or it will result in a horrible mess.
    * Bad: If you have a conflict, you'll see contradicting commits (one adding something, the other one reverting this something back). Which will be quite hard to clean up. (But you can always do `git regabse --abort` and merge instead)
    * Bad: never use rebase on public branches, as rebasing rewrites history, and if somebody is synchronized with this branch, they'll get a weird conflict of histories. The only way to use it with remote branches is with hard-pushing at the end (rewriting the remote branch), which is of course an extreme measure (and often forbidden anyways). [1](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)
* `git rebase --onto [target_branch] [list of branches]` - allows advanced tree manipulation that I don't understand for now: [1](https://git-scm.com/docs/git-rebase) 🔥 
* `git pull --rebase` - in practice, this command integrates unpulled (relatively recent) commits form the origin, placing them before (sic!) recent commits in the local branch. Equivalent to virtually rebasing to the origin, and then pulling the result to the local branch. The end-result is exactly as if you pulled origin before making your commits, so it may be considered a fix for "omg I forgot to pull before committing". In this case rebase also rewrites the history (your old commits disappear, and are recommitted a new at the HEAD), but this way it is safe: it can be pushed back to origin without any issues, as rebasing happened locally, and origin haven't seen your commits yet. [1](http://thelazylog.com/git-rebase-or-git-pull/), [2](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)

Footnotes:
* https://womanonrails.com/git-rebase

# Destructive commands
* `git branch branch_name --force` - to leave weird states like "Detached head with uncommited changes after a failed rebase"
* `git clean` - from the current dir, removes all files that are not under version control (all untracked files)
* `git reset --hard [commit_id]` - reset to commit (last one by default), mercilessly discarding and erasing everything that happened since (all changes, committed and uncommited).
* `git push --force` - hard push to origin, overwriting it: A _super_ dangerous a wrong thing to do; forbidden on all normal repos.
* `git fetch origin master; git reset --hard origin/master` - hard-pull from origin completely erasing any local changes.
* `git branch -d branch_name` - deletes branch
* `git reset --hard HEAD~3` - destroy and delete last 3 commits, reset HEAD three positions lower. Note: If you want to save their content just in case, before running this, do `git branch [branch_name]`. Then when you destroy commits in the original branch, they will still be present in this new branch. Note also that rewriting history like that is a very bad idea if those commits were already shared.

Footnotes:
* https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files

# Github

## Working with github from a terminal

As github switched to using tokens, go here to generate a token: https://github.com/settings/tokens

Then type this for the git repo:
`git remote set-url origin https://<githubtoken>@github.com/<username>/<repositoryname>`
For these contributions to show on the main page, make sure your user email is set up correctly. This can be done locally at a repo level using `git config user.email "blabla@gmail.com"`

Source: https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed-please-use-a-personal

# Open questions

* `--no-ff` for `git merge
* `git pull -f` - what does this f mean?
* `pick`
* `fixup` - condenses several commits into one?
* why `commit -am` (for committing all) is even an option? What's the point of committing all?
* pull requests?

# References

* Funny short cheatsheet "Dangit" (without swears) or this one with: https://ohshitgit.com/