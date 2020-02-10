Cgywin: Taking back control over Windows (lab computers)
================
Philipp Baumann || <philipp.baumann@usys.ethz.ch>

-   [Overview](#overview)
-   [Updating packages](#updating-packages)
-   [Mounted drives](#mounted-drives)

Overview
--------

[Cgywin](https://cygwin.com/) is aimed at bringing Unix tools to the Windows environment. This repo contains an opinionated set of recipes, and is particularly tailored to people suffering from Windows computers of measurement devices in laboratories.

Updating packages
-----------------

The Cgywin distribution ships only base packages by default. The executable `setup-x86.exe` provides updates and serves to install new packages.

``` bash
# Add terminal command
```

Mounted drives
--------------

All system drives such as `C:` are mapped under `/cgydrive`, for example `C:` can be accessed via `/cgydrive/c`.
