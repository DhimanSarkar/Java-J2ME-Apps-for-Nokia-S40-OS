Here we store translated text and strings used in program.

Each translation is stored in separate subfolder, which name should (yet doesn't have to) be a two-letter language code. Following this rule will enable that language to be auto-detected on first program run.

Each language subfolder must contain following files:

1. strings.ini - an ini-formatted file containing numeric string identifiers and strings assigned to them.
2. about.txt - some sort of description of the program, possibly containing some help info.
3. license.txt - no comments.

Additionally there may be file named offsets.ini - an ini-formatted file containing class names and string table offset values assigned for those classes.

Next, in order for the program to see those translations, there is a file lang.ini. It maps two-letter language codes with human-readable language names.

All files listed above may be saved either in Unicode UTF-8 or UTF-16 (both big endian and little endian) text encodings with BOM signature, or in one-byte encoding for which there is a codepage and which is marked as default (see ReadMe.txt in enc folder).

Files about.txt and license.txt are simple text files, just like this one, and require no special discussion. But the other two do.

Here is the most complicated strings.ini file example possible:

;--------------------------------------------------------------

[0] ; offset of zero - no offset for strings below

0	=	Zero string	; tabs are used
1 = First string		; spaces
2=Second string		; no spaces at all

-1	=	String one	; negative identifiers are allowed
-2	=	String two

100	=	File %A in folder %B is %C, whih caused an error %D ; This string contains placeholders for runtime substitution. And some parts of this program expect some strings to contain such placeholders in order to display something really usefull, e.g. "File some_filename.txt is read-only!" instead of "File is read-only!".
101	=	Operation %B failed: %A ; Another example of placeholders. As you can see, they do not have to follow in alphabetical order at all. More even, you can relatively painless omit some or even all. As the payback the program will become less informative.

102	=	This is \n multiline \n\n string, \t containing \t tabs, slashes \\, \\ and \\\\, octal symbols \007 \000 \015 \177, and unicode symbols \u263A \u263C \u00B0	; This string contains  escaped characters. When strings are loaded into runtime string table, they are unescaped, so it is possible to have multiline strings in string table and some specific characters in strings. Just don't forget to replace every \ with \\.

; Those strings above will be stored in runtime string table with indices 0, 1 and 2, just as the are.
; But ones below will be stored with indices 1001, 1002, 1010, 1041.
; This offset stuff is introduced to enable convinient movement of string blocks along the table. Why wold someone bother to do so is discussed further in this document.

[1000] ; offset of 1000 for strings below

1	=	Alpha
2	=	Beta
10	=	Gamma
41	=	Delta

;--------------------------------------------------------------

And the same for offsets.ini file:

;--------------------------------------------------------------

b.e	=	0	; redundant thing, we could have just skipped this line with the same result
filemanager.cvsTextView		=	1000	; now if something in class cvsTextView asks the Locale class for string identified by number 15, it will return string identified by number 1015
filemanager.cvsImageView	=	-1000	; same here, except that instead of string #15 string #-985 will be returned.

;--------------------------------------------------------------

Offsets are introduced to resolve string identifier conflicts. Imagine that someone developed a module and used string identifiers from 250 to 300, and another someone developed another module with string identifiers from 275 to 315. Obviously we can't put both string sets into one string table, because they overlap. So we assign an offset of, say, 1000 to all classes (if there are more than one) of one module in offsets.ini and place those strings in strings.ini under offset [1000]. Now, for example, first module is still using strings form 250 to 300, and second module is using strings from 1275 to 1315, without overlapping.