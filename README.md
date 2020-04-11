# zettelkasten
A note taking application following the Zettelkasten method.

## Installation
```bash
curl -s -o /usr/local/bin/zk https://raw.githubusercontent.com/AndrewCopeland/zettelkasten/master/zk && chmod +x /usr/local/bin/zk
```

## Getting started
To initialize the zettelkasten config just run `zk`.
```bash
$ zk
'~/.zettelkasten' could not be found
Zettelkasten Directory: /Users/acopeland/git/AndrewCopeland/zettelkasten/kasten
Zettelkasten auto git push[yes/no]: yes
Zettelkasten git directory(optional): kasten
Initialized  '/Users/acopeland/.zettelkasten'
$
```
If you need to re-initialize the config run `zk init`.

## Usage
```
Usage:	zk COMMAND

A note taking application following the Zettelkasten method.

Commands:
  n, new            Create a new zettel
  o, open           Open an existing zettel in vi
  oc, open-code     Open an existing zettel in Visual Studio Code
  ob, open-browser  Open an existing zettel in github using Google Chome
  t, tag            Search zettels for a specific tag
  s, search         Search zettels for a sub string
  ls, list          List all zettels
  rm, remove        Remove a zettel
  l, link           Link 2 zettels together
  rml, rm-link      Remove a link between 2 zettels
  s, sync           Sync git zettelkasten to local zettelkasten
  home              Display zettelkasten home directory
  init              Initialize the $HOME/.zettelkasten
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

## Limitations
- Currently only tested on Mac
- Google Chrome and Visual Code must be installed to use `open-code` or `open-browser`
- When `ZETTELKASTEN_AUTO_GIT_PUSH=yes` then `ZETTELKASTEN_DIR` must be a git repo.
