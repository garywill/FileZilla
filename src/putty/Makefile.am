if SFTP_UNIX
  FRONTEND = unix
endif
if SFTP_MINGW
  FRONTEND = windows
endif

SUBDIRS = $(FRONTEND) .

noinst_LIBRARIES = libfzputtycommon.a

libfzputtycommon_a_SOURCES = \
			conf.c \
			import.c \
			misc.c \
			sshaes.c \
			sshbcrypt.c \
			sshblowf.c \
			sshbn.c \
			sshdes.c \
			sshdss.c \
			sshecc.c \
			sshmd5.c \
			sshpubk.c \
			sshrand.c \
			sshrsa.c \
			sshsh256.c \
			sshsh512.c \
			sshsha.c \
			tree234.c \
			fzprintf.c

bin_PROGRAMS = fzsftp fzputtygen

fzsftp_SOURCES = \
		be_misc.c \
		be_none.c \
		callback.c \
		cproxy.c \
		errsock.c \
		fzsftp.c \
		portfwd.c \
		proxy.c \
		ssh.c \
		sshccp.c \
		sshcrc.c \
		sshsha.c \
		sshshare.c \
		sshdh.c sshcrcda.c sshzlib.c \
		sshdss.c \
		x11fwd.c \
		wildcard.c pinger.c ssharcf.c \
		sftp.c int64.c logging.c \
		psftp.c cmdline.c \
		timing.c \
		version.c \
		settings.c

fzputtygen_SOURCES = cmdgen.c \
		     notiming.c \
		     version.c


noinst_HEADERS = fzprintf.h \
		 fzsftp.h \
		 int64.h misc.h network.h \
		 pageant.h \
		 proxy.h psftp.h putty.h \
		 puttymem.h puttyps.h sftp.h ssh.h \
		 sshblowf.h \
		 sshbn.h \
		 storage.h \
		 tree234.h

if USE_RESOURCEFILE
  RESOURCEFILE = windows/psftp.o
endif

AM_CPPFLAGS = -I$(srcdir)/$(FRONTEND) -I../../config.h

fzsftp_LDADD = libfzputtycommon.a
fzsftp_LDADD += $(NETTLE_LIBS)


if SFTP_UNIX
  libfzputtycommon_a_CPPFLAGS = $(AM_CPPFLAGS) -DNO_GSSAPI -D_FILE_OFFSET_BITS=64

  fzsftp_SOURCES += time.c
  fzsftp_LDADD += unix/libfzsftp_ux.a unix/libfzputtycommon_ux.a
  fzsftp_CPPFLAGS = $(AM_CPPFLAGS) -D_FILE_OFFSET_BITS=64 -DNO_GSSAPI

  fzputtygen_SOURCES += tree234.c
  fzputtygen_CPPFLAGS = $(AM_CPPFLAGS) -DNO_GSSAPI
  fzputtygen_LDADD = unix/libfzputtycommon_ux.a libfzputtycommon.a $(NETTLE_LIBS)
else
  libfzputtycommon_a_CPPFLAGS = $(AM_CPPFLAGS) -DNO_GSSAPI -D_WINDOWS

  fzsftp_SOURCES += pproxy.c
  fzsftp_CPPFLAGS = $(AM_CPPFLAGS) -D_WINDOWS -DNO_GSSAPI
  fzsftp_LDADD += windows/libfzsftp_win.a windows/libfzputtycommon_win.a $(RESOURCEFILE)
  fzsftp_LDADD += -lws2_32 -lole32

  fzputtygen_CPPFLAGS = $(AM_CPPFLAGS) -D_WINDOWS -DNO_GSSAPI
  fzputtygen_LDADD = windows/libfzputtycommon_win.a libfzputtycommon.a $(RESOURCEFILE) $(NETTLE_LIBS)
  fzputtygen_LDADD += -lole32
endif

libfzputtycommon_a_CPPFLAGS += $(NETTLE_CFLAGS)
fzsftp_CPPFLAGS += $(NETTLE_CFLAGS)
fzputtygen_CPPFLAGS += $(NETTLE_CFLAGS)

if MACAPPBUNDLE
noinst_DATA = $(top_builddir)/FileZilla.app/Contents/MacOS/fzsftp$(EXEEXT)
endif

$(top_builddir)/FileZilla.app/Contents/MacOS/fzsftp$(EXEEXT): fzsftp fzputtygen
	mkdir -p $(top_builddir)/FileZilla.app/Contents/MacOS
	cp -f fzsftp $(top_builddir)/FileZilla.app/Contents/MacOS/fzsftp
	cp -f fzputtygen $(top_builddir)/FileZilla.app/Contents/MacOS/fzputtygen
