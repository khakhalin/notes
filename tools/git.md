# GIT
## Typical everyday use
* `git pull` - 	fetch latest changes, merge them, and rebase HEAD to the latest commit
* `git diff` - show differences to unstaged files
* `git add .` - stage everytyhing
* `git commit -m "Message"` - commit
* `git push` - push to origin ('Origin' is a silly name they use for a remote repo)

## Analyzing stuff
* `git status` - see uncommited files + comparison to origin
* `git diff` - see unstaged changes
* `git diff HEAD` - see both staged and unstaged changes
* `git show [branch]:[file]` - show file changes. Tons of output formatting. On itself, describes head. [1](https://git-scm.com/docs/git-show)
* `git blame [file]` - describes changes for a file. Note that filenames go without quotation marks.
* `git diff commit1 commit2` - compare two commits.
* `git log -p [file/dir]` - full history for this file, with diffs.
* `git add [file]` - add one file only.

## Backing up, carefully
* `git reset [file]` - when a file was staged, but not yet committed, unstage it.
* `git stash` - put all uncommited files to a buffer
* `git stash pop` - pop files from a buffer
* `git commit --amend` #todo
* `git rebase -i HEAD~3` - squash last 3 commits into one, interactively. **Do it only for commits that weren't pushed yet** (or you'll get a conflict with a remote repo), unless you're morally prepared to fix everything locally and force-push to origin, rewriting it. "Interactively" here means that a list of commits is generated, and you leave `pick` for those you want to leave; replace it with `drop` for those you want to delete, and `squash` for those that needs to be squashed (just make sure there's a 'pick' before it, for somewhere to squash to). This workflow is a reason why we shouldn't push to public repo too often (don't push micro-commits); wait until a reasonable piece of work is done, **clean the commits**, then push. [1](https://git-scm.com/docs/git-rebase#_interactive_mode), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa)

## Backing up, panicky
* `git reflog` → find last commit that is good → `git reset HEAD@{index}`- resets head to this commit, deletes everything after.

## Branching
* `git branch` - to know the current branch
* `git checkout branch_name` - move to a target branch.
* `git branch branch_name` - creates a new branch.
* `git checkout -p branch_name` - an alias for creating a new branch at current HEAD, and checking it out. May be a good idea after a pull, to do all sorting in a safe branch, without endangering Master. [1](https://blog.carbonfive.com/2017/08/28/always-squash-and-rebase-your-git-commits/)

The concept of a **detached HEAD**: when HEAD points to an old commit. In this situation you cannot commit anything, as there's no branch to commit to (committing can only be done to the end of a branch). If you create a new branch there in the past however, you can commit, and then you can merge these changes if you need to. [1](https://www.atlassian.com/git/tutorials/using-branches/git-checkout)

## Dangerous practices
* `git rebase [target_branch] <source_branch>` - recommits new commits from the current branch (the one that is currently checked out), or the source_branch (if given) to the end of the target branch; then moves HEAD to the target branch. Pluses and minuses:
        * Good: it makes history linear, and thus clear, as there's no branching and merging. Say, if a month ago you created a side-project file in a side branch, and nobody touched it since, but kept working on the main thing, there's no need in merging this file in; you can as well just recommit it on top of master (rebase).
        * Good: allows code clean-up (e.g. described above for interactive rebase and commit squishing)
        * Bad: unlike for merging, commits are not inhereted, but are committed anew, and if branching was conceptually important, the history of it will be lost. Even worse, b1→rebase to b2→ merge to b1 will duplicate commits in b1 (they will be recommitted to b2 as new commits, but then merged back). "Always merge" may be a messy option, but it's the safest one for unexperienced teams ([ref](https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase))
        * Bad: If you have a conflict, you'll see contradicting commits (one adding something, the other one reverting this something back). Which can be hard to clean up.
        * Bad: never use rebase on public branches, as rebasing rewrites history, and if somebody is synchronized with this branch, they'll get a weird conflict of histories. The only way to use it with remote branches is with hard-pushing at the end (rewriting the remote branch), which is of course an extreme measure. [1](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [2](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)
* `git rebase --onto [target_branch] [list of branches]` - allows advanced tree manipulation that I don't understand for now: [1](https://git-scm.com/docs/git-rebase)
* `git pull --rebase` - in practice, this command integrates unpulled (relatively recent) commits form the origin, placing them before (sic!) recent commits in the local branch. Equivalent to virtually rebasing to the origin, and then hard-pulling the result to the local branch. **The end-result is exactly as if you pulled origin before making your commits.** Note how in this case rebase also rewrite the history, but in a safer way (it can be pushed back to origin without any issues), because rebasing happens locally. [1](http://thelazylog.com/git-rebase-or-git-pull/), [2](https://www.atlassian.com/git/tutorials/merging-vs-rebasing), [3](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)
* `git push --force` - hard push to origin, overwriting it: generally, a very dangerous idea.
* `git fetch origin master; git reset --hard origin/master` - hard-pull from origin overwriting local files. Equally dangerous. [r1](https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files)

## Destructive commands
* `git clean` - from the current dir, removes all files that are not under version control (all untracked files)
* `git reset --hard [commit_id]` - purge all uncommited changes; reset to commit (last one by default)
* `git branch -d branch_name` - deletes branch

## Open questions
* `git reset`
* `--no-ff` for `git merge
* `git pull -f` - what does this f mean?
* `pick`
* `fixup` - condenses several commits into one?
* why `commit -am` (for committing all) is even an option? What's the point of committing all?
* pull requests?

## References
* Funny short cheatsheet "Dangit": http://dangitgit.com/