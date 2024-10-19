---
title: "How I set up this Website"
date: 2020-06-13T21:35:01-07:00
tags:
 - dev
 - distractions 
 - website
categories:
 - Dev
---

As a reminder for future me, and anyone who is curious, this is the set of things
I used to set up this website.

<!--more-->

This site is set up using a combination of Github Pages for hosting, [Hugo](https://gohugo.io/)
for static site generation. On top of that, the actual "gruntwork" is being done on a Windows 10
machine, using [WSL2](https://docs.microsoft.com/en-us/windows/wsl/) for all the fun Linux tools.

The process for this site started with a Windows update. WSL2 was first included with the May 2020
update.  WSL2 has full kernel support, and IIRC, runs on a hypervisor, so it's pretty speedy.
In particular, I followed [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

Once I had an Ubuntu terminal running, I downloaded and install hugo from apt.  `sudo apt-get update` was necessary for `sudo apt-get install hugo` to work. 
From there I followed the [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/) to get
my bearings with Hugo, and get a shell of a site working locally. I cloned the [smol](https://themes.gohugo.io/smol/) theme for the site since it's minimal, and and I don't require a lot from the
theme right now. Hugo lets you change that up easily later, which I'll probably do if I manage
to keep up this hobby.

I do my editing using [VS Code](https://code.visualstudio.com/) which  I already had 
installed from a previous abandoned foray into using WSL. To my great pleasure, it was able to be
updated to the latest version easily enough. It does appear to be running on top of WSL, since
it's included in the title bar. 

For safety's sake I installed the 
[WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). I'm unsure if it's doing anything presently, since it's for opening folders in WSL, and I'm running in
WSL already. Better safe than inconvenienced. 

Since I'm using TOML for Hugo configuration, I also installed [Better TOML](https://marketplace.visualstudio.com/items?itemName=bungcip.better-toml). It mostly lets syntax highlighting and 
block commenting work properly. It's working out so far.

My process is simple right now: I open two WSL terminals, one is for the hugo dev server using `hugo server -D`  and the other is for arbitrary commands. I open VS code with `code`, and
create new pages with the default `hugo new posts/foo.md`. I'll probably see how much I can get
working in vs code natively eventually.

Once I had the initial site content, there was the matter of getting it to be available as a 
Github Pages site at https://lostluck.github.io. Fortunately, this is common enough that Hugo
has [a tutorial for it](https://gohugo.io/hosting-and-deployment/hosting-on-github/). I set up
a separate private repo for the "raw" site, and leave the Pages repo public.

The main gotchas I ran into revolved around Git and Github.

1. For user Github Pages, you *must* use a `master` branch. No other branches will work. `gh-pages` is
only for project pages.
    * That abandoned attempt from years ago had left me with a broken Pages site, and debugging 
    that was a few hours. Do not recommend.
    * This caused me to reset the `/public/` submodule several times too.
    * I wish I could use a different branch name, and avoid the slavery language. Instead, I'll
    have to convince myself it just means a master copy.
1. Setting up secure convenient command line push access. I used a Github Personal Access Token for this, and [this StackOverflow answer](https://stackoverflow.com/questions/18935539/authenticate-with-github-using-a-token) to correctly use it with the command line.
    * It was important to do this for the public directory as well. Otherwise the `deploy.sh` script Hugo would asking for my github credentials password every time. 
1. Since I had downloaded the `smol` theme with a `git clone` into the site repo, it was a submodule.
This would be fine if I didn't prefer a YYYY.MM.DD date format. The solution there was to rename the
smol directory, and commit the "delete" change, delete the smol directory's `.git` folder, and rename
it back to smol. This let me commit the theme to the site repo, vendoring it for my use. Now I can
have my own date format, but also mutate the theme without extra complexity.

Finally, I wanted to make use of the custom .dev domain I picked up last year. Github Pages
[supports custom Domains](https://help.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site) easily enough. I'm using Google Domains
as my DNS provider, so I found [an appropriate tutorial](https://dev.to/trentyang/how-to-setup-google-domain-for-github-pages-1p58) to streamline the setup, but the Github Documentation
is pretty clear.

-----

Edit 2020.06.21: When it needed to update, I discovered that my VS Code instance is
 installed in the Windows drive, and not in WSL. 
 That it all works, and I can seamlessly start the IDE from the WSL terminal, and
access the files is fantastic. 
It seems to be using the [remote development](https://code.visualstudio.com/docs/remote/remote-overview) under the head.
