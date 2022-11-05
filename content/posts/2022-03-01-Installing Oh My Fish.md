---
title: "Installing 'Oh My Fish' Shell"
date: 2022-03-01
description: About installing fish, a very helpful modern bash alternative in WSL2/Ubuntu. Enjoy the benefits of the powerline and nice helpers and visualizations in your common workflows.
draft: false
tags:
  - terminal
  - fish
  - wsl
categories:
  - developer
---

About installing fish, a very helpful modern bash alternative in WSL2/Ubuntu. Enjoy the benefits of the powerline and nice helpers and visualizations in your common workflows.

## Install Fish

1. Install fish
  `sudo apt install fish`
1. Switch to fish
  `fish`
1. Set fish as default shell
  `chsh -s /usr/bin/fish`

*Hint*: Switch back to bash with  `chsh -s /usr/bin/bash`

## Install 'Oh My Fish' with the theme 'Bob The Fish'

1. Install Oh My Fish
  `curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish`

1. Install Theme: Bob the Fish
  `omf install bobthefish`

## Install powerline fonts

1. Install Powerline fonts (Ubuntu/Debian/Mint Linux)
  `sudo apt-get update`
  `sudo apt-get install fonts-powerline`

## Fix fonts using WSL or Remote

The powerline and theme of oh my fish / bob the fish will not be shown correctly. Instead fancy symbols are shown.

Make sure that your console / terminal application like Windows Terminal or Visual Studio Code are using a font with is patched with powerline symbols.

Under windows, a good suggestion is to use MS font Cascadia in its variant **Cascadia (Code|Mono) PL**. You can find it at  [Cascadia Code on Github](https://github.com/microsoft/cascadia-code).

### Useful links

- [Shell: Fish](https://fishshell.com)
- [Project: Oh-my-fish](https://github.com/oh-my-fish/oh-my-fish)
- [Theme: Bob-the-fish](https://github.com/oh-my-fish/theme-bobthefish)
- [Font: Cascadia Code](https://github.com/microsoft/cascadia-code).