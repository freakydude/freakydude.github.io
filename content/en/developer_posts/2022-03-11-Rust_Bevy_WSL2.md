---
title: "Setup Rust and Bevy under WSL2 in Windows 11"
date: 2022-03-11
description: Some easy steps to setup an initial Rust development environment in Windows 11 WSL2/Ubuntu Linux together with the needed libraries to compile and run all of the Bevy examples. All that is running within the brand new preview of the graphical WSL implementation. 
author: freakydude
draft: false
---

## Motivation

Are you one of the nerds, who likes to develop your own game, fancy graphics or any aspect that comes with that or would you do your first steps with the young programming language Rust, which unites speed of C/C++ with memory- and thread-safety?

Why not combining both aspects to have some fun learning new things?

## Environment
Just to be even more nerdy, I show you a way to do that all within the Windows Subsystem for Linux and it's brand new graphical extension under Windows 11. At the moment of writing it's still in preview, so be aware of some shortcomings.

### Preconditions
- Microsoft Windows 11
- Enabled CPU virtualisation

## Setup Linux in WSL2

### Install WSL2

If you havn't installed WSL2 yet, follow the manual [here](https://docs.microsoft.com/en-us/windows/wsl/install)

In short:
1. Open a windows terminal with administrator privilegies [Windows Terminal on Github](https://github.com/microsoft/terminal)
1. Run
   ```sh 
   wsl --install
   ```    
   and follow the instructions.

### Configure Linux in WSL2

1. Open your Ubuntu Linux within WSL2

1. Install rust with 
   ```sh
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh 
   rustup update
   rustup component add rls rust-analysis rust-src
   ```

   *Remarks*: The Ubuntu 20.04 packages from repository are missing libs for code completion in VS Code.

   1. If you are using fish shell
      - Edit/Create file: `~/.config/fish/conf.d/myconfig.fish`
      - Add `set -g -x PATH ~/.cargo/bin $PATH`

1. Install bevy linux dependencies with 
   ```sh
   sudo apt install g++ pkg-config libx11-dev libasound2-dev libudev-dev libwayland-dev libxkbcommon-dev mesa-vulkan-drivers
   ``` 
   Some more information can be found [here](https://github.com/bevyengine/bevy/blob/main/docs/linux_dependencies.md)

1. Install git 
   ```sh
   sudo apt install git
   ```

## Bevy (a slim and fast Rust game engine)

1. Clone the [Bevy repo](https://github.com/bevyengine/bevy):
   ```sh 
   git clone https://github.com/bevyengine/bevy
   ```

2. Try the examples in the examples folder
   ```sh
   cargo run --example breakout
   ```

Be aware, that the setup and use of the graphical WSL for Linux GUI applications isn't hardware accelerated by default and not free of errors at the time of writing. But hey, Microsoft is working on it hard. More infos about it [here](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gui-apps). With special drivers, you can already check that out, too.

## Have fun

This is a very good startpoint to learn rust by playing around with a game engine. Let me know about your progress with that.

## Useful Links
- [Learn Rust](https://www.rust-lang.org/learn)
- [Bevy Engine](https://bevyengine.org/)
- [Bevy Getting Started](https://bevyengine.org/learn/book/getting-started/)
- [Bevy Cheat Book](https://bevy-cheatbook.github.io/)
- [Linux GUI Apps on WSL](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gui-apps)