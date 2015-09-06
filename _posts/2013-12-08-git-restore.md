---
layout: post
title: How to find and restore deleted files with git
date: '2013-12-08'
comments: true
tags: tools

---

Don't be afraid to delete files from your git repository.  You can get restore
them.  You can even search for a string in a deleted file.  Here is how to find
a deleted file and its commit:

    git log --diff-filter=D --summary                  # all deleted files ever
    git log --diff-filter=D --summary .                # all deleted files in cwd 
    git log --diff-filter=D --author=Batman --summary  # all files deleted by Batman

How to restore a deleted file:

    git checkout <commit>~1 <filename>

To make this process a little easier next time I need to do it, I created a git
alias for finding deleted files by adding this to my .gitconfig file:

    [aliases]
    deleted = log --diff-filter=D --summary

Now I can find and restore files like this:

    git deleted                         # find a deleted file and its commit
    git checkout <commit>~1 <filename>  # restore the deleted file

## How to search the contents of deleted files

But lets say I don't remember the filename of that file I deleted in a fit of
cleanup passion.  I do remember the name of one of the functions in it though.
Here is how to deal with that.  Search the contents of all files that have ever
existed in git for a string:

    git log --summary -S<string> [<path/to/file>] [--since=2009.1.1] [--until=2010.1.1]

Another way to do this:                                                                                                                                      
        
    git rev-list --all | xargs git grep 'string'

Git is all knowing and all seeing and all powerful.  Hail git, powerful arcane
lord of source control.  

