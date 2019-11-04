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
