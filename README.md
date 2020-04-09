# zettelkasten
Creating notes with the zettelkasten note taking method and storing all notes on github

## Installation
```bash
curl -s -o /usr/local/bin/zk https://raw.githubusercontent.com/AndrewCopeland/zettelkasten/master/zk && chmod +x /usr/local/bin/zk
```

## Getting started
Before you start using the `zk` script you will need to set the following environment variables:
- `ZETTELKASTEN_DIR`: The directory in which the zettels will be stored. In this case it is `$(pwd)/kasten`
- `ZETTELKASTEN_AUTO_GIT_PUSH`(optional): If this is set to `true` then zettels will sync to the git associated with the `ZETTELKASTEN_DIR`
- `ZETTELKASTEN_GIT_DIR`(optional): If we are creating Zettelkastens within a specific directory in the git repo. 

## Usage
```
Usage:	zettel COMMAND

A note taking application following the Zettelkasten method.

Commands:
  n, new            Create a new zettel
  o, open           Open an existing zettel in vi
  oc, open-code     Open an existing zettel in Visual Studio Code
  ob, open-browser  Open an existing zettel in github using Google Chome
  rm, remove        Remove a zettel
  l, link           Link 2 zettels together
  rml, rm-link      Remove a link between 2 zettels
  s, sync           Sync git zettelkasten to local zettelkasten
  home              Display zettelkasten home directory
  init              Initialize the $HOME/.zettelkasten
```

To create a zettel perform the following. This will open up vi and once you are done save the zettel will be created.
```bash
zettel new "my first zettel"
```

To open an existing zettel perform the following. This will open up vi on the existing zettel, you can update zettels using this command. This command will search for the zettel using the substring `my-first-zettel` and will open it if only 1 result was found.
```bash
zettel open my-first-zettel
```

To link zettels together perform the following. This performs a search just like `open`. All zetels have a unique id associated with the epoch time when the zettel was created which can be used when linking also.
```bash
zettel link my-first-zettel my-second-zettel
```

## How was the example zettelkasten created
```bash
./zettel new conjur-appliance
./zettel new conjur-authn-iam
./zettel link conjur-appliance conjur-authn-iam
./zettel new conjur-authn-oidc
./zettel link conjur-appliance conjur-authn-oidc
./zettel new conjur-policy
./zettel link conjur-appliance conjur-policy
./zettel new conjur-resource-variable
./zettel link conjur-policy conjur-resource-variable
```
