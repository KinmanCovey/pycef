# Core development


Table of contents:
* [Cython code](#cython-code)
* [Version numbers explained](#version-numbers-explained)
* [Updating to a newer CEF version](#updating-to-a-newer-cef-version)
* [Debug CEF stack trace](#debug-cef-stack-trace)
* [Debug Cython](#debug-cython)


## Cython code

Editors:
* PyCharm - https://www.jetbrains.com/pycharm/
* LiClipse (PyDev) - http://www.liclipse.com/
* For [Submlime Text 3](https://www.sublimetext.com/3) editor use the [Cython syntax highlighting](https://github.com/NotSqrt/sublime-cython) and the [CTags](https://github.com/SublimeText/CTags) plugin.


Style guidelines:
* try to comply with the [PEP 8 style guide](http://www.python.org/dev/peps/pep-0008/)
* use 4 spaces for indentation
* commit unix-style newlines (\n)
* limit all lines to a maximum of 79 characters (comments should be shorter, max 60-65 chars)
* do your best for the new code to be consistent with existing code base


## Version numbers explained

When building CEF and CEF Python from sources, you should use the CEF and Chromium versions listed in the BUILD_COMPATIBILITY.txt file.

The naming convention for CEF Python versions is XX.YY (for example 29.4), where XX is the Chromium major version, and YY is the cefpython internal version.

An example Chromium version is 29.0.1547.80, where 29 is the major version, 0 is the minor version, 1547 is the build version (branch), and 80 is the patch version.

Numbers in CEF version eg "3.2526.1366.g8617e7c" mean:
* CEF 3
* [branch](https://bitbucket.org/chromiumembedded/cef/branches/)
* git commit number
* git commit short hash

Old CEF versions naming included branch and [SVN revision](https://code.google.com/p/chromiumembedded/source/list).


## Updating to a newer CEF version

To see changes in the CEF API compare "cefpython/include/" directory with
the new CEF "include/" directory (exclude the "capi/" directory). When
doing so compare the CEF header files downloaded directly from the CEF git
repository, as the CEF "include/" directory obtained from binary build will
not include platform specific header files.


## Debug CEF stack trace

On Windows you need to download release symbols (.pdb) and install
Visual Studio.

On Linux:

1. Install gdb with the command "sudo apt-get install gdb"
2. Type "gdb python"
3. Inside gdb type "run pyqt.py"
4. On segmentation fault to display stack trace type "bt"


## Debug Cython

Debugging Cython is currently supported only on Linux. Before you can
debug you have to install the following packages:
```
python-dbg
python-wxgtk2.8-dbg
```

Install Cython with the debug version of Python:
```
cd Cython-0.19.2/
sudo python-dbg setup.py install
```

To debug CEF Python add the "debug" argument to the "compile.py" script:
`python-dbg compile.py 99.99 --debug` .

After a while you should see a GDB console awaiting a command, to run
the app type `cy run` .

To get the stack trace type `cy backtrace` .

The command for running and debugging script is:
`cygdb . --args python-dbg wxpython.py` .

To run commands automatically in gdb add "-x gdb.cmds" flag. Some example
commands in gdb.cmds file: `cy run` and `quit` .

The final command would look like:
`cygdb . -x gdb.cmds --args python-dbg wxpython.py` .

For more commands see the "Using the debugger" section in the Cython documentation:
http://docs.cython.org/src/userguide/debugging.html#using-the-debugger
