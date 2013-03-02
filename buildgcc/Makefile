TARGET=arm-none-eabi
PREFIX=/gnuarm
GENOPTS=--target=$(TARGET) --prefix=$(PREFIX)
PATH +=:$(PREFIX)/bin

all: binutils gcc libgcc
install: all
	cd build-binutils; make install
	cd build-gcc; make install-gcc

clean:
	rm -rf build-* gcc-* binutils-* binutils gcc libgcc


BINVER=binutils-2.22
BINBZ=$(BINVER).tar.bz2

binutils: $(BINVER)
	-mkdir build-binutils
	cd build-binutils; ../$(BINVER)/configure $(GENOPTS) && make all
	touch $@

$(BINVER): $(BINBZ)
	tar xf $<

$(BINBZ):
	wget http://ftp.gnu.org/gnu/binutils/$@
	openssl sha1 $@ | grep 65b304a0b9a53a686ce50a01173d1f40f8efe404

GCCVER=gcc-4.7.1
GCCBZ=$(GCCVER).tar.bz2
gcc: $(GCCVER)
	-mkdir build-gcc
	cd build-gcc; ../$(GCCVER)/configure $(GENOPTS) \
		--without-headers --with-newlib  --with-gnu-as --with-gnu-ld \
	&& make all-gcc
	touch $@

$(GCCVER): $(GCCBZ)
	tar xf $(GCCBZ)

$(GCCBZ):
	wget http://ftp.gnu.org/gnu/gcc/$(GCCVER)/$@
	openssl sha1 $@ | grep 3ab74e63a8f2120b4f2c5557f5ffec6907337137

libgcc:
	cd build-gcc; make all-target-libgcc CFLAGS_FOR_TARGET="-g -O2"
	cd build-gcc; make install-target-libgcc
	touch $@
