# libtcod makefile
# to build debug version, run "make -f makefile-linux debug"
# to build release version, run "make -f makefile-linux release"
SRCDIR=src
INCDIR=include

#dependencies
# SDL
SDL_FLAGS=`sdl-config --cflags`
SDL_LIBS=`sdl-config --libs`

# ZLIB

#OPENGL
OPENGL_LIB=-lGL

LIBS=$(ZLIB_LIBS) $(SDL_LIBS) $(OPENGL_LIB) -lm -ldl -lX11 -lpthread -lz

CFLAGS=$(FLAGS) -I$(INCDIR) -I$(INCDIR)/gui -Wall $(SDL_FLAGS) -fPIC
TEMP=/tmp
CC=gcc
CPP=g++
.SUFFIXES: a .o .h .c

$(TEMP)/libtcod/release/%.o : $(SRCDIR)/%.c
	@ echo Compiling release $(basename $<).c
	@$(CC) $(CFLAGS) -O2 -DNDEBUG -s -o $@ -c $<
$(TEMP)/libtcod/release/%.o : $(SRCDIR)/%.cpp
	@ echo Compiling release $(basename $<).cpp
	@$(CPP) $(CFLAGS) -O2 -DNDEBUG -s -o $@ -c $< 
$(TEMP)/libtcod/release/gui/%.o : $(SRCDIR)/gui/%.cpp
	@ echo Compiling release $(basename $<).cpp
	@$(CPP) $(CFLAGS) -O2 -DNDEBUG -s -o $@ -c $< 
$(TEMP)/libtcod/debug/%.o : $(SRCDIR)/%.c
	@echo Compiling debug $(basename $<).c
	@$(CC) $(CFLAGS) -g -o $@ -c $<
$(TEMP)/libtcod/debug/%.o : $(SRCDIR)/%.cpp
	@echo Compiling debug $(basename $<).cpp
	@$(CPP) $(CFLAGS) -g -o $@ -c $< 
$(TEMP)/libtcod/debug/gui/%.o : $(SRCDIR)/gui/%.cpp
	@echo Compiling debug $(basename $<).cpp
	@$(CPP) $(CFLAGS) -g -o $@ -c $< 


LIBOBJS_C= \
	bresenham_c.o \
	bsp_c.o \
	color_c.o \
	console_c.o \
	fov_c.o \
	fov_circular_raycasting.o \
	fov_diamond_raycasting.o \
	fov_recursive_shadowcasting.o \
	fov_permissive2.o \
	fov_restrictive.o \
	heightmap_c.o \
	image_c.o \
	lex_c.o \
	list_c.o \
	mersenne_c.o \
	noise_c.o \
	parser_c.o \
	path_c.o \
	sys_c.o \
	sys_opengl_c.o \
	sys_sdl_c.o \
	sys_sdl_img_bmp.o \
	sys_sdl_img_png.o \
	tree_c.o \
	txtfield_c.o \
	wrappers.o \
	zip_c.o \
	namegen_c.o \
	png/lodepng.o

LIBOBJS_CPP= \
	bresenham.o \
	bsp.o \
	color.o \
	console.o \
	fov.o \
	heightmap.o \
	image.o \
	lex.o \
	mersenne.o \
	mouse.o \
	noise.o \
	parser.o \
	path.o \
	sys.o \
	txtfield.o \
	zip.o \
	namegen.o

GUIOBJS= \
	gui/button.o \
	gui/container.o \
	gui/flatlist.o \
	gui/hbox.o \
	gui/image.o \
	gui/label.o \
	gui/radiobutton.o \
	gui/slider.o \
	gui/statusbar.o \
	gui/textbox.o \
	gui/togglebutton.o \
	gui/toolbar.o \
	gui/vbox.o \
	gui/widget.o


LIBOBJS_C_DEBUG=$(addprefix $(TEMP)/libtcod/debug/, $(LIBOBJS_C))
LIBOBJS_CPP_DEBUG=$(addprefix $(TEMP)/libtcod/debug/, $(LIBOBJS_CPP))
GUIOBJS_DEBUG=$(addprefix $(TEMP)/libtcod/debug/, $(GUIOBJS))
LIBOBJS_C_RELEASE=$(addprefix $(TEMP)/libtcod/release/, $(LIBOBJS_C))
LIBOBJS_CPP_RELEASE=$(addprefix $(TEMP)/libtcod/release/, $(LIBOBJS_CPP))
GUIOBJS_RELEASE=$(addprefix $(TEMP)/libtcod/release/, $(GUIOBJS))

all : release

release : libtcod.so libtcodxx.so libtcodgui.so

$(TEMP)/libtcod/debug/png :
	mkdir -p $@

$(TEMP)/libtcod/debug/gui :
	mkdir -p $@

$(TEMP)/libtcod/release/png :
	mkdir -p $@

$(TEMP)/libtcod/release/gui :
	mkdir -p $@


libtcod.a : $(LIBOBJS_C_DEBUG)
	ar r $@ $(LIBOBJS_C_DEBUG)


libtcodxx.a : $(LIBOBJS_CPP_DEBUG)
	ar r $@ $(LIBOBJS_CPP_DEBUG)

libtcod.so : $(TEMP)/libtcod/release/png $(LIBOBJS_C_RELEASE)
	@echo Linking $@
	@gcc -shared -Wl,-soname,libtcod.so.1 $(LDFLAGS) -o $@ $(LIBOBJS_C_RELEASE) $(LIBS) $(CFLAGS)

libtcodxx.so : $(TEMP)/libtcod/release/png $(LIBOBJS_CPP_RELEASE)
	@echo Linking $@
	@g++ -shared -Wl,-soname,libtcodxx.so.1 $(LDFLAGS) -o $@ $(LIBOBJS_CPP_RELEASE) -L. -ltcod $(CFLAGS)

libtcodgui.so : $(TEMP)/libtcod/release/gui $(GUIOBJS_RELEASE)
	@echo Linking $@
	@g++ -shared -Wl,-soname,libtcodgui.so.1 $(LDFLAGS) -o $@ $(GUIOBJS_RELEASE) -L. -ltcod -ltcodxx $(CFLAGS)

libtcod_debug.so : $(TEMP)/libtcod/debug/png $(LIBOBJS_C_DEBUG) lib/libz.a
	@echo Linking $@
	@gcc -shared -Wl,-soname,$@ $(LDFLAGS) -o $@ $(LIBOBJS_C_DEBUG) $(LIBS) $(CFLAGS)

libtcodxx_debug.so : $(TEMP)/libtcod/debug/png $(LIBOBJS_CPP_DEBUG)
	@echo Linking $@
	@g++ -shared -Wl,-soname,$@ $(LDFLAGS) -o $@ $(LIBOBJS_CPP_DEBUG) -L. -ltcod_debug $(CFLAGS)

libtcodgui_debug.so : $(TEMP)/libtcod/debug/gui $(GUIOBJS_DEBUG)
	@echo Linking $@
	@g++ -shared -Wl,-soname,$@ $(LDFLAGS) -o $@ $(GUIOBJS_DEBUG) -L. -ltcod_debug -ltcodxx_debug $(CFLAGS)

clean : 
	\rm -f $(LIBOBJS_C_RELEASE) $(LIBOBJS_CPP_RELEASE) $(GUIOBJS_RELEASE) $(LIBOBJS_C_DEBUG) $(LIBOBJS_CPP_DEBUG) $(GUIOBJS_DEBUG) libtcod.a libtcodxx.a libtcod.so libtcodxx.so libtcodgui.so libtcodpy.pyc



