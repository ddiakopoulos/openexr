
ADD_LIBRARY ( PyImath ${LIB_TYPE}
    PyImath.cpp
    PyImathAutovectorize.cpp
    PyImathBox2Array.cpp
    PyImathBox3Array.cpp
    PyImathBox.cpp
    PyImathColor3.cpp
    PyImathColor4.cpp
    PyImathEuler.cpp
    PyImathFixedArray.cpp
    PyImathFrustum.cpp
    PyImathLine.cpp
    PyImathMatrix33.cpp
    PyImathMatrix44.cpp
    PyImathPlane.cpp
    PyImathQuat.cpp
    PyImathRandom.cpp
    PyImathShear.cpp
    PyImathStringArray.cpp
    PyImathStringTable.cpp
    PyImathTask.cpp
    PyImathUtil.cpp
    PyImathFixedVArray.cpp
    PyImathVec2fd.cpp
    PyImathVec2si.cpp
    PyImathVec3fd.cpp
    PyImathVec3siArray.cpp
    PyImathVec3si.cpp
    PyImathVec4fd.cpp
    PyImathVec4siArray.cpp
    PyImathVec4si.cpp
)

IF(BUILD_SHARED_LIBS)
    target_compile_definitions( PyImath PRIVATE -DPYIMATH_EXPORTS )
ENDIF()

target_compile_definitions( PyImath PRIVATE -DPYIMATH_BUILD )

TARGET_INCLUDE_DIRECTORIES ( PyImath 
    PRIVATE ${CMAKE_INSTALL_PREFIX}/include/OpenEXR
    ${PYTHON_INCLUDE_PATH}
    ${Boost_INCLUDE_DIRS})

TARGET_LINK_LIBRARIES ( PyImath
    IlmBase::Iex
    IlmBase::IexMath
    IlmBase::Imath
    PyIex
	${Boost_LIBRARIES}
	${PYTHON_LIBRARIES}
)

INSTALL ( TARGETS PyImath
	DESTINATION
	lib
)

INSTALL (DIRECTORY ./
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

ADD_LIBRARY ( imathmodule ${LIB_TYPE}
    imathmodule.cpp
    PyImathFun.cpp
    PyImathBasicTypes.cpp
)

if (WIN32)
    SET_TARGET_PROPERTIES ( imathmodule
        PROPERTIES 
            PREFIX ""
            OUTPUT_NAME "imath" 
            SUFFIX ".pyd"
    )
else()
    SET_TARGET_PROPERTIES ( imathmodule
        PROPERTIES PREFIX "" SUFFIX ".so" BUILD_WITH_INSTALL_RPATH ON
    )
endif()

TARGET_INCLUDE_DIRECTORIES ( imathmodule 
    PRIVATE ${CMAKE_INSTALL_PREFIX}/include/OpenEXR
    ${PYTHON_INCLUDE_PATH}
    ${Boost_INCLUDE_DIRS})

TARGET_LINK_LIBRARIES ( imathmodule
	PyImath
	PyIex
	IlmBase::Imath
	IlmBase::Iex
	${Boost_LIBRARIES}
	${PYTHON_LIBRARIES}
)

INSTALL ( TARGETS imathmodule
	DESTINATION lib/python${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR}/site-packages
)

