# hext
Do you find yourself exploring file formats, such as GIF, and finding yourself
annoyed with hex editors? Do you find yourself wanting to craft 802.11 packets,
but you know you'll never be able to keep all the fields straight in your head?
Write those packets in hext! hext (`*.hxt`) is a file format for writing small
(or large, if you dare) binary files with comments. You can switch between hex,
binary, and decimal and even stick some string literals in there.

The CLI tool is called `hxt` and it can be found [here](https://crates.io/crates/hxt).
To install it, run
```
cargo install hxt
```

This is what a small GIF looks like:
```
~little-endian msb0
"GIF89a"    # File header and version string

# Logical Screen Descriptor
u16=4 u16=4 # Canvas width/height
.1          # Global Color Table Flag
.000        # Color Resolution
.0          # Reserved in 87a
.000        # Number of colors. 2^(this_value + 1) colors
00          # Background Color Index
00          # Pixel Aspect Ratio

# Global Color Table
=0   =0   =0   # Black
=255 =255 =255 # White 

# Image Descriptor
","         # Image Separator
u16=0 u16=0 # Image offset left/top
u16=4 u16=4 # Image width/height
.1          # Local Color Table Flag
.0          # Interlaced Flag
.000        # Reserved in 87a
.000        # Size of local color table

# Local Color Table
=0   =0   =0    # Black
=128 =0   =255  # Purple

# Image Data
02               # LZW Minimum Code Size
05 841D817A50 00 # Data sub-block

# Graphic Control Extension
"!" F9   # Extension Introducer and Graphic Control Label
=4       # Extension data block size
	.000 # Reserved
	.010 # Disposal Method (010 is restore to background color)
	.0   # User Input Flag
	.0   # Transparent color flag

	u16=50 # Delay (1/100ths of a second)
	00     # Transparent Color Index
00 # Block terminator

# 2nd Image Descriptor
"," u16=0 u16=0 u16=4 u16=4 .00000000

# 2nd Image's Data (same as the first)
02 05 841D817A50 00

";" # GIF Terminator
```

I really do mean *small*. Here it is:
![a very small gif](test.gif)