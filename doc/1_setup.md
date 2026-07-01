# Setup

Create ssh-keys - they will end up in the directory ~/.ssh/

```
ssh-keygen -t ed25519 -C "<email>"
```

Create ssh configuration for github in ~/.ssh/config

```
Host github.com
        HostName github.com
        User git
        AddKeysToAgent yes
        IdentitiesOnly yes
        UseKeychain yes
        IdentityFile <full path to local private ssh key file of user>
```

If needed, like if there are several keys on a given machine, clear the keychain of keys - with elevated rights / sudo!
List keys to make sure there are none.
Then add the relevant key for a given repository.

```
ssh-add -D
ssh-add -l
ssh-add --apple-use-keychain ~/.ssh/<private key>

```

Create a new repository on the command line

```
mkdir sample-git
cd sample-git
git init
```

For the local repository, set username and email

```
git config user.name "<name>"
git config user.email <email>
```

..or set it globally for the machine (all local repositories)

```
git config --global user.name "<name>"
git config --global user.email <email>
```

Continue setting up the repository, commit and push it to remote
 
```
echo "# sample-git" >> README.md
git add README.md
git commit -m "First commit"
git branch -M main
git remote add origin git@github.com:swesource/sample-git.git
git push -u origin main
```

…or connect an existing repository from the command line

```
git remote add origin git@github.com:swesource/sample-git.git
git branch -M main
git push -u origin main
```

