---
layout:     post
title:      Multiple git Identities
date:       2018-08-10
description:    As far as I know, there is no _nice_ way to manage multiple identities in git, this is how I get round that
tags:       git
---

I use my work machine for personal stuff sometimes. Shocker.

Because I don't want to commit to personal projects using my work git identity (and vice versa) I was looking at ways to manage multiple identities in git. No joy.

I use the fantastic [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt) which allows me to see my branch, upstream origin, and the status of my changes. I amended this slight to add in the value of `git config user.email` to see which identity was currently being used. My PS1 in a git directory now looks like this:

```
✔ ~/personal_repos/ols.wtf [oliver@leaversmith.com|master {origin/master}|✔]
```

I achieved this by adding the following to line 534 and 537 of `$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh`

```
local STATUS_PREFIX="${PROMPT_LEADING_SPACE}${GIT_PROMPT_PREFIX}$(git config user.email)|${GIT_PROMPT_MASTER_BRANCH}\${GIT_BRANCH}${R    esetColor}${GIT_FORMATTED_UPSTREAM}"
```

I then put these aliases in my `.bashrc` to switch between the two:

```
alias gitper='echo -n "Was " && git config user.email && git config user.signingkey && git config --global user.email "oliver@leaversmith.com" && git config --global user.signingkey 16503BFB && echo -n "Now " && git config user.email && git config user.signingkey'

alias gitwork='echo -n "Was " && git config user.email && git config user.signingkey && git config --global user.email "<work email>" && git config --global user.signingkey <work key> && echo -n "Now " && git config user.email && git config user.signingkey'
```

I have `git config --global commit.gpgsign true` set too, so that all my commits are signed, which is the reason for the signing key change too.
