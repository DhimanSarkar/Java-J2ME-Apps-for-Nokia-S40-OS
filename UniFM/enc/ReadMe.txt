Encodings, or code pages (CP), are now supported as text files along with binary files. Here only text variation is described, binaries are considered as obsolete.

A typical code page would consist of 128 lines - for 128 bytes from 0x80 to 0xFF. It's those bytes the meaning of which varies from CP to CP, the meaning of the upper half of the code page (bytes 0x0 to 0x7F) is constant.
Each line maps byte value to Unicode character value, like that:

80	=	20AC	;	ˆ

Here 80 is a byte value, 20AC is a character value, and the rest - semicolon and actual symbol - is an optional comment, placed here just to make code page easier for human reading. All numbers are hexadecimal.

All code pages must be either in Unicode with BOM, or in a one-byte encoding denoted in enc.ini (see) as DEFAULT.