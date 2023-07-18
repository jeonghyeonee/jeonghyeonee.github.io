---
layout: post
title: Git rebase
categories: Git
lang: en
lang-ref: GitReabse
tags:  [Git, git, github, command]
---

## Git rebase

### Intro
When I tried to remove multiple commits which is not want to commit in GitHub, I used `git format-patch` command.  
But it is failed. Because the base is different to the first commit.
In this case, to change or merge the multiple commits in 1 commit, you can use `git rebase`.

### What is Git rebase
Reapply commits on top of another base tip

### How to Use
```
git rebase [-i | --interactive] [<options>] [--exec <cmd>]
	[--onto <newbase> | --keep-base] [<upstream> [<branch>]]
git rebase [-i | --interactive] [<options>] [--exec <cmd>] [--onto <newbase>]
	--root [<branch>]
git rebase (--continue | --skip | --abort | --quit | --edit-todo | --show-current-patch)
```

```
e.g.
# Move to the commit you want
git rebase -i <commitID>
```

### The Perils of Rebasing
**Do not rebase commits that exist outside your repository and that people may have based work on.**
Because git-rebase is making a new commit. Not use existing commis, but create new ones that are simillar.

### Rebase vs. Merge
There are two ways to integrate changes from one branch into another: `merge` and `rebase`.
Then, which one is better to use?  
The history commits show a record of what actually happened. In this point, if you change the history commits, you are lying about the history. In the other point of view, the history means the story of how your project was made. If you record the missteps, you don't want to publish the details of your code. So, in this case, you can rewrite commits using `rebase` to tell the story in the best ways to readers.
The answer of the question is that it is up to you to decide which one is best for your particulart situation.

### Outro
I use `git rebase` to clean up my commits in my local branch. Because I commited all of the process containing I don't want to publish to others, so I want to integrate in 1 commit to tell this project clearly to others. In this reason, I used rebase command.

#### References
- [Git rebase](https://git-scm.com/docs/git-rebasei)
- [Git rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
