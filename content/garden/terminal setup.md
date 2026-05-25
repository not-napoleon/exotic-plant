---
title: "My Terminal Configuration"
date: 2026-05-24
lastmod: 2026-05-24
draft: false
garden_tags: [ ]
summary: "Notes on my current attempt at a modern command line"
status: "seeding"
---

# Terminal Setup

In May of 2026, I decided to revamp my terminal setup from scratch, using some
more modern tools.  This is all a work in progress, and I'm not claiming any of
these are the best solution to any given person's needs.  I'm just reporting
what's working for me, right now.  You can find my config files 
[here](https://github.com/not-napoleon/dotfiles).

For reference, all of this is set up on a MacBook and tools are installed with
[homebrew](https://brew.sh/).

Also note, I won't be discussing my vim configuration here. 

## Configuration Management
I'm using [chezmoi](https://www.chezmoi.io/) for my dotfile management. This
feels like a big upgrade over my previous method, which was just using git
directly.  There's a few things I really like about chezmoi's model.

First off, I like the opt-in design, as opposed to git's default opt-out via
`.gitignore`.  There's a fair bit of stuff in my `.conifg` directory that I
don't need or want to keep in sync across my various machines, nor share in a
public repo (y'all don't need to see my Stardew Valley preferences).

I also like that it isn't tied to my `.config` directory.  If I want to manage a
file that is just in my home directory, or in some arcane path, that's easy
enough to just add in.  This feels much cleaner than turning my home directory
into a git repo, or managing a bunch of symlinks (although I know there are
tools that do this via symlinking which would probably also be fine).  Although
it hasn't come up for me yet, I think this could be especially useful for
syncing configuration of tools that are themselves in git.  I'll report back if
that comes up.

Chezmoi also provides some intrinsic utility just by being a dedicated tool.
Being able to list managed and unmanaged files is particularly helpful.  Being
able to preview config changes, do merges, and apply templates all seem useful,
although I haven't done much with those features yet.

## Fonts and Colors
I use a [nerdfont](https://www.nerdfonts.com/), because I am a nerd.  As of this
writing, I'm using [Fira Code Mono](https://www.programmingfonts.org/#firacode)
as my main font, although I've used [Hack](https://www.programmingfonts.org/#hack) 
in the past. I like the ligatures in FiraCode.  [JetBrains Mono](https://www.programmingfonts.org/#jetbrainsmono)
also looks good, and I might switch to that at some point.

For colors, I use [catppuccin](https://catppuccin.com/).  It's purple-ish, dark
mode (at least in the mocha variation that I use) and soft. It also has ports
for tons of tools, so I can generally have the same color scheme everywhere.

## Kitty and Fish
Okay, full disclosure, I picked [Kitty](https://sw.kovidgoyal.net/kitty/)
because I'm a sucker for anything cat themed.  Turns out Kitty is awesome
though, so I feel like my strategy continues to be justified.  I'm only using a
few of Kitty's features, but I hope to be integrating more in the future. I
especially like that the very detailed config is fully text based, and well
documented.

[Fish](https://fishshell.com/) has been my shell of choice for at least a 
decade. It has good out-of-the-box behavior and is easy to configure. Fish has
smart autocomplete, a much more readable scripting language than bash, and a lot
of quality of life features I enjoy. 

One thing I've particularly liked in fish is the abbreviation system. Fish
supports aliases like bash does, but it also has abbreviations, which are subtly
different.  Basically, an abbreviation gets expanded in the prompt, instead of
behind the scenes in the shell.  This means your shell history has the full
command, not just the alias, which can be helpful.  It also means you can write
partial abbreviations that can be context-sensitive, such as the git sub
commands.

## Starship Prompt
[Starship](https://starship.rs/) is a shell independent prompt system written in
Rust. It's got a nice text-based modular configuration system, which feels nice.
There's a [catppuccin](https://github.com/catppuccin/starship) theme for it,
naturally.  It has all the features I'd expect from a modern prompt already set
up: Python Virtual Env integration, git information, shortened directory
listings, VI mode display, etc.  

## Tool Upgrades
These are some upgrades to basic shell utilities that I've been getting some
good results with:

- Instead of `ls`, try [lsd](https://github.com/lsd-rs/lsd).  
- Instead of `cat`, try [bat](https://github.com/sharkdp/bat).
- Instead of `grep`, try [rg](https://github.com/BurntSushi/ripgrep)
- Instead of `cd`, try [zoxide](https://github.com/ajeetdsouza/zoxide)

## Snippet Management
Finally, I'm using [pet](https://github.com/knqyf263/pet) for command line
snippet management. This has quite a few advantages over just finding things in
my history.  First off, it's synced across my machines (`pet` has a built-in
sync via GitHub gists, but I'm syncing it via chezmoi since I already have that
setup).  It supports desciptions and search, so I can actually find the snippet
I want. This pairs especially nice with [fzf](https://github.com/junegunn/fzf),
since I only need to set up a complicated `fzf` pipeline once, and then I can
save it for easy future use.

# Conclusion
The terminal continues to be a relevant dev tool, and it's worth taking the time
to make your environment work for you.  Like having a comfortable office, making
your terminal more pleasant to use will make working in it slightly more
enjoyable.  And who doesn't need more joy in this world.
