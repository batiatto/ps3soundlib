.SUFFIXES:
ifeq ($(strip $(PSL1GHT)),)
$(error "PSL1GHT must be set in the environment.")
endif

include $(PSL1GHT)/Makefile.base


#---------------------------------------------------------------------------------

BUILD		:=	build



#---------------------------------------------------------------------------------
ifeq ($(strip $(PLATFORM)),)
#---------------------------------------------------------------------------------
export BASEDIR		:= $(CURDIR)

export INCDIR		:= $(BASEDIR)

#---------------------------------------------------------------------------------
else
#---------------------------------------------------------------------------------



#---------------------------------------------------------------------------------
endif
#---------------------------------------------------------------------------------

DEPSDIR= .
DEFINCS		:= -I$(BASEDIR) -I$(PSL1GHT)/include
INCLUDES	:=	$(DEFINCS) 

MACHDEP		:= -DBIGENDIAN 



CFLAGS		+= -O2 -Wall $(MACHDEP) -fno-strict-aliasing $(INCLUDES)



LIBOGG   		:=  vorbisfile.o mdct.o block.o  window.o \
			synthesis.o info.o barkmel.o bitrate.o envelope.o lookup.o lpc.o lsp.o psy.o tone.o \
			floor1.o floor0.o vorbisenc.o smallft.o\
			res0.o  mapping0.o registry.o codebook.o sharedbook.o\
			bitwise.o  framing.o mdct.o synthesis.o

$(BUILD):  libogg.a install
#---------------------------------------------------------------------------------
libogg.a: $(LIBOGG)
#---------------------------------------------------------------------------------
	@echo "[AR]  $(notdir $@)"
	@$(AR) -rcs $@ $^
	

#---------------------------------------------------------------------------------
install: libogg.a 
#---------------------------------------------------------------------------------
	
	@echo "[CP]  libogg.a"
	@cp -f libogg.a $(PSL1GHT)/lib/
	@echo "[MKDIR] include/ogg"
	@mkdir -p  $(PSL1GHT)/include/ogg
	@echo "[CP]  include/ogg"
	@cp -f ogg/*.h $(PSL1GHT)/include/ogg/
	@echo "[MKDIR] include/vorbis"
	@mkdir -p  $(PSL1GHT)/include/vorbis
	@echo "[CP]  include/vorbis"
	@cp -f vorbis/*.h $(PSL1GHT)/include/vorbis/
	@rm -f *.d
	@rm -f *.o
	@rm -f *.a

#---------------------------------------------------------------------------------
clean:
#---------------------------------------------------------------------------------
	
	rm -f *.d
	rm -f *.o
	rm -f *.a

