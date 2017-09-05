CPP	        		= g++
RM	        		= rm -f
CPPFLAGS			= -Wall -c -I. -std=c++11 -MD
LD	        		= g++
LD_FLAGS			= -Wall -shared
SHARED_LIB			= lib$(LIBRARY_NAME).so.$(VERSION)
STATIC_LIB			= lib$(LIBRARY_NAME).a.$(VERSION)
PURE_SHARED_LIB		= lib$(LIBRARY_NAME)-no-tcp.so.$(VERSION)
PURE_STATIC_LIB		= lib$(LIBRARY_NAME)-no-tcp.a.$(VERSION)
SOURCES				= $(wildcard *.cpp)
PURE_SOURCES        = $(filter-out tcpconnection.cpp, $(SOURCES))
SHARED_OBJECTS		= $(SOURCES:%.cpp=%.o)
STATIC_OBJECTS		= $(SOURCES:%.cpp=%.s.o)
DEPENDENCIES		= $(SOURCES:%.cpp=%.d)
PURE_SHARED_OBJECTS	= $(PURE_SOURCES:%.cpp=%.o)
PURE_STATIC_OBJECTS	= $(PURE_SOURCES:%.cpp=%.s.o)


ifeq ($(shell uname -s),Darwin)
    SONAMEPARAMETER = -install_name
else
    SONAMEPARAMETER = -soname
endif

-include ${DEPENDENCIES}

all:			CPPFLAGS += -g
all:			LD_FLAGS += -g
all:			shared static

pure:			CPPFLAGS += -g -DAMQP_CPP_BUILD_PURE
pure:			LD_FLAGS += -g -DAMQP_CPP_BUILD_PURE
pure:			shared_pure static_pure

release:		CPPFLAGS += -O2
release:		LD_FLAGS += -O2
release:		shared static

shared:			${SHARED_LIB}
shared_pure:	${PURE_SHARED_LIB}

static:			${STATIC_LIB}
static_pure:	${PURE_STATIC_LIB}

${SHARED_LIB}: ${SHARED_OBJECTS}
	${LD} ${LD_FLAGS} -Wl,${SONAMEPARAMETER},lib$(LIBRARY_NAME).so.$(SONAME) -o $@ ${SHARED_OBJECTS}

${STATIC_LIB}: ${STATIC_OBJECTS}
	ar rcs ${STATIC_LIB} ${STATIC_OBJECTS}

${PURE_SHARED_LIB}: ${PURE_SHARED_OBJECTS}
	${LD} ${LD_FLAGS} -Wl,${SONAMEPARAMETER},lib$(LIBRARY_NAME)-no-tcp.so.$(SONAME) -o $@ ${PURE_SHARED_OBJECTS}

${PURE_STATIC_LIB}: ${PURE_STATIC_OBJECTS}
	ar rcs ${PURE_STATIC_LIB} ${PURE_STATIC_OBJECTS}

clean:
	${RM} *.obj *~* ${SHARED_OBJECTS} ${STATIC_OBJECTS} ${SHARED_LIB} ${PURE_SHARED_LIB} ${STATIC_LIB} ${PURE_STATIC_LIB} ${DEPENDENCIES}

${SHARED_OBJECTS}:
	${CPP} ${CPPFLAGS} -fpic -o $@ ${@:%.o=%.cpp}

${STATIC_OBJECTS}:
	${CPP} ${CPPFLAGS} -o $@ ${@:%.s.o=%.cpp}

${PURE_SHARED_OBJECTS}:
	${CPP} ${CPPFLAGS} -fpic -o $@ ${@:%.o=%.cpp}

${PURE_STATIC_OBJECTS}:
	${CPP} ${CPPFLAGS} -o $@ ${@:%.s.o=%.cpp}

