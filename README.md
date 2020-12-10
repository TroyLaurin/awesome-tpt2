# AI scripts for use in The Perfect Tower 2
https://games.fs-studios.com/games/perfecttower2

All of these scripts use relative screen positioning - the bottom left of the screen is `(0.0,0.0)`, and the top right of the screen is `(1.0,1.0)` - think of it as a percentage distance of the screen size.  To use these scripts requires modifying the below script and setting your own screen coordinates for `XMAX` and `YMAX`:
UNIVERSAL_MOUSE_EXTENTS
`F1VOSVZFUlNBTF9NT1VTRV9leHRlbnRzAQAAAAZ3YWtldXAAAAAAAgAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQEWE1BWAhjb25zdGFudAMAAAAAAPiPQBFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQEeW1heAhjb25zdGFudAMAAAAAAPiBQA==`

Many thanks need to go to the discord users for either directly contributing scripts, or making the scripts possible through discussion.  A link to the discord can be found on the game website.

## Creating scripts

The easiest way to obtain screen coordinates is to use these scripts (credit: Tortoise Box).  Turn on AI then move the mouse to the top-right corner of the screen to record the max screen size, then read the relative mouse coordinates from the variable list on the right.  Note that you should probably disable the `UNIVERSAL_MOUSE_EXTENTS` script when using this to avoid conflict with the `XMAX` and `YMAX` variables.

UNIVERSAL_MOUSE_DEBUG (5 lines)
`FXVuaXZlcnNhbF9tb3VzZV9kZWJ1ZwEAAAAGd2FrZXVwAAAAAAUAAAAPZ2VuZXJpYy5leGVjdXRlCGNvbnN0YW50BApfbWF4ZmluZGVyDGdlbmVyaWMud2FpdAhjb25zdGFudAMAAAAAAADgPxFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQGbW91c2V4EWFyaXRobWV0aWMuZG91YmxlBnZlYzIueA5tb3VzZS5wb3NpdGlvbghjb25zdGFudAQBLxFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQEeG1heBFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQGbW91c2V5EWFyaXRobWV0aWMuZG91YmxlBnZlYzIueQ5tb3VzZS5wb3NpdGlvbghjb25zdGFudAQBLxFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQEeW1heAxnZW5lcmljLmdvdG8IY29uc3RhbnQCAwAAAA==`

_MAXFINDER (3 lines)
`Cl9tYXhmaW5kZXIAAAAAAAAAAAMAAAARZ2xvYmFsLmRvdWJsZS5zZXQIY29uc3RhbnQEBHhtYXgKZG91YmxlLm1heBFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQEeG1heAZ2ZWMyLngObW91c2UucG9zaXRpb24RZ2xvYmFsLmRvdWJsZS5zZXQIY29uc3RhbnQEBHltYXgKZG91YmxlLm1heBFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQEeW1heAZ2ZWMyLnkObW91c2UucG9zaXRpb24MZ2VuZXJpYy5nb3RvCGNvbnN0YW50AgAAAAA=`
