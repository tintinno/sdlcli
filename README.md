# sdlcli
A command-line interface to an SDL Knowledge Center repository.

[SDL Knowledge Center](https://docs.rws.com/787645/156929/sdl-tridion-docs-14/product-overview-and-architecture) 
(rebranded as Tridion Docs) is a content management system for technical writers.

## Installation 

1. Install `zeep` and `lxml`.

```
pip3 install zeep
pip3 install lxml
```

2. Copy `sdl` into your $PATH.

```
git clone https://github.com/tintinno/sdlcli/
cp sdlcli/sdl ~/bin/
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

## Management Types

```
usage: sdl <mgmttype> <command> [<args>]

Management types:
  doc        Manage a document
  pub        Manage a publication
  dir        Manage a directory
```
Currently, `sdlcli` supports 3 items you can manage or interact with.

| mgmttype | Examples     |
|----------|--------------|
| doc      | Maps, Topics, Images |
| pub      | Publications |
| dir      | Directories  |

### Manage Topics

```
$ sdl doc -h
usage: sdl doc <command> [<args>] 

Commands:
    showmd      Show the available metadata fields
    getmd       Get metadata for a document (topic,map,image)
    setmd       Set metadata for a document (topic,map,image)
    getxml      Get the XML of a document (topic,map)
```

Run `sdl doc <command> -h` to see the arguments for each command.

**Examples**

1. Get the value of the FTITLE for a specific GUID.
   ```
   [tintinno@localhost:~]$ sdl doc getmd --guid GUID-C20F2738-F47C-4709-90B2-AF37779EF161 --version 1 --field ftitle
   Configure Your ClouD Profile
   [tintinno@localhost:~]$
   ```

1. Set the value of the FTITLE for a specific GUID.
   ```
   [tintinno@localhost:~]$ sdl doc setmd --guid GUID-C20F2738-F47C-4709-90B2-AF37779EF161 --version 1 --field ftitle --newvalue "Configure Your Cloud Profile"
   success
   [tintinno@localhost:~]$
   ```

1. Download the XML of a map or topic.
   ```
   [tintinno@localhost:~]$ sdl doc getxml -g GUID-C20F2738-F47C-4709-90B2-AF37779EF161 -v 1 -o cloud-profile.xml
   [tintinno@localhost:~]$
   ```

### Manage Publications

```
usage: sdl pub <command> [<args>]

Commands:
    showmd      Show the available metadata fields
    getmd       Get metadata for a publication
    setmd       Set metadata for a publication
    getpath     Get the path of a publication
    getbl       Get a list of GUIDs and versions in the baseline
    release     Release all topics in a publication version
```
Run `sdl pub <command> -h` to see the arguments for each command.

**Examples**

1. Get the GUID of the bookmap used in version 1 of a publication.
   ```
   [tintinno@localhost:~]$ sdl pub getmd -g GUID-23E1AB83-C467-4D6E-9CC6-4FAF1EF628DF -v 1 -f fishmasterref
   GUID-F4D13FDB-5D1C-4CB0-8ED0-A6B1BCAF170F
   [tintinno@localhost:~]$
   ```

1. Get the GUIDs of the variables used in version 2 of a publication.
   ```
   [tintinno@localhost:~]$ sdl pub getmd -g GUID-8AAD6C3E-4D54-45CE-A0CF-9D6364ACED5A -v 2 -f fishresources
   GUID-9A10B97F-EC38-473D-843F-D8FF6B853856, GUID-CFE568A5-5104-44DE-BD8A-E0BAA68DD66E
   [tintinno@localhost:~]$
   ```

1. Get the topics from a baseline that are in a draft state.
   ```
   [tintinno@localhost]$ sdl pub getbl -g GUID-756127B8-17F6-4FFF-B549-2DDC34C6A3D3 -v 1 --drafts
   Baseline ID = 'GUID-29E1C577-A389-44AF-9559-2F22C570A2ED'
    GUIDs                                     | Version | Created             | Last Modified       | Status | Title
    ------------------------------------------+---------+---------------------+---------------------+--------+----------
    GUID-604ACB49-6FDD-4A40-A893-56179EC438D5 | 1       | 08/07/2020 10:04:08 | 08/07/2020 10:04:08 | Draft  | 'review1'
    GUID-DD16EE1B-2405-4099-AAB1-8DE38D93ABAC | 1       | 24/04/2020 11:06:29 | 24/04/2020 11:06:29 | Draft  | 'review1a'
   [tintinno@localhost]$
   ```

1. Set every topic in a publication to its released state.
   ```
   [tintinno@localhost]$ sdl pub release -g GUID-756127B8-17F6-4FFF-B549-2DDC34C6A3D3 -v 1
   Released GUID-604ACB49-6FDD-4A40-A893-56179EC438D5 v1 'review1'
   Released GUID-DD16EE1B-2405-4099-AAB1-8DE38D93ABAC v1 'review1a'
   [tintinno@localhost]$
   ```

### Manage Directories
```
usage: sdl dir <options> <path>

Options:
    folders     List the subfolders of a folder
    files       List the contents of a folder
    all         List subfolders and contents of a folder
    mkdir       Create a new folder
    rmdir       Delete an empty folder
    rename      Rename a folder
    move        Move a folder to a new location
```

When listing folders and files, it's often convenient to define bash variables for frequently used 
locations rather than write out the full path each time.
```
export TEST="MyOrg/MyProduct/Testing/tintinno"
```

**Examples**

1. List all the subfolders of a repository folder.
   ```
   [tintinno@localhost]$ sdl dir folders "$TEST"
   Subfolders of 'MyOrg/MyProduct/Testing/tintinno':
    - maps            (type = Map)
    - pub             (type = Publications)
    - topics          (type = Topic)
    - images          (type = Image)
   [tintinno@localhost]$
   ```
1. List all the files of a folder.
   ```
   [tintinno@localhost]$ sdl dir files "$TEST/maps"
   GUID-D81FC37D-378A-4493-93DF-3AF6708DFDD1 | 1 | review0
   GUID-DD16EE1B-2405-4099-AAB1-8DE38D93ABAC | 1 | review1
   GUID-AE041C27-4A1B-4210-8701-391E0959E660 | 1 | review18
   GUID-53CC60BC-D093-416E-9834-72259AA1B81D | 1 | review2
   GUID-C1F0FB91-E724-4652-8626-4FD356301E49 | 1 | review3
   GUID-084D0813-0D0E-40C2-8D4A-29B4520C8E4E | 1 | review4
   GUID-F4D13FDB-5D1C-4CB0-8ED0-A6B1BCAF170F | 1 | review5
   GUID-F62C1BC0-2D0E-4612-8036-B3248852BA6D | 1 | review6
   GUID-F9EFC278-7253-4D43-8730-DC53CCC6F5A8 | 1 | review7
   GUID-8DC5EFBB-FCC0-42AE-A266-266622413CE6 | 1 | review8
   GUID-EEBA1312-C7D8-4433-8C92-1ACD4EB5CD64 | 1 | review9
   GUID-3399631C-07E4-4E5A-B4D7-8C6C0471063F | 1 | test
   GUID-70F4E32B-74E6-4120-8BAE-F78C4AF7D300 | 1 | testpub
   GUID-DBEA03EE-4D96-4955-8A18-025045F1AD6B | 4 | testpub - chapter
   [tintinno@localhost]$
   ```
   The output lists GUID, version, and title.

## Log

A log of each action is written to `~/.sdlcli/sdl.log`.

## Metadata

In SDL, metadata exists at specific levels: the logical level, the version level, and
the language level. The logical level contains one or more versions; versions contain
one or more languages. When you get or set metadata, you have to specify the level at
which your metadata field exists using `-l` or `--level`. For most publications, maps
and topics, metadata levels are either `logical`, `version`, or `lng`.

If you don't want to specify the level every time (because, for example, the FTITLE is
always on the logical level and only there), add your metadata fields and versions to
`metadata.yaml`. This file maps metadata fields to metadata levels. 

1. If you customized your workflow, open `~/.sdlcli/metadata.yaml` and set the 
   values for the default draft state and the default released state for topics.
   Otherwise, leave them as they are.

1. If you use customized SDL metadata, open `~/.sdlcli/metadata.yaml` and add 
   the field name in uppercase and the metadata level in lowercase.

   For example, if you've added an FPARTNUMBER field at the version level to track 
   publication part numbers, add the following under the `pub` section. 
   (The `desc` field is an optional description.)

   ```
   FPARTNUMBER:
     level: version
     desc: the part number for this version of the publication 
   ```

