---
title: First steps after installing Pop OS
updated: 2025-06-30 19:19:13Z
created: 2022-01-29 16:56:52Z
latitude: 51.14120000
longitude: 5.60450000
altitude: 0.0000
tags:
  - linux
  - mona
  - work
---

# First steps after installing Pop OS

This is still a disaster of a 2 day project to switch from one computer to the next.

It should be a couple of bullet-proof simple ass commands.

DejaDup is definitely great (but making sure correct `/home` directory names was a problem).

See [Dear God give me a simple one-true-way way of migrating between Ubuntu laptops](../undefined)

## Package sources

In the settings for the Pop!\_Shop, I noticed that it was using the US archive for the sources.

Reading up on this [linuxconfig apt mirror howto](https://linuxconfig.org/how-to-select-the-fastest-apt-mirror-on-ubuntu-linux), its clear that using the belgian archive (<http://be.archive.ubuntu.com/ubuntu/>) is much faster than the US one (<http://us.archive.ubuntu.com/ubuntu/>). So I've added the Belgian one and removed the US one. There is a slightly faster one that you can find via <http://mirrors.ubuntu.com/mirrors.txt>, but I'd rather use the most official looking one. 

**24/07/2024 On later installs I left it as the pop os repository**. I think they switched from a default Ubuntu US repository to their own.

Reminder:

`sudo apt update && sudo apt upgrade -y` 

This is the correct way to [update Ubuntu via the cli](https://itsfoss.com/update-ubuntu/)

Note use `apt` not `apt-get` - apt is like `dnf` and is the simple wrapper command.

## Install DejaDup

- Deja Dup (flatpak)
	- `flatpak install flathub org.gnome.DejaDup -y`
 - restore backup from External hard drive

If you have a proper backup ([Backing up Pop!\_OS](../undefined)) this should help a lot with the following.

It didn't solve everything but it has some lovely touches:

- Installs the Iosevka fonts
- Installs the gnome-terminal colours and uses the Iosevka fonts
- Keeps the bash history
- keeps the dotfile (so .bashrc and .vimrc)
- once I properly restored neovim everything was still in `.vim` folder so worked beautifully - treesitter and everything
- all work projects neatly installed
	- was missing pnpm though
- still need to check docker, azure-cli etc
- still need to check python venv

## Installed any firmware

The I got popups suggesting 'Firmware updates are available'. I installed these one by one. There was 4 of them.

## Update flatpak list

The DejaDup backup installs the programs in the `~/.var` directory but doesn't handle updating the flatpak list.

I had to copy across the flatpaks list <https://www.reddit.com/r/linux/comments/u3wcm7/easy_flatpak_apps_backupinstallation/>

```sh
flatpak list --columns=application --app > flatpaks_$(date +%F).txt
```

I put this in the MEGA/DownloadSync folder.

restore with 

```sh
xargs -a flatpaks_2024-07-24.txt flatpak install -y
```

## Install apt packages

I did a backup of the list of apt packages installed (this includes PPAs it would seem) using `apt-clone`

<https://askubuntu.com/a/486634/8989>

```sh
sudo apt-clone clone apt-clone-state-ubuntu-$(lsb_release -sr)-$(date +%F).tar.gz
```

I didn't backup the manually installed `*.deb` packages that maybe I should have done. This included:

- MEGA
- 1Password
- tmetric
- rescuetime

### PPAs

Also quite a few others didn't get installed. At least the PPAs.

- Neovim unstable/nightly didn't get installed
- Postgres got installed but only v14 not v15

The `apt-clone` would appear to be incompatible with Pop OS. It tries to use a `sources.list` and creates a `/etc/apt/sources.list.apt-clone` but that gets rejected - the file just contains comments that say `jammy main restricted`.

Jesus fucking Christ... Linux has its own package manager, its had it for 20 years and its still a fucking mess. Moving from one computer to another should be a total fucking breeze. But here I am 5 hours later trying to get shit working.

So even if you do get the correct sources list, [you don't get any of the keys](https://askubuntu.com/questions/148932/how-can-i-get-a-list-of-all-repositories-and-ppas-from-the-command-line-into-an?noredirect=1&lq=1#comment2673659_148968). The keys are somehow spread out across different directories that different people decide to use.

For extra fun, the main command `apt-key` is deprecated: <https://askubuntu.com/questions/1286545/what-commands-exactly-should-replace-the-deprecated-apt-key> / <https://unix.stackexchange.com/questions/332672/how-to-add-a-third-party-repo-and-key-in-debian/582853#582853>

DejaDup was really nice - home directory + flatpak apps installed - but it missed off updating the flatpak list...

So with some gypting (gypsying???) I got a script to get all the PPA sources and their keys, the restore the PPAs and do an apt update.

There are two files `backup_ppas.sh` and `restore_ppas.sh` that are in `MEGA/Downloadsync`.

But then you also need to run the `apt install` commands??? I think these got run somehow. I had to run a `sudo apt update && sudo apt upgrade -y` after the `restore_ppas.sh` script which tried and failed 37 times to install 37 installable packages.

This script should give you all the `apt(-get) install` commands:

```sh
zcat /var/log/apt/history.log.*.gz | cat - /var/log/apt/history.log | grep -Po '^Commandline:(?=.* install ) \K.*' | sed '1,4d'
```

-- ([source](https://askubuntu.com/a/680405/8989))

## Install 1Password

This is simply the most important tool - everything else is based off of it. 

If you have restored a backup then 1Password should be properly setup once you install it.

Follow the [ubuntu guide](https://support.1password.com/install-linux/#debian-or-ubuntu).

* I ticked the checkboxes when installing it to unlock 1Password when unlocking my account - but it doesn't seem to work with Ubuntu. It works beautifully with Fedora but not with Ubuntu or PopOS.
	* I found that this was because of using Firefox flatpak
	* Also you can link the App and the Login but you always have to unlock it once with the password for each login session

To make sure that the Firefox extension gets unlocked follow the advice in [ThinkPad E15 Pop OS teething problems](../undefined) then get the app to start at login. There doesn't appear to be an option to start it within the app settings so I added it to the 'Startup Applications', the command is just `1password` 


## Linked Google and Next cloud as online accounts

**I'm not sure if this is worth it**

You can link Google contacts to the Gnome Contacts, but you need to add a Google account to Thunderbird to sync the Google contacts with Thunderbird.

## Install Vim / NVim

### Pre-requisites

#### Installing node

You can't run Vim with coc or neovim until node is installed.

Via [Node Ubuntu installation](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions) I installed it using [Node source](https://github.com/nodesource/distributions/blob/master/README.md#installation-instructions) 

This installs npm and then I also [installed yarn](https://yarnpkg.com/getting-started/install). This now recommends using `sudo corepack enable` \- note that I have to manually add the sudo as I install node via `apt`

~~From copying Jonathan I installed the `n` [node manager](https://github.com/tj/n)~~

I've switched to `nvm` as its one of the default ways of installing node. Plus its a nightmare trying to search for `n` on the god damn internet.

#### Install fzf, ripgrep, fd, git, bat, pandoc

**24/07/2024 With a restored backup of the home directory with `.fzf` in it, FZF works fine**

**24/07/2024 With the restored apt packages, `fd`, `bat` and `rg` were all good**

git and curl should be installed, but make sure.

install fzf from git

then `sudo apt install ripgrep fd-find bat pandoc -y`

I also installed the Github CLI - again, same as ripgrep they [recommend against installing the snap package](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#snap-do-not-use).

Both `fd` and `bat` have conflict packages so in `.bashrc`:

```sh
alias fd='fdfind'
alias bat='batcat'
```

> **Note:** bat is recommended by [nb](https://github.com/xwmx/nb#optional)

#### Install

**24/07/2024 With the restored apt packages, these were all good**

Actually no - I had neovim installed, but not the nightly package that I need. Its strictly called the 'neovim-ppa-ubuntu-unstable' PPA.

`sudo apt install vim-gtk3 neovim python3-neovim -y`

#### Nvim git integration/config

(Not required if you restore a backup)

```
git config --global user.email <mail git email for this laptop>
git config --global core.editor nvim
# https://github.com/tpope/vim-fugitive/issues/1306
git config --global mergetool.fugitive.cmd 'nvim -f -c "Gvdiffsplit!" "$MERGED"'
git config --global merge.tool fugitive
```

See this [gist](https://gist.github.com/karenyyng/f19ff75c60f18b4b8149) for `git mergetool` info.

## Install MEGA and joplin
- Mega + nautilus (deb) - only download basics to prevent going over download limit
	- [MEGA Desktop App](https://mega.io/sync)
- Joplin (flatpak) - **needs MEGA** 
	- `flatpak install flathub net.cozic.joplin_desktop -y`
 
## Goodies / deb
- VS Code (Pop!\_shop) - this installs GPG key etc for regular updates
- Slack (Pop!\_shop)

## Goodies / flatpak

Went to the Pop!\_shop and installed whatever goodies I liked the look of.

~~- uninstalled Firefox (deb)~~
~~- installed Firefox (flatpack) - the flatpak is more up-to-date~~

> **Note:** from trouble shooting the 1Password app/browser integration ([ThinkPad E15 Pop OS teething problems](../undefined)), I think that it only properly works if you have the deb installed and not Flatpak

- Spotify
- gimp
- meld
- postman
- signal desktop
- thunderbird - if you install the ian.c.channnig google account it will also sync your contacts
- DBeaver

### Flatpak list

As of 2022-01-30 17:31 installed flatpak apps:

> **Note:** you don't need all of these, but this is like a full reference list

```
ian@stafke:~$ flatpak list --columns=application
Application ID
com.getpostman.Postman
com.github.micahflee.torbrowser-launcher
com.spotify.Client
io.dbeaver.DBeaverCommunity
io.github.shiftey.Desktop
net.cozic.joplin_desktop
org.chromium.Chromium
org.gimp.GIMP
org.gnome.DejaDup
org.gnome.meld
org.mozilla.Thunderbird
org.mozilla.firefox
org.signal.Signal
org.videolan.VLC
```

You can add `flatpak install flathub -y` in front and then use that as the install list.

> **Note:** you can now update all with `flatpak update -y`  - so add this to the end of the `sudo apt update`

**See above for using the flatpak list to reinstall flatpak apps**

### Matching VS Code with NVim/Vim

I want to try to get nvim/vim as close as possible to vs code.

First steps:

1. In both - Match Solarized light/dark themes
2. In both - Match fonts - Source Code Pro for Powerline
3. In VS Code - Match status bar size - increase 'Zoom level' by 20% then decrease font size to 14
4. In VS Code - Hide the menu bar, activity bar and sidebar leaving just tabs
5. In VS Code - Set a shortcut Ctrl+k Ctrl+b to show hide sidebar
6. In Vim - Add `set number` and `set nowrap` to `.vimrc`

![Screenshot from 2022-01-30 17-06-00.png](../_resources/Screenshot%20from%202022-01-30%2017-06-00.png)

## Keyboard shortcuts

Turn on the tiling windows (Super + Y)

Then the main 4 that I needed to customise were all in Settings > Keyboard > Keyboard Shortcuts > Launchers:

* Home folder (super + e)
* Launch email (disabled)
* Launch terminal (ctrl + alt + t)
	* You might need a custom one to start `gnome-terminal` if Alacritty has taken over
* Launch browser (disabled)

There's some others that are different that I accepted:

* Lock (super + esc)
* Super + left / right doesn't snap 

## Database tools and Postgres

DBeaver (installed with flatpak above) - <https://github.com/dbeaver/dbeaver>

### Postgres

From <https://www.postgresql.org/download/linux/ubuntu/>

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt update
sudo apt -y install postgresql
```

Also from [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart) some instructions for running psql commands.

#### ~~pgadmin~~ (deprecated for DBeaver)

pgadmin is now [available for impish](https://www.pgadmin.org/download/pgadmin-4-apt/):

```
sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'
# Install for desktop mode only:
sudo apt install pgadmin4-desktop
```

## Fonts

**25/07/2024 These get restored by Deja Dup now**

I have these two requirements for code fonts so that I can use them everywhere:

1. It works in gnome-terminal.
2. It works perfectly with powerline (no fracture lines)

### Iosevka

Installing - ~~download the TTC font files~~ the Super TTC font file will work, unzip and copy to `~/.local/share/fonts`

This copy command worked nicely for ~~all base, term and fixed TTC files~~ the Super TTC file:

```sh
cd Downloads
cp */*.ttc ~/.local/share/fonts

# if using Super TTC
cp sgr-iosevka-term.ttc ~/.local/share/fonts
```

I searched for ages for a font that I like that I can install on everything - my prayers were finally answered when I came across [Iosevka](https://github.com/be5invis/iosevka).

Hat tip to the [generic list from slant.co](https://www.slant.co/topics/7014/~fonts-to-use-in-a-terminal-emulator) where I found it.

It took a while to figure out how to download it, but it has a lovely [packaged system](https://github.com/be5invis/Iosevka/blob/v14.0.0/doc/PACKAGE-LIST.md).

I love when maintainers go to this level of depth to make it easy for you to install.

| Option         | Contents                                            | Description                                                  |
| -------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| Super TTC | 1 `.ttc` file                                       | Bundles all fonts in the scope together into a single file. It is the recommended way to install fonts for Desktop usage, if your operating system is updated to date. Package files with `-sgr-` infix in the filename only contains fonts for one single group (variant and spacing). |
| TTC            | 9 `.ttc` files                                      | Each TTC file bundles fonts with the same weight together.  Package files with `-sgr-` infix in the filename only contains fonts for one single group (variant and spacing).   |
| TTF            | 54 `.ttf` files                                     | Each TTF file contains one font for a specific weight, width, slope and spacing variant. This option is ideal for embedding Iosevka into applications, or for desktop usage if TTC options have compatibility issues.<br/>TTF packages also provide *unhinted* version which removes [hints](https://en.wikipedia.org/wiki/Font_hinting), which reduced file size, but will be less clear on certain platforms. |
| WebFont        | 1 `.css` file + 54 `.woff2` files + 54 `.ttf` files | Contains contents required to use Iosevka on websites.       |

<table>
<tr><td colspan="3"><b>&#x1F4E6; Iosevka</b> — <i>Monospace, Default</i></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/super-ttc-iosevka-14.0.0.zip">Super TTC</b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttc-iosevka-14.0.0.zip">TTC</b></td><td colspan="3">&nbsp;</td></tr>
<tr><td><b>&nbsp;&nbsp;└ Sub-packages</b></td><td><b>Spacing</b></td><td><b>Ligatures</b></td><td colspan="5"><b>Downloads</b></td></tr>
<tr><td>&nbsp;&nbsp;&nbsp;&nbsp;├&nbsp;<b>Iosevka</b></td><td>Default</td><td><b>Yes</b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/super-ttc-sgr-iosevka-14.0.0.zip">Super TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttc-sgr-iosevka-14.0.0.zip">TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-iosevka-14.0.0.zip">TTF</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-unhinted-iosevka-14.0.0.zip">Unhinted</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/webfont-iosevka-14.0.0.zip">WebFont</a></b></td></tr>
<tr><td>&nbsp;&nbsp;&nbsp;&nbsp;├&nbsp;<b>Iosevka Term</b></td><td>Terminal</td><td><b>Yes</b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/super-ttc-sgr-iosevka-term-14.0.0.zip">Super TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttc-sgr-iosevka-term-14.0.0.zip">TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-iosevka-term-14.0.0.zip">TTF</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-unhinted-iosevka-term-14.0.0.zip">Unhinted</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/webfont-iosevka-term-14.0.0.zip">WebFont</a></b></td></tr>
<tr><td>&nbsp;&nbsp;&nbsp;&nbsp;└&nbsp;<b>Iosevka Fixed</b></td><td>Fixed</td><td>No</td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/super-ttc-sgr-iosevka-fixed-14.0.0.zip">Super TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttc-sgr-iosevka-fixed-14.0.0.zip">TTC</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-iosevka-fixed-14.0.0.zip">TTF</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/ttf-unhinted-iosevka-fixed-14.0.0.zip">Unhinted</a></b></td><td><b><a href="https://github.com/be5invis/Iosevka/releases/download/v14.0.0/webfont-iosevka-fixed-14.0.0.zip">WebFont</a></b></td></tr>
<tr><td colspan="8"><img src="https://raw.githubusercontent.com/be5invis/Iosevka/v14.0.0/images/iosevka.light.png#gh-light-mode-only"/><img src="https://raw.githubusercontent.com/be5invis/Iosevka/v14.0.0/images/iosevka.dark.png#gh-dark-mode-only"/></td></tr>
</table>

Lovely things:

1. It works in gnome-terminal.
2. It works perfectly with powerline without needing the patched fonts.

It's similar to Hack in that respect, but I like the look of Iosevka more.

These two requirements are fundamental and rule out lots of lovely fonts.

Source Code Pro was great for a long time, but it's a bit boring.

I'm a fan of Typewriter fonts - especially the Latin Modern Mono (The 'Prop' font derivative has a perfectly round 'O' that I prefer).

The DSE Typewriter font works in gnome-terminal, but not in powerline. It use the powerline extra glyphs at all for some reason.

## Photos - gThumb

For managing Photos I've decided that [importing via gThumb](https://help.gnome.org/users/gthumb/unstable/gthumb-importing.html.en) is the best:

```sh
sudo apt install gthumb -y
```


## Mona specific instructions

Install Docker - most notes should be in the README of the health-app now.

* [Working through install](../undefined)
* [Mona app trouble-shooting](../undefined)
* [ThinkPad E15 Pop OS teething problems](../undefined)
* [Backing up Pop!\_OS](../undefined)


## WezTerm

More fucking around and finding out...

WezTerm allows customisation through Lua, so we can fuck with systems using Lua.

But then you need a custom instruction to open the terminal.

This **requires** that you have [downloaded the AppImage](https://wezterm.org/install/linux.html) and then installed it using the AppImageLauncher into `~/Applications`

> Lol, don't forget the damn `launch-wezterm.sh` custom script

```sh
bash -c "~/.local/bin/launch-wezterm.sh"
```

(Mini trick for WezTerm - `alt + enter` gives full screen)
 
tags: #linux #mona #work