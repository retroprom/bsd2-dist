
CFLAGS?=-Wall -Wextra -std=c99

# List of files to extract
EXTRACT=
# List of distribution files
DISTRIBUTE=

###############################################################################
# Distribution of 2.11BSD at patch level 431
###############################################################################

# distribution has patch 431 but patches up to 470
PL431=../PUPS/2.11BSD_patch470
# tape bootstrap created by concatenation
PL431_TAPEBOOT=bsd211-patch431-tapeboot.bin
# tape with minimal root
PL431_MINITAPE=bsd211-patch431-dist-mini.tap
PL431_MINISPEC= \
	${PL431_TAPEBOOT}:512 \
	${PL431}/disklabel:1024 \
	${PL431}/mkfs:1024 \
	${PL431}/restor:1024 \
	${PL431}/icheck:1024 \
	${PL431}/root.dump:10240
# tape with user binaries
PL431_BASETAPE=bsd211-patch431-dist-base.tap
PL431_BASESPEC= \
	${PL431_MINISPEC} \
	${PL431}/file6.tar:10240
# tape with complete source
PL431_FULLTAPE=bsd211-patch431-dist-full.tap
PL431_FULLSPEC= \
	${PL431_BASESPEC} \
	${PL431}/file7.tar:10240 \
	${PL431}/file8.tar:10240

EXTRACT+=${PL431}/file6.tar ${PL431}/file7.tar ${PL431}/file8.tar
DISTRIBUTE+=${PL431_MINITAPE} ${PL431_BASETAPE} ${PL431_FULLTAPE}
COMPRESSED+=$(DISTRIBUTE:%=%.gz)

default: distribute

distribute: ${DISTRIBUTE}
	gzip -k ${DISTRIBUTE}
	sha256sum ${DISTRIBUTE} > SHA256SUMS
	sha512sum ${DISTRIBUTE} > SHA512SUMS

clean:
	rm -f mksimtape ${DISTRIBUTE} ${COMPRESSED} ${PL431_TAPEBOOT}

main: mksimtape

mksimtape: mksimtape.c

# PL431 distribution
${PL431_TAPEBOOT}:
	rm -f ${PL431_TAPEBOOT}.tmp
	dd obs=512 seek=0 count=1 if=${PL431}/mtboot of=${PL431_TAPEBOOT}.tmp
	dd obs=512 seek=1 count=1 if=${PL431}/mtboot of=${PL431_TAPEBOOT}.tmp
	dd obs=512 seek=2 if=${PL431}/boot of=${PL431_TAPEBOOT}.tmp
	mv ${PL431_TAPEBOOT}.tmp ${PL431_TAPEBOOT}
${PL431_MINITAPE}: mksimtape ${PL431_TAPEBOOT}
	./mksimtape ${PL431_MINISPEC} > $@
${PL431_BASETAPE}: mksimtape ${PL431_TAPEBOOT}
	./mksimtape ${PL431_BASESPEC} > $@
${PL431_FULLTAPE}: mksimtape ${PL431_TAPEBOOT}
	./mksimtape ${PL431_FULLSPEC} > $@

# file extraction
extract: ${EXTRACT}
${PL431}/file6.tar: ${PL431}/file6.tar.gz
	gunzip -k $<
${PL431}/file7.tar: ${PL431}/file7.tar.gz
	gunzip -k $<
${PL431}/file8.tar: ${PL431}/file8.tar.gz
	gunzip -k $<

