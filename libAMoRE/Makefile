# $Id: Makefile 1481 2011-04-08 15:07:26Z neider $
# libAMoRE Makefile

.PHONY: all clean doc install uninstall installHeaders uninstallHeaders installDocs uninstallDocs

include config.mk

VERSIONTAG:=$(shell cat "version")
ifeq (${VERSIONTAG},)
# subversion branch
LOCALCHANGES := $(shell test "`svn status | wc -c`" != "0" && echo "-LocalChanges")
SVNREVISION := $(shell LC_ALL=POSIX svn info | awk '/^Revision:/ {print $$2}')
VERSION ?= libalf-svn-r${SVNREVISION}${LOCALCHANGES}
LIBVERSIONTAG?=.svn
else
# release branch
VERSION?=v${VERSIONTAG}
LIBVERSIONTAG?=.${VERSIONTAG}
endif

TARGET=${LIBPREFIX}AMoRE.${LIBEXT}

INSTALL_SHARED_NAME=${TARGET}${LIBVERSIONTAG}
INSTALL_STATIC_NAME=libAMoRE.a${LIBVERSIONTAG}

CFLAGS += -DDEBUG -DJAVAEXCEPT -DLIBAMORE -DBIT32
CFLAGS += -DVERSION="\"${VERSION}\""
CFLAGS += -Wall -Iinclude -fpic
CFLAGS += -fno-stack-protector

LDFLAGS += -fpic -shared

# data objects
LdoOBJ = nfa.o dfa.o regexp.o sfexp.o mon.o language.o rexFromString.o

# transformations
TransfOBJ   = dfa2nfa.o dfamdfa.o enfa2nfa.o nfa2dfa.o nfa2rex.o  \
	nfa2mnfa_sol.o nfa2mnfa_help.o nfa2mnfa.o rex2nfa.o  \
	regexp-infix.o genrex2nfa.o \
	dfa2mon.o  unaryB.o binary.o mon2dcl.o mon2rel.o mon2sfx.o

# tests
TestOBJ = testUnary.o testBinary.o

# all object files
OBJ = global.o buffer.o liberror.o parse.o fileIO.o \
	$(addprefix ldo/,   $(LdoOBJ))       \
	$(addprefix transf/,$(TransfOBJ))    \
	$(addprefix test/,  $(TestOBJ))      \
	debugPrint.o 

all: libAMoRE.a ${TARGET}

libtest: libtest.c libAMoRE.so
	${CC} ${CFLAGS} -o $@ $< -g -L. -lAMoRE

libAMoRE.a: $(OBJ)
	${AR} crs $@ $?

${TARGET}: $(OBJ)
	${CC} ${LDFLAGS} -o $@ $?

clean:
	-rm -f *~ */*~ $(OBJ) libAMoRE.a ${TARGET} libtest
	-rm -Rf docs/html docs/latex 

doc:
	doxygen docs/Doxyfile-libAMoRE


install: installHeaders all
	@echo
	@echo installing libAMoRE library to ${INCLUDEDIR} ...
	@echo
	-${INSTALL} -v -m 755 -d ${LIBDIR}
	${INSTALL} -T -v -m 755 ${TARGET} ${LIBDIR}/${INSTALL_SHARED_NAME}
	${INSTALL} -T -v -m 755 libAMoRE.a ${LIBDIR}/${INSTALL_STATIC_NAME}
	# symlinks
	-rm -f ${LIBDIR}/${TARGET}
	ln -s ${LIBDIR}/${INSTALL_SHARED_NAME} ${LIBDIR}/${TARGET}
	-rm -f ${LIBDIR}/libAMoRE.a
	ln -s ${LIBDIR}/${INSTALL_STATIC_NAME} ${LIBDIR}/libAMoRE.a
	-${RUN_LDCONFIG}


uninstall: uninstallHeaders
	@echo
	@echo removing libAMoRE library from ${INCLUDEDIR} ...
	@echo
	rm -f ${LIBDIR}/${INSTALL_SHARED_NAME}
	rm -f ${LIBDIR}/${INSTALL_STATIC_NAME}
	rm -f ${LIBDIR}/${TARGET}
	rm -f ${LIBDIR}/libAMoRE.a
	-${RUN_LDCONFIG}


installHeaders:
	@echo
	@echo installing libAMoRE headers to ${INCLUDEDIR} ...
	@echo
	${INSTALL} -v -m 755 -d ${INCLUDEDIR}/amore
	${INSTALL} -v -m 644 include/amore/* ${INCLUDEDIR}/amore
uninstallHeaders:
	@echo
	@echo removing libAMoRE headers from ${INCLUDEDIR} ...
	@echo
	rm -Rf ${INCLUDEDIR}/amore


installDocs: doc
	@echo
	@echo installing libAMoRE doc to ${DOCDIR} ...
	@echo
	${INSTALL} -v -m 755 -d $(DOCDIR)/libAMoRE/html 
	${INSTALL} -v -m 644 docs/html/* $(DOCDIR)/libAMoRE/html
uninstallDocs:
	@echo
	@echo removing libAMoRE doc from ${DOCDIR} ...
	@echo
	rm -Rf ${DOCDIR}/libAMoRE

