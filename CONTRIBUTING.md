# Developing for ReitOS

In this document, I will detail how ReitOS works and how to write Applications for the system.

## Overview
ReitOS is a Danmakufu package script that aims to mimic the appearance of a proper OS (but it is of course nowhere near an actual OS). It has all the benefits and isses of Danmakufu (no networking, unable to delete files, no try/catch, crash on error in a script, etc.).

When starting ReitOS, the package will run, using the 800x600 dimensions. This is hardcoded and should NOT be changed.

The package will install fonts, hook into libraries, handle events, and track the mouse. It also handles windowing, closing applications, and all the general stuff. 

As for applications, they should be self-contained for the most part. They can use system libraries but SHOULD NEVER, EVER touch each other. 

## Package and Package Components
The Package contains all of the event listening for the system, and tracks aspects of the system that are not Application specific.

For example, the package has a `MouseTracker` task that saves the mouse's position and states to CommonData so that they can be accessed from other parts of ReitOS and Applications.

These states are as follows:
```java
SetCommonData("reitos_mouse_x", GetMouseX);
SetCommonData("reitos_mouse_y", GetMouseY);
SetCommonData("reitos_mouse_statel", GetMouseState(MOUSE_LEFT));
SetCommonData("reitos_mouse_stater", GetMouseState(MOUSE_RIGHT));
```

The package also has a `WindowTabManager` task that saves the current list of active applications to CommonData so that it can be read in other parts of ReitOS and Applications. This `reitos_running` field contains the Object IDs of all Window Objects currently active, in render order (active Window/foreground at the end).

As for the events in the package, it has three events (+10, 11, and 12) for staging new Window Objects, unstaging Window Objects, and setting Window Objects to background.

## Core Library
The Core Library is the intermediary between libraries, the package, and Applications.

The `Mouse_OnWindow` function determines if mouse is currently on the provided Window object. This means that given the render order of all existing windows, it returns true if the mouse is on the provided Window object AND the part of the Window object in question is visible (i.e. not covered by another Window Object).

## Windowing Library
The Windowing Library is a key library that creates and handles Window Objects. Window Objects are tied to Applications and have a variety of fields, such as their x and y coordinates (top left of the header), their height and width (height does not include 32 pixel header), their components (things rendered onto the window), and more. 

Window Objects use render priorities 10 through 19 and 90 through 99.

Windows are created with their type, height and width, and name. They always spawn in the center of the screen.

Window headers are 32 pixels tall and contain the minimize and close buttons as well as the title.

In regards to interacting with Windows, clicking a Window should bring it to the foreground. Header dragging has yet to be implemented as of writing.

# Writing an Application for ReitOS
// Step by step, TODO

# Conventions

### Components and Core Systems

The core of ReitOS is located in the package and the package components (`syslib/comp_reitos_*.dnh`). These functions and tasks, using the format `__COMP_REITOS_BOOT_*()`, utilize CommonData under Area `reitos_core`, with format `comp_reitos_boot_*`. 

Component .dnh files SHOULD NOT `#include` other scripts. This is because all Components are #included by the main package, which should be the single place for #including the dependencies. Individual Applications of course are standalone scripts run by the Package, so these will #include whatever they need from syslib.

`EV_USER_PACKAGE + 0..2000` are reserved for built-in systems and those numbers should not be used in custom Application events.

### Render Priorities

Render Priorities from 1 through 10 and 90 through 100 are reserved for the core system and built-in applications, while 11 through 19 and 81 through 89 are reserved for user applications.

Application windows use render priority 10 (background) and 90 (foreground).

### Applications

All Applications are standalone scripts that are allowed to `#include` as many of the libraries in `syslib/lib-*.dnh` as they want to. However, they cannot interact with each other. They are allowed to use EV_USER.

Each Application must have a `PROPERTIES.TXT` file with the following:
```
APPLICATION_NAME\&lt;Insert your application's name here&gt;
VERSION_NUMBER\&lt;Insert your application's version number here&gt;
AUTHOR\&lt;Insert your name here&gt;
ICON\&lt;Insert path to icon here&gt;
DRIVER\&lt;Insert path to script here&gt;
```

For example:

(Example will be provided once this step in development is reached)
