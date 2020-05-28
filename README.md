# zettelkasten
A note taking application following the Zettelkasten method.

## Installation
```bash
curl -s -o /usr/local/bin/zk https://raw.githubusercontent.com/AndrewCopeland/zettelkasten/master/zk && chmod +x /usr/local/bin/zk
```

## Getting started
To initialize the zettelkasten config just run `zk init`.
```bash
$ zk init
Zettelkasten Directory: /Users/acopeland/git/AndrewCopeland/zettelkasten/kasten
Zettelkasten auto git push[yes/no]: no
Zettelkasten git directory(optional):
Default command line text editor[vi/emacs/nano]: vi
$
```

## Usage
```
Usage:	zk COMMAND

A note taking application following the Zettelkasten method.

Commands:
  n, new            Create a new zettel and use default editor
  nv, new-vi        Create a new zettel and edit with vi
  ne, new-emacs     Create a new zettel and edit with emacs
  nc, new-code      Create a new zettel and edit with Visual Studio Code
  o, open           Open an existing zettel in default editor
  ov, open-vi       Open an existing zettel in vi
  oe, open-emacs    Open an existing zettel in emacs
  oc, open-code     Open an existing zettel in Visual Studio Code
  ob, open-browser  Open an existing zettel in github using OS default browser
  t, tag            Search zettels for a specific tag
  s, search         Search for a sub string within all zettels
  ls, list          List all zettels
  rm, remove        Remove a zettel
  ln, link          Link 2 zettels together
  rml, rm-link      Remove a link between 2 zettels
  home              Display zettelkasten home directory
  init              Initialize the $HOME/.zettelkasten
  variables         List all variables defined in $HOME/.zettelkasten
  add, add-media    Add specific media (images, music, pdf, etc) to kasten
  lsm, list-media   List all media zettels
```

To create a zettel perform the following. This will open up vi and once you are done save the zettel.
```bash
zk new "my first zettel"
```

To open an existing zettel perform the following. This will open up vi on the existing zettel, you can update zettels using this command. This command will search for the zettel using the substring `my-first-zettel` and will open it if only 1 result was found.
```bash
zk open my-first-zettel
```

To link zettels together perform the following.
```bash
zk link my-first-zettel my-second-zettel
```

## Zettelkasten configuration
When executing `zk init` a configuration file is created in your home directory called `.zettelkasten`.
This file contains variables that are used by `zk`. An explaination of each of these variables are below:
- `ZETTELKASTEN_DIR`: Directory in which all zettels reside
- `ZETTELKASTEN_AUTO_GIT_PUSH`: Auto push zettels to git when created, updated or deleted. Valid values are `yes` or `no`.
- `ZETTELKASTEN_GIT_DIR`: Set this variable if the kasten resides within a specific folder in your git repo. This is used when opening a zettel in github.
- `ZETTELKASTEN_ENVIRONMENT`: The OS environment in which the zk application is running. This value should be set automatically by `zk init`. Valid values are `osx` or `wsl`.
-  `ZETTELKASTEN_MD5_COMMAND`: The command used when md5 hashing zettels. This value should be set automatically by `zk init` depending on the `ZETTELKASTEN_ENVIRONMENT`.
- `ZETTELKASTEN_BASENAME_COMMAND`: The command used when getting the basename of zettels. This value should be set automatically by `zk init` depending on the `ZETTELKASTEN_ENVIRONMENT`.
- `ZETTELKASTEN_BROWSER_COMMAND`: The command used when executing 'open-browser' action. This value should be set automatically by `zk init` depending on the `ZETTELKASTEN_ENVIRONMENT`.
- `ZETTELKASTEN_DEFAULT_TEXT_EDITOR`: The command used when using 'open'. This is configurable and can be set to `vi`, `vim`, `emacs` or `nano`.
- `ZETTELKASTEN_DATE_TIME_STAMP_FORMAT`: The datetime stamp format used when creating the zettels. This will default to `+s` which is EPOCH time.
- `ZETTELKASTEN_VI_COMMAND`: The command used when executing vi. Default is `vi +2`.
- `ZETTELKASTEN_EMACS_COMMAND`: The command used when execuing emacs. Default is `emacs +2`
- `ZETTELKASTEN_VISUAL_STUDIO_CODE_COMMAND`: The command used when executing Visual Studio Code. Default is `code --new-window --wait`

## Uninstall
To remove the application perform the following:
```bash
rm /usr/local/bin/zk
rm "${HOME}/.zettelkasten"
```

## Limitations
- Currently only tested on Mac
- Google Chrome and Visual Code must be installed to use `open-code` or `open-browser`
- When `ZETTELKASTEN_AUTO_GIT_PUSH=yes` then `ZETTELKASTEN_DIR` must be a git repo.
