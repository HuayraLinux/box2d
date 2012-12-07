all: libbox2d.so libbox2d.a

fixed: libbox2d-fixed.so libbox2d-fixed.a

MAJOR=1
MINOR=2

SOURCES = \
	Source/Collision/b2Distance.cpp \
	Source/Collision/b2TimeOfImpact.cpp \
	Source/Collision/b2CollideCircle.cpp \
	Source/Collision/b2CollidePoly.cpp \
	Source/Collision/Shapes/b2PolygonShape.cpp \
	Source/Collision/Shapes/b2CircleShape.cpp \
	Source/Collision/Shapes/b2Shape.cpp \
	Source/Collision/b2PairManager.cpp \
	Source/Collision/b2Collision.cpp \
	Source/Collision/b2BroadPhase.cpp \
	Source/Dynamics/b2WorldCallbacks.cpp \
	Source/Dynamics/Joints/b2PrismaticJoint.cpp \
	Source/Dynamics/Joints/b2MouseJoint.cpp \
	Source/Dynamics/Joints/b2GearJoint.cpp \
	Source/Dynamics/Joints/b2Joint.cpp \
	Source/Dynamics/Joints/b2PulleyJoint.cpp \
	Source/Dynamics/Joints/b2DistanceJoint.cpp \
	Source/Dynamics/Joints/b2RevoluteJoint.cpp \
	Source/Dynamics/Contacts/b2CircleContact.cpp \
	Source/Dynamics/Contacts/b2PolyAndCircleContact.cpp \
	Source/Dynamics/Contacts/b2Contact.cpp \
	Source/Dynamics/Contacts/b2PolyContact.cpp \
	Source/Dynamics/Contacts/b2ContactSolver.cpp \
	Source/Dynamics/b2Island.cpp \
	Source/Dynamics/b2Body.cpp \
	Source/Dynamics/b2ContactManager.cpp \
	Source/Dynamics/b2World.cpp \
	Source/Common/b2BlockAllocator.cpp \
	Source/Common/b2StackAllocator.cpp \
	Source/Common/b2Settings.cpp \
	Source/Common/b2Math.cpp

OBJS = $(SOURCES:.cpp=.o)
STATIC_OBJS = $(SOURCES:.cpp=.static.o)
FIXED_OBJS = $(SOURCES:.cpp=.fixed.o)
STATIC_FIXED_OBJS = $(SOURCES:.cpp=.static_fixed.o)

STATIC_CFLAGS= -O2 -g -Wall
CFLAGS= $(STATIC_CFLAGS) -fPIC
LDFLAGS= -Wl,-z,defs -Wl,--as-needed -Wl,--no-undefined

libbox2d.so.$(MAJOR).$(MINOR): $(OBJS)
	g++ $(LDFLAGS) -shared \
		-Wl,-soname,libbox2d.so.$(MAJOR) \
		-o libbox2d.so.$(MAJOR).$(MINOR) \
		$+ -o $@ $(LIBS)

libbox2d.so: libbox2d.so.$(MAJOR).$(MINOR)
	rm -f $@.$(MAJOR)
	ln -s $@.$(MAJOR).$(MINOR) $@.$(MAJOR)
	rm -f $@
	ln -s $@.$(MAJOR) $@

libbox2d.a: $(STATIC_OBJS)
	ar cru $@ $+

libbox2d-fixed.so.$(MAJOR).$(MINOR): $(FIXED_OBJS)
	g++ $(LDFLAGS) -shared \
		-Wl,-soname,libbox2d-fixed.so.$(MAJOR) \
		-o libbox2d-fixed.so.$(MAJOR).$(MINOR) \
		$+ -o $@ $(LIBS)

libbox2d-fixed.so: libbox2d-fixed.so.$(MAJOR).$(MINOR)
	rm -f $@.$(MAJOR)
	ln -s $@.$(MAJOR).$(MINOR) $@.$(MAJOR)
	rm -f $@
	ln -s $@.$(MAJOR) $@

libbox2d-fixed.a: $(STATIC_FIXED_OBJS)
	ar cru $@ $+

%.o: %.cpp
	g++ -o $@ -c $+ $(CFLAGS) $(PKGCONFIG_CFLAGS)

%.o: %.c
	gcc -o $@ -c $+ $(CFLAGS) $(PKGCONFIG_CFLAGS)

%.so : %.o
	g++ $(LDFLAGS) -shared $^ -o $@

%.static.o: %.cpp
	g++ -o $@ -c $+ $(STATIC_CFLAGS) $(PKGCONFIG_CFLAGS)

%.static.o: %.c
	gcc -o $@ -c $+ $(STATIC_CFLAGS) $(PKGCONFIG_CFLAGS)

%.fixed.o: %.cpp
	g++ -o $@ -c $+ $(CFLAGS) -DTARGET_FLOAT32_IS_FIXED $(PKGCONFIG_CFLAGS)

%.fixed.o: %.c
	gcc -o $@ -c $+ $(CFLAGS) -DTARGET_FLOAT32_IS_FIXED $(PKGCONFIG_CFLAGS)

%.static_fixed.o: %.cpp
	g++ -o $@ -c $+ $(STATIC_CFLAGS) -DTARGET_FLOAT32_IS_FIXED $(PKGCONFIG_CFLAGS)

%.static_fixed.o: %.c
	gcc -o $@ -c $+ $(STATIC_CFLAGS) -DTARGET_FLOAT32_IS_FIXED $(PKGCONFIG_CFLAGS)

clean:
	rm -f $(OBJS)
	rm -f $(STATIC_OBJS)
	rm -f $(FIXED_OBJS)
	rm -f $(STATIC_FIXED_OBJS)
	rm -f *.so *.so* *.a

DESTDIR=

install:
	mkdir -p "$(DESTDIR)/usr/lib/"
	cp -a *.a "$(DESTDIR)/usr/lib/"
	cp -a *.so* "$(DESTDIR)/usr/lib/"
	mkdir -p "$(DESTDIR)/usr/include/"
	cat Include/Box2D.h | sed -e 's|^#include "../Source/\(.*\)"|#include <Box2D/\1>|g' > "$(DESTDIR)/usr/include/Box2D.h"
	find Source/ -name "*.h" | \
		while read f; do \
			t=`echo "$$f" | sed -e 's|^Source/|/usr/include/Box2D/|'` ; \
			d=`dirname "$$t"`; \
			mkdir -p "$(DESTDIR)/$$d" ; \
			cp "$$f" "$(DESTDIR)/$$t"; \
		done
