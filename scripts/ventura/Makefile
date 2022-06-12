# Builds a full installer (Ventura-full.img) for Ventura.

# To build the full installer you must run this on macOS, opted into the Apple Beta
# program, and grabbed the Ventura system update from System Update

# For macOS you'll probably need to run xcode-select --install to get the commandline tools

VENTURA_APP=/Applications/Install\ macOS\ 13\ beta.app

LINUX_TOOLS = qemu-img

OS :=
UNAME_S := $(shell uname -s)

ifeq ($(UNAME_S),Darwin)
	OS = MACOS
endif

# If this is Linux make sure we have all our build tools available:
ifeq ($(OS),)
	$(error "Building an installer for Ventura current requires this script to be run on a Mac")
endif

all: Ventura-full.img

%.img : %.dmg
	ln $< $@ || cp $< $@

Ventura-full.dmg : $(VENTURA_APP)
	hdiutil create -o "$@" -size 14g -layout GPTSPUD -fs HFS+J
	hdiutil attach -noverify -mountpoint /Volumes/install_build "$@"
	sudo "$</Contents/Resources/createinstallmedia" --volume /Volumes/install_build --nointeraction
	hdiutil detach "/Volumes/Install macOS 13 beta"

clean :
	rm -f BaseSystem.chunklist BaseSystem.dmg SharedSupport.dmg InstallAssistant.pkg Ventura-recovery.img Ventura-full.img
	rm -rf content
