I have built a little launcher editor in a desktop file called "createlauncher.desktop" that opens up a second, read-only desktop launcher file called "SHORTCUT.desktop" in a text editor where a PATH to a file executable and a NAME are filled in and the launcher is saved to a new location.  It mostly uses xdg-open and the main target is pdf files or else directories.  the xdg-open part is working fine.  This is so that I can have windows-style shortcuts (the desktop files) that Dropbox doesn't foul up like it does symbolic links.  

I can use the launcher if I specify the full path to the read-only desktop file, but I have to invoke a specific text editor on an absolute path for it to work.  I want it to look for the read-only .desktop file in the same directory, wherever that is, so that installation is simple, and I want it to launch the system editor, namely whatever `$VISUAL` is set to.  The launcher provided here works fine when I launch it from the command line using `dex -v createshortcut.desktop` but when I launch it using from the GUI (Cinnamon/nemo) by clicking it does not load the text editor, but gives no error.  When I launch using `desktop-file-validate createshortcut.desktop` it throws errors that say 
```
createshortcut.desktop: error: value "sh -c "exec $VISUAL $(dirname $0)\/SHORTCUT.desktop" %k" for key "Exec" in group "Desktop Entry" contains a non-escaped character '$' in a quote, but it should be escaped with two backslashes ("\\$")
```
3 times.  I then go through and escape the three instances of `$` and then errors go away in `desktop-file-validate createshortcut.desktop` but dex no longer works to launch the editor correctly, and it will still not launch from the desktop click.

Here is the `createlauncher.desktop` file that does launch the second .desktop file in the editor properly, but only from the command line using `dex`:
```

    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Name=create shortcut
    Exec=bash -c -e "exec $VISUAL $(dirname $0)\/SHORTCUT.desktop" %k
    Icon=text-editor
    Type=Application
    Comment=SAVE AS!!!!!
    Terminal=false

```

And here is the read-only target, `SHORTCUT.desktop` that should open in the `$VISUAL` editor when `createlauncher.desktop` works right:

```

    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Type=Application
    Encoding=UTF-8
    Name=NAME
    Exec=xdg-open "PATHTOFILE"
    then Icon=/usr/share/icons/HighContrast/scalable/emblems/emblem-symbolic-link.svg
    Terminal=false
    Comment= (1) make NAME for target (2) enter full absolute PATHTOFILE  of source (3) SAVE AS to the target directory

```

My guess is that there is some ENV variable that bash has picked up that is not in the interactive bash launched from the `createlauncher.desktop` file, but I am in over my head at that point.


https://unix.stackexchange.com/questions/3052/is-there-a-bashrc-equivalent-file-read-by-all-shells seems to point toward my solution, but not sure.

