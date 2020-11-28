cmake_minimum_required(VERSION 3.0)
project(foxy-super)
include(ExternalProject)

set(DEVROOT ${CMAKE_BINARY_DIR}/devroot)

set(CMAKE_ARGS
        -DCMAKE_BUILD_TYPE=Debug
        -DCMAKE_PREFIX_PATH=${DEVROOT}
        -DCMAKE_INSTALL_PREFIX=${DEVROOT}
	-DCMAKE_SKIP_BUILD_RPATH:BOOL=OFF
        -DCMAKE_SKIP_INSTALL_RPATH:BOOL=OFF
        -DCMAKE_BUILD_WITH_INSTALL_RPATH:BOOL=ON
        -DCMAKE_INSTALL_RPATH:STRING=$ORIGIN/../lib:${DEVROOT}/lib
        )

ExternalProject_Add(
        boost
	#URL https://dl.bintray.com/boostorg/release/1.74.0/source/boost_1_74_0.tar.bz2
	#URL_HASH SHA256=83bfc1507731a0906e387fc28b7ef5417d591429e51e788417fe9ff025e116b1
	URL https://dl.bintray.com/boostorg/release/1.71.0/source/boost_1_71_0.tar.bz2
	URL_HASH SHA256=d73a8da01e8bf8c7eda40b4c84915071a8c8a0df4a6734537ddde4a8580524ee
        BUILD_IN_SOURCE 1
        UPDATE_COMMAND ""
        CONFIGURE_COMMAND ./bootstrap.sh --prefix=${DEVROOT}
        BUILD_COMMAND ""
        INSTALL_COMMAND ./b2 install --prefix=${DEVROOT}
)

ExternalProject_Add(
        catch2
        URL https://github.com/catchorg/Catch2/archive/v2.13.3.tar.gz
        URL_HASH SHA256=fedc5b008f7eb574f45098e7c7138211c543f0f8ad04792090e790511697a877
        CMAKE_ARGS ${CMAKE_ARGS}
            -DCATCH_BUILD_TESTING=OFF
            -DCATCH_ENABLE_WERROR=OFF
            -DCATCH_INSTALL_DOCS=OFF
)

ExternalProject_Add(
        foxy
        DEPENDS catch2 boost
        SOURCE_DIR ..
        CMAKE_ARGS ${CMAKE_ARGS} -DBUILD_TESTING=ON
)
