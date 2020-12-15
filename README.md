# Paht's Fortran learning README
Documenting my Fortran learning with examples. 

## Makefile Autodependancy generation
These sets of codes are compiled using a makefile and fortdepend for auto-dependancy generation. This means that you do not need to specify 
```
something.o:
  depends_on_1.o
  depends_on_2.o
```

This is automatically generated by fortdepend and is included in the makefile `include(my_project.dep)`
To get started (this works on linux but can work on windows if you want)
<ol>
  <li>Install fortdepends using `pip install fortdepend`</li>
  <li>In terminal do `find -name fortdepend` that is your reference path that you need to set in the make file. It should reference the binary. 
  You should simply be able to call fortdepend in your folder and have it generate a dependancy file `fortdepend -o Makefile.dep`. If this doesn't work then that's not the right file</li>
</ol>

Give this project a try once you've set up fortdepends by running `make`

## Debugging using Visual Code 
### Windows WSL and Mac
Step 1. Install Visual studio code and modern fortran library
Step 2. Install GDB. Windows (http://www.gdbtutorial.com/tutorial/how-install-gdb) Mac (https://www.ics.uci.edu/~pattis/common/handouts/macmingweclipse/allexperimental/mac-gdb-install.html)

Step 3. Install Modern Fortran Library. Note: syntax highlighting depends on the file extension so it's important to name your files either F90 or F95 for propper highlighting.

Step 4 (optional) Set up visual studio code to build by creating a task.json inside the .vscode folder  https://code.visualstudio.com/docs/editor/tasks
  Note: It doesn't matter what is in this file because you will be replacing it with the following
  
```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": ["make clean","make all","",""],
            "args": [],
            "group": "build",
            "presentation": {
                // Reveal the output only if unrecognized errors occur.
                "reveal": "silent"
            },
            // Use the standard MS compiler pattern to detect errors, warnings and infos
            "problemMatcher": "$msCompile"
        }
    ]
}
```

Step 5. Now that you've created the build task, it's time to set up the debug task. Create a Launch.json (https://code.visualstudio.com/docs/editor/debugging). Replace the launch json with the following code.

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Debug Fortran",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/main.exe",
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
            "preLaunchTask": "build"
        }
    ]
}
```

Now you are ready to add breakpoints and debug your code. 
