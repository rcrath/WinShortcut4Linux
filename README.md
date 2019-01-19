# WinShortcut4Linux
windows-sytle shortcut for for linux file systems, especially for Dropbox users on Linux.

## Rationale

Why make a windows style shortcuts for Linux?  It is for using Dropbox on Linux.  Dropbox does not handle linux symbolic links gracefully at all. Dropbox translates symlinks as absolute paths, causing all sorts of misplaced file troubles.

Dropbox's solution, which works for many cases, is to only symlink to targets that are outside of the Dropbox folder hierarchy and put the actual files (`SOURCEFILE`) in the Dropbox hierarchy (`ln -s ~\Dropbox\SOURCEFILE ~\TARGETFILE`). Clunky, but what do you do if you want to link to something with a source and target both *within* Dropbox?  At that point, the handling of symlinks as absolute paths creates troubles that will lead to missing and duplicated files.

That is the case for the use of WinShortcut4Linux.  Regular Windows "shortcuts" are little `.lnk` files that do *not* pass themselves off as the original like a symlink, but rather just open the file or folder by putting the launch exec in the `.lmk` file.  In linux under the freedesktop specification, this is done with .desktop files, but you have to write the files manually or open a launcher app to artificially create the .desktop file, both of which are time-consuming and sometimes inconvenient.   

WinShortcut4Linux simply loads a read-only  desktop file template with built in instructions  into your default graphical text editor (`$VISUAL`) that uses `xdg-open` to execute the default application for the file or folder you wish to open. The template is read only to prevent overwriting, so after filling in two fields, you save it to the new location where you would have put your symlink.  

Now when I have a media related book in my media folder that I also think should be in my technology folder, instead of trying to remember which folder I ended up deciding on, I can search for "Shortcut" in my desktop menu operation and  that will give me the template. Once you create the new shortcut, save it to the tech folder and set it to be a trusted launcher (see below for example), it will launch the file.

## Installation

1. Copy the top level folder into your home folder.  
2. Navigate to `~/.local/share/applications` in your graphical file browser of choice and click once on `.shortcut to .desktop`to mark it as trusted. Its name in the browser should change to `NAME`.  
3. Do the same for `createshortcut.desktop`. That should make it searchably show up in your desktop environment's application menu if you just type in "shortcut" in the DE's search bar.  
4. The two files can be run from anywhere as long as the two desktop files are in the same folder, but you will lose the DE menu availability if you change the location from `!/.local...`. 
5. After you save a new desktop file from the template you will need to mark your new desktop file as trusted the same way as above.  After that, it will launch the source file from its new location within dropbox without using symlinks for Dropbox to mess up.

## example

I have a pdf called `MEDIA_TECH.PDF` stored in my `~/Dropbox/media` folder but I also want to be able to access it from `~/Dropbox/tech/`.  Here's how:

- using the DE's search function, launch "shortcut" which should open in your `$VISUAL` text editor, for example, gedit or kate or atom or xed or .....
- replace `NAME` with `MEDIA_TECH.PDF` (you can use any name you wish, it does not need to match the filename of the source).
- replace `PATHTOFILE` with  `~/Dropbox/media/MEDIA_TECH.PDF`--Here the filname must match the source.
- While editing the template file, you can double click on `NAME` or `PATHTOFILE`and the item will be selected so that you can just type or copy-paste the new name and path without fiddling around too much.  
- `SAVE AS` to the target folder, here `~/Dropbox/tech/`using a descriptive filename instead `shortcut to .desktop`.  The filename does not need to be the same as anything, but as soon as you mark it trusted, as above, it changes to whatever you replace `NAME` with in the template, but I would recommend something similar to the `NAME` field.  And keep the `.desktop`extension. 
- Double click on your new launcher and set it to trusted, then from there on you can double-click in the tech folder and it should open the file in the media folder without getting in Dropbox's way.

I need to do this for lots of files, so I find that the few seconds it shaves off of making a launcher saves me time and trouble.  All you need is the path to the source, which can be a folder or a file, and let `xdg-open` do the rest.  

## troubleshooting

1. Are you in Linux?  The shortcuts do not work in Windows.  You cannot make or launch them from Windows.  In the Windows context they are just little empty useless things, just like how Windows `.lnk` files don't work in a Linux.
2. Are the two github .desktop files installed in the same folder?  They must be in `~/local/share/applications` for your DE to pick them up.  
3. Do you have xdg-open configured properly?
4. Have you set $VISUAL to use your editor of choice? Type `echo $VISUAL` from a command prompt in linux to see.  
5. For problems 3 and 4, refer to Google!

For anything outside of these problems, fire up an issue.

### future plans

1. use Zenity to fill in the `NAME` and `PATHTOFILE` and automagically do a `SAVE AS` to make things smoother, but this works well enough for me so probably not going to happen soon. Anyone want to take a shot at this?
2. find all my stuff without trying to put it in one-and-only-one place and smile contentedly!

I hope you find this useful.
~Rich



