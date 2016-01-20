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

## Reference