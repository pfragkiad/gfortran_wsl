# gfortran_wsl


1. Be sure that the correct extensions are loaded:

### Locally

![local extensions](local_extensions.png)

### In Ubuntu (WSL)

![extensions](extensions.png)

2. First be sure that the correct apps are installed.
```bash
sudo apt-get update
sudo apt-get install build-essential gdb
sudo apt-get install gfortran
```

3. Then include the following blocks for the `tasks.json`:
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
4. The following block should be included within the `launch.json`:
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

Write a sample program, for example:
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

## Notes

### Install fortls

```console
cliff@DESKTOP-D131KNR:~/mycode/fortran/fort1$ pip3 install fortls
Defaulting to user installation because normal site-packages is not writeable
Collecting fortls
  Downloading fortls-2.13.0-py3-none-any.whl (92 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 92.5/92.5 KB 877.0 kB/s eta 0:00:00
Collecting packaging
  Downloading packaging-22.0-py3-none-any.whl (42 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 42.6/42.6 KB 4.3 MB/s eta 0:00:00
Collecting json5
  Downloading json5-0.9.10-py2.py3-none-any.whl (19 kB)
Installing collected packages: json5, packaging, fortls
  WARNING: The script pyjson5 is installed in '/home/cliff/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The script fortls is installed in '/home/cliff/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed fortls-2.13.0 json5-0.9.10 packaging-22.0
```
