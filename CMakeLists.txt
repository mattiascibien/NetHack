cmake_minimum_required (VERSION 2.6)
project (NetHack)

include_directories(include)
include_directories(sys/winnt)


find_package(BISON)
find_package(FLEX)

# BEGIN uudecode
set (uudecode_Sources sys/share/uudecode.c)
add_executable(uudecode ${uudecode_Sources})
# END uudecode

# BEGIN dlb_main
set (dlb_main_Sources
        src/alloc.c
        src/dlb.c
        util/dlb_main.c
        util/panic.c )
add_executable(dlb_main ${dlb_main_Sources})
# END dlb_main

# BEGIN makedefs
set (makedefs_Sources
        util/makedefs.c
        src/monst.c
        src/objects.c)
add_executable(makedefs ${makedefs_Sources})

add_custom_command(
        OUTPUT include/date.h
        COMMAND makedefs -v
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT include/onames.h
        COMMAND makedefs -o
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT include/pm.h
        COMMAND makedefs -p
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT dat/quest.dat
        COMMAND makedefs -m
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT src/vis_tab.c include/vis_tab.h
        COMMAND makedefs -z
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT dat/data
        COMMAND makedefs -d
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT rumors
        COMMAND makedefs -r
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT quest.dat
        COMMAND makedefs -q
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT oracles
        COMMAND makedefs -h
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT dat/dungeon.pdf
        COMMAND makedefs -e
        DEPENDS makedefs
)

add_custom_command(
        OUTPUT dat/data
        COMMAND makedefs -d
        DEPENDS makedefs
)

# END makedefs

# BEGIN tile2bmp
set (tile2bmp_Sources
        src/decl.c
        src/drawing.c
        src/monst.c
        src/objects.c
        win/share/tile2bmp.c
        win/share/tiletext.c
        #win/share/tiletxt.c
        #win/share/tilemap.c
        )
add_executable(tile2bmp ${tile2bmp_Sources})

set (tile_Sources
        win/share/monsters.txt
        win/share/objects.txt
        win/share/other.txt)

add_custom_command(
        OUTPUT src\tiles.bmp
        COMMAND tile2bmp ${tile_Sources}
        DEPENDS tile2bmp
        )

# END tile2bmp

# BEGIN recover
set (recover_Sources
        util/recover.c)
add_executable(recover ${recover_Sources})
# END recoved

# BEGIN levcomp
set (levcomp_Sources
        src/alloc.c
        src/decl.c
        src/drawing.c
        sys/share/lev_lex.c
        util/lev_main.c
        sys/share/lev_yacc.c
        src/monst.c
        src/objects.c
        util/panic.c)
add_executable(levcomp ${levcomp_Sources})
# END levcomp


# BEGIN tilemap
set (tilemap_Sources
        win/share/tilemap.c)
add_executable(tilemap ${tilemap_Sources})
# END tilemap


# BEGIN dgncomp
set (dgncomp_Sources
        src/alloc.c
        sys/share/dgn_lex.c
        util/dgn_main.c
        sys/share/dgn_yacc.c
        util/panic.c)
add_executable(dgncomp ${dgncomp_Sources})
# END dgncomp

# BEGIN nethackw
set (NetHackW_Sources
        src/allmain.c
        src/alloc.c
        src/apply.c
        src/artifact.c
        src/attrib.c
        src/ball.c
        src/bones.c
        src/botl.c
        src/cmd.c
        src/dbridge.c
        src/decl.c
        src/detect.c
        src/dig.c
        src/display.c
        src/dlb.c
        src/do.c
        src/do_name.c
        src/do_wear.c
        src/dog.c
        src/dogmove.c
        src/dokick.c
        src/dothrow.c
        src/drawing.c
        src/dungeon.c
        src/eat.c
        src/end.c
        src/engrave.c
        src/exper.c
        src/explode.c
        src/extralev.c
        src/files.c
        src/fountain.c
        win/tty/getline.c
        src/hack.c
        src/hacklib.c
        src/invent.c
        src/light.c
        src/lock.c
        src/mail.c
        src/makemon.c
        src/mapglyph.c
        src/mcastu.c
        src/mhitm.c
        src/mhitu.c
        src/minion.c
        src/mklev.c
        src/mkmap.c
        src/mkmaze.c
        src/mkobj.c
        src/mkroom.c
        src/mon.c
        src/mondata.c
        src/monmove.c
        src/monst.c
        #src/monstr.c
        src/mplayer.c
        src/mthrowu.c
        src/muse.c
        src/music.c
        sys/winnt/ntsound.c
        src/o_init.c
        src/objects.c
        src/objnam.c
        src/options.c
        src/pager.c
        sys/share/pcmain.c
        sys/share/pcsys.c
        sys/share/pcunix.c
        src/pickup.c
        src/pline.c
        src/polyself.c
        src/potion.c
        src/pray.c
        src/priest.c
        src/quest.c
        src/questpgr.c
        sys/share/random.c
        src/read.c
        src/rect.c
        src/region.c
        src/restore.c
        src/rip.c
        src/rnd.c
        src/role.c
        src/rumors.c
        src/save.c
        src/shk.c
        src/shknam.c
        src/sit.c
        src/sounds.c
        src/sp_lev.c
        src/spell.c
        src/steal.c
        src/steed.c
        src/teleport.c
        #src/tile.c
        src/timeout.c
        src/topten.c
        src/track.c
        src/trap.c
        src/u_init.c
        src/uhitm.c
        src/vault.c
        src/version.c
        src/vision.c
        src/weapon.c
        src/were.c
        src/wield.c
        src/windows.c
        sys/winnt/winnt.c
        win/tty/wintty.c
        src/wizard.c
        src/worm.c
        src/worn.c
        src/write.c
        src/zap.c
        win/win32/mhaskyn.c
        win/win32/mhdlg.c
        win/win32/mhfont.c
        win/win32/mhinput.c
        win/win32/mhmain.c
        win/win32/mhmap.c
        win/win32/mhmenu.c
        win/win32/mhmsgwnd.c
        win/win32/mhsplash.c
        win/win32/mhrip.c
        win/win32/mhstatus.c
        win/win32/mhtext.c
        win/win32/mswproc.c
        win/win32/winhack.c
        win/win32/winMS.h
)
add_executable(NetHackW ${NetHackW_Sources})
# END nethackw