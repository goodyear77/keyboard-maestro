# keyboard-maestro

The following repository contains useful Keyboard Maestro macros that can be used to create a workflow centered around personal productivity. the workflow is detailed [here](https://zakaria-bennani.medium.com/workflow-for-personal-productivity-241cc604f17b) and this repository acts as a repository for all configuration files needed in the workflow.

The following macros have been created:

1. Create task in Microsoft ToDo from OneNote page
2. Global hotkey to create new Microsoft OneNote page from anywhere
3. Global hotkey to create new Microsoft ToDo task from anywhere
4. Outlook for MAC (Old Outlook 2016 version): Select subject line in ToDo task and search in Outlook
5. Outlook for MAC (New Outlook 2020 version): Select subject line in ToDo task and search in Outlook

To import the Keyboard Maestro files, download the .kmmacros-files you want to use, and import the files into Keyboard Maestro via "File->Import Macros Safely...".

Some configuration details for the more complex macros in case you would like to create them from scratch. PLease note that you should copy the apple scripts into the apple script editor (open in spotlight search) and fix the syntax by pressing the "hammer" symbol before pasting them into Keyboard Maestro.

**1. Create task in Microsoft ToDo from OneNote page**

*RegExp syntax:*

\A(.+)(\n+)([\s\S]+)

*Apple script:*

```
tell application "Keyboard Maestro Engine"
	set reminderName to getvariable "onenoteTitle"
	set reminderLink to getvariable "onenoteLink"
	set reminderBody to getvariable "onenoteBody"
end tell

set myReminderApp to "Apple" -- either Microsoft or Apple
set theList to "Tasks"

if myReminderApp is "Microsoft" then
	tell application "Microsoft Outlook"
		set myList to task folder theList
		tell myList
			make new task with properties {name:reminderName, content:reminderBody}
		end tell
	end tell
else
	tell application "Reminders"
		set myList to list theList
		tell myList
			make new reminder with properties {name:reminderName, body:reminderBody}
		end tell
	end tell
end if
display notification "Reminder added to " & theList
```

**4. Outlook for MAC (Old Outlook 2016 version): Select subject line in ToDo task and search in Outlook:**

*RegExp syntax:*

\A(.+)(\n+)(.+)(\n+)([\s\S]+)

*Apple script to move focus to Archive folder:*

```
tell application "Microsoft Outlook"
	tell the first main window
		set view to mail view
	end tell
	set selected folder to mail folder named "Archive"
end tell
```

**5. Outlook for MAC (New Outlook 2020 version): Select subject line in ToDo task and search in Outlook**

*RegExp syntax:*

\A(.+)(\n+)(.+)(\n+)([\s\S]+)
