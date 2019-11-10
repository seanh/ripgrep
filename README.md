ripgrep
=======

My [ripgrep](https://github.com/BurntSushi/ripgrep) config files.

Features
--------

* Searches hidden files by default, which you want because many projects contain hidden
  files that are actually part of the project, e.g. `.isort.cfg`, `.gitignore`,
  `.travis.yml`, `.python-version`, etc.
* Still ignores files that're ignored by your `.gitignore` or other VCS ignore files.
* Doesn't search the `.git` directory. Normally if you turn on hidden files with
  `--hidden` ripgrep will search `.git` directories (unless they're listed in
  `.gitinore`, which they usually aren't)

* Turns on smart-case by default: searches will be case-insensitive, but if the search
  term contains any capital letters then the entire search becomes case-sensitive.

* Sorts results by path by default. This is slower, but still really fast (you won't
  notice the difference except when searching huge directories) and makes the output
  a lot more consistent and easier to read.

  You can disable sorting to speed things up on a per-command basis with
  `rg --sort none ...`

* Applies a black and white color scheme.

  All output is white text on black except for the matches themselves which are
  black on white.
  This is different than `--color never`, which doesn't highlight the output at all.

Installation
------------

```terminal
git clone git@github.com:seanh/ripgrep.git ~/.ripgrep
```

Now tell ripgrep to use the rc file from this repo. Put this in your `~/.bashrc`:

```bash
export RIPGREP_CONFIG_PATH="~/.ripgrep/rc"
```

Or for fish, put this in `~/.config/fish/config.fish`:

```fish
set -x RIPGREP_CONFIG_PATH ~/.ripgrep/rc
```

If your home directory isn't `/home/seanh/` then you'll also have to replace
the `/home/seanh/` in [rc](rc) with the path to your home directory.
Unfortunately there doesn't seem to be any way to get ripgrep to expand the
user's home directory (e.g. `~` or `$HOME`) in a config file.

Vim Integration
---------------

There are plugins like [vim-ripgrep](https://github.com/jremmen/vim-ripgrep) and
[vim-grepper](https://github.com/mhinz/vim-grepper/) that add ripgrep support to vim.
But I prefer just to manually configure vim's builtin `:grep` command (a wrapper for
external grep-alikes):

    if executable("rg")
        set grepprg=rg\ --vimgrep\ --no-heading
        set grepformat=%f:%l:%c:%m,%f:%l:%m
    endif

Now to search across all files in vim's current working directory (and immediately jump to the first result):

    :silent grep <QUERY>

Run `:copen` and `:cclose` to open and close the search results in the quickfix window.

Use `:grepadd` to append more results to the current quickfix list instead of appending a new quickfix list.

Other quickfix list commands:

* `ccN` jump to the Nth result
* `:cnext` go to the next result
* `:Ncnext` go to the Nth next result
* `:cprev` and `:Ncprev`
* `:cnfile` and `:cpfile` go to the first/last result in the next/previous file. `:Ncnfile` and `:Ncpfile` also work
* `:cfirst` and `:clast` jump to the very first/last result (or `:cfirst N`/`:clast N` to go to the Nth result). `:crewind` is also an alias for `:cfirst`
* `:colder` and `:cnewer` move to the next oldest and newest quickfix lists. Vim remembers the last 10 quickfix lists and lets you move between them. `:Ncolder` and `:Ncnewer` also work

[vim-unimpaired](https://github.com/tpope/vim-unimpaired) adds a bunch of keyboard shortcuts for navigating the quickfix list:

* <kbd><kbd>]</kbd><kbd>q</kbd></kbd> and <kbd><kbd>[</kbd><kbd>q</kbd></kbd> for `:cnext` and `:cprev`
* <kbd><kbd>]</kbd><kbd>Q</kbd></kbd> and <kbd><kbd>[</kbd><kbd>Q</kbd></kbd> for `:cfirst` and `:clast`
* <kbd><kbd>]</kbd> <kbd><kbd>Ctrl</kbd> + <kbd>q</kbd></kbd></kbd> and <kbd><kbd>[</kbd> <kbd><kbd>Ctrl</kbd> + <kbd>q</kbd></kbd></kbd> for `:cnfile` and `:cpfile`

You can also run `:lgrep` and `:lgrepadd` to put the results into vim's _location list_
instead of into the quickfix list. The location list is a per-window quickfix list.
It has all the same commands as for working with the quickfix list, but they all begin with `l` instead of `c`:
`:llN`, `:lnext`, `:lprev`, etc.
vim-unimpaired also provides all the same shortcuts for the location list as for the quickfix list, but with l instead of q: <kbd><kbd>]</kbd><kbd>l</kbd></kbd> and <kbd><kbd>[</kbd><kbd>l</kbd></kbd> etc.
