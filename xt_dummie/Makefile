MODULES := xt_dummie

CONFIGURE_MODULE_SIG=n

ifneq (,$(KERNELRELEASE))
	obj-m += $(MODULES:=.o)
else
	include ../config.mk
endif

EXTRA_CFLAGS += -I$(src)

all: module

module:
	make -C ${KBUILDDIR} M=$(PWD) modules

config:
	make -C ${KBUILDDIR} oldconfig && \
	make -C ${KBUILDDIR} prepare

install:
	sudo make -C ${KBUILDDIR} M=$(PWD) modules_install

load:
	$(shell for mod in $(MODULES); do \
		sudo insmod $$mod.ko; \
	done)
	@lsmod | grep lkb

unload:
	$(shell for mod in $(MODULES); do \
		sudo rmmod $$mod; \
	done)
	@lsmod | grep lkb

clean:
	make -C ${KBUILDDIR} M=$(PWD) clean

.PHONY: module config install load unload clean
