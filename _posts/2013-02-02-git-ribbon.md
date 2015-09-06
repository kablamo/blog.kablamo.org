---
layout: post
title: git-ribbon
date: '2013-02-02'
comments: true
tags: perl

---

I wrote a little Perl script called
[git-ribbon](https://github.com/kablamo/git-ribbon) to help me review the
latest changes in a git repository.  

The way I used to review changes was by reading through the `git log`.  I try
to do this every morning at work to keep up with whats going on.  But I was
having a few problems:

 1. Its hard to know exactly which changes are new.
 2. I want to review commits in the order they happened (instead of most recent first).
 3. `git log` diff output can be hard to read and may not have enough context
    -- sometimes I want a side by side diff like I get from `vimdiff` or `git
difftool`.

Basically I wanted a quick and easy way to review the latest changes in a way
that feels a little more like an RSS feed.  So I wrote this script.

How to use git-ribbon
-------------

**First** mark your current place in the commit history.  This command will
place a tag named \_ribbon at origin/master.  Basically its a bookmark at your
current location.

    ⚡ git ribbon --save

**Next**, pull the latest changes made by your fellow conspirators from the
remote repository.  

    ⚡ git pull

**Then** use `git ribbon` to review only the changes that have occurred since \_ribbon:

    ⚡ git ribbon
    Eric Johnson 6 weeks ago ecf43db
    Css tweaks.
    root/html/calculator/realCost.tt

    press 's' to skip 

    Eric Johnson 4 weeks ago 9595fa0
    fix css margin class.
    root/css/networth.css
    root/css/style.less
    root/css/style.less.old
    root/html/calculator/realCost.tt
    root/html/fi.tt

    press 's' to skip 

    Eric Johnson 2 weeks ago 5ef0fb2
    Added daysPerYear.
    lib/Networth/Controller/Calculator.pm
    lib/Networth/Out/RealCost.pm
    root/html/calculator/realCost.tt

    press 's' to skip 

The script will pause and wait for input when it prints `press 's' to skip`.
If you type anything other than `s`, it will show you the side by side diff
using `git difftool`.

<a href="http://farm9.staticflickr.com/8107/8457314152_7f8b3c955c_b.jpg" title="click to view large version"><img src="http://farm9.staticflickr.com/8107/8457314152_7f8b3c955c.jpg" width="500" height="201" alt="vimdiff"></a>

After you have reviewed all the changes, be sure to mark your place again so
its ready to go next time you want to do a pull:

    git ribbon --save

Bonus tips
-----------

In your .gitconfig try this:

    [diff]
        tool = vimdiff

The default colors for vimdiff look like they were created by strange clowns so
try this instead:

    ⚡ mkdir -p ~/.vim/colors/
    ⚡ wget https://github.com/kablamo/dotfiles/blob/master/links/.vim/colors/iijo.vim -O ~/.vim/colors/iijo.vim
    ⚡ echo "colorscheme iijo" >> ~/.vimrc

Then learn to use vimdiff:

 * To switch windows type `ctl-w l` and `ctl-w h`. 
   For more help type `:help window-move-cursor`.
 * To open and close folds type `zo` and `zc`. 
   For more help type `:help fold-commands`.
 * To close vimdiff with less typing try `ZZ`.  

Alternatives to vimdiff
-----------------------

If you don't want to invest the time just yet to learn vim, use an alternative like meld, opendiff,
p4merge, xxdiff, etc.  Side by side diffs are worth it!

See also
--------

This script was inspired by a great [blog
post](http://gitready.com/advanced/2011/10/21/ribbon-and-catchup-reading-new-commits.html)
on gitready.com which has a number of awesome git tricks for both beginners and
advanced users.

I also ended up writing a [vim plugin](https://github.com/kablamo/vim-ribbon)
that is probably better user experience if you very comfortable in vim.


