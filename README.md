# gfortran_wsl


1. First be sure that the correct apps are installed.
```bash

sudo apt-get update
sudo apt-get install build-essential gdb
sudo apt-get install gfortran

```

2. Then include the following blocks for the `tasks.json`:
```json
{
    "version": "2.0.0",
    "tasks": [
      {
        "type": "shell",
        "label": "gfortran build active file",
        "command": "/usr/bin/gfortran",
        "args": ["-g", "${file}", "-o", "${fileDirname}/${fileBasenameNoExtension}.exe"],
        "options": {
          "cwd": "/usr/bin"
        },
        "problemMatcher": ["$gcc"],
        "group": {
          "kind": "build",
          "isDefault": true
        }
      }
    ]
}
```
