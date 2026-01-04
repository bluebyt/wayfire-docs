# Style guide

In general, you should use [`uncrustify`](https://github.com/uncrustify/uncrustify) with the `uncrustify.ini` file from the repo. There are also [automatic checks for your PR](https://github.com/WayfireWM/wayfire/blob/master/.github/workflows/ci.yaml).

To run `uncrustify` on all project files:
```sh
$ git ls-files | grep "hpp$\|cpp$" | xargs uncrustify -c uncrustify.ini --no-backup
```

## Names

Class, method and variable names are all in `snake_case`. Class names should end in `_t`.

File names are all lowercase, with `-` as word separator. Headers end in `.hpp`. So, a header could be called `this-is-example-header.hpp`.

## Headers

1. Use #pragma once instead of `#ifdef`.

## Environment variables

* WAYFIRE_PLUGIN_XML_PATH: Wayfire will search this path for plugin XML files. Useful if you're developing custom plugins.
* WAYFIRE_PLUGIN_PATH: Wayfire will search this path for plugin shared object files. Useful if you're developing custom plugins.

# Wayfire Tests

The [`wayfire-tests`](https://github.com/ammen99/wayfire-tests) are a suite of tests for wayfire.

Wayfire pull requests accompanied by a wayfire test pull request to demonstrate a bug fix will be prioritized.