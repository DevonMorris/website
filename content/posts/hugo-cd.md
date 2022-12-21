+++
draft = false
date = 2022-12-21T13:10:26-05:00
title = "Cheap Continuous Deployment for Hugo"
description = "Cheap Continuous Deployment for Hugo"
slug = ""
authors = []
tags = ['website']
categories = []
externalLink = ""
series = []
+++

I host my own website on a [Linode VPS](https://www.linode.com/). At this point,
I've forgotten all the details of how I set up my VPS a couple of years ago but
the system has been rock solid. I'm pretty sure I followed [guides](https://www.linode.com/docs/guides/set-up-web-server-host-website/)
[like](https://wiki.debian.org/UnattendedUpgrades)
[these](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
[ones](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04).

I even get emails about the upgrades that take place on the server and generally
things just work. I mostly manage this server by forgetting that it exists and
creating a couple of git repositories every once in a while.
Overall, I'm just happy to have a pretty cheap VPS that performs backups and
that I _mostly_ understand.

One downside to managing my own server, however, is that I don't get to take
advantage of cool [Github actions](https://github.com/features/actions) or
[Gitlab CI](https://docs.gitlab.com/ee/ci/). The features that these CI pipelines
have out of the box are incredible. I know that I could host my own instance
of Gitlab, Jenkins, Gitea + Drone CI, *<insert your favorite CI pipeline here>*,
but honestly, it just seems too heavy for my use case which is to run a single
`hugo` command and copy some files to my production directory. So here's my
cheap, quick and _dirty_ continuous deployment "pipeline" for building my
website.

## Git Hooks

If you don't know about [git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks),
you should really go and read about them. Basically, hooks are custom code that
run when certain git events are triggered. You can use these hooks to do all
kinds of things:
* Format code on commit
* To check commit convention
* Prevent rebases or merges
* Send an email when certain branches update
* Generate a static website

**Disclaimer**: Sometimes this can be **really** annoying. A pre-commit hook can make a
commit fail if certain conditions aren't met. You can always skip hooks with the
`--no-verify`, but my general advice would be "just because you can, doesn't
mean you should." Think of your team (or your future self) before going ham
on the hooks. Furthermore, client-side hooks are [generally non-enforceable](https://stackoverflow.com/questions/40156102/git-hook-automatic-installation)
(nor should they be from a security perspective). You'll have to _encourage_
your team to manually install the hooks. In short, there are usually better
ways to accomplish automation. The use case presented in this post, in my opinion, is the
perfect usage of hooks since it's small, it's personal and it only impacts me (in a
positive way).

## Hugo Continuous Deployment

I'm hesitant to call this "continuous integration" since the amount of error
checking isn't very intelligent, however, it most definitely is continuous
deployment. Every time an update is pushed to the site, the website will be
rebuilt and deployed. Here is a `post-receive` hook that does the job for us.
It's also POSIX shell compliant (for people who care about that).

```bash
#!/bin/sh -e

# The production directory
TARGET="/var/www/devonmorris.dev"
# A temporary directory for deployment
TEMP="/tmp/devonmorris.dev"
# The Git repo
REPO="/srv/git/website.git"

# Checkout the content to the temporary directory
rm -rf $TEMP
mkdir -p $TEMP
git --work-tree=$TEMP --git-dir=$REPO checkout -f
cd $TEMP
git --work-tree=$TEMP --git-dir=$REPO submodule update --init --recursive

# Generate site with Hugo
hugo

# Deploy website
rsync -av --delete public/ $TARGET
```

This script should be pretty self-explanatory, but at a high-level it performs
the following

* Sets error flag to exit script if any command fails[^error]
* Checks out the repository and submodules into a clean temporary directory
* Attemps to generate the website
* Deploys the website via `rsync`

[^error]: This will prevent blowing away your website if it fails to generate. But also, shame on you if you push code that doesn’t build
