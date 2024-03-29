# Debian setup (mosml packages for Debian by JP Secher)
MOSMLHOME=/usr/lib/mosml
MOSMLRUNTIME=$(MOSMLHOME)/include
MOSMLBIN=/usr/bin

# Debian setup
MLTONHOME=/usr/lib/mlton
MLTONINCLUDE=/usr/lib/mlton/include
MLTONBIN=/usr/bin

# Update to point to were you installed mGTK, if it is not in the parent dir
MGTKHOME=..

MLTON_VERSION:=$(shell mlton)

# Broken, but works for now
ifeq ($(word 2, $(MLTON_VERSION)), 20041109)
MLTONFLAGS=-default-ann 'allowImport true'  -default-ann 'allowExport true' \
           -link-opt "`pkg-config --libs-only-l gtk+-2.0`"
else
MLTONFLAGS=-default-ann 'allowFFI true' -link-opt "`pkg-config --libs-only-l gtk+-2.0`"
endif


.PHONY: all mosml mlton

all: mosml mlton

mosml: helloworld buttons-galore signup menus listview treeview editor

mlton: helloworld-mlton buttons-galore-mlton signup-mlton listview-mlton treeview-mlton editor-mlton


# mosml stuff
%: %.sml $(MGTKHOME)/mgtk.ui
	$(MOSMLBIN)/mosmlc -standalone -o $* -I $(MGTKHOME) mgtk.ui $*.sml
#	mosmlc -o $* -I $(MGTKHOME) mgtk.ui $*.sml


# MLton stuff
mgtk-mlton.o: $(MGTKHOME)/mgtk-mlton.c
	gcc -c -O2 -I$(MLTONINCLUDE) `pkg-config --cflags gtk+-2.0` $<
#%-mlton: %-mlton.cm mgtk-mlton.o \
#                      $(shell mlton -stop f %-mlton.cm)

%-mlton: %.sml %-mlton.mlb mgtk-mlton.o 
	$(MLTONBIN)/mlton -output $*-mlton \
			  $(MLTONFLAGS) \
	                  $*-mlton.mlb mgtk-mlton.o

# cleaning
clean:
	rm -f *.{o,so,uo,ui} *~
	rm -f helloworld helloworld-mlton
	rm -f buttons-galore buttons-galore-mlton
	rm -f signup signup-mlton
	rm -f listview listview-mlton
	rm -f treeview treeview-mlton
	rm -f editor editor-mlton
	rm -f menus

realclean: clean
