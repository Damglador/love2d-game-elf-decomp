# Description
Extracts an archive with source of the game from ELF binaries made with LÖVE.
Doesn't work with PE files.

Dependencies:
- `tail`
- `xxd`  - optional, outputs detected zip headers in HEX

Only tested with Balatro.

# Explanation
«Compilation» of LÖVE games is basically just [stitching two binaries together](https://www.love2d.org/wiki/Game_Distribution), the first one is love executable and the second one is a zip with your game. The resulting PE (Windows executable) can be opened with a regular archiver (Ark in my case), but it's not that simple with an ELF file (executable type used on Linux).

So I thought if I can stitch two files together, I will be able to unstich them if I find the seam.

So this script searches for zip signatures in the file, takes their offset and turns everything after that in archives using `tail`. Since I only need one archive and it finds a lot of such signatures, I check if the file name contains only one `/` and is followed by `UT`. One `/` indicates that this is the root folder, and UT just happens to always be after a file name.
