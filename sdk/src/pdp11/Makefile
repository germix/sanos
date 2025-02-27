#
# Makefile for PDP-11 emulator with Seventh Edition UNIX
#

SANOS=..\..\..
INCLUDE=$(SANOS)\src\include
TOOLS=$(SANOS)\tools
BIN=$(SANOS)\bin
SDK=..\..

!IFNDEF MSVC
MSVC=7
!ENDIF

COMMONCFLAGS=/I$(INCLUDE) /D SANOS

!IF $(MSVC) == 8 || $(MSVC) == 9 || $(MSVC) == 10
CFLAGS=/nologo /O2 /Ob1 /Oi /Ot /Oy /GS- /GR- /X /GF /Gy /W1 $(COMMONCFLAGS)
!ELSE
CFLAGS=/nologo /O2 /Og /Ob1 /Oi /Ot /Oy /X /GF /Gy /W1 $(COMMONCFLAGS)
!ENDIF

all: pdp11.exe siminit.exe

pdp11.exe: pdp11_cis.c pdp11_cpu.c pdp11_fp.c pdp11_lp.c pdp11_rk.c pdp11_rl.c pdp11_rp.c pdp11_rx.c pdp11_stddev.c pdp11_sys.c pdp11_tm.c scp.c scp_tty.c

pdp11.exe : $(SANOS)/win/lib/os.lib $(SANOS)/win/lib/libc.lib
    $(CC) $(CFLAGS) /Fe$@ $** /link /NODEFAULTLIB /FIXED:NO /HEAP:33554432,131072

siminit.exe : siminit.c $(SANOS)/win/lib/os.lib $(SANOS)/win/lib/libc.lib
    $(CC) $(CFLAGS) /Fe$@ $** /link /NODEFAULTLIB /FIXED:NO /HEAP:33554432,131072

pdp11.iso: pdp11.exe siminit.exe
    -@if not exist img mkdir img
    -@if not exist img\boot mkdir img\boot
    -@if not exist img\bin mkdir img\bin
    -@if not exist img\etc mkdir img\etc
    -@if not exist img\dev mkdir img\dev
    -@if not exist img\proc mkdir img\proc
    -@if not exist img\unix mkdir img\unix
    -@if not exist img\var mkdir img\var
    copy /Y pdp11.exe img\bin
    copy /Y siminit.exe img\bin
    copy /Y krnl.ini img\boot
    copy /Y os.ini img\etc
    copy /Y $(SANOS)\win\install\boot\os.dll img\boot
    copy /Y unix_v7_rl.dsk img\unix
    copy /Y unixv7 img\unix
    $(BIN)\mkdfs -d img\BOOTIMG.BIN -b $(SANOS)\win\install\boot\cdemboot -l $(SANOS)\win\install\boot\osldr.dll -k $(SANOS)\win\install\boot\krnl.dll -c 512 -C 1440 -I 8192 -i -f -K rootdev=cd0,rootfs=cdfs,silent
    $(TOOLS)\mkisofs -J -c BOOTCAT.BIN -b BOOTIMG.BIN -o pdp11.iso img

install: pdp11.iso
    $(TOOLS)\zip pdp11.zip pdp11.iso
    $(TOOLS)\zip pdp11-src.zip *.ini *.c *.h Makefile unix_v7_rl.dsk unixv7
     
clean:
    del *.exe *.obj *.iso *.zip
    rmdir /S /Q img
