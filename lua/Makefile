PLUGINS = \
   lua.so \

ifeq ($(HAVE_SPANK_OPTION_GETOPT), 1)
  EXTRA_CFLAGS += -DHAVE_SPANK_OPTION_GETOPT
endif

all: $(PLUGINS)

.SUFFIXES: .c .o .so

.c.o:
	$(CC) -I.. $(EXTRA_CFLAGS) $(CFLAGS) -o $@ -fPIC -c $<

lua.so : lua.o ../lib/list.o
	$(CC) -shared -o $*.so $^ -llua

clean:
	rm -f *.so *.o

install:
	@mkdir -p --mode=0755 $(DESTDIR)$(LIBDIR)/slurm
	@for p in $(PLUGINS); do \
	   echo "Installing $$p in $(LIBDIR)/slurm"; \
	   install -m0755 $$p $(DESTDIR)$(LIBDIR)/slurm; \
	 done
