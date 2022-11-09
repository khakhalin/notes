# SSH

Parents: [[bash]]
See also: [[git]]

#tools


Public key goes to the machine you're connecting to; private key is stored locally. All of them are typically stored in the `.ssh` hidden folder in the main (user's root, aka `~`) folder.

Some usage profiles:
`ssh username@ip` - connect to that machine
`ssh -i path/custom_private_key username@ip` - connect using a custom key
`scp source_file username@ip:path` - secure copy to a remote machine

To generate new keys (a pair of public and private keys), use `ssh-keygen -o -f path/file_name`. (🔥  _Not sure the keys are correct. Before using, actually google, as there are different standards for these keys, and some are newer than the other_)

# Secure file copy

To send files to a remote location we use `scp`, which is like `cp`, but secure. Basic format:
`scp file_name user@host:path`

# With version control

To use [[git]] tools (like [[gitlab]] for example), and assuming that you use different keys for different purposes, you'll need to cpecify which key to use for which host. Create `~/.ssh/config` file, and write to it something like:
```
Host gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_com_rsa
```

Footnotes:
* https://docs.gitlab.com/ee/user/ssh.html