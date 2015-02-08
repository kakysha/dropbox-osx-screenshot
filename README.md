# screenshot-2-dropbox
OS X screenshots uploader to Dropbox Public folder

1) Create if not exists Public folder inside Dropbox:
`mkdir ~/Dropbox/Public`

2)Set your screenshot location to Dropbox's Public folder
`defaults write com.apple.screencapture location ~/Dropbox/Public;killall SystemUIServer`

3) Create new folder action for the Public folder using context menu on it

4) Paste this AppleScript there.
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
			
			set theURL to "http://dl.getdropbox.com/u/7401144/" & theWebSafeFileName
			set the clipboard to theURL as text
			do shell script "/usr/bin/terminal-notifier.app/Contents/MacOS/terminal-notifier -message " & theURL & " -title 'DropBox Link'"
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
