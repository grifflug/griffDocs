# Sublime Setup

![Sublime Setup](http://i.imgur.com/38AYv1t.png)

## Packages
+ **[SublimeREPL](https://github.com/wuub/SublimeREPL)**
  - I've been playing around with this to include a shell at the bottom of the window. Useful for making quick files etc. You can also run full interpreters for various languages.
+ **[SplitScreen](https://github.com/spadgos/sublime-SplitScreen)**
  - Can use a shortcut to quickly switch to a custom split screen setup
+ **[Emmet](http://emmet.io/)**
  - For all my HTML needs
+ **[RubyTest](https://github.com/maltize/sublime-text-2-ruby-tests)**
  - Found this through the Rails tutorial I've been reading. Runs your test code with a keyboard shortcut
+ **[SublimeERB](https://github.com/eddorre/SublimeERB)**
  - Another Ruby package, makes inserting embedded Ruby easier and comes with some good autocomplete features
+ **[SideBar Git](https://github.com/SublimeText/SideBarGit)**
  - Run git commands from the sidebar

## Theme and colours
+ **Dark.sublime-theme**
  - To get the tabs, I took the tab code from the 'Nil' theme (available through package manager)
+ **Tomorrow Color Schemes** (Tomorrow-Night-Eighties)

## User settings
**Packages/User/Preferences.sublime-settings**  
    
    {
      "color_scheme": "Packages/Tomorrow Color Schemes/Tomorrow-Night-Eighties.tmTheme",
      "font_size": 18.0,
      "ignored_packages":
      [
        "Vintage"
      ],
      "tab_size": 2,
      "theme": "Dark.sublime-theme",
      "translate_tabs_to_spaces": true,
      "highlight_modified_tabs": true
    }

**Packages/User/Default (OSX).sublime-keymap**  
The first keymap allows the SublimeERB package to work  
The second keymap displays the split screen setup shown in the screenshot  

    [
      { "keys": ["ctrl+shift+."], "command": "erb" },
      { "keys": ["super+alt+6"], "command": "set_layout",
        "args": {
          "cols": [0.0, 0.5, 1.0],
          "rows": [0.0, 0.75, 1.0],
          "cells": [
              [0, 0, 1, 1],
              [1, 0, 2, 1],
              [0, 1, 2, 2]
          ]
        } 
      }
    ]
