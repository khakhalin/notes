# Github

Parents: [[tools]], [[git]]
See also: [[gitlab]]

#tools


# Connecting from a terminal

As github switched from passwords to tokens, go here to generate a token: https://github.com/settings/tokens

### Option 1: including credentials in the URL
Then type this for the git repo:
`git remote set-url origin https://<githubtoken>@github.com/<username>/<repositoryname>`
(Here `<username>` is not the email, but a shorter username that github uses, for example, one form your url)

⚠️ As of end-2024 I am not sure whether this approach works, and also I'm not sure how to use it if you're trying to work with a repository that is not yours but that was shared with you (as in this case `username` and `repositoryname` seem to start clashing with each other?) But either way, there's a simpler optino 2 (below).

### Option 2: install cli

Do one, then another, and follow the instructions. For authentication, pick "authenticate via Web" and use 2FA as requested.
```
conda install gh --channel conda-forge
gh auth login
```
Footnotes: 
* Install cli: https://github.com/cli/cli#installation
* Use cli: https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git

### Other

For contributions to show on GitHub's main page, make sure your user email is set up correctly. This can be done locally at a repo level using `git config user.email "blabla@gmail.com"`

Refs:
* https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed-please-use-a-personal