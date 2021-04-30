# createnewtextfileservice
This is an automator action (as a zipfile, because Github has no clue about automator actions, calm down)

THe automator action runs a single line of AppleScript: 
tell application "Finder" to make new file with properties {file type:"TEXT"} at (the target of the front window) as alias

I used file type instead of setting the name extension because for some reason, setting the name extension in properties fails (I'm filing a bug, yes), and if I do it as a second step, then I get a weird error every other time I try to create the file, because it creates "file" then makes it "file.txt", so when it goes to create the next "file" it thinks that's already there, but it isn't. Which is weiard, and I'll try to figure out how to file that as a bug, because I think it is.

You can replicate the weirder error in script editor with:

tell application "Finder"
set theFile to make new file with properties {file type:"TEXT"} at (the target of the front window) as alias
set name extension of theFIle to "txt"
end tell

anyway, the zipfile is the action, unzip it and put it in ~/Finder/Services and you should be set to use it from the finder services menu. 
