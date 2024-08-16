project(wren)


option(WREN_OPT_META "Enable the optional Meta feature" ON)
option(WREN_OPT_RANDOM "Enable the optional Random generator" ON)

set(WREN_SRC
        src/vm/wren_compiler.c
        src/vm/wren_core.c
        src/vm/wren_debug.c
        src/vm/wren_primitive.c
        src/vm/wren_utils.c
        src/vm/wren_value.c
        src/vm/wren_vm.c
)

if(WREN_OPT_META)
    set(WREN_SRC
            ${WREN_SRC}
            src/optional/wren_opt_meta.c
    )
endif()

if(WREN_OPT_RANDOM)
    set(WREN_SRC
            ${WREN_SRC}
            src/optional/wren_opt_random.c
    )
endif()


add_library(wren
        ${WREN_SRC}
)

if(WREN_OPT_META)
    target_compile_definitions(wren PRIVATE WREN_OPT_META=1)
else()
    target_compile_definitions(wren PRIVATE WREN_OPT_META=0)
endif()

if(WREN_OPT_RANDOM)
    target_compile_definitions(wren PRIVATE WREN_OPT_RANDOM=1)
else()
    target_compile_definitions(wren PRIVATE WREN_OPT_RANDOM=0)
endif()


target_include_directories(${PROJECT_NAME}
        PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:
        ${CMAKE_CURRENT_SOURCE_DIR}/src/include>
        PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src/vm
        ${CMAKE_CURRENT_SOURCE_DIR}/src/optional
)

