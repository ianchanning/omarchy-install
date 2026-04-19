---
title: first-steps-after-installing-windows
updated: 2026-04-19 14:44:17Z
created: 2024-09-06 21:12:27Z
---

# First steps after re-installing Windows

I need to install the most important to the least important.

I also want a setup that I can recreate easily.

The maslo hierarchy of needs for a computer.

## Bottom - Physiological

1. Create drive partitions before installing anything (in Windows 8)
1. Firefox
1. [Note down anything you forgot to update][1]
1. Install updates
1. Uninstall junk
   1. Market apps
   1. Programs
   1. Startup
   1. Turn off Bluetooth
1. Windows configurations
    1. What the power buttons do
1. Console (so that the following commands are tracked)
    1. [Console solarized registry colours][3] / Hard drive: `cmd-colors-solarized-dark.reg`
    1. [History][4] - I only need the custom history command. This can be done by creating a batch file using `type %USERPROFILE%\history.log` and putting it in the `C:\Windows\System32` directory.
    1. Actually that doesn't work - clink knackers up my history - doskey doesn't appear to work anymore
    1. But clink also saves a history which is in `%localappdata%\clink\.history` - so my bat file just needs to read that
    1. Chocolatey
    1. [Clink][2] `choco install clink -y`

## Saftey

1. Sparkleshare
1. Keepass
1. We get into a bit of a loop problem here
1. I need the keepass database - that is in sparkleshare
1. I don't have the backup of the key for sparkleshare
1. I can't download the keepass database from bitbucket as it is encrypted
1. I need to login to bitbucket to approve the new key for sparkleshare

## Love / belonging

1. Skype (Chocolatey didn't have the latest version and the upgrade of Skype via the Help menu caused a [crash][5])
1. Slack
1. Dropbox

## Esteem

1. Git
1. Sublime Text
1. Google Drive
1. VPN
1. SVN
1. PHP
1. Permasense
    1. 7-zip
    1. SQL Server `choco install mssqlserver2012express -y`
        1.**N.B. This installs SQL Server in only Windows Authentication mode**
    1. IIS (Make sure WWW Services > Application Development Features > CGI is installed)
    1. [Skype for Business][6]
    1. node.js (for grunt / bootstrap LESS)
    1. pandoc (converting help)
    1. evince (reading VPN details)
    1. libreoffice
1. VSN (I possibly won't install any of these on Windows so that VSN dev is only on linux)
    1. MySQL
1. Chromium

## Self-actualisation

1. Python
1. R
1. Virtual Box - this was a possible cause for the internet problems I had
1. Hadoop
1. Haskell

  [1]: https://notes.pinboard.in/u:ianchanning/notes/da84af0413817ebf9c11
  [2]: http://mridgers.github.io/clink/
  [3]: https://github.com/neilpa/cmd-colors-solarized/
  [4]: https://ianchanning.wordpress.com/2014/10/29/dos-command-history/
  [5]: https://support.skype.com/en/skype/windows-desktop/?intsrc=client-_-windows-_-7.25-_-go-help.faq.installer&LastError=1603
  [6]: https://www.microsoft.com/en-us/download/details.aspx?id=49440