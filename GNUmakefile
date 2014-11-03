# User-defined section (flags, compiler, include paths, libraries)
override CXXFLAGS += -Wall -std=gnu++11
CXX = g++
INC = inc /usr/include/cryptopp
LIB = cryptopp
BINNAME = pkeep-server

# Builds
ifneq ($(STATIC),yes)
WXSTATIC = --static=no
else
WXSTATIC = --static=yes
endif

ifneq ($(BUILD),debug)
BUILD = release
override CXXFLAGS := $(CXXFLAGS) -O2
LDFLAGS = -s
WXBUILD = --debug=no
else
override CXXFLAGS := $(CXXFLAGS) -g
WXBUILD = --debug=yes
endif

# Paths
OBJPATH = obj/$(BUILD)
BINPATH = bin/$(BUILD)
$(shell mkdir -p $(OBJPATH))
$(shell mkdir -p $(BINPATH))

# wx-config output
WXCONFIG = wx-config --version=3.0 --toolkit=base $(WXSTATIC) --unicode=yes $(WXBUILD)
WXFLAGS = $(shell $(WXCONFIG) --cflags)
WXLIBS = $(shell $(WXCONFIG) --libs)

# Includes
INC := $(patsubst %,-I%,$(INC))

# Libs
LIB := $(patsubst %,-l%,$(LIB))

vpath %.cpp src
vpath %.o $(OBJPATH)
vpath $(BINNAME) $(BINPATH)

# Sources from
SRCS = $(patsubst src/%,%,$(wildcard src/*.cpp))

# Object files
OBJS = $(SRCS:.cpp=.o)
OBJRES = $(patsubst %,$(OBJPATH)/%,$(OBJS))

.PHONY: all clean install remove

all: $(BINNAME)

install:
	cp -f bin/release/$(BINNAME) /usr/local/bin/

remove:
	rm /usr/local/bin/$(BINNAME)

clean:
	rm -rf obj bin

$(BINNAME): $(OBJS)
	$(CXX) -o $(BINPATH)/$(BINNAME) $(OBJRES) $(LDFLAGS) $(WXLIBS) $(LIB)

%.o : %.cpp
	$(CXX) $(CXXFLAGS) $(WXFLAGS) $(INC) -c $< -o $(OBJPATH)/$@

