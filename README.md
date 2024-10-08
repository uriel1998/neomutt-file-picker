# NeomuttFilePicker

- Attach files to your emails in [NeoMutt](https://github.com/neomutt/) using [Ranger](https://github.com/ranger/ranger) or [Vifm](https://github.com/vifm) as your file manager.
- Choose a directory to store attachments with Ranger or Vifm


filepicker options:

-h show help
-f : Choose fzf as your picker 
-fd DIRECTORY : specify starting directory for fzf
-n : use nnn as picker
-r : use ranger as picker
-v : use vifm as picker
-m : process for mutt attachments (otherwise just puts filename)

The main script is based on the [this topic](https://www.reddit.com/r/commandline/comments/cbxvdf/combine_neomutt_with_ranger/) and improved to allow attaching mulptiple files with spaces in the name and path.

## Attaching files

1. Copy the `filepicker` file to your `.config/mutt` or wherever is your config directory.
2. Add the following line to your `.muttrc` so that you can attach files with `A`

```
macro compose A "<shell-escape>bash $HOME/.config/mutt/filepicker -m<enter><enter-command>source ${HOME}/.config/wombat_filepicker_tmp<enter><shell-escape>bash $HOME/.config/mutt/filepicker -c<enter>" "Attach with your file manager"
```
3. Now, on the email sending screen (when you already wrote the text and saved it) type `A` instead of standard `a` to call the script. The file manager should appear. Choose files that you want to attach (tag them if multiple files) and hit Enter. Hit Enter twice more when asked. 


If you wish to bind it to a key in tmux to invoke (for example, to select and paste a filename), use the following in your `tmux.conf`:

```
unbind e
bind e if-shell -b '/path/to/filepicker -l | xclip ' "send-keys ''"
```

Or if you'd rather, without `-l | xclip` and just have your program read the filename from
`$XDG_CONFIG_DIR/wombat_filepicker_tmp` (probably `${HOME}/.config/wombat_filepicker_tmp` )  Or maybe even just with the `-l` switch, depending on what works with your flow.


## Saving attachments

1. Copy the `dirpicker` file to your `.config/mutt` or wherever is your config directory.
2. Add the following line to your `.muttrc` so that you can choose the folder where to store files with `S`

```
macro attach S "<shell-escape>bash $HOME/.config/mutt/dirpicker -m<enter><enter-command>source ${HOME}/.config/wombat_dirpicker_tmp<enter>o" "Choose folder!"
```
3. Now, on the attachment screen (by default, reached with `v`), type `S` instead of standard `s` to call the script. The file manager should appear. Choose the folder with ranger, quit with `q`. Mutt may then ask for the filename in the folder. 

Use similar arrangements as with the file picker for other programs as well, including tmux.


## Settings

In the `filepicker` and `dirpicker` files you can choose which file manager to use. Ranger by default, but you can uncomment Vifm and comment Ranger if you like.

## Issues

- You might need to make the script executable if you have error about permissions. For example: `chmod =rwx filepicker`

## TODO 

- The dirpicker should be adapted to handle several attachments at once (perhaps with ripmime or munpack?)
