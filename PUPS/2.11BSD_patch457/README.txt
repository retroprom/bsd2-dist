'tape0.bz2' is a bzip2'd SIMH tape image for 2.11BSD Patch Level 457 
(current as of November 25, 2019).  All 9 files of a 2.11 distribution are 
present on 1 "tape".

The file 'simh.ini' is a minimal SIMH file suitable for running the 2.11BSD
installation process from a 'ts' tape drive (TS0) to a 'rp06' (RP0).  If other
devices are wanted edit the file accordingly.

The name of the tape image file can be anything but must match the name used
on the line "ATTACH TS0 ./tape0" in simh.ini

Place the tape0 and simh.ini files into a directory and run "pdp11".  At the
SIMH prompt type "boot ts0" and follow the normal 2.11 install process.
