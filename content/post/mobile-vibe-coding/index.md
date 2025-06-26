---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Mobile Vibe Coding: Taking AI Agents Anywhere"
subtitle: ""
summary: "Experimenting with mobile development setups using VMs, Zellij sessions, and SSH clients to code from anywhere."
authors: []
tags: ["development", "mobile", "ssh", "zellij", "remote-work"]
categories: ["Development"]
date: 2025-06-26T17:18:50Z
lastmod: 2025-06-26T17:18:50Z
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I started experimenting with mobile development setups yesterday, inspired by an article I read called ["You Are The Main Thread"](https://claudelog.com/mechanics/you-are-the-main-thread/) that got me thinking about parallel processing and opportunity costs. The basic idea is that with how good AI agents are getting, you should have them running in the background doing meaningful work and delivering value to you. Mobile access enables that.

This is very much a work in progress - consider this living documentation of what I'm figuring out as I go.

## The Problem

I wanted to continue my exact development workflow while away from my main machine, not just handle small tasks or quick fixes. The goal isn't mobile-optimized development - it's maintaining the same development process I use at home, but accessible from anywhere.

The traditional approach of "just wait until you're back at your desk" felt like wasted opportunity. With AI capabilities advancing and mobile dictation getting better, the technical barriers to remote development are dissolving.

## The Setup

After some experimentation, here's what I settled on:

**The Backend**: A dev VM running on my home PC I keep running, with Tailscale for secure access. This gives me a consistent environment that's always on, and always available from anywhere with internet.

**Session Management**: I'm using Zellij (think modern tmux) to keep persistent sessions that survive disconnections. I wrote a simple script that handles the session logic:

```bash
#!/bin/bash

export PATH="$HOME/bin:$PATH"

case "$1" in
    claude)
        if zellij list-sessions | grep -q "dev"; then
            zellij attach dev
            zellij action go-to-tab 1
        else
            zellij --layout dev --session dev
        fi
        ;;
    server)
        if zellij list-sessions | grep -q "dev"; then
            zellij attach dev
            zellij action go-to-tab 2
        else
            zellij --layout dev --session dev
            zellij action go-to-tab 2
        fi
        ;;
    *)
        if zellij list-sessions | grep -q "dev"; then
            zellij attach dev
        else
            zellij --layout dev --session dev
        fi
        ;;
esac
```

So I can run `./code claude` to jump into a session with Claude Code running, or `./code server` to check on my dev server. The sessions persist even when I disconnect, so I can pick up exactly where I left off.

**The Clients**: The main bottleneck to mobile development is starting to clear. With dictation and Claude Code, I can just speak my intentions and it figures it out and goes to work. I've been testing two approaches:

- **Termius**: Much nicer interface, better keyboard, feels more native. But it disables iOS dictation support, which is a problem when you want to voice-code.
- **WebSSH**: iOS dictation works perfectly. Then I can view the results in my mobile browser at the Tailscale dev address.

## What Actually Works

The setup lets me continue my normal development workflow remotely. I can:
- SSH in, attach to my session, and immediately see where I left off
- Use Claude Code for pair programming on quick fixes
- Keep a dev server running in the background
- Switch between projects without losing context

The dictation support in WebSSH is surprisingly useful. I can literally just dicate what I would normally type to Claude and it converts to text and sends to Claude. All natural language, and no typing of bunch of code on a small mobile keyboard.

## Current Limitations

There are still some significant challenges to work through:

**Scrolling and Output Management**: Mobile terminals struggle with scrolling through long outputs from Claude Code or development logs. I've been experimenting with tmux and screen for better session control, but mobile scrolling remains clunky for reviewing what AI agents have done.

**Frontend Debugging**: Without access to browser dev tools on mobile, debugging frontend issues is nearly impossible. There's no good way to inspect console logs, network requests, or DOM changes when developing client-side code from a phone.

## What's Actually Possible

I can actually get real features delivered from my phone. Not just TODO comments or quick fixes - actual meaningful work that moves projects forward.

The "vibe" part is real too. There's something satisfying about fixing a bug while sitting in a coffee shop, knowing that your changes are already deployed and your CI is running, all from a device that fits in your pocket.

## Quick Setup Guide

Here's exactly what I'm using to get this working:

**Required Tools:**
- VM or server with persistent SSH access
- [Tailscale](https://tailscale.com) for secure networking
- [Zellij](https://zellij.dev) session manager
- [Claude Code](https://claude.ai/code) AI pair programming

**Mobile Clients:**
- [WebSSH](https://webssh.net) (supports iOS dictation)
- [Termius](https://termius.com) (better UI, no dictation)

**Setup Steps:**
1. Install Tailscale on your dev machine and phone
2. Install Zellij: `cargo install zellij` or use package manager
3. Create the session script above as `~/bin/code`
4. Make it executable: `chmod +x ~/bin/code`
5. SSH in and run `./code claude` to start coding

**Dictation Workflow:**
1. Connect via WebSSH on mobile
2. Hold dictation button and speak naturally to Claude
3. View results in mobile browser at dev server URL
4. Repeat for iterative development

## Looking Forward

The next logical step is native mobile development environments with better voice integration. AR glasses could eventually provide visual code feedback while maintaining the voice-first interaction model.

The main thread concept applies here: instead of blocking on being at your desk, you can run development tasks in parallel with your daily life. The constraint is shifting from hardware limitations to communication bandwidth between developer intent and implementation.
