---
layout: post
title:  "useful git operation"
date:   2016-01-12 21:21
categories: useful
tags: git useful
excerpt: 
---
* content
{:toc}

## Multiple git account[^3]

[^3]: [Multiple git account](http://mherman.org/blog/2013/09/16/managing-multiple-github-accounts/#.Vp7VHTZZEUV)

You can configure git to associate your login credentials specific to the repository path rather than the domain (which is its default behavior).

In terminal enter the command

```
git config --global --edit
```
This will open a configuration file. If you haven't already, you may want to set your default editor so the file opens in your preferred application.. For example, to set Sublime Text as your default editor: git config --global core.editor "subl -n -w"

With the config file opened, search for useHttpPath (or define it if it doesn't exist). And set it's value to true. It should look like this:

```
[credential]
helper = osxkeychain
useHttpPath = true
```

This will instruct git (as well as github) that any credentials used to login should only be associated with the full repository path that was queried, not for the entire domain (in the case of github) all repositories on Github.com..


## Switching remote URLs[^0]
[^0]: [Switching remote URLs](https://help.github.com/articles/changing-a-remote-s-url/)

List your existing remotes in order to get the name of the remote you want to change.

```
git remote -v
```

Change your remote's URL

```
git remote set-url origin https://github.com/USERNAME/OTHERREPOSITORY.git
```


## Use repo tags[^1]
### Creating and listing tags
To create an annotated tag, enter the following:

```
$ git tag -a 1.0 -m 'the initial release'
```

To create a lightweight tag, enter the following:

```
$ git tag 'tempTag'
```

A regular push command won't push a tag, to push all your tags:

```
git push origin --tags
```

To push a single tag:

```
git push origin : tempTag
```

To list the tags in a repo, enter the following:

```
$ git tag
```

### delete tags
You just need to push an 'empty' reference to the remote tag name:

```
git push origin :tagname
```

Or, more expressively, use the --delete option:

```
git push --delete origin tagname
```

If you also need to delete the local tag, use:

```
git tag -d tagname
```

[^1]: [Use repo tags](https://confluence.atlassian.com/bitbucket/use-repo-tags-321860179.html)

### Contribute to an existing project

1. fork to your repository
2. git remote add newname the-url-of-original-project
3. git pull newname master

### sync with latest upsteam code

```
git stash //stash your change
git fetch main master
git rebase main/master
git stash appy
```

if has conflicts, solve conflicts and use 

```
git add conflict_file
```
## Reference
