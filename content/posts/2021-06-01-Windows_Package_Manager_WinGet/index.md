---
title: "Install and Update Windows Applications with WinGet 1.0"
date: 2021-06-01
lastmod: 2026-11-17
description: A short intro to the finally released WinGet 1.0, the native Microsoft package manager to install, uninstall, and update most of your daily used applications on Windows.
draft: false
tags:
  - windows
categories:
  - developer
---

This week, Microsoft released its first native package manager for Windows 10. If you have ever worked with a package manager for Linux (e.g. apt, pacman), you know the benefits. Or, if you are a developer, you have surely heard of nuget, maven, npm, or similar. It makes it easy to install, uninstall, or update one or all of the applications on your system.

Now you will say that's old hat, because [Chocolatey](https://chocolatey.org/) already exists. You are right, and at the moment it's even more advanced.

Anyway, [WinGet](https://github.com/microsoft/winget-cli) is officially supported and developed under the guidance of Microsoft, and it is now part of Windows itself. So for me, that's the way to go.

> **Update (November 2026):** WinGet is now included in Windows 11 and current Windows 10 as part of the App Installer, so the preview-period instructions for installing it manually are mostly obsolete. The requirements and the install sections are updated accordingly.

## Requirements

The client requires Windows 10 1809 (build 17763), Windows 11, Windows Server 2025, or later.

## Install WinGet

### Automatically with the Microsoft Store

The client is distributed within the [App Installer](https://apps.microsoft.com/detail/9nblggh4nns1) package. On Windows 11 and current Windows 10, App Installer is a system component that is delivered and updated by the Microsoft Store, so WinGet is usually already there. If `winget` is not found right after your first login, give Windows a moment to register it, or install or update App Installer from the Microsoft Store.

### Manually

If the Store is not available (e.g. in a sandbox), you can install it manually. You get the same package from the [WinGet releases page](https://github.com/microsoft/winget-cli/releases). Download and execute the Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle. Note that this package does not update itself via the Microsoft Store. See the official [Readme.md](https://github.com/microsoft/winget-cli/blob/master/README.md) for more information.

## Configure WinGet

After the installation, open the Windows command line (`cmd.exe`) or a PowerShell.

> If you start it as an administrator, you don't have to accept the elevation dialogs later on.

Before you install your applications, I recommend modifying some settings first. Customize some basic settings before you install or upgrade applications with WinGet for the first time.

Edit the settings file by executing

```bat
winget settings
```

In the opened settings.json file, set up your preferred language priority list (e.g. to German) and set your installation scope to machine (instead of user local).

```json
{
  "installBehavior": {
    "preferences": {
      "locale": ["de-DE", "en-US"],
      "scope": "machine"
    }
  }
}
```

A complete list of settings can be found in the [WinGet settings documentation](https://github.com/microsoft/winget-cli/blob/master/doc/Settings.md).

## Use WinGet

As an example, let us search for [PeaZip](https://peazip.github.io/), a nice open-source compression and decompression application that supports most of the compression algorithms out there.

Just run the following command.

```bat
winget search PeaZip
```

If you are unsure whether the result is the application you searched for, you can run the `winget show` command with one of the found application IDs.

```bat
winget show Giorgiotani.Peazip
```

You should get more information about the package to be installed.

If you got the right package, install the found Giorgiotani.Peazip package.

```bat
winget install PeaZip -h
```

The parameter `-h` hides the installer assistant and just installs the application silently to the program files folder. If you used the scope `local` in the settings preferences above, the installed application is placed in your user account's `AppData` folder.

To upgrade an installed application to the latest version, run the `upgrade` option. As always, if you are unsure about the application name, use its ID instead.

```bat
winget upgrade PeaZip -h
```

And finally, one of my favorites. Like the Linux command `apt-get upgrade`, you can just run

```bat
winget upgrade --all -h
```

to update all installed applications that can be found in the repository to their latest version. Because we also set the `-h` parameter, it's done silently again.

## Conclusion

Here it is, a native package manager for Windows applications. Yes, it's version 1.0, and you can't expect to find your lovely package every time or to get a fancy UI. But for the applications it finds, the repeated sessions of searching, downloading, and updating are gone. Just run an `upgrade --all` from time to time (or automate it) and you are done.

This tool saves you a lot of time.

{{< support >}}
