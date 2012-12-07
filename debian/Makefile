all: libbox2d.so libbox2d.a

fixed: libbox2d-fixed.so libbox2d-fixed.a

MAJOR=1
MINOR=2

SOURCES = \
	                Box2D/Collision/b2Distance.cpp \
	Box2D/Collision/b2TimeOfImpact.cpp \
	Box2D/Collision/b2CollideCircle.cpp \
	Box2D/Collision/b2CollidePoly.cpp \
	Box2D/Collision/Shapes/b2PolygonShape.cpp \
	Box2D/Collision/Shapes/b2CircleShape.cpp \
	Box2D/Collision/Shapes/b2Shape.cpp \
	Box2D/Collision/b2PairManager.cpp \
	Box2D/Collision/b2Collision.cpp \
	Box2D/Collision/b2BroadPhase.cpp \
	Box2D/Dynamics/b2WorldCallbacks.cpp \
	Box2D/Dynamics/Joints/b2PrismaticJoint.cpp \
	Box2D/Dynamics/Joints/b2MouseJoint.cpp \
	Box2D/Dynamics/Joints/b2GearJoint.cpp \
	Box2D/Dynamics/Joints/b2Joint.cpp \
	Box2D/Dynamics/Joints/b2PulleyJoint.cpp \
	Box2D/Dynamics/Joints/b2DistanceJoint.cpp \
	Box2D/Dynamics/Joints/b2RevoluteJoint.cpp \
	Box2D/Dynamics/Contacts/b2CircleContact.cpp \
	Box2D/Dynamics/Contacts/b2PolyAndCircleContact.cpp \
	Box2D/Dynamics/Contacts/b2Contact.cpp \
	Box2D/Dynamics/Contacts/b2PolyContact.cpp \
	Box2D/Dynamics/Contacts/b2ContactSolver.cpp \
	Box2D/Dynamics/b2Island.cpp \
	Box2D/Dynamics/b2Body.cpp \
	Box2D/Dynamics/b2ContactManager.cpp \
	Box2D/Dynamics/b2World.cpp \
	Box2D/Common/b2BlockAllocator.cpp \
	Box2D/Common/b2StackAllocator.cpp \
	Box2D/Common/b2Settings.cpp \
	Box2D/Common/b2Math.cpp

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
	cat Include/Box2D.h | sed -e 's|^#include "../Box2D/\(.*\)"|#include <Box2D/\1>|g' > "$(DESTDIR)/usr/include/Box2D.h"
	find Box2D/ -name "*.h" | \
		while read f; do \
			t=`echo "$$f" | sed -e 's|^Source/|/usr/include/Box2D/|'` ; \
			d=`dirname "$$t"`; \
			mkdir -p "$(DESTDIR)/$$d" ; \
			cp "$$f" "$(DESTDIR)/$$t"; \
		done
