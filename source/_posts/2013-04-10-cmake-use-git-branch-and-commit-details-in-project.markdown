---
layout: post
title: "CMake: Use Git Branch &amp; Commit Details in Project"
date: 2013-04-10 19:16
comments: true
categories: [cmake, git]
---

To use git branch and commit hash information in a CMake project, add the
following lines to `CMakeLists.txt`

```cmake
# Get the current working branch
execute_process(
  COMMAND git rev-parse --abbrev-ref HEAD
  WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
  OUTPUT_VARIABLE GIT_BRANCH
  OUTPUT_STRIP_TRAILING_WHITESPACE
)

# Get the latest abbreviated commit hash of the working branch
execute_process(
  COMMAND git log -1 --format=%h
  WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
  OUTPUT_VARIABLE GIT_COMMIT_HASH
  OUTPUT_STRIP_TRAILING_WHITESPACE
)
```

[execute_process()][cmake_execute_process] runs a single command or a
sequence of commands and optionally gathers the output into CMake variables. The
`COMMAND` is executed in `WORKING_DIRECTORY` and the STDOUT is saved to
`OUTPUT_VARIABLE` after stripping trailing whitespace. The output of `git log`
can be [customized][git_log_format].

<!--more-->

Now that these values are available for CMake, there are two ways to use them in
a project.

* The easiest and simplest way is to use [add_definitions()][cmake_add_definitions] in `CMakeLists.txt` to make
  the variables available to the C/C++ project.
{% codeblock lang:cmake %}
add_definitions("-DGIT_COMMIT_HASH=${GIT_COMMIT_HASH}")
add_definitions("-DGIT_BRANCH=${GIT_BRANCH}")
{% endcodeblock %}

* CMake also has the capability to generate a header file (or any file for that
  matter) with the [configure_file()][cmake_configure_file] command. We can
utilize this to generate a header file with these definitions and include it in
the project.

You'll need to include a template header file in the project source with some
CMake specific annotations to achieve this. I choose to put `version.h.in` in
the `include` folder in my source directory.

{% codeblock version.h.in lang:cpp %}
#ifndef VERSION_H
#define VERSION_H

#define GIT_BRANCH "@GIT_BRANCH@"
#define GIT_COMMIT_HASH "@GIT_COMMIT_HASH@"

#endif
{% endcodeblock %}

`@GIT_BRANCH@` & `@GIT_COMMIT_HASH@` will be substituted with the
corresponding values obtained as the output of `execute_process()` command. The
double quotes around the substitution variables are necessary if these variables
have to be used as strings in a C/C++ project.

Add the following command to `CMakeLists.txt` to perform the generation.

```cmake
configure_file(
  ${CMAKE_SOURCE_DIR}/include/version.h.in
  ${CMAKE_BINARY_DIR}/generated/version.h
)
```

This will write a `version.h` file to `generated` directory of `${CMAKE_BINARY_DIR}`. Next, the folder `generated` has to be added to the C/C++ `INCLUDE_PATH` in `CMakeLists.txt`

```cmake
include_directories(${CMAKE_BINARY_DIR}/generated)
```

Include `version.h` in a C/C++ file and you are done.

I've created a sample C project which uses this on [Github][github_project].

PS: Ryan Pavlik has a CMake module for this available on [Github][github_ryan].
[stackoverflow][stackoverflow] has a thread detailing it's usage.

[cmake_execute_process]: http://www.cmake.org/cmake/help/cmake2.6docs.html#command:execute_process "CMake execute_process"
[cmake_add_definitions]: http://www.cmake.org/cmake/help/cmake2.6docs.html#command:add_definitions "CMake add_definitions"
[cmake_configure_file]: http://www.cmake.org/cmake/help/cmake2.6docs.html#command:configure_file "CMake configure_file"
[git_log_format]: https://www.kernel.org/pub/software/scm/git/docs/git-log.html#_pretty_formats "Git log formatting"
[github_project]: https://github.com/pmirshad/cmake-with-git-metadata "Sample project on Github"
[github_ryan]: https://github.com/rpavlik/cmake-modules "rpavlik/cmake-modules"
[stackoverflow]: http://stackoverflow.com/a/4318642 "How to read a CMake Variable in C++ source code"
