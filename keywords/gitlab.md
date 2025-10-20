# Gitlab

Parents: [[tools]], [[git]]
See also: [[github]]

#tools


# Merge request

One possible workflow with Merge requests:, assign it to yourself (yep, this is confusing), put another person as “Reviewer”. Don’t press the “Merge” button yourself - if the repo is set up in a way that people can merge their branches to master themselves, this “Merge” button won’t disappear even if you appoint a reviewer for your code. So don’t take the bait, don’t merge before the review, wait.

Rolling back a merge seems to be super-messy, and honestly I’m not sure why someone would even implement something like that. It creates a new commit that rewrite changes back to the previous state, so on the code-level, it achieves the roll-back. However you cannot then merge the changes again if you wanted to, as from the git tree point of view, they were already merged once! They are no longer upstream! Which means that the only way to-remerge them that I know is to go to the branch, do soft reset a few commits to the past (to get the changes staged), then recommit them anew, and only try to merge these new commits. Which is weird. (The option of hard-resetting master branch also exists of course, but it really shouldn’t be allowed, as it is too dangerous).