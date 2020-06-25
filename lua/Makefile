LUA_VER ?= 5.3
LUA_LIB ?= -llua$(LUA_VER)
LUA_INCLUDES ?= -I/usr/include/lua$(LUA_VER)

PLUGINS = \
   lua.so \

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
