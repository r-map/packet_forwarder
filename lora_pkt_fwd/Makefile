### Application-specific constants

APP_NAME := lora_pkt_fwd

### Environment constants 

PREFIX := $(INSTALLDIR)/usr
BINDIR = bin
LGW_PATH ?= /usr/include/libloragw
ARCH ?=
CROSS_COMPILE ?=

OBJDIR = obj
INCLUDES = $(wildcard inc/*.h)

### External constant definitions
# must get library build option to know if mpsse must be linked or not

#include $(LGW_PATH)/library.cfg
RELEASE_VERSION := `cat ../VERSION`

### Constant symbols

CC := $(CROSS_COMPILE)gcc
AR := $(CROSS_COMPILE)ar

CFLAGS := -O2 -Wall -Wextra -std=c99 -Iinc -I.
VFLAG := -D VERSION_STRING="\"$(RELEASE_VERSION)\""

### Constants for Lora concentrator HAL library
# List the library sub-modules that are used by the application

### Linking options

LIBS := -lloragw -lwiringPi -lmpsse -lrt -lpthread -lm

### General build targets

all: $(APP_NAME)

clean:
	rm -f $(OBJDIR)/*.o
	rm -f $(APP_NAME)


dist: clean
	tar  cvfz ../$(APP_NAME)-`cat ../VERSION`.tar.gz --directory=.. $(APP_NAME) VERSION


.PHONY: install
install: all
	install -d $(PREFIX)/$(BINDIR)
	install -m 0755 $(APP_NAME) $(PREFIX)/$(BINDIR)
	install -d $(INSTALLDIR)/etc/lora_pkt_fwd
	install -D -m 0755 *.json $(INSTALLDIR)/etc/lora_pkt_fwd/


### Sub-modules compilation

$(OBJDIR):
	mkdir -p $(OBJDIR)

$(OBJDIR)/%.o: src/%.c $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) -I$(LGW_PATH) $< -o $@

### Main program compilation and assembly

$(OBJDIR)/$(APP_NAME).o: src/$(APP_NAME).c $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) $(VFLAG) -I$(LGW_PATH) $< -o $@

$(APP_NAME): $(OBJDIR)/$(APP_NAME).o  $(OBJDIR)/parson.o $(OBJDIR)/base64.o $(OBJDIR)/jitqueue.o $(OBJDIR)/timersync.o
	$(CC) -L$(LGW_PATH) $< $(OBJDIR)/parson.o $(OBJDIR)/base64.o $(OBJDIR)/jitqueue.o $(OBJDIR)/timersync.o -o $@ $(LIBS)

### EOF
