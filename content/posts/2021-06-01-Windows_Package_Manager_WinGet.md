---
title: "Windows package manager WinGet 1.0"
date: 2021-06-01
description: A short intro to the finally released WinGet 1.0. It's a native Microsoft Package Manager to install, uninstall and update most of your daily used applications.
draft: false
tags:
  - windows
categories:
  - developer
---

## In short

This week, Microsoft has released their first native package manager for Windows 10. If you ever worked with a package manager for Linux (e.g. apt, pacman) you know the benefits. Or, if you are a developer, you surely heard about nuget, maven, npm or similar. Easy install, uninstall or update of one or up to all installed applications on your system.

Now you will say, that's a old hat, it already exists [Chocolatey](https://chocolatey.org/). You are right - and at the moment it's even more advanced.

Anyways, [WinGet](https://github.com/microsoft/winget-cli) is official supported and developed by guidance of Microsoft and will be integrated in future Windows releases. So, for me that's the way to go.

## Installation

### Prerequisites

The client requires Windows 10 1809 (build 17763) or later at this time.

### Automatic by MS-Store

The client is distributed within the [App Installer](https://www.microsoft.com/en-us/p/app-installer/9nblggh4nns1) package. While this package is pre-installed on Windows, the client will not be made generally available during the Preview period.

### Manual install

Until the Preview period is over, the straightforward way ist to install it - yes - manually. You get the same package [here](https://github.com/microsoft/winget-cli/releases). Download and execute the Microsoft.DesptopAppInstaller_xxx.appxbundle. See the official [Readme.md](https://github.com/microsoft/winget-cli/blob/master/README.md) for more information.

### Recommendations

After installation open the windows command line (`cmd.exe`) or a powershell.

>If you start it as an administrator, you don't have to accept the elevation dialogs later on.

Before you install your applications, I recommend to modify some settings first.

#### Permanent Settings

Customize some basic settings before you first install or upgrade applications with WinGet.

Edit the settings file by with executing

```bat
winget settings
```

In the opened settings.json file, setup your preferred language priority list (e.g. to German) and your installation scope to machine (instead of user local)

```json
{
    "installBehavior": {
        "preferences": {
            "locale": [ "de-DE", "en-US" ],
            "scope": "machine"
        }
    }
}
```

A complete list of settings can be found [here](https://github.com/microsoft/winget-cli/blob/master/doc/Settings.md)

## Usage

As an example, let us search for [PeaZip](https://peazip.github.io/), a nice open source compress and decompress application for the most compress algorithms out there.

Just run the following command.

```bat
winget search PeaZip
```

If you are unsure if the result is the application you searched for, you can run the `winget show` command with one of the found application IDs.

```bat
winget show Giorgiotani.Peazip
```

You should get more information about the package to be installed.

If you got the right package, install the found Giorgiotani.Peazip package.

```bat
winget install PeaZip -h
```

The parameter `-h` is used to hide the installer assistent and just install the application silently to the program files folder. If you used the scope `local` in the settings preferences above, then the installed application is placed in your user accounts `AppData` folder.

To upgrade an installed application to the latest version, run the `upgrade` option. As always, if unsure about the application name, use its ID instead.

```bat
winget upgrade PeaZip -h
```

And finally one of my favorites, like the Linux command `apt-get upgrade` you can just run

```bat
winget upgrade --all -h
```

To update all installed applications which can be found in the repository to their latest version. Because we also set the `-h` parameter, it's done silently again.

## Final words

Here it is, a native package manager for Windows applications. Yes, it's version 1.0 and you can't expect that you found your lovely package or get a fancy UI. But, for found applications, the repeated sessions to search, download and update are gone. Just run an `upgrade --all` from time to time (or automate it) and your are done.

This tool, saves you a lot of time.
Have fun.
