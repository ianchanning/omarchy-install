---
title: First steps after upgrading to Pop OS v22.04
updated: 2025-04-08 17:47:46Z
created: 2022-06-21 09:49:51Z
latitude: 51.02587610
longitude: 4.47753620
altitude: 0.0000
tags:
  - mona
  - work
---

# First steps after upgrading to Pop OS v22.04

Following on from this:
[First steps after installing Pop OS](../undefined)

I let the GUI do the upgrade.

Downloaded over Oboelo network for faster downloads.

Most things upgraded fine but a few things had to be re-done.

1. Set the Pop OS store official sources to the Belgian mirror
2. 1Password, MEGA, Node, docker had all been removed. I think because all had PPAs pointing at old version of Pop OS
	1. For some reason when installing nodesource v16 it somehow conflicted with a node v12 that was already installed
	2. Had to do a `sudo apt remove nodejs`
	3. It looks like I don't really need the node manager `n` I'm going to be sticking to which ever version of node we have for mona portal - currently v16

The UI seems to have been improved for background transparency. I set the background back to the 'darkish' default Pop OS background and tried a solarized dark terminal theme. This seems to work quite well with transparency too.

I then set VS Code to have solarized dark and set the [dark background terminal colours](https://stackoverflow.com/a/59005337/327074) via <https://glitchbone.github.io/vscode-base16-term/>. 

These are the PPAs that didn't get updated:
```
 500 https://deb.nodesource.com/node_16.x impish/main amd64 Packages
 500 https://mega.nz/linux/repo/xUbuntu_21.10 ./ Packages
 500 https://download.docker.com/linux/ubuntu impish/stable amd64 Packages
 500 https://downloads.1password.com/linux/debian/amd64 stable/main amd64 Packages
```

No idea why :shrug:

I've got the Mona app running again. :tada:
tags: #mona #work