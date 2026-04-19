---
title: First time install for Charp Pop_OS account
updated: 2023-12-11 13:40:07Z
created: 2023-10-02 08:56:58Z
latitude: 50.87984380
longitude: 4.70051760
altitude: 0.0000
tags:
  - charp
  - work
---

# First time install for Charp Pop_OS account

Read this: [First steps after installing Pop OS](../undefined)

### Already installed, just needed configuring:

- 1Password
- Firefox
- tMetric
- VSCode
- Alacritty
	- `alacritty.yml` is in dotfile
	- need to install Iosevka (see [First steps after installing Pop OS](../undefined))
	- need to install the `atom_one_light.yaml` theme <https://github.com/alacritty/alacritty-theme#installation>
- Pop OS settings
	- Dock 
		- Center
		- Smart hide
	- Light theme

I didn't connect Google

### Needed installing

- Any flatpak programs
	- Slack
	- DBeaver
	- Thunderbird + ian@ianchanning.com account + ian_channing@hotmail.com account
- Github invitation was sent to ian_channing@hotmail.com
	- Ooh now I can use GitHub desktop
- dotfile (I added `.bashrc` and `alacritty.yml`)
- fzf
    ```sh
	git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ~/.fzf/install
	```
- duckdb:
    ```sh
	curl -JLO https://github.com/duckdb/duckdb/releases/download/v0.9.0/duckdb_cli-linux-amd64.zip
	unzip duckdb_cli-linux-amd64.zip
	# https://unix.stackexchange.com/a/36874/8224
	mkdir ~/.local/bin/
    mv duckdb ~/.local/bin/
	```

### ssh

I can install this from 1Password. I first did the `ssh-keygen` trick to create example keys with the correct permissions.


### Accounts created / logged into

- Google
- Notion
- Slack
- Figma
- GitHub

### Migration settings from mona account

Flatpak:

```sh
Name                              Application ID                            Version                 Branch    Installation
calibre                           com.calibre_ebook.calibre                 6.27.0                  stable    user
Foliate                           com.github.johnfactotum.Foliate           2.6.4                   stable    user
Flatseal                          com.github.tchx84.Flatseal                2.1.0                   stable    user
Google Chrome                     com.google.Chrome                         117.0.5938.132-1        stable    user
VueScan                           com.hamrick.VueScan                       9.8.01                  stable    user
draw.io                           com.jgraph.drawio.desktop                 22.0.0                  stable    user
Webfont Kit Generator             com.rafaelmardojai.WebfontKitGenerator    1.1.0                   stable    user
Slack                             com.slack.Slack                           4.34.120                stable    user
Spotify                           com.spotify.Client                        1.2.13.661.ga588f749    stable    user
PDF Mix Tool                      eu.scarpetta.PDFMixTool                   1.1.1                   stable    user
DBeaver Community                 io.dbeaver.DBeaverCommunity               23.2.0                  stable    user
Flowtime                          io.github.diegoivanme.flowtime            4.1                     stable    user
GitHub Desktop                    io.github.shiftey.Desktop                 3.3.1-linux1            stable    user
Joplin                            net.cozic.joplin_desktop                  2.12.18                 stable    user
Chromium Web Browser              org.chromium.Chromium                     117.0.5938.132          stable    user
GNU Image Manipulation Program    org.gimp.GIMP                             2.10.34                 stable    user
Déjà Dup Backups                  org.gnome.DejaDup                         45.1                    stable    user
Meld                              org.gnome.meld                            3.22.0                  stable    user
Inkscape                          org.inkscape.Inkscape                     1.3                     stable    user
Thunderbird                       org.mozilla.Thunderbird                   115.3.0                 stable    user
Signal Desktop                    org.signal.Signal                         6.32.0                  stable    user
Zeal                              org.zealdocs.Zeal                         0.6.1                   stable    user
```

Hmm - I need to add charp account to Deja Dup backups...

## Later updates

### Alacritty

Alacritty became annoying - it seemed to cause slowdowns when using Neovim inside Alacritty, which is insane when it is supposed to improve performance. So I scrapped it and went back to the Gnome terminal.

### Gnome terminal colours

I then wanted to change the terminal colours.

I don't like the default light VSCode colours.

I like the One Half Light colours.

But there was no good One Half Light VS Code theme. Actually the 'Atom One Light Theme' seems ok.

Close enough to the neovim one, although frustratingly different... ah Solarized...

### AppImage

I need two AppImage programs which I installed into `~/.local/bin`: 

- [MQTT Explorer](https://mqtt-explorer.com/)
- [Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager).

It became a pain to keep launching these from the command line and closing the terminal, so I installed [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) via the [PPA](https://launchpad.net/~appimagelauncher-team/+archive/ubuntu/stable).

These get moved to `~/Applications` and added to the launcher.