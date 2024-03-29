+++
draft = false
date = 2021-05-12T17:09:10-05:00
title = "Termdebug and Ros"
description = "Termdebug and Ros"
slug = ""
authors = []
tags = ['ros', 'vim']
categories = []
externalLink = ""
series = []
+++

# Post - Termdebug and ROS

Today I was trying to use vim's [termdebug](https://vimhelp.org/terminal.txt.html#terminal-debug) feature for some
development w/ ROS.  Getting this to work properly in vim isn't as straight
forward as I would like (mostly because ROS is a tad infectious). If you run
vim in a ROS workspace, you'll quickly notice that you can't run
```vim
:!rosrun my_pkg my_executable
```
This is because your workspace is not sourced properly. All subshells will have
the same environment as vim, so you can do something like

```vim
:!source devel/setup.bash && rosrun my_pkg my_executable --flags
```

But that affects only the subshell it was run in. The easiest way to avoid this
is to run the source command before you launch vim.

[ROS recommends](https://answers.ros.org/question/222530/running-rosrun-with-gdb/) running gdb by doing something like
```bash
rosrun --prefix gdb my_pkg my_executable
(gdb) r --flags
```

This does not play well with vim's termdebug. I understand how this `rosrun`
command is "convenient" but it leads to the infantilization of developers since
many times we don't even think about where our built executables end up. Anyway,
the quickest way to find the executable is
```bash
find . -name "my_executable"
```

The executable should be found in `./build/my_pkg/devel/lib/my_pkg`. From within
vim you can load[^load] this with

```bash
:packadd termdebug
:Termdebug ./build/my_pkg/devel/lib/my_pkg
```

Now you _should_ be able to run term debug commands from within vim and all
appropriate shared libraries should load correctly.

(Don't forget to build in debug mode so all the debug symbols are there)

[^load]: You can also load this with `file` within the gdb window.
