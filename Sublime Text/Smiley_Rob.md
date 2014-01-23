# Smiley Rob's Sublime Setup

![Sublime Setup](http://i.imgur.com/u2WgdVK.png)

## Packages
+ Boron Colour Scheme
+ Decent Colour Scheme
+ DocBlockr
+ Emmet
+ Git
+ SideBarEnhancements
+ SideBarGit
+ SublimeERB
+ SublimeLinter
+ Theme - Nil

[How to install Package Control](https://sublime.wbond.net/installation)

## User settings
**Packages/User/Preferences.sublime-settings**

    {
      "always_show_minimap_viewport": true,
      "auto_complete_delay": 0,
      "auto_id_class": true,
      "bold_folder_labels": true,
      "close_windows_when_empty": true,
      "color_scheme": "Packages/User/Samuel's decent (SL).tmTheme",
      "copy_with_empty_selection": false,
      "create_window_at_startup": true,
      "drag_text": false,
      "ensure_newline_at_eof_on_save": true,
      "fade_fold_buttons": false,
      "font_face": "monaco",
      "font_size": 14.0, // I change this regularly
      "ignored_packages":
      [
        "SublimeREPL",
        "Vintage"
      ],
      "indent_guide_options":
      [
        "draw_active"
      ],
      "indent_to_bracket": true,
      "line_padding_bottom": -1,
      "remember_open_files": false,
      "scroll_past_end": false,
      "tab_size": 2,
      "theme": "Nil.sublime-theme",
      "translate_tabs_to_spaces": true,
      "trim_automatic_white_space": false,
      "trim_trailing_white_space_on_save": true
    }


**Packages/User/Default (OSX).sublime-keymap**

    [
        // DEFAULTS
        {"keys": ["super+shift+v"], "command": "paste"},
        {"keys": ["super+v"], "command": "paste_and_indent"},
        {"keys": ["super+shift+r"], "command": "reindent","args": {"single_line": true}},
        //{ "keys": ["super+alt+ctrl+["], "command": "fold" },
        {"keys": ["super+alt+right"], "command": "unfold"},

        // BRACKETS
        // Fold contents between brackets
        {
            "keys": ["super+alt+left"],
            "command": "bh_key",
            "args":{
                "plugin": {
                    "type": ["__all__"],
                    "command" : "bh_modules.foldbracket"
                }
            }
        },

        // Toggle high visibility mode
        {"keys": ["ctrl+alt+super+b"],"command": "bh_toggle_high_visibility"},
        {"keys": ["ctrl+t"], "command": "side_bar_new_file2"},
        {"keys": ["f2"], "command": "side_bar_rename"},
        {"keys": ["ctrl+alt+f"], "command": "side_bar_find_files_path_containing"},

        // Got to beggining of line with Command Left
        { "keys": ["super+left"], "command": "move_to", "args": {"to": "hardbol", "extend": false} },
        { "keys": ["shift+super+left"], "command": "move_to", "args": {"to": "hardbol", "extend": true} },

        // SublimeERB
        { "keys": ["ctrl+shift+."], "command": "erb" }
    ]

## Shortcuts

+ `Cmd Shift P` // Great for quick access to everything, ie syntax highlighting, package control
+ `Cmd T "file#keyword"` // find a keyword in any folder in your project
