== UPDATE: NOT WORKING ANYMORE, DROPBOX DISABLED `Public` FOLDER FUNCTIONALITY ==

# Mac OS X default screenshots are auto uploaded to Dropbox with direct link.

1) Create if not exists Public folder inside Dropbox:

`mkdir ~/Dropbox/Public`

2) Set your screenshot location to Dropbox's Public folder

`defaults write com.apple.screencapture location ~/Dropbox/Public;killall SystemUIServer`

3) Install `terminal-notifier` for notifications (optional, you can rely on dropbox tray icon displaying sync progress)

`brew install terminal-notifier`

4) Create new text file `/Library/Scripts/Folder Action Scripts/Dropbox - Copy Public Link.scpt` (under sudo) with the following text, replacing **YOUR_USER_ID_HERE** with your real DropBox user id:
```
on adding folder items to this_folder after receiving added_items
	try
		set the item_count to the number of items in the added_items
		if the item_count is equal to 1 then
			set theFile to item 1 of added_items
			set theRawFilename to ("" & theFile)
			
			set tid to AppleScript's text item delimiters
			set AppleScript's text item delimiters to ":"
			set theFileName to (text item 6 of theRawFilename) as text
			set AppleScript's text item delimiters to tid
			
			set theWebSafeFileName to switchText from theFileName to "%20" instead of " "
			
			set theURL to "http://dl.getdropbox.com/u/YOUR_USER_ID_HERE/" & theWebSafeFileName
			set the clipboard to theURL as text
			do shell script "terminal-notifier -message " & theURL & " -title 'DropBox Public Link'"
		end if
	end try
end adding folder items to

to switchText from t to r instead of s
	set d to text item delimiters
	set text item delimiters to s
	set t to t's text items
	set text item delimiters to r
	tell t to set t to item 1 & ({""} & rest)
	set text item delimiters to d
	t
end switchText
```

5) Create new folder action for the Public folder using context menu on it and select newly created script.
