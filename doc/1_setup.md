# Setup

**Table of contents**

- [About](#about)
- [SSH config](#ssh-config)
  - [SigningKey config](#signingkey-config)
- [Repository setup](#repository-setup)
- [References](#references)

## About

The steps below will setup ssh keys, create [git](https://git-scm.com) repositories locally and in 
[GitHub](https://github.com), connect them and make an initial commit-push.

The steps are how this repository itself was created. 

> **Note:** All examples here are currently for macOS, but will be similar/same on other operating systems.

## SSH config

Create ssh keys on the local computer - they will end up in the directory ```~/.ssh/```.

```
ssh-keygen -t ed25519 -C "<email>"
```

Copy the private ssh key (the one w/o a .pub extension) to *GitHub*.

Create ssh configuration for *GitHub* in ```~/.ssh/config```.

```
Host github.com
        HostName github.com
        User git
        AddKeysToAgent yes
        IdentitiesOnly yes
        UseKeychain yes
        IdentityFile <full path to local private ssh key file of user>
```

If needed, e g if there are several ssh keys on a given machine, 
clear the keychain of keys - possibly with elevated rights or using sudo. 
Then list keys to make sure there are none active.
Finally add the relevant private ssh key for a given repository/computer and (your) user. 
Note: the flag (--apple-use-keychain) of the final command is macOS specific.

```
ssh-add -D
ssh-add -l
ssh-add --apple-use-keychain ~/.ssh/<private key>
```

### SigningKey config

Optional but recommended; 
 
Add the public ssh key to *GitHub* as a *Signing Key* for improved security.

In the CLI, set git to use "ssh" for the *Signing Key* and define what key to use.

```
git config gpg.format ssh
git config user.signingkey <signing key>
```

## Repository setup

Create a new empty repository in *GitHub*. 
Here using the name "sample-git". Add nothing.

In the CLI, create a new local repository with the exact same name as as for the one in *GitHub*.

```
mkdir sample-git
cd sample-git
git init
```

For the local repository, set username and email to be used by git...

```
git config user.name "<name>"
git config user.email <email>
```

...or set it globally for the machine (i e all local git repositories).

```
git config --global user.name "<name>"
git config --global user.email <email>
```

Continue setting up the repository, commit and push it to remote...

> **Note:** If the commit shall be **signed** using the *SigningKey* described above, 
> the ```git commit``` command must also be accompanied by the ```-S```flag set.
```
echo "# sample-git" >> README.md
git add README.md
git commit -S -m "First commit"
git branch -M main
git remote add origin git@github.com:swesource/sample-git.git
git push -u origin main
```

…or connect an existing repository from the command line.

```
git remote add origin git@github.com:swesource/sample-git.git
git branch -M main
git push -u origin main
```

## References

* [GitHub Docs](https://docs.github.com/en)
* [GitHub - Connecting GitHb with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
* [GitHub - Authentication documentation](https://docs.github.com/en/authentication)
* [GitHub - Managing commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification)
* [GitHub - Tell Git about your signing key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key#telling-git-about-your-ssh-key)[2_use_cases.md](2_use_cases.md)