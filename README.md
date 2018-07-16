# ReitOS

Welcome to ReitOS, the real life? version of Reito Aoba's operating system. 

ReitOS is a Touhou Danmakufu ph3 script that aims to mimic a proper operating system (without being one, of course). It's primarily a proof-of-concept, though if I can get things to work, it may actually be useful (beyond being a gag, of course).

# Usage

This project is still in its infancy

# Built-in Libraries and Applications

### Libraries
None

### Applications
None

# Conventions

### Components and Core Systems

The core of ReitOS is located in the package and the package components (`syslib/comp_reitos_*.dnh`). These functions and tasks, using the format `__COMP_REITOS_BOOT_*()`, utilize CommonData under Area `reitos_core`, with format `comp_reitos_boot_*`. 

### Render Priorities

Render Priorities from 1 through 10 and 90 through 100 are reserved for the core system and built-in applications, while 11 through 19 and 80 through 89 are reserved for user applications.

Application windows use render priority 10.

### Applications

All Applications are standalone scripts that are allowed to `#include` as many of the libraries in `syslib/lib-*.dnh` as they want to. However, they cannot interact with each other.

Each Application must have a `PROPERTIES.TXT` file with the following:
```
APPLICATION_NAME\&lt;Insert your application's name here&gt;
VERSION_NUMBER\&lt;Insert your application's version number here&gt;
AUTHOR\&lt;Insert your name here&gt;
```

For example:

(Example will be provided once this step in development is reached)

# TODO

* Initial Desktop Window
* Dock for applications
* Implement Desktop Icons (Documents, Trash hardcoded)
* Design template for a ReitOS 'Application' (metadata, script format, usable render priorities)

* Design ReitOSnav (Finder system) for clicking on desktop, opening folders, and navigating directory structure
* Design lib_reitos_window.dnh (Window system) for applications to hook into (creates windows that intercept mouse clicks)

# Contributing

Once this system is up and running, I'll consider features, etc.

However, for now, contributions are bug-report only. Please use GitHub Issues for bug reports (if you're crazy enough to be running this to begin with).