# $Id: Makefile 1472 2011-03-31 23:03:06Z davidpiegdon $
# DeLeTe2 testsuite Makefile

.PHONY: all clean jpegs

include ../../config.mk
CXXFLAGS+=-Xlinker -rpath=${LIBDIR}

CPPFLAGS+=-I../../include -I${INCLUDEDIR} -Wall -ggdb3

LDFLAGS+=-L../../src -L${LIBDIR} -lm -lalf -lAMoRE++ -llangen

all: delete2_static delete2_regex delete2_file

nodata:
	-rm -f *.dot *.jpg

clean: nodata
	-rm -f *.o delete2_regex delete2_static delete2_file

jpegs:
	for EACH in *.dot; do dot -Tjpg "$$EACH" -o "$$EACH.jpg"; done;

delete2_static: CPPFLAGS += -DDELETE2_DEBUG_INCLUSION_RELATION
delete2_static: delete2_static.cpp

delete2_regex: delete2_regex.cpp

delete2_file: delete2_file.cpp

