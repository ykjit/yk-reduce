# Makefile to assist in generating/building a standalone yklua source file
# which can be used for creduce/cvise.

LUA_MAJOR = 5.4

# Keep in sync with ../Makefile.
#
# FIXME: put in a yk.mk
ifneq ($(strip $(YK_BUILD_TYPE)),)
CC=             `yk-config ${YK_BUILD_TYPE} --cc`
AR=             `yk-config ${YK_BUILD_TYPE} --ar` rcu
RANLIB=         `yk-config ${YK_BUILD_TYPE} --ranlib`
CFLAGS=         `yk-config ${YK_BUILD_TYPE} --cflags --cppflags` -O0 -Wall -Wextra -DLUA_COMPAT_5_3 -DUSE_YK -DLUA_USE_JUMPTABLE=0
LDFLAGS=        `yk-config ${YK_BUILD_TYPE} --ldflags` $(SYSLDFLAGS)
LIBS=           `yk-config ${YK_BUILD_TYPE} --libs` -lm $(SYSLIBS)
endif

all:
	false

clean:
	rm -rf one onelua.c onelua src one.tar.gz

#
# Targets for generating the initial onelua.c -- the file to be reduced.
#

one.tar.gz:
	wget https://www.lua.org/extras/${LUA_MAJOR}/one.tar.gz

one: one.tar.gz
	tar zxvf one.tar.gz
	touch one # update timestamp to prevent re-untars

src/lua.c:
	cp -r ../src .

onelua.c: src/lua.c one
	cd src && ${CC} ${CFLAGS} -c -I. -E ../one/one.c -o ../onelua.c

# Target to build the reduced onelua.c
#
# You will use this from your creduce/cvise interenestingness test.
#
# This deliberately doesn't depend on onelua.c to prevent make from
# re-generating the file that we are trying to reduce.
#
# `-Wfatal-errors` is reccommended in the cvise docs. May speed things up.
onelua:
	${CC} -Wfatal-errors -o onelua ${CFLAGS} ${LDFLAGS} ${LIBS} onelua.c
