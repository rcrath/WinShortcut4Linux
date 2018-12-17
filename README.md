# WinShortcut4Linux
windows-sytle shortcuts for for linux file systems, especially for Dropbox users on Linux.

## Rationale

Why make a windows style shortcut for Linux?  It is for using Dropbox cross-platform with windows.  Dropbox does not handle linux symbolic links gracefully at all. They translate symlinks as absolute paths, causing all sorts of misplaced file troubles where you save something in one folder in windows and expect it to be in the other if you have symlinked to something, but when you go back to linux it is not there and your symlinked folder has become an absolute path.  

Dropbox's solution, which works for many cases is to only symlink to targets that are outside of the Dropbox folder hierarchy and put the actual files (`SOURCEFILE`) in the Dropbox hierarchy (`ln -s ~\Dropbox\SOURCEFILE ~\TARGETFILE`).  This is clunky, but I'll give an example.  I want to back up a presets folder for an audio synth called "`SYNTH`" with a subfolder "`presets`" in linux (you can also do this in windows after v. 7 I think, using the linking functions. I use a file explorer extension called LinkSHellExtension to do make this easier).  Here is the somewhat cludgy process:

```
$ cd ~/SYNTH
$ cp -R presets ~/Dropbox/links/SYNTH/
$ rename presets presets.bk
$ ln -s ~/Dropbox/links/SYNTH/presets presets
```

Clunky, but what do you do if you want to link to something woth a source and target both within Dropbox?  At that point, the handling of symlinks as absolute paths creates troubles that will lead to missing and duplicated files if you go back and forth between Win and Linux.

That is the case for the use of WinShortcut4Linux.  Regular windows shortcuts are little `.lnk` files that do not pass themselves off as the original like a symlink, but rather just open the file or folder by putting the launch exec in the little  .lmk file and acting as a launcher.  In linux under the freedesktop specification, this is done with .desktop files, but you have to write the files manually or open a launcher app to artificially create the .desktop file, both of which are sometimes inconvenient.   

WinShortcut4Linux simply loads a read-only  desktop file template with built in instructions  into your default graphical text editor (`$VISUAL`) that uses `xdg-open` to execute the default application for the file or folder you wish to open.  You can see what is in the text file by looking at the source. It is read only so that you are not constantly overwriting the template.  

So now, when I have a media related book in my media folder that I also think should also  be in my technology folder, instead of trying to remember which folder I ended up deciding on, I can search for "Shortcut" in my desktop menu operation and  that will give me the template. once I have created the new shortcut, saved it to my tech folder and set it to be a trusted launcher (see below for example).

## Installation

Copy the top level folder into your home folder,  then navigate to `~/.local/share/applications` in your graphical file browser of choice and click once on `.shortcut to .desktop`to mark it as trusted. Its name in the browser should change to `NAME`.  Do the same for `createshortcut.desktop`. That should make it searchably show up in your desktop environment's application menu if you just type in "shortcut" in the DE's search bar.  The two files can be run from anywhere as long as the two desktop files are in the same folder, but you will lose the DE menu availability if you change the location from `!/.local...`. After you save a new desktop file from the template you will need to mark your new desktop file as trusted the same way as above.  After that, it will launch the source file from its new location within dropbox without using symlinks.

## example

I have a pdf called `MEDIA_TECH.PDF` stored in my `~/Dropbox/media` folder but I also want to be able to access it from `~/Dropbox/tech/`.  Here's how:

- using the DE's search function, launch "shortcut" which should open in your `$VISUAL` text editor, for example, gedit or kate or atome or xed or .....
- replace `NAME` with `MEDIA_TECH.PDF` (you can use any name you wish, it does not need to match the filename of the source).
- replace `PATHTOFILE` with  `~/Dropbox/media/MEDIA_TECH.PDF`--Here the filname must match the source.
- you can double click on `NAME` and then `PATHTOFILE`and the item will be selected so that you can just type or copy-paste the new name and path without fiddling around too much.  
- `SAVE AS` to the target folder, here `~/Dropbox/tech/`using a descriptive filename instead `shortcut to .desktop`.  The filename does not need to be the same as anything, but as soon as you mark it trusted, as above, it changes to whatever you replace `NAME` with in the template, but I would recommend something similar to the `NAME` field.  And keep the `.desktop`extension.

I need to do this for lots of files, so I find that the few seconds it shaves off of making a launcher saves me time and trouble.  All you need is the path to the source, which can be a folder or a file, and let `xdg-open` do the app choice.  

## troubleshooting

1. Are you in Linux?  The shortcuts do not work in Windows.  You cannot make or launch them from Windows.  In the Windows context they are just little empty useless things, just like how Windows `.lnk` files don't work in a Linux.
2. Are the two files in the same folder?  They must be in `~/local/share/applications` for your DE to pick them up.  
3. Do you have xdg-open configured properly?
4. Have you set $VISUAL to use your editor of choice? Type `echo $VISUAL` from a command prompt in linux to see.  
5. For problems 3 and 4, refer to Google!

For anything else, fire up an issue.

### future plans

1. use Zenity to fill in the `NAME` and `PATHTOFILE` and automagically do a `SAVE AS` to make things smoother, but this works well enough for me so probably not going to happen soon. ANyone want to take a shot at this?
2. find all my stuff without trying to put it in one-and-only-one place!

Enjoy
~Rich

I hope you find this useful

