#!/usr/bin/make -f

PACKAGE=vyatta-cjdns

DESTDIR=$(PWD)/opt/vyatta
TEMPLATECFGDIR=$DESTDIR/share/vyatta-cfg/template
TEMPLATEOPDIR=$DESTDIR/share/vyatta-op/template
# SBINDIR=$DESTDIR/sbin/

BUILDDIR_CJD=/dev/shm/tmp/cjdns

PREFIX='mipsel-linux-gnu-'

CJDNS_BRANCH='master'
CJDNS_REMOTE='github.com/wfleurant/cjdns'

#CJDNS_BRANCH='crashey'
#CJDNS_REMOTE='github.com/cjdelisle/cjdns'

CFLAGS:=$(shell dpkg-buildflags --get CFLAGS)
LDFLAGS:=$(shell dpkg-buildflags --get LDFLAGS)

all: clean build package cleanpackage

build: cjdnsclone cjdnsbuild cjdnsstage

package:
	tar czvf ../data.tar.gz opt
	cd debian && tar czvf ../../control.tar.gz *
	echo 2.0 > ../debian-binary
	ar -r ../$(PACKAGE).deb ../debian-binary ../control.tar.gz ../data.tar.gz

cleanpackage:
	rm ../debian-binary
	rm ../control.tar.gz
	rm ../data.tar.gz

cjdnsclone:
	git clone https://$(CJDNS_REMOTE) -b $(CJDNS_BRANCH) $(BUILDDIR_CJD)
	touch $(BUILDDIR_CJD)/cjdroute

cjdnsbuild:
	cd $(BUILDDIR_CJD) && TARGET_ARCH='mips' CROSS_COMPILE=$(PREFIX) CROSS_CFLAGS="-mips32r2" Seccomp_NO=1 ./cross-do

cjdnsstage:
	cp $(BUILDDIR_CJD)/cjdroute $(DESTDIR)/sbin/cjdroute

clean:
	rm -rf sbin/cjdroute
	rm -rf $(BUILDDIR_CJD)

distclean:	clean
