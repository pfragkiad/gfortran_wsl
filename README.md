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
        "args": ["-g", "${file}", "-o", "${fileDirname}/${fileBasenameNoExtension}.out"],
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
3. The following block should be included within the `launch.json`:
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "gfortran build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}/${fileBasenameNoExtension}.out",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "gfortran build active file",
      "miDebuggerPath": "/usr/bin/gdb"
    }
  ]
}
```

Write a sample program:
```fortran
program test
    implicit none

    integer(4) :: i = 50

    print *,"KEFTEDES!"

    print *,calc(i)
    

contains
    pure integer(4) function calc(x)
        integer(4),intent(in)::x
        calc = 10*x
    end function
end program
```
