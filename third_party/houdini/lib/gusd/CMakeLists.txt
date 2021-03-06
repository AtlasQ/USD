
set(PXR_PACKAGE gusd)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${HOUDINI_DEFINITIONS}")

if (UNIX)
    set(HOUDINI_LIB_NAMES
        ${HOUDINI_UI_LIBRARY}
        ${HOUDINI_OPZ_LIBRARY}
        ${HOUDINI_OP3_LIBRARY}
        ${HOUDINI_OP2_LIBRARY}
        ${HOUDINI_OP1_LIBRARY}
        ${HOUDINI_SIM_LIBRARY}
        ${HOUDINI_GEO_LIBRARY}
        ${HOUDINI_PRM_LIBRARY}
        ${HOUDINI_UT_LIBRARY})
elseif(WIN32)
    # The library names on Windows don't match the linux ones, so we have a 
    # higher level CMakeLists.txt that links in all the Houdini libraries always
    # on Windows.  Future work on this would be to consolidate these two
    # mechanisms.
    set(HOUDINI_LIB_NAMES "")
endif()


pxr_shared_library(${PXR_PACKAGE}
    LIBRARIES
        ar 
        gf 
        sdf 
        tf
        usd
        usdGeom
        usdRi
        usdShade
        usdUtils
        ${HOUDINI_LIB_NAMES}

    INCLUDE_DIRS
        ${HOUDINI_INCLUDE_DIRS}

    PUBLIC_CLASSES
        agentUtils
        boundsCache
        curvesWrapper
        debugCodes
        error
        groupBaseWrapper
        instancerWrapper
        meshWrapper
        packedUsdWrapper
        pointsWrapper
        primWrapper
        purpose
        refiner
        scopeWrapper
        shaderWrapper
        stageCache
        stageEdit
        tokens
        writeCtrlFlags
        xformWrapper
        GEO_IOTranslator
        GT_OldPointInstancer
        GT_PackedUSD
        GT_PointInstancer
        GT_PrimCache
        GT_Utils
        GU_PackedUSD
        GU_USD
        NURBSCurvesWrapper
        OP_ParmChangeMicroNode
        PRM_Shared
        USD_CustomTraverse
        USD_DataCache
        USD_StdTraverse
        USD_ThreadedTraverse
        USD_Traverse
        USD_Utils
        USD_VisCache
        USD_XformCache
        UT_TypeTraits

    PUBLIC_HEADERS
        api.h
        context.h
        defaultArray.h
        gusd.h
        stageOpts.h
        GT_VtArray.h
        GU_USD.h
        USD_PropertyMap.h
        USD_TraverseSimple.h
        UT_Assert.h
        UT_CappedCache.h
        UT_Gf.h
        UT_StaticInit.h
        UT_Version.h

    PRIVATE_HEADERS
        GT_VtStringArray.h
        UT_VtArray.h

    CPPFILES
        plugin.cpp

    PYMODULE_FILES
        __init__.py
        treemodel.py
        treeview.py

    PYMODULE_CPPFILES
        module.cpp
        wrapStageCache.cpp
        wrapStageEdit.cpp
        wrapStageOpts.cpp
)

file(GLOB ICON_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "site/icons/*")

install( 
    FILES
        ${ICON_FILES}
    DESTINATION
        ${PXR_INSTALL_SUBDIR}/config/Icons
)

# Disabling temporarily: need to resolve tbb link errors.
#pxr_build_test(testGusdErrors
#    LIBRARIES
#        gusd
#        tf
#        ${HOUDINI_UT_LIBRARY}
#    CPPFILES
#        testenv/testGusdErrors.cpp
#)

#pxr_register_test(testGusdErrors
#    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testGusdErrors"
#    EXPECTED_RETURN_CODE 0
#)
