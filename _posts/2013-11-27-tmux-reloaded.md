---
layout: post
title: Tmux reloaded
date: '2013-11-27'
comments: true
categories: tools

---

To reload your tmux configuration without restarting the server, add this to
your ``~/.tmux.conf`` file:

    # reload the config file without restarting the tmux server
    bind R source-file ~/.tmux.conf \; display-message "Config reloaded"

Notice thats a capital `R` not a lowercase `r`.  I keep forgetting that. 
