#
#  Compface - 48x48x1 image compression and decompression
#
#  Copyright (c) James Ashton - Sydney University - June 1990.
#
#  Written 11th November 1989.
#
#  Permission is given to distribute these sources, as long as the
#  copyright messages are not removed, and no monies are exchanged. 
#
#  No responsibility is taken for any errors on inaccuracies inherent
#  either to the comments or the code of this program, but if reported
#  to me, then an attempt will be made to fix them.

#--------------------------------------------------------------------------
# BINDIR, LIBDIR, and MANDIR are expected to be overridden by the
# calling Makefile

prefix=/usr/local
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
mandir=${prefix}/share/man
srcdir=.


BINDIR		= ${exec_prefix}/bin
LIBDIR		= ${exec_prefix}/lib
MANDIR		= ${prefix}/share/man

NAME		= compface
UNNAME		= uncompface
EXECUTABLE	= $(BINDIR)/$(NAME)
UNEXECUTABLE	= $(BINDIR)/$(UNNAME)
LIBNAME		= lib$(NAME).a
LIBRARY		= $(LIBDIR)/$(LIBNAME)
MAN1DIR		= $(MANDIR)/man1
MAN3DIR		= $(MANDIR)/man3
OBJECTS		= arith.o file.o compress.o gen.o uncompface.o
SOURCES		= compface.c uncompface.o arith.c file.c compress.c gen.c \
		  cmain.c uncmain.c
HDRS		= compface.h data.h
OTHERS		= README $(NAME).1 $(NAME).3 Makefile

CC		= gcc
CDEFS		= -g -O2
CCOMP		= -g
CFLAGS		= $(CDEFS) $(CCOMP)

all:		$(NAME) $(UNNAME)

$(NAME) :	cmain.o compface.o $(LIBNAME)
		$(CC) $(CFLAGS) -o $(NAME) cmain.o compface.o $(LIBNAME)

$(UNNAME) :	uncmain.o $(LIBNAME)
		$(CC) $(CFLAGS) -o $(UNNAME) uncmain.o $(LIBNAME)

$(LIBNAME) :	$(OBJECTS)
		ar rc $(LIBNAME) $(OBJECTS)
		-ranlib $(LIBNAME)

lint :
		lint -abchx $(SOURCES)

clean :
		rm -f *.o *.a *.sh core a.out $(NAME) $(UNNAME)

install :	$(NAME) $(UNNAME) $(LIBNAME)
		-cp $(NAME) $(EXECUTABLE)
		strip $(EXECUTABLE)
		-cp $(UNNAME) $(UNEXECUTABLE)
		strip $(UNEXECUTABLE)
		-cp $(NAME).1 $(MAN1DIR)
		rm -f $(MAN1DIR)/$(UNNAME).1
		-ln $(MAN1DIR)/$(NAME).1 $(MAN1DIR)/$(UNNAME).1
		-cp $(LIBNAME) $(LIBRARY)
		-cp $(NAME).3 $(MAN3DIR)
		rm -f $(MAN3DIR)/$(UNNAME).3
		-ln $(MAN3DIR)/$(NAME).3 $(MAN3DIR)/$(UNNAME).3

shar :
		shar.script $(OTHERS) $(HDRS) $(SOURCES) > $(NAME).sh
		compress $(NAME).sh

arith.o:	arith.c compface.h data.h
cmain.o:	cmain.c compface.h data.h
compface.o:	compface.c compface.h data.h
compress.o:	compress.c compface.h data.h
file.o:		file.c compface.h data.h
gen.o:		gen.c compface.h data.h
uncmain.o:	uncmain.c compface.h data.h
uncompface.o:	uncompface.c compface.h data.h
