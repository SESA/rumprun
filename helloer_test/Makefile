default: build

build: rumprun helloer

SOLO5_OBJ=~/solo5/kernel/ebbrt/solo5.o

RUMP_SOLO5_X86_64=../rumprun-solo5/rumprun-x86_64
RUMP_SOLO5_UKVM=$(RUMP_SOLO5_X86_64)/lib/rumprun-solo5/libsolo5_ukvm.a
RUMP_LIBC=$(RUMP_SOLO5_X86_64)/lib/libc.a

$(SOLO5_OBJ):
	make -C ~/solo5 ebbrt

$(RUMP_SOLO5_UKVM): $(SOLO5_OBJ) | $(RUMP_LIBC)
	install -m 664 -D $(SOLO5_OBJ) $@

$(RUMP_LIBC):
	cd .. && git submodule update --init
	make -C ../ build

rumprun: $(RUMP_SOLO5_UKVM) $(RUMP_LIBC)

helloer:
	../rumprun-solo5/bin/x86_64-rumprun-netbsd-gcc -o helloer-rumprun helloer.c
	../rumprun-solo5/bin/rumprun-bake solo5_ukvm_net helloer.ebbrt helloer-rumprun

distclean: clean
	make -C ../ clean

clean:
	-rm helloer-rumprun helloer.ebbrt

.PHONY: default build helloer clean rumprun

