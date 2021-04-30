# createnewtextfileservice
This is an automator action (as a zipfile, because Github has no clue about automator actions, calm down)

UPDATE

there's now two services, the first one, "New Text File" runs

tell application "Finder"
	set theTime to (time of (current date)) as text
	set theFile to (make new file with properties {file type:"TEXT", name extension:"txt"} at (the target of the front window) as alias)
	set theName to name of theFile & "_" & theTime & ".txt"
	set name of theFile to theName
end tell

the second one "New Text File Service" runs:

tell application "Finder"
	set theTime to (time of (current date)) as text
	try
		set theFile to (make new file with properties {file type:"TEXT", name extension:"txt"} at (the target of the front window) as alias)
	on error errMsg number errNum
		if errNum is -1700 then
			set theFile to (make new file with properties {file type:"TEXT", name extension:"txt"} at desktop as alias)
		end if
	end try
	set theName to name of theFile & "_" & theTime & ".txt"
	set name of theFile to theName
end tell

Which handles an error that pops up if you run that service on the desktop with no other folder open

this is the workaround for the bug described below. One action, New Text File, takes files and folders as input, so you can ctrl-click on a folder, and it will show up in the services context menu. The other one takes no inputs, and only lives in the Finder's services menu, so you can run it from there with nothing selected

I originally used file type instead of setting the name extension because for some reason, setting the name extension in properties fails (I'm filing a bug, yes), and if I do it as a second step, then I get a weird error every other time I try to create the file, because it creates "file" then makes it "file.txt", so when it goes to create the next "file" it thinks that's already there, but it isn't. Which is weiard, and I'll try to figure out how to file that as a bug, because I think it is.

I also discovered that using the name extension in the Finder is just fraught with error, so I just do the entire name. The date stuff appends the number of seconds since midnight onto the file name followed by .txt, so you shouldn't have duplicate naming errors.

I just left the file type in there, because it doesn't hurt anything.

You can replicate the weirder error in script editor with:

tell application "Finder"
set theFile to make new file with properties {file type:"TEXT"} at (the target of the front window) as alias
set name extension of theFIle to "txt"
end tell

anyway, the zipfile contains the actions, unzip it and put it in ~/Finder/Services and you should be set to use it from the finder services menu or the popup menu when you ctrl-click on a folder
