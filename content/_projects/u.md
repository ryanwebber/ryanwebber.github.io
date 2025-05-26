---
name: u
github: u
description: A hackable script manager
layout: project
---

```bash
$ u --help

User script manager and executor

Usage: u <script> [args...]

Options:
    --help  Print this help message and exit

Available scripts:
  scripts/create  (create)      Create a new script
  scripts/delete  (delete)      Permanently delete a script
  scripts/edit    (edit)        Edit a script with $EDITOR
  scripts/help    (help, list)  List available scripts

Available templates:
  bash        A shell script runnable with bash
  perl        A perl script
  python2     A python2 script
  python3     A python3 script
  python3-uv  A python3 script runnable with uv
  ruby        A ruby script
  sh          A shell script runnable with sh
```

## About

I had an unorganized collection of scripts that were lying around in various directories,
and I thought it would be interesting to build a small tool to manage them.

There are two novel ideas about this implementation:
 1. The script manager uses itself to expose the scripts that actually perform script management
    tasks like creating, deleting, and listing scripts.
 2. The scripts, script templates, and script runner are embedded in the release binary as a
    tarball which is unpacked at runtime. After the first run, the release binary just passes
    the arguments to the script runner (a python script). This means the entire script directory
    that contains templates, user added scripts, script management scripts, the manifest, and
    the script runner can all be source controlled and modified. The unpacked directory contents
    look like this:
    ```
    $HOME/.uscripts
    ├── scripts/
    │   ├── scripts.create/
    │   ├── scripts.delete/
    │   ├── scripts.edit/
    │   └── scripts.help/
    ├── templates/
    │   ├── bash/
    │   ├── perl/
    │   ├── python2
    │   ├── python3/
    │   ├── python3-uv/
    │   ├── ruby/
    │   └── sh/
    ├── utils/
    ├── main.py
    └── manifest.json
    ```



I've actually found this to be pretty useful, especially using `uv` to eval python scripts with
[dependencies declared in a special comment at the top of the file](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) -- thus
eliminating the need for a virtualenv or a requirements.txt file.

There's a lot more I could do with this (semantic search??) but for now it's suited my needs well.
