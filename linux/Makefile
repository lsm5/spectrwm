CFLAGS+= -std=c99 -Wmissing-prototypes -Wall -Wextra -Wshadow -Wno-uninitialized -g
CFLAGS+= -D_GNU_SOURCE -I. -I/usr/include/freetype2 -DSWM_LIB=\"libswmhack.so.$(LVERS)\"
LDADD+= -lX11 -lX11-xcb -lxcb -lxcb-icccm -lxcb-randr -lxcb-keysyms -lxcb-util -lxcb-xtest -lXft -lXcursor

PREFIX?= /usr/local
BINDIR?= $(PREFIX)/bin
LIBDIR?= $(PREFIX)/lib
MANDIR?= $(PREFIX)/share/man

CC?= cc

LVERS= $(shell . ../lib/shlib_version; echo $$major.$$minor)

BUILDVERSION= $(shell sh $(CURDIR)/../buildver.sh)
ifneq ("${BUILDVERSION}", "")
CFLAGS+= -DSPECTRWM_BUILDSTR=\"$(BUILDVERSION)\"
endif

all: spectrwm libswmhack.so.$(LVERS)

spectrwm.c:
	ln -sf ../spectrwm.c
	ln -sf ../version.h

swm_hack.c:
	ln -sf ../lib/swm_hack.c

spectrwm: spectrwm.o linux.o
	$(CC) $(LDFLAGS) -o $@ $+ $(LDADD)

%.so: %.c
	$(CC) $(CFLAGS) -c -fpic -DPIC $+ -o $@

libswmhack.so.$(LVERS): swm_hack.so
	$(CC) $(LDFLAGS) -Wl,-soname,$@ -shared -fpic -o libswmhack.so.$(LVERS) swm_hack.so $(LDADD)

install: all
	install -m 755 -d $(DESTDIR)$(BINDIR)
	install -m 755 -d $(DESTDIR)$(LIBDIR)
	install -m 755 -d $(DESTDIR)$(MANDIR)/man1
	install -m 755 spectrwm $(DESTDIR)$(BINDIR)
	install -m 755 libswmhack.so.$(LVERS) $(DESTDIR)$(LIBDIR)
	install -m 644 ../spectrwm.1 $(DESTDIR)$(MANDIR)/man1/spectrwm.1
	install -m 644 ../spectrwm_es.1 $(DESTDIR)$(MANDIR)/man1/spectrwm_es.1
	install -m 644 ../spectrwm_it.1 $(DESTDIR)$(MANDIR)/man1/spectrwm_it.1
	install -m 644 ../spectrwm_pt.1 $(DESTDIR)$(MANDIR)/man1/spectrwm_pt.1
	install -m 644 ../spectrwm_ru.1 $(DESTDIR)$(MANDIR)/man1/spectrwm_ru.1
	ln -sf spectrwm $(DESTDIR)$(BINDIR)/scrotwm
	ln -sf libswmhack.so.0.0 $(DESTDIR)$(LIBDIR)/libswmhack.so.0
	ln -sf libswmhack.so.0.0 $(DESTDIR)$(LIBDIR)/libswmhack.so

clean:
	rm -f spectrwm *.o *.so libswmhack.so.* spectrwm.c swm_hack.c version.h

.PHONY: all install clean
