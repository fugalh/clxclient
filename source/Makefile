# ---------------------------------------------------------------------------------
#
#  Copyright (C) 2003-2018 Fons Adriaensen <fons@linuxaudio.org>
#    
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 2 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program.  If not, see <http:#www.gnu.org/licenses/>.
#
# ---------------------------------------------------------------------------------


# Modify as required.
#
SUFFIX := $(shell uname -m | sed -e 's/^unknown/$//' -e 's/^i.86/$//' -e 's/^x86_64/$/64/')
PREFIX ?= /usr/local
INCDIR ?= $(PREFIX)/include
LIBDIR ?= $(PREFIX)/lib$(SUFFIX)

MAJVERS = 3
MINVERS = 9.2
VERSION = $(MAJVERS).$(MINVERS)


CPPFLAGS += $(shell pkgconf --cflags freetype2) 
CPPFLAGS += -DVERSION=\"$(VERSION)\" -D_REENTRANT -D_POSIX_PTHREAD_SEMANTICS $(PCONFCFL)
CXXFLAGS += -Wall -O2 -fPIC 
CXXFLAGS += -march=native
LDLIBS +=


CLXCLIENT_SO = libclxclient.so
CLXCLIENT_MAJ = $(CLXCLIENT_SO).$(MAJVERS)
CLXCLIENT_MIN = $(CLXCLIENT_MAJ).$(MINVERS)
CLXCLIENT_DEP = -lpthread -lXft -lX11
CLXCLIENT_O = xdisplay.o xresman.o xhandler.o xwindow.o xdraw.o \
	button.o textip.o enumip.o menuwin.o scale.o slider.o scroll.o mclist.o meter.o
CLXCLIENT_H = clxclient.h


$(CLXCLIENT_MIN): $(CLXCLIENT_O)
	$(CXX) -shared $(LDFLAGS) -Wl,-soname,$(CLXCLIENT_MAJ) -o $(CLXCLIENT_MIN) $(CLXCLIENT_O) $(CLXCLIENT_DEP)


install:	$(CLXCLIENT_MIN)
	install -d $(DESTDIR)$(INCDIR)
	install -d $(DESTDIR)$(LIBDIR)
	install -m 644 $(CLXCLIENT_H) $(DESTDIR)$(INCDIR)
	install -m 755 $(CLXCLIENT_MIN) $(DESTDIR)$(LIBDIR)
	ldconfig
	ln -sf $(CLXCLIENT_MIN) $(DESTDIR)$(LIBDIR)/$(CLXCLIENT_SO)

uninstall:
	rm -rf $(DESTDIR)$(INCDIR)/$(CLXCLIENT_H)
	rm -rf $(DESTDIR)$(LIBDIR)/libclxclient*

clean:
	/bin/rm -f *~ *.o *.a *.d *.so.*

