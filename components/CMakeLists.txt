project (Components)

# Version file
set (VERSION_IN_FILE "${OpenMW_SOURCE_DIR}/files/version.in")
set (VERSION_FILE_PATH_BASE "${OpenMW_BINARY_DIR}")
set (VERSION_FILE_PATH_RELATIVE resources/version)
if (GIT_CHECKOUT)
    get_generator_is_multi_config(multi_config)
    add_custom_target (git-version
       COMMAND ${CMAKE_COMMAND}
            -DGIT_EXECUTABLE=${GIT_EXECUTABLE}
            -DPROJECT_SOURCE_DIR=${PROJECT_SOURCE_DIR}
            -DVERSION_IN_FILE=${VERSION_IN_FILE}
            -DVERSION_FILE_PATH_BASE=${VERSION_FILE_PATH_BASE}
            -DVERSION_FILE_PATH_RELATIVE=${VERSION_FILE_PATH_RELATIVE}
            -DOPENMW_VERSION_MAJOR=${OPENMW_VERSION_MAJOR}
            -DOPENMW_VERSION_MINOR=${OPENMW_VERSION_MINOR}
            -DOPENMW_VERSION_RELEASE=${OPENMW_VERSION_RELEASE}
            -DOPENMW_VERSION=${OPENMW_VERSION}
            -DMACROSFILE=${CMAKE_SOURCE_DIR}/cmake/OpenMWMacros.cmake
            "-DCMAKE_CONFIGURATION_TYPES=${CMAKE_CONFIGURATION_TYPES}"
            -Dgenerator_is_multi_config_var=${multi_config}
            -P ${CMAKE_CURRENT_SOURCE_DIR}/../cmake/GitVersion.cmake
            VERBATIM)
else (GIT_CHECKOUT)
    configure_resource_file(${VERSION_IN_FILE} ${VERSION_FILE_PATH_BASE} ${VERSION_FILE_PATH_RELATIVE})
endif (GIT_CHECKOUT)

# source files

add_component_dir (settings
    settings parser
    )

add_component_dir (bsa
    bsa_file compressedbsafile memorystream
    )

add_component_dir (vfs
    manager archive bsaarchive filesystemarchive registerarchives
    )

add_component_dir (resource
    scenemanager keyframemanager imagemanager bulletshapemanager bulletshape niffilemanager objectcache multiobjectcache resourcesystem
    resourcemanager stats animation
    )

add_component_dir (shader
    shadermanager shadervisitor removedalphafunc
    )

add_component_dir (sceneutil
    clone attach visitor util statesetupdater controller skeleton riggeometry morphgeometry lightcontroller
    lightmanager lightutil positionattitudetransform workqueue unrefqueue pathgridutil waterutil writescene serialize optimizer
    actorutil detourdebugdraw navmesh agentpath shadow mwshadowtechnique recastmesh shadowsbin osgacontroller
    )

add_component_dir (nif
    controlled effect niftypes record controller extra node record_ptr data niffile property nifkey base nifstream
    )

add_component_dir (nifosg
    nifloader controller particle matrixtransform
    )

add_component_dir (nifbullet
    bulletnifloader
    )

add_component_dir (to_utf8
    to_utf8
    )

add_component_dir (esm
    attr defs esmcommon esmreader esmwriter loadacti loadalch loadappa loadarmo loadbody loadbook loadbsgn loadcell
    loadclas loadclot loadcont loadcrea loaddial loaddoor loadench loadfact loadglob loadgmst
    loadinfo loadingr loadland loadlevlist loadligh loadlock loadprob loadrepa loadltex loadmgef loadmisc
    loadnpc loadpgrd loadrace loadregn loadscpt loadskil loadsndg loadsoun loadspel loadsscr loadstat
    loadweap records aipackage effectlist spelllist variant variantimp loadtes3 cellref filter
    savedgame journalentry queststate locals globalscript player objectstate cellid cellstate globalmap inventorystate containerstate npcstate creaturestate dialoguestate statstate
    npcstats creaturestats weatherstate quickkeys fogstate spellstate activespells creaturelevliststate doorstate projectilestate debugprofile
    aisequence magiceffects util custommarkerstate stolenitems transport animationstate controlsstate mappings
    )

add_component_dir (esmterrain
    storage
    )

add_component_dir (misc
    constants utf8stream stringops resourcehelpers rng messageformatparser weakcache thread
    )

add_component_dir (debug
    debugging debuglog gldebug
    )

IF(NOT WIN32 AND NOT APPLE)
    add_definitions(-DGLOBAL_DATA_PATH="${GLOBAL_DATA_PATH}")
    add_definitions(-DGLOBAL_CONFIG_PATH="${GLOBAL_CONFIG_PATH}")
ENDIF()
add_component_dir (files
    linuxpath androidpath windowspath macospath fixedpath multidircollection collections configurationmanager escape
    lowlevelfile constrainedfilestream memorystream
    )

add_component_dir (compiler
    context controlparser errorhandler exception exprparser extensions fileparser generator
    lineparser literals locals output parser scanner scriptparser skipparser streamerrorhandler
    stringparser tokenloc nullerrorhandler opcodes extensions0 declarationparser
    quickfileparser discardparser junkparser
    )

add_component_dir (interpreter
    context controlopcodes genericopcodes installopcodes interpreter localopcodes mathopcodes
    miscopcodes opcodes runtime types defines
    )

add_component_dir (translation
    translation
    )

add_component_dir (terrain
    storage world buffercache defs terraingrid material terraindrawable texturemanager chunkmanager compositemaprenderer quadtreeworld quadtreenode viewdata cellborder
    )

add_component_dir (loadinglistener
    loadinglistener
    )

add_component_dir (myguiplatform
    myguirendermanager myguidatamanager myguiplatform myguitexture myguiloglistener additivelayer scalinglayer
    )

add_component_dir (widgets
    box fontwrapper imagebutton tags list numericeditbox sharedstatebutton windowcaption widgets
    )

add_component_dir (fontloader
    fontloader
    )

add_component_dir (sdlutil
    gl4es_init sdlgraphicswindow imagetosurface sdlinputwrapper sdlvideowrapper events sdlcursormanager
    )

add_component_dir (version
    version
    )

add_component_dir (fallback
    fallback validate
    )

if(WIN32)
    add_component_dir (crashcatcher
        windows_crashcatcher
        windows_crashmonitor
        windows_crashshm
    )
elseif(NOT ANDROID)
    add_component_dir (crashcatcher
        crashcatcher
    )
endif()

add_component_dir(detournavigator
    debug
    makenavmesh
    findsmoothpath
    recastmeshbuilder
    recastmeshmanager
    cachedrecastmeshmanager
    navmeshmanager
    navigatorimpl
    asyncnavmeshupdater
    recastmesh
    tilecachedrecastmeshmanager
    recastmeshobject
    navmeshtilescache
    settings
    navigator
    findrandompointaroundcircle
    raycast
    navmeshtileview
    oscillatingrecastmeshobject
    )

set (ESM_UI ${CMAKE_SOURCE_DIR}/files/ui/contentselector.ui
    )

if (USE_QT)
    add_component_qt_dir (contentselector
        model/modelitem model/esmfile
        model/naturalsort model/contentmodel
        model/loadordererror
        view/combobox view/contentselector
        )
    add_component_qt_dir (config
        gamesettings
        launchersettings
        settingsbase
        )

    add_component_qt_dir (process
        processinvoker
        )

    add_component_dir (misc
        helpviewer
        )

    QT5_WRAP_UI(ESM_UI_HDR ${ESM_UI})
    QT5_WRAP_CPP(MOC_SRCS ${COMPONENT_MOC_FILES})
endif()

if (CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    if("${CMAKE_SYSTEM_PROCESSOR}" STREQUAL "x86_64" AND NOT APPLE)
        add_definitions(-fPIC)
    endif()
endif ()

include_directories(${BULLET_INCLUDE_DIRS} ${CMAKE_CURRENT_BINARY_DIR})

add_library(components STATIC ${COMPONENT_FILES} ${MOC_SRCS} ${ESM_UI_HDR})

target_link_libraries(components
    # CMake's built-in OSG finder does not use pkgconfig, so we have to
    # manually ensure the order is correct for inter-library dependencies.
    # This only makes a difference with `-DOPENMW_USE_SYSTEM_OSG=ON -DOSG_STATIC=ON`.
    # https://gitlab.kitware.com/cmake/cmake/-/issues/21701
    ${OSGPARTICLE_LIBRARIES}
    ${OSGVIEWER_LIBRARIES}
    ${OSGSHADOW_LIBRARIES}
    ${OSGANIMATION_LIBRARIES}
    ${OSGGA_LIBRARIES}
    ${OSGTEXT_LIBRARIES}
    ${OSGDB_LIBRARIES}
    ${OSGUTIL_LIBRARIES}
    ${OSG_LIBRARIES}
    ${OPENTHREADS_LIBRARIES}

    ${Boost_SYSTEM_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}
    ${Boost_PROGRAM_OPTIONS_LIBRARY}
    ${Boost_IOSTREAMS_LIBRARY}

    ${SDL2_LIBRARIES}
    ${OPENGL_gl_LIBRARY}
    ${MyGUI_LIBRARIES}
    LZ4::LZ4
    RecastNavigation::DebugUtils
    RecastNavigation::Detour
    RecastNavigation::Recast

    Base64
    )

target_link_libraries(components ${BULLET_LIBRARIES})

if (WIN32)
    target_link_libraries(components
    ${Boost_LOCALE_LIBRARY}
    ${Boost_ZLIB_LIBRARY})
endif()

if (USE_QT)
    target_link_libraries(components Qt5::Widgets Qt5::Core)
endif()

if (GIT_CHECKOUT)
    add_dependencies (components git-version)
endif (GIT_CHECKOUT)

if (WIN32)
    target_link_libraries(components shlwapi)
endif()

# Fix for not visible pthreads functions for linker with glibc 2.15
if (UNIX AND NOT APPLE)
target_link_libraries(components ${CMAKE_THREAD_LIBS_INIT})
endif()

if (BUILD_WITH_CODE_COVERAGE)
    add_definitions(--coverage)
    target_link_libraries(components gcov)
endif()


# Make the variable accessible for other subdirectories
set(COMPONENT_FILES ${COMPONENT_FILES} PARENT_SCOPE)

target_compile_definitions(components PUBLIC BT_USE_DOUBLE_PRECISION)

target_link_libraries(components "z" "X11" "Xrandr" "Xinerama" "fontconfig")
