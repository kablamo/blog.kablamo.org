---
layout: post
title: Vim plugin keybindings
date: '2017-11-26'
comments: true
categories:
  - uncategorized

---

I pick up all kinds of vim tricks and plugins but I can never remember all the
shortcuts.  I wrote up this quick reference to help myself memorize how to use
these tools.  

Caveat: some of these keybindings are unique to my [.vimrc](https://github.com/kablamo/yadm/blob/master/.vimrc).

<style>
table {
  margin-top: 0rem;
}
td {
  vertical-align : top;
  padding-left   : .5em;
  padding-right  : .5em;
  background     : #999;
  color          : #333;
}
th {
  text-align:left;
  background    : #666;
  color         : #ccc;
  white-space   : nowrap;
  font-weight   : normal;
  padding-left  : .5em;
  padding-right : .5em;
}
a.plugin, a.plugin:visited {
  color     : #666;
  font-size : 2rem;
}
kbd {
  background  : #666;
  border      : 0px;
  box-shadow  : none;
  color       : #333;
  font-weight : bold;
  font-weight : normal;
}
code {
  background : #111;
  color      : #fff;
}
*, *:before, *:after {
  -webkit-box-sizing: border-box;
      -moz-box-sizing: border-box;
          box-sizing: border-box;
}
</style>


<h3><a class="plugin" href="https://vimawesome.com/plugin/fzf">fzf</a></h3>
Fuzzy Finder
<table>
  <tr>
    <th><kbd>&lt;leader&gt;f</kbd></th>
    <td>Find files with rg by filename</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;*</kbd></th>
    <td>Search file contents with rg (<kbd>alt-a</kbd> to select all). Results go in a QuickFix window.</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;* -t perl</kbd></th>
    <td>Same as above but only search perl files</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;&lt;tab&gt;</kbd></th>
    <td>Search all vim mappings</td>
  </tr>
  <tr>
    <th><kbd>&lt;c-x&gt;&lt;c-f&gt;</kbd></th>
    <td>Complete file names</td>
  </tr>
  <tr>
    <th><kbd>&lt;c-x&gt;&lt;c-l&gt;</kbd></th>
    <td>Complete line</td>
  </tr>
</table>
<br>
QuickFix keybindings: Search and replace across multiple files
<table>
  <tr>
    <th><kbd>fn</kbd></th>
    <td>next</td>
  </tr>
  <tr>
    <th><kbd>fp</kbd></th>
    <td>prev</td>
  </tr>
  <tr>
    <th><kbd>:cdo &lt;cmd&gt;</kbd></th>
    <td>For each entry run <kbd>&lt;cmd&gt;</kbd></td>
  </tr>
  <tr>
    <th><kbd>:cdo s/&lt;c-r&gt;"//c</kbd></th>
    <td>Same as above but don't need to retype the search regexp that was used by fzf</td>
  </tr>
  <tr>
    <th><kbd>:cfdo &lt;cmd&gt;</kbd></th>
    <td>For each file run <kbd>&lt;cmd&gt;</kbd></td>
  </tr>
</table>
<br>

<h3><a class="plugin" href="https://vimawesome.com/plugin/tagbar">Tagbar</a></h3>
Browse the tags (packages, labels, subroutines, etc) of the current file and get an overview of its structure.
<table>
  <tr>
    <th><kbd>&lt;leader&gt;m</kbd></th>
    <td>Show tags</td>
  </tr>
</table>

<h3><a class="plugin" href="https://vimawesome.com/plugin/buffergator">Buffergator</a></h3>
List, navigate between, and select buffers to edit.
<table>
  <tr>
    <th><kbd>&lt;leader&gt;b</kbd></th>
    <td>Open a window listing all buffers</td>
  </tr>
</table>

<h3><a class="plugin" href="https://vimawesome.com/plugin/vim-easy-align">EasyAlign</a></h3>
Vertically align stuff
<table>
  <tr>
    <th><kbd>&lt;leader&gt;a</kbd></th>
    <td>Align something</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a&lt;ctrl-p&gt;</kbd></th>
    <td>Align something in interactive mode</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a=</kbd></th>
    <td>Align around first occurance of <kbd>=</kbd></td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a2=</kbd></th>
    <td>Align around 2nd <kbd>=</kbd></td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a*=</kbd></th>
    <td>Align around all <kbd>=</kbd></td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a-=</kbd></th>
    <td>Align around last <kbd>=</kbd></td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;a&lt;ctrl-x&gt;</kbd></th>
    <td>Align around a regular expression</td>
  </tr>
</table>

<h3><a class="plugin" href="https://vimawesome.com/plugin/vim-gurl">gurl</a></h3>
Provides links to the current line/selection on the github website so you easily talk about code with others.
<table>
  <tr>
    <th><kbd>&lt;leader&gt;t</kbd></th>
    <td>Get a link to the current line/selection</td>
  </tr>
</table>

<h3><a class="plugin" href="https://vimawesome.com/plugin/perlhelp-vim">PerlHelp</a></h3>
<p>Quick access to perldoc</p>
<table>
  <tr>
    <th><kbd>&lt;leader&gt;pd</kbd></th>
    <td>perldoc on the package name under cursor</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;ph</kbd></th>
    <td>perldoc on the package name under cursor</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;pf</kbd></th>
    <td>perldoc on the function name under cursor</td>
  </tr>
  <tr>
    <th><kbd>&lt;leader&gt;pv</kbd></th>
    <td>perldoc on the special Perl variable under cursor</td>
  </tr>
</table>

<h3><a class="plugin" href="https://vimawesome.com/plugin/perl-nextmethod">perl-nextmethod</a></h3>
Jump to the next/prev Perl method
<table>
  <tr>
    <th><kbd>]m</kbd></th>
    <td>Jump to the next Perl subroutine start</td>
  </tr>
  <tr>
    <th><kbd>]M</kbd></th>
    <td>Jump to the next Perl subroutine end</td>
  </tr>
  <tr>
    <th><kbd>[m</kbd></th>
    <td>Jump to the previous Perl subroutine start</td>
  </tr>
  <tr>
    <th><kbd>[M</kbd></th>
    <td>Jump to the previous Perl subroutine end</td>
  </tr>
</table>

