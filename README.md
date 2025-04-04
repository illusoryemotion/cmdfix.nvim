## Cmdfix

Allow user-defined commands to be typed lowercase in command line mode by hijacking the command line and fixing commands case just before execution. Also fixes typing mistakes such as `:W` and `:Set`, which are respectively fixed to `:w` and `:set` (but only if `W` and `Set` are not already user-defined commands)

### Installation

Your favorite plugin manager should work just fine.

### Configuration

```lua
require("cmdfix").setup({
    enabled = true,  -- enable or disable plugin
    threshold = 2, -- minimum characters to consider before fixing the command
    ignore = { "Next" },  -- won't be fixed (default value)
    aliases = { VeryLongCommand = "vlc" },  -- custom aliases
})
```

#### Lazy.nvim

```lua
  {
    'gcmt/cmdfix.nvim',
    lazy = true,
    event = 'CmdlineEnter',
    opts = {
      enabled = true,  -- enable or disable plugin
      threshold = 2, -- minimum characters to consider before fixing the command
      ignore = { "Next" },  -- won't be fixed (default value)
      aliases = { VeryLongCommand = "vlc" },  -- custom aliases
    }
  },
```


### Commands shadowing

If you have for example a user-defined command `:Marks`, the vim-native command `:marks` will effectively never be executed. If you want to retain the ability to execute both commands independently, just add the `Marks` command to the ignore list.

This plugin leverages the `CmdlineLeave` autocommand for its functionality. According to the neovim documentation (`:help CmdlineLeave`), this autocommand is triggered even in non-interactive uses of `:`. This means it might potentially break some existing functionality (eg. you have a user-defined command with the same name of a native command). It also might impact performace in some cases. Use `<Cmd>` in your mappings instead of `:` to avoid this.

### Fixing rules

1. A command is fixed just before execution or after pressing space after the command (this allows command completion to still work for every command).
2. A lowercase command is fixed if a matching user-defined command is found and it's not ignored.
3. An uppercase command (a command containing an uppercase letter) is transformed to lowercase if a matching user-defined command IS NOT found and is not ignored. The `threshold` option is ignored in this case.

### Notes

- The native `:cabbrev` command can be used for defining abbreviations in command line mode. However, they are rather impractical as the expansion can be triggered anywhere in the command line.
