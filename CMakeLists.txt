cmake_minimum_required (VERSION 3.16 FATAL_ERROR)

# KDE Application Version, managed by release script
set(RELEASE_SERVICE_VERSION_MAJOR "21")
set(RELEASE_SERVICE_VERSION_MINOR "08")
set(RELEASE_SERVICE_VERSION_MICRO "0")
set(RELEASE_SERVICE_VERSION "${RELEASE_SERVICE_VERSION_MAJOR}.${RELEASE_SERVICE_VERSION_MINOR}.${RELEASE_SERVICE_VERSION_MICRO}")

project(kturtle VERSION ${RELEASE_SERVICE_VERSION})

set(QT_MIN_VERSION "5.15.0")
set(KF5_MIN_VERSION "5.83.0")


find_package(ECM ${KF5_MIN_VERSION} REQUIRED NO_MODULE)

set(CMAKE_MODULE_PATH
    ${CMAKE_MODULE_PATH}
    ${ECM_MODULE_PATH}
    
)
include(KDEInstallDirs)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)
include(FeatureSummary)
include(ECMAddAppIcon)
include(ECMSetupVersion)

find_package(Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED COMPONENTS
    Core
    Gui
    Svg
    Widgets
    PrintSupport
)

find_package(KF5 ${KF5_MIN_VERSION} REQUIRED
    Crash
    KIO
    NewStuff
    DocTools
    TextWidgets
    I18n
    CoreAddons
)

#Allows QString concatenation to use a single memory allocation per source line.
add_definitions(-DQT_USE_FAST_CONCATENATION -DQT_USE_FAST_OPERATOR_PLUS)
add_definitions(-DQT_NO_URL_CAST_FROM_STRING)

add_subdirectory (doc)
add_subdirectory (src)
add_subdirectory (icons)

ki18n_install(po)
kdoctools_install(po)

install(FILES org.kde.kturtle.appdata.xml DESTINATION ${KDE_INSTALL_METAINFODIR})
feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
include(ECMOptionalAddSubdirectory)
ecm_optional_add_subdirectory(po)
