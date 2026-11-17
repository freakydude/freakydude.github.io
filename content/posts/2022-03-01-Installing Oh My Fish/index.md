---
title: "Install fish and Oh My Fish with the Bob the Fish Theme in WSL2"
date: 2022-03-01
description: How to install fish, a very helpful modern bash alternative, together with Oh My Fish and the Bob the Fish theme in WSL2/Ubuntu, and how to fix the powerline fonts.
draft: false
tags:
  - terminal
  - fish
  - wsl
  - linux
categories:
  - developer
---

This post is about installing fish, a very helpful modern bash alternative, in WSL2/Ubuntu. Enjoy the benefits of the powerline, nice helpers, and visualizations in your common workflows.

## Install fish

1. Install fish
   `sudo apt install fish`
1. Switch to fish
   `fish`
1. Set fish as the default shell
   `chsh -s /usr/bin/fish`

_Hint_: Switch back to bash with `chsh -s /usr/bin/bash`

## Install Oh My Fish with the Bob the Fish theme

1. Install Oh My Fish
   `curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish`

1. Install the theme Bob the Fish
   `omf install bobthefish`

## Install powerline fonts

1. Install the powerline fonts (Ubuntu/Debian/Mint Linux)
   `sudo apt-get update`
   `sudo apt-get install fonts-powerline`

## Fix fonts in WSL or remote sessions

The powerline and the theme of Oh My Fish / Bob the Fish will not be shown correctly. Instead, fancy symbols are shown.

Make sure that your console or terminal application, like Windows Terminal or Visual Studio Code, uses a font that is patched with powerline symbols.

Under Windows, a good suggestion is the Microsoft font Cascadia in its variant **Cascadia (Code|Mono) PL**. You can find it at [Cascadia Code on GitHub](https://github.com/microsoft/cascadia-code).

## Further links

- [Shell: Fish](https://fishshell.com)
- [Project: Oh-my-fish](https://github.com/oh-my-fish/oh-my-fish)
- [Theme: Bob-the-fish](https://github.com/oh-my-fish/theme-bobthefish)
- [Font: Cascadia Code](https://github.com/microsoft/cascadia-code).

{{< support >}}
