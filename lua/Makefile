LUA_VER ?= 5.3
PKGCONF := pkg-config --silence-errors

LUA_LIB ?= \
	$(shell $(PKGCONF) --libs lua$(LUA_VER) || $(PKGCONF) --libs lua)

LUA_INCLUDES ?= \
	$(shell $(PKGCONF) --cflags lua$(LUA_VER) || $(PKGCONF) --cflags lua)

LIBLUA_DSO ?= \
	$(shell echo $(LUA_LIB) | sed  's/^.*-l\(lua[^ ]*\).*/lib\1.so/')

PLUGINS = \
   lua.so \

CFLAGS = -g -Wall -DLIBLUA_DSO=\"$(LIBLUA_DSO)\"

ifeq ($(HAVE_SPANK_OPTION_GETOPT), 1)
  EXTRA_CFLAGS += -DHAVE_SPANK_OPTION_GETOPT
endif

all: $(PLUGINS)

.SUFFIXES: .c .o .so

.c.o:
	$(CC) -I.. $(LUA_INCLUDES) $(EXTRA_CFLAGS) $(CFLAGS) -o $@ -fPIC -c $<

lua.so : lua.o ../lib/list.o
	$(CC) -shared -o $*.so $^ $(LUA_LIB)

clean:
	rm -f *.so *.o

install:
	@mkdir -p --mode=0755 $(DESTDIR)$(LIBDIR)/slurm
	@for p in $(PLUGINS); do \
	   echo "Installing $$p in $(LIBDIR)/slurm"; \
	   install -m0755 $$p $(DESTDIR)$(LIBDIR)/slurm; \
	 done
