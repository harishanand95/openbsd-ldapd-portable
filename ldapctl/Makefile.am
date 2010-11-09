AUTOMAKE_OPTIONS = subdir-objects
bin_PROGRAMS = ldapctl

ldapctl_SOURCES = \
	$(top_srcdir)/ldapd/attributes.c \
	$(top_srcdir)/ldapd/ber.c \
	$(top_srcdir)/ldapd/btree.c \
	$(top_srcdir)/ldapd/index.c \
	$(top_srcdir)/ldapd/log.c \
	$(top_srcdir)/ldapd/matching.c \
	$(top_srcdir)/ldapd/parse.y \
	$(top_srcdir)/ldapd/schema.c \
	$(top_srcdir)/ldapd/syntax.c \
	$(top_srcdir)/ldapd/util.c \
	ldapctl.c

AM_CFLAGS = -g -std=c99 -D_GNU_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64
AM_CFLAGS += -I$(top_srcdir)/ldapd -I$(top_srcdir)/compat
#AM_LDFLAGS = -pg

ldapctl_LDADD = @LIBOBJS@

dist_man_MANS = ldapctl.8

