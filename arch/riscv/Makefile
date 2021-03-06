# This file is included by the global makefile so that you can add your own
# architecture-specific flags and dependencies. Remember to do have actions
# for "archclean" and "archdep" for cleaning up and making dependencies for
# this architecture
#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#

LDFLAGS         :=
OBJCOPYFLAGS    := -O binary
LDFLAGS_vmlinux :=
KBUILD_AFLAGS_MODULE += -fPIC
KBUILD_CFLAGS_MODULE += -fPIC

ifeq ($(ARCH),riscv)
	KBUILD_DEFCONFIG = spike64_defconfig
else
	KBUILD_DEFCONFIG = $(subst riscv,spike,$(ARCH))_defconfig
endif

export BITS
ifeq ($(CONFIG_ARCH_RV64I),y)
	BITS := 64
	UTS_MACHINE := riscv64

	KBUILD_CFLAGS += -mabi=lp64
	KBUILD_AFLAGS += -mabi=lp64
	KBUILD_MARCH = rv64im
	LDFLAGS += -melf64lriscv
else
	BITS := 32
	UTS_MACHINE := riscv32

	KBUILD_CFLAGS += -mabi=ilp32
	KBUILD_AFLAGS += -mabi=ilp32
	KBUILD_MARCH = rv32im
	LDFLAGS += -melf32lriscv
endif

KBUILD_CFLAGS += -Wall

ifeq ($(CONFIG_ISA_A),y)
	KBUILD_ARCH_A = a
endif
ifeq ($(CONFIG_ISA_C),y)
	KBUILD_ARCH_C = c
endif

KBUILD_AFLAGS += -march=$(KBUILD_MARCH)$(KBUILD_ARCH_A)fd$(KBUILD_ARCH_C)

KBUILD_CFLAGS += -march=$(KBUILD_MARCH)$(KBUILD_ARCH_A)$(KBUILD_ARCH_C)
KBUILD_CFLAGS += -mno-save-restore
KBUILD_CFLAGS += -mstrict-align

head-y := arch/riscv/kernel/head.o

core-y += arch/riscv/kernel/ arch/riscv/mm/

libs-y += arch/riscv/lib/

all: vmlinux
