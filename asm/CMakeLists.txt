if(WIN32)
  if (MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "")
    enable_language(ASM_MASM)

    set(tbb_src_asm)
    set(tbbmalloc_src_asm)
    if (CMAKE_SIZEOF_VOID_P EQUAL 8)
      list(APPEND tbb_src_asm
        ../src/tbb/intel64-masm/atomic_support.asm
        ../src/tbb/intel64-masm/itsx.asm
        ../src/tbb/intel64-masm/intel64_misc.asm)
      list(APPEND tbbmalloc_src_asm
        ../src/tbb/intel64-masm/atomic_support.asm)
      set(CMAKE_ASM_MASM_FLAGS "/DEM64T=1 ${CMAKE_ASM_MASM_FLAGS}")
      if (TBB_BUILD_TBBMALLOC AND TBB_BUILD_STATIC)
        add_library(tbbmalloc_asm STATIC ${tbbmalloc_src_asm})
      endif()
    else()
      list(APPEND tbb_src_asm
        ../src/tbb/ia32-masm/atomic_support.asm
        ../src/tbb/ia32-masm/itsx.asm
        ../src/tbb/ia32-masm/lock_byte.asm)
      # Enable SAFESEH feature for assembly (x86 builds only).
      set(CMAKE_ASM_MASM_FLAGS "/safeseh ${CMAKE_ASM_MASM_FLAGS}")
    endif()
    add_library(tbb_asm STATIC ${tbb_src_asm})
  endif()
endif()
