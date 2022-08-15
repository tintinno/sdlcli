# sdlcli
A command-line interface to an SDL Knowledge Center repository.

## Installation 

1. Install `zeep` and `lxml`.

```
pip3 install zeep
pip3 install lxml
```

2. Copy `sdl` into your $PATH.

```
cp sdl ~/bin/
```

## Setup

1. Run `sdl setup` to initialize the program with your SDL URL and credentials.

```
[tintinno@localhost]$ sdl setup
Warning: No metadata file. Run `sdl setup`.
Creating ~/.sdlcli directory.
No metadata file. Downloading metadata.yaml from GitHub
Enter the SDL URL [https://example.com/InfoShareWS/]: https://ccms.example.com/InfoShareWS/    
Enter your SDL username: tintinno 
Enter your SDL password:
[tintinno@localhost]$
```

2. If you customized your workflow, open `~/.sdlcli/metadata.yaml` and set the 
   values for the default draft state and the default released state for topics.
   Otherwise, leave them as they are.

3. If you use customized SDL metadata, open `~/.sdlcli/metadata.yaml` and add 
   the field name in uppercase and the metadata level in lowercase.

   For example, if you've added an FPARTNUMBER field at the version level to track 
   publication part numbers, add the following under the `pub` section. 
   (The `desc` field is an optional description.)

   ```
   FPARTNUMBER:
     level: version
     desc: the part number for this publication 
   ```

## Management Types
