# Github

Parents: [[01_Tools]], [[git]]
See also: [[gitlab]]

#tools


# Connecting from a terminal

As github switched from passwords to tokens, go here to generate a token: https://github.com/settings/tokens

Then type this for the git repo:
`git remote set-url origin https://<githubtoken>@github.com/<username>/<repositoryname>`

For contributions to show on GitHub's main page, make sure your user email is set up correctly. This can be done locally at a repo level using `git config user.email "blabla@gmail.com"`

Refs:
* https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed-please-use-a-personal