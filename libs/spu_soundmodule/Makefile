.SUFFIXES:
ifeq ($(strip $(PSL1GHT)),)
$(error "PSL1GHT must be set in the environment.")
endif

include $(PSL1GHT)/Makefile.base

TARGET		:=	$(notdir $(CURDIR))
BUILD		:=	build
SOURCE		:=	source
INCLUDE		:=	include
DATA		:=	data
LIBS		:=	-lc

CFLAGS		:=	-g -O2 -Wall --std=gnu99
CXXFLAGS	:=	-g -O2 -Wall
LDFLAGS		:=	

PREFIX		:= spu-
AR		:= $(PREFIX)ar
AS		:= $(PREFIX)gcc
CC		:= $(PREFIX)gcc
CXX		:= $(PREFIX)g++
LD		:= $(CC)

ifneq ($(BUILD),$(notdir $(CURDIR)))

export OUTPUT	:=	$(CURDIR)/$(TARGET)
export VPATH	:=	$(foreach dir,$(SOURCE),$(CURDIR)/$(dir)) \
					$(foreach dir,$(DATA),$(CURDIR)/$(dir))
export BUILDDIR	:=	$(CURDIR)/$(BUILD)
export DEPSDIR	:=	$(BUILDDIR)

CFILES		:= $(foreach dir,$(SOURCE),$(notdir $(wildcard $(dir)/*.c)))
CXXFILES	:= $(foreach dir,$(SOURCE),$(notdir $(wildcard $(dir)/*.cpp)))
SFILES		:= $(foreach dir,$(SOURCE),$(notdir $(wildcard $(dir)/*.S)))
BINFILES	:= $(foreach dir,$(DATA),$(notdir $(wildcard $(dir)/*.bin)))

export OFILES	:=	$(CFILES:.c=.o) \
					$(CXXFILES:.cpp=.o) \
					$(SFILES:.S=.o)

export BINFILES	:=	$(BINFILES:.bin=.bin.h)

export INCLUDES	:=	$(foreach dir,$(INCLUDE),-I$(CURDIR)/$(dir)) \
					-I$(CURDIR)/$(BUILD)

.PHONY: $(BUILD) clean

$(BUILD):
	@[ -d $@ ] || mkdir -p $@
	@make --no-print-directory -C $(BUILD) -f $(CURDIR)/Makefile
	@[ -d lib ] || mkdir -p lib
	@$(RAW2H) $(TARGET).elf lib/$(TARGET).h lib/$(TARGET).S $(TARGET)_bin
	@ppu-as -mcell lib/$(TARGET).S -o lib/$(TARGET).o
	@ppu-ar -rcs $(TARGET).a lib/$(TARGET).o

install: $(OUTPUT).elf
	@[ -d $(PSL1GHT)/modules ] || mkdir -p $(PSL1GHT)/modules
	@cp lib/*.h $(PSL1GHT)/modules
	@cp $< $(PSL1GHT)/modules/$(TARGET)

clean:
	@echo Clean...
	@rm -rf $(BUILD) $(OUTPUT).elf $(OUTPUT).a lib

else

DEPENDS	:= $(OFILES:.o=.d)

$(OUTPUT).elf: $(OFILES)
$(OFILES): $(BINFILES)

-include $(DEPENDS)

endif
