THREADS ?= $(shell nproc)
ifeq ($(shell [ $(THREADS) -gt 8 ] && echo yes),yes)
THREADS := 8
endif

.PHONY: all clones e_clone po6_clone libtreadstone

all: libtreadstone

clones: e_clone po6_clone

e_clone:
	@if [ -d e/.git ]; then \
	    echo "Using existing clone in e"; \
	else \
	    git clone https://github.com/AaronFriel/e.git e; \
	fi

e: po6 e_clone
	cd e && autoreconf -i && ./configure && make -j$(THREADS) && make install && ldconfig

po6_clone:
	@if [ -d po6/.git ]; then \
	    echo "Using existing clone in po6"; \
	else \
	    git clone https://github.com/AaronFriel/po6.git po6; \
	fi

po6: po6_clone
	cd po6 && autoreconf -i && ./configure && make -j$(THREADS) && make install && ldconfig

libtreadstone: po6 e
	cd .. && autoreconf -i
	cd .. && ./configure --prefix=/opt/libtreadstone
	$(MAKE) -C .. -j$(THREADS)
	$(MAKE) -C .. check
