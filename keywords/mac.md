# Mac and everything apple

Parents: [[01_Tools]]

#tools #hardware


# Keyboard

Mac keyboard is strictly worse than a normal keyboard in three aspects:
* Modifier key placement
* Behavior of Home and End keys
* Absence of a normal EurKey layout

How to fight this: 
* The modifier keys can be swapped in the normal Mac keyboard settings.
* Use CapsLock for changing languages - it's better than the alternatives (assuming that you only use 2 languages). External keyboards also often have a "languages" button that helps if you use more than 2 langauges.
* To create an EurKey-like keyboard, use UkeLele app.
* To make Home and End keys behave normally, do this: 

Create a file  `DefaultKeyBinding.dict` in the `~/Library/KeyBindings` folder (if the folder doesn't exst, create it as well). In the file, write:
```
{ 
"\\UF729" = moveToBeginningOfParagraph:; // home 
"\\UF72B" = moveToEndOfParagraph:; // end 
"$\\UF729" = moveToBeginningOfParagraphAndModifySelection:; // shift-home 
"$\\UF72B" = moveToEndOfParagraphAndModifySelection:; // shift-end 
"^\\UF729" = moveToBeginningOfDocument:; // ctrl-home 
"^\\UF72B" = moveToEndOfDocument:; // ctrl-end 
"^$\\UF729" = moveToBeginningOfDocumentAndModifySelection:; // ctrl-shift-home 
"^$\\UF72B" = moveToEndOfDocumentAndModifySelection:; // ctrl-shift-end 
}
```
Note that this trick still works only in some interfaces (notably, Slack and Dropbox Paper seem to be processing keyboard events differently). Source for the code: https://damieng.com/blog/2015/04/24/make-home-end-keys-behave-like-windows-on-mac-os-x/ 

Other shortcuts:
* Paste without formatting on Mac: Command-Shift-Option-V (on my keyboard, Ctrl-Shift-Option-V)

# Other interface tricks

To see hidden folders in Finder, press `Ctrl - Shift - .`