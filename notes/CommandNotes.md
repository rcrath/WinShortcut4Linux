Could not find the file /home/rich/Dropbox/audio…ions/\./SHORTCUT.desktop\.

bash -e -c "exec \\$VISUAL \\"\\$(dirname \\"\\$0\\")/SHORTCUT.desktop\\"" %k

produces file with an illegal name and this error, and saving throws this error:
Could not find the file /home/rich/Dropbox/audio…ions/\.\/SHORTCUT.desktop\.

bash -e -c "exec \\$VISUAL"

works properly, opening an unsaved, untitled doc

bash -e -c "exec \\$VISUAL \\"\\$(dirname \\"\\$0\\")/ 

produces blank unsaveable file called "\."

bash -e -c "exec \\$VISUAL \\"\\$(dirname)"" %k

opens unsaveable file named "\"

bash -e -c "exec \\$VISUAL "$(dirname)"" %k

opens as it should an unsaved doc in the folder

bash -e -c "exec \\$VISUAL "$(dirname "$0")"" %k
gives error that this is a directory

bash -e -c "exec \\$VISUAL "$(dirname "$0")\/SHORTCUT.desktop"" %k
works in bash script and cmd line

-r-xr-xr-x 1 rich rich 319 01.03.2019 18:11 SHORTCUT.desktop*

Exec=bash -e -c "exec $VISUAL $(dirname $0)\/SHORTCUT.desktop" %k
works in dex but not from clicking

Exec=bash -c -e 'exec "$VISUAL "$(dirname "$0")\/SHORTCUT.desktop""' %k
failes in dex with "Autostart file: /home/rich/.local/share/applications/createshortcut.desktop
Executing command: bash -c -e exec "$VISUAL "$(dirname "$0")/SHORTCUT.desktop"" %k
%k: /usr/bin/xed ./SHORTCUT.desktop: No such file or directory"

dex -v createshortcut.desktop 

desktop-file-validate createshortcut.desktop 








