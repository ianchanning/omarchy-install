---
title: first-steps-after-installing-fedora
updated: 2022-01-30 01:35:22Z
created: 2021-10-31 16:11:36Z
tags:
  - laptop
---

# First steps after re-installing Fedora

- Installed ansible

## Link up keepass and firefox
- created some playbooks (all in the `~/ansible` directory)
- got this to install keepass
- keepass can be installed via `sudo dnf install keepass`
- need to install keepasshttp too
- these two steps are now in `~/ansible/keepass.yml`
- I was assuming that I needed to install Dropbox then to be able to use the keepass database, but the file is in Windows
- So connect to the windows share and run the `Dropbox/main.kdbx` file

## Install Dropbox
- I forgot to do these first 3 steps - but it matters so that the path to the Windows share is set nicely
- I went into Windows and (via Add / Remove Partitions) set the name to the Windows partition to `Windows`. If this is already set then you don't need to change it.
- On GHANDI this had been `Windows8_OS`
- Then the Windows partition is mounted as `/run/media/ian/Windows`
- I've now created an Ansible playbook to download and install Dropbox
- You then need to set a custom path for the installation to point to your Windows dropbox location
- for me this is `/run/media/ian/Windows/Dropbox` (you don't specify the Dropbox, Dropbox adds that for you)

## Install Sublime Text 3


# First steps after installing Fedora 34 on Stafke
### 2021-05-15

## Installed flatpak

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub org.gimp.GIMP
flatpak install flathub nz.mega.MEGAsync
flatpak install flathub com.spotify.Client
flatpak install flathub com.discordapp.Discord
flatpak install flathub com.microsoft.Teams
flatpak install flathub com.github.micahflee.torbrowser-launcher
flatpak install flathub org.videolan.VLC
flatpak install flathub com.visualstudio.code
flatpak install flathub org.chromium.Chromium
flatpak install flathub org.mozilla.Thunderbird
flatpak install flathub org.zealdocs.Zeal
flatpak install flathub org.keepassxc.KeePassXC
flatpak install flathub com.simplenote.Simplenote
flatpak run io.neovim.nvim
```

The only thing I definitely can't install from flatpak (which is what stops me from using Silverblue) is virt-manager.

## Neovim

I hit [problems with the flatpak Neovim](https://vi.stackexchange.com/questions/31300/neovim-terminal-shows-a-different-list-of-programs-available-in-usr-bin/31301#31301).

```sh
flatpak install flathub io.neovim.nvim
```

The terminal is stuck inside a sandbox, so Neovim can't find node which is needed for coc.nvim.

So I just reverted to `sudo dnf install -y neovim python3-neovim`.

There are some [Flatpak install notes](https://github.com/neovim/neovim/wiki/Installing-Neovim#flatpak) but I don't really care, I just want it to work.

## Install virt-manager

See [fedora-virtualization.md](../undefined).

## Install Firefox sync

Once KeepassXC and MEGASync are installed I can sync down the passwords file and enter the sync password for my firefox account. This synced history and I've included extensions as well now. My list of extensions is fairly standard. With Leechblock you can save the settings to the sync storage and then import them on the new machine.

## Install Private Key

I did a simple `ssh-keygen` and created a test key to see what the permissions should be. Then I created the `id_rsa` key from Keepass.

## Install Node

Neovim needs node installed

```sh
sudo dnf module install nodejs:14/default
```

## Install ripgrep, fzf, fd

```sh
sudo dnf install -y ripgrep fzf fd-find
```

# First steps after installing Fedora 34 on XPS imec laptop
### 2021-10-19

## Remember

The primary reason for this system is work and a stable system.

Software wants to be kept to an absolute minimum.

## Upgrade fedora

Run `sudo dnf upgrade --refresh`

**Restart...**

Install any firmware upgrades via Software > Updates (tab)

**Restart...**


## Install flatpak

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

**RESTART**

```sh
#flatpak install flathub nz.mega.MEGAsync # install via deb/rpm
flatpak install flathub com.microsoft.Teams
flatpak install flathub org.chromium.Chromium
```

**Note: It is better to install MEGA via deb/rpm then you can integrate it with Nautilus and set it to auto-start**

For MEGA, I did a Full Install to `/home/ian/MEGA`.

Need the Pass, notes and log directories from MEGA.


```sh
cd ~
ln -s MEGA/Notes
ln -s MEGA/log
```

These might be useful for the future but aren't required for now.

```sh
flatpak install flathub com.spotify.Client
flatpak install flathub com.simplenote.Simplenote
```

## What *not* to install via flatpak

VS Code - this complains that it is a non-standard build that won't have proper network access

Neovim - problems with it being trapped in a sandbox

Full list on Stafke:

```sh
Adwaita theme
Chromium Web Browser
Codecs
Discord
Fedora Media Writer
Freedesktop Platform
Freedesktop SDK
GNOME Application Platform version 40
GNU Image Manipulation Program
GnuCash
Intel
KDE Application Platform
KeePassXC
MEGAsync
Manual
Mesa
Name
QGnomePlatform
QGnomePlatform-decoration
QtSNI
Signal Desktop
Simplenote
Spotify
Thunderbird
Tor Browser Launcher
VLC
Zeal
calibre
html5-codecs
openh264
```

## Firefox account sync

Just type password from other machine

Setup 1password

Once GNOME Extensions plugin shows in Firefox enable the extension that allows displaying taskbar icons

This synced history and I've included extensions as well now. My list of extensions is fairly standard. With Leechblock you can save the settings to the sync storage and then import them on the new machine.

## Neovim

Need this for private key writing

`sudo dnf install -y neovim python3-neovim`.

## Install Private Key

Need this for dotfile and any git access

I did a simple `ssh-keygen` and created a test key to see what the permissions should be. Then I created the `id_rsa` key from Keepass/1Password.

The first time you use git clone with the SSH key and you get asked for the password - fedora asks you if you want to open the SSH key every time you log-in. **Click yes** ;) Otherwise you have to handle various problems involving running `ssh-add` at logon see <https://rabexc.org/posts/pitfalls-of-ssh-agents>. But most of my attempts failed except for manually running `ssh-add` once I'd logged in.

## Install 1password?

<https://support.1password.com/install-linux/#centos-fedora-or-red-hat-enterprise-linux>

It does seem to be helpful - and on Ubuntu you can unlock the 1password when you log in.

## Install Node

Neovim needs node installed

```sh
sudo dnf module install nodejs:12/default
```

## Install ripgrep, fzf, fd

```sh
sudo dnf install -y ripgrep fzf fd-find
```

To add the FZF key-bindings see this [issue #2542 comment](https://github.com/junegunn/fzf/issues/2542#issuecomment-951988229) I posted, then append this line to `~/.bashrc` to enable fzf keybindings for Bash:

```sh
source /usr/share/fzf/shell/key-bindings.bash
```

We have to do something similar on the Ubuntu VM

## Install dotfile repo

N.B. On my work machine initially I didn't install the vimrc file because I want to keep it as simple as possible until I need it. So I'll use the basic NVim until I need some other parts of it.

Need this for vimrc with neovim. Neovim does work ok without needing all my customisations.

```sh
cd ~
git clone git@github.com:ianchanning/dotfile.git
cd dotfile
git checkout linux
```

## Install virt-manager

See [fedora-virtualization.md](../undefined).

