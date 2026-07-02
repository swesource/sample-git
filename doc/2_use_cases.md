# Use-cases

**Table of contents**

- [Feature branch](#feature-branch)
- [Release branch](#release-branch)
- [Release merge and tag](#release-merge-and-tag)
- [Patch release](#patch-release)

Note that all work here will only be performed in your local git repository. 
I e no ```git push``` to the remote repository will be made.

For each step you can check status with your favourite git client (like 
[GitHub Desktop](https://desktop.github.com/download/), 
[Sourcetree](https://www.sourcetreeapp.com), 
[GitKraken](https://www.gitkraken.com)) 
or via the wonderful CLI using commands such as 

```
git log
git status
```

And remember that help is always close at hand with

```
git --help
```

Clone code to local repo.

```
git clone git@bitbucket.org:swesource/sample-git.git
```

## Feature branch

Change to develop branch.

```
git checkout develop
```

Create new feature branch named *fb_workitem001* from develop branch. 
Note the *fb_* prefix for feature branches.

```
git branch fb_workitem001
```

Change to the new feature branch *fb_workitem001*.

```
git checkout fb_workitem001
```

> **Note:** The two previous steps  - create branch and switch to it - can be performed with one command.
>
> ```
> git checkout -b fb_workitem001
> ```

Confirm what branch you are in with

```
git branch
```

Commit new changes to the feature branch. 
Make a few of these - at least three with unique comments/messages. 
Before each ```git add```, make sure to create/change/delete some file(s).

```
# Make some changes here
git add .
git commit -m "Message1"
# And some more changes here
git add .
git commit -m "Message2"
# And finally some here
git add .
git commit -m "Message3"
```

Merge the feature branch with the *develop* branch.
Change to the branch to merge __to__ (here: *develop*) and then perform the merge.

```
git checkout develop
```

Before merging, make some change in the develop branch.

```
# Do some change - like a new file or something the does not(!) conflict with previous changes
git add .
git commit -m "Message4"
```

Now merge.
```
git merge fb_workitem001
```

Look at the git log and the graph of your graphical git client.

Delete the feature branch when it's no longer needed.

```
git branch -d fb_workitem001
```

## Release branch

Create and switch to a release branch named *rb_18_06*

```
git checkout -b rb_18_06
```

Commit new changes to the release branch.
The changes can be commits - directly or through various feature branches merged to this release branch.

Feature branches can exist directly on a release branch iff (if and only if) these features will be released in this release. 
Otherwise a feature should be on the  *develop* branch or the relevant release branch. 

> **Note:** As a feature can often be moved between releases, 
it is recommended to keep feature branches on a non-release branch, like a develop branch.

## Release merge and tag

> **Note:** The following is one working workflow of code management in software development. 
Other workflows are possible and should be explored depending on use-cases & requirements.

When ready to release 18_06, the release branch will be merged with the head of the
*main* branch. 
Tag:s will be added to the merge-point of the head of the *main* 
branch and to the last (release) commit of the release branch.
The release will also be merged to the head of the develop branch for consistency.

```
# set rb-tag in the release brach
git tag rbt_18_06
# checkout main branch
git checkout the main branch
# merge main with the release branch
git merge rb_18_06
# set release-tag in master
git tag 18_06
# checkout the develop branch
git checkout develop
# merge develop with the release branch
git merge rb_18_06
```

After a release is complete, no further work is made on the release branch - unless there is a need for a patch release.

## Patch release

A patch release is pretty much the same as a regular release. 
Work is just continued on the relevant release branch and when done, 
the same procedure for a regular release is performed - but with new a tag denoting that it is patch release.

```
# checkout the release branch
git checkout rb_18_06
# make some changes...
git add .
git commit -m "Changes for patch release"
# set tag in the release brach
git tag rbt_18_06_01
# checkout master
git checkout the master branch
# merge master with the release branch
git merge rb_18_06
# set tag in master
git tag 18_06_01
# checkout the develop branch
git checkout develop
# merge develop with the release branch
git merge rb_18_06
```
