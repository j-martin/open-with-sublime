# Open With
> ...something other than Sublime Text

## What?

Opens the file you are working with into another editor or application. While:
 - Preserving your cursor position.
 - Allow flexible key binding.
 - Allow templating/variables (line number, columns number, etc.).
 - Activates the target editor window (if required).

It currently only support Sublime Text 3 on OSX.

## Why?

Some text editors work better than some other for some kind of work (e.g.:
IntelliJ for refactoring, VIM for quick edits/macros, Emacs for orgmode, etc.),
but Sublime shines when it comes to its multi-cursor support (for example).

- IntelliJ as multi cursor support, but it's slow and wonky at best.
- VIM is VIM. Sublime, Emacs and IntelliJ have decent VIM emulators, but it's
  not the same once you hit more advanced features.
- Emacs has org mode.

The idea is to be able to switch between them with minimal cognitive load and
preserve the cursor location when switching between editors.

## Possible use cases / scenarios

- Editing some file in IntelliJ.
- Call Sublime [as an IntelliJ external
  tool](https://stackoverflow.com/questions/16130586/is-there-a-way-to-connect-intellij-idea-to-sublime-text-on-mac-os-x)
  with a keybinding (of your choice).
- Edit something in Sublime.
- Trigger Open With Intellij with a keybinding (of your choice).

If both editors are configured to save on loss of focus, it makes things super
smooth.

## Installation

1. Install [Package Control for SublimeText 3](https://packagecontrol.io/installation).
1. Type `cmd + shift + p` or `ctrl + shift + p` | `Package Install` | `Open With`

## Configuration

By default, using `Open with...` from the command palette will let you use IntelliJ and Finder.

To add your own editor, add the following settings.
- the `name` key is the window name (to be activated)
- Adding your own application/editor to your user settings will help populating
  the command palette (`cmd+shift+p`).

#### Variables/Placeholder:
The are template placeholders and will be replaced when launching the editor/application.

- `{filename}` (fully qualified)
- `{directory}` (fully qualified)
- `{line}` (number)
- `{column}` (number)

### Example

#### `Preferences: Settings - User`

```
{
  "open_with": [
    {
      "name": "IntelliJ IDEA",
      "command": ["/usr/local/bin/idea", "{filename}:{line}"]
    },
    {
      "name": "NeoVim",
      "command":
      ["/usr/local/Cellar/neovim-dot-app/HEAD/bin/gnvim", "{filename}", "+{line}"],
    },
    {
      "name": "MacVim",
      "command": ["/usr/local/bin/mvim", "{filename}", "+{line}"]
    },
    {
      "name": "Emacs",
      "command": ["/usr/local/bin/emacsclient", "+{line}:{column}", "{filename}"]
    },
    {
      "name": "Mou",
      "command": ["open", "-a", "Mou", "{filename}"]
    },
    {
      "name": "Finder",
      "command": ["open", "{directory}"]
    },
    {
      "name": "VMD",
      "command": ["/usr/local/bin/vmd", "{filename}"]
    }
  ]
}
```

#### `Preferences: Key Bindings - User`

```
[
  { "keys": ["ctrl+alt+super+shift+-"], "command": "open_with", "args": {"name": "IntelliJ IDEA"} },
  { "keys": ["ctrl+alt+super+shift+d"], "command": "open_with", "args": {"name": "MacVim"} }
]
```

Alternatively if you don't want to add anything to your settings and just want
the key bindings, just specify the `name` and `command` as `args`.

```
[
  { "keys": ["ctrl+alt+super+shift+d"], "command": "open_with", "args": {
      "name": "MacVim", "command": ["/usr/local/bin/mvim", "{filename}", "+{line}"]}
  }
]
```

## Different approaches

- Defining the editor as a build tool (kinda wonky).
- [Sidebar Enhancements](https://github.com/titoBouzout/SideBarEnhancements),
  Wonderful plugin (you should install it), but last time I checked it doesn't
  preserve line numbers and a bit awkward to set key bindings and `On OSX,
  invoking shell commands is NOT supported.`
