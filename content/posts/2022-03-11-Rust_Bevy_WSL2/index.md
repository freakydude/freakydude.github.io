---
title: "Set Up Rust and Bevy in WSL2 on Windows 11"
date: 2022-03-11
lastmod: 2026-11-17
description: Some easy steps to set up an initial Rust development environment in Windows 11 WSL2/Ubuntu Linux, together with the libraries needed to compile and run all of the Bevy examples, using the graphical WSL implementation (WSLg).
draft: false
tags:
  - rust
  - bevy
  - wsl
  - windows
  - linux
categories:
  - developer
---

Are you one of the nerds who like to develop your own game, fancy graphics, or any aspect that comes with that? Or would you like to take your first steps with the young programming language Rust, which unites the speed of C/C++ with memory and thread safety?

Why not combine both to have some fun learning new things?

Just to be even more nerdy, I show you a way to do all of that within the Windows Subsystem for Linux and its graphical extension (WSLg) under Windows 11. When I wrote this post, WSLg was still a preview with some shortcomings.

> **Update (November 2026):** The Rust setup now uses rust-analyzer instead of the deprecated RLS, the fish PATH step is simplified, the Bevy dependency list follows the current Bevy documentation, and the WSLg remarks reflect that it is now part of regular WSL.

## Requirements

- Microsoft Windows 11
- CPU virtualization enabled

## Install WSL2

If you haven't installed WSL2 yet, follow the [official WSL installation guide](https://learn.microsoft.com/en-us/windows/wsl/install).

In short:

1. Open a Windows terminal with administrator privileges ([Windows Terminal on GitHub](https://github.com/microsoft/terminal)).
1. Run

   ```bat
   wsl --install
   ```

   and follow the instructions.

## Configure Linux in WSL2

1. Open your Ubuntu Linux within WSL2.

1. Install Rust with

   ```sh
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   rustup update
   rustup component add rust-analyzer rust-src
   ```

   _Remarks_: RLS is deprecated and replaced by [rust-analyzer](https://rust-analyzer.github.io/book/rust_analyzer_binary.html), which is why the command installs `rust-analyzer` instead of `rls` and `rust-analysis`. With the original post's Ubuntu 20.04, the packages from the repository were missing libraries for code completion in VS Code. I have not re-checked that with a current Ubuntu.

   1. If you are using the fish shell:

      - The rustup installer sets up fish itself if fish is installed when you run it: it creates `~/.config/fish/conf.d/rustup.fish`, which sources `env.fish`. If `cargo` is not found in fish anyway, create or edit the file `~/.config/fish/conf.d/myconfig.fish`.
      - Add

        ```fish
        fish_add_path ~/.cargo/bin
        ```

1. Install the Bevy Linux dependencies with

   ```sh
   sudo apt install g++ pkg-config libx11-dev libasound2-dev libudev-dev libxkbcommon-x11-0 libwayland-dev libxkbcommon-dev mesa-vulkan-drivers
   ```

   The `libwayland-dev` and `libxkbcommon-dev` packages are only optional if you disable Bevy's `wayland` feature, and `mesa-vulkan-drivers` is one of several Vulkan driver options depending on your graphics card. You can find more information in the [Bevy Linux dependencies documentation](https://github.com/bevyengine/bevy/blob/main/docs/linux_dependencies.md).

1. Install git

   ```sh
   sudo apt install git
   ```

## Try Bevy

Bevy is a slim and fast Rust game engine.

1. Clone the [Bevy repo](https://github.com/bevyengine/bevy):

   ```sh
   git clone https://github.com/bevyengine/bevy
   ```

2. Try the examples in the examples folder:

   ```sh
   cargo run --example breakout
   ```

WSLg is no longer a preview: Linux GUI apps are now officially supported on WSL2. To get hardware accelerated OpenGL rendering through a virtual GPU, install the matching Intel, AMD, or NVIDIA vGPU driver described in the [guide to Linux GUI apps on WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps).

## Conclusion

This is a very good starting point to learn Rust by playing around with a game engine. Let me know about your progress with it.

## Further links

- [Learn Rust](https://www.rust-lang.org/learn)
- [Bevy Engine](https://bevy.org/)
- [Bevy Getting Started](https://bevy.org/learn/quick-start/getting-started/)
- [Bevy Cheat Book](https://bevy-cheatbook.github.io/)
- [Linux GUI Apps on WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps)

{{< support >}}
