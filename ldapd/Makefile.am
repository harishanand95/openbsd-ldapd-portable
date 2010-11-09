AUTOMAKE_OPTIONS = subdir-objects

sbin_PROGRAMS = ldapd

ldapd_SOURCES = \
	attributes.c \
	auth.c \
	ber.c \
	btree.c \
	conn.c \
	control.c \
	filter.c \
	imsgev.c \
	index.c \
	ldapd.c \
	ldape.c \
	log.c \
	matching.c \
	modify.c \
	namespace.c \
	parse.y \
	schema.c \
	search.c \
	ssl.c \
	ssl_privsep.c \
	syntax.c \
	util.c \
	uuid.c \
	validate.c

noinst_HEADERS = \
	aldap.h \
	ber.h \
	btree.h \
	imsgev.h \
	ldapd.h \
	schema.h \
	uuid.h

dist_noinst_DATA = btree.3

AM_CFLAGS = -g -std=c99 -D_GNU_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64
AM_CFLAGS += -I$(top_srcdir)/compat
#AM_LDFLAGS = -pg

ldapd_LDADD = @LIBOBJS@

dist_man_MANS = ldapd.8 ldapd.conf.5

