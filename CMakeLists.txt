cmake_minimum_required (VERSION 3.0)

project(kcachegrind)

# minimal requirements
set (QT_MIN_VERSION "5.6.0")
set (KF_MIN_VERSION "5.18.0")

find_package(ECM ${KF_MIN_VERSION} REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(ECMOptionalAddSubdirectory)
include(ECMInstallIcons)
include(ECMSetupVersion)
include(ECMMarkNonGuiExecutable)
include(ECMGenerateHeaders)
include(GenerateExportHeader)
include(FeatureSummary)
include(ECMAddAppIcon)
include(ECMPoQmTools)

find_package(Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED Core DBus Gui Widgets)

find_package(KF5 ${KF_MIN_VERSION} REQUIRED
    Archive
    CoreAddons
    DocTools
    WidgetsAddons
    XmlGui
    I18n
    Config
    KIO
    DBusAddons
)

if ("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
add_definitions(-DQT_STRICT_ITERATORS)
endif()
add_definitions(-DQT_NO_URL_CAST_FROM_STRING)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set( KCACHEGRIND_VERSION "0.8.0kde")

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/version.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/version.h )

IF(NOT WIN32)
  configure_file(${CMAKE_CURRENT_SOURCE_DIR}/kcachegrind.spec.cmake ${CMAKE_CURRENT_BINARY_DIR}/kcachegrind.spec )
ENDIF(NOT WIN32)

ecm_create_qm_loader(kcachegrind_QM_LOADER kcachegrind_qt)

add_subdirectory( doc )
add_subdirectory( libcore )
add_subdirectory( cgview )
add_subdirectory( libviews )
add_subdirectory( kcachegrind )
add_subdirectory( qcachegrind )
add_subdirectory( pics )
add_subdirectory( converters )

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
