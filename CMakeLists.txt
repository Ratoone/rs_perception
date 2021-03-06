cmake_minimum_required(VERSION 2.8.3)
project(line_detection)
find_package(catkin REQUIRED 
	robosherlock
	sensor_msgs
	std_msgs
)
#generate_messages(
#	DEPENDENCIES
#	sensor_msgs)
################################################################################
## Constants for project                                                      ##
################################################################################
set(NAMESPACE line_detection)
set(TYPESYSTEM_CPP_PATH ${PROJECT_SOURCE_DIR}/include/line_detection/types)
set(TYPESYSTEM_XML_PATH ${PROJECT_SOURCE_DIR}/descriptors/typesystem)
set(ANNOTATOR_PATH      ${PROJECT_SOURCE_DIR}/descriptors/annotators)
set(ENGINE_PATH         ${PROJECT_SOURCE_DIR}/descriptors/analysis_engines)
set(CMAKE_MODULE_PATH   ${PROJECT_SOURCE_DIR}/cmake)
################################################################################
## Configure project configuration files                                      ##
################################################################################
configure_file(${RS_PROJECT_CONFIG} ${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}_config.cmake @ONLY)
################################################################################
## Find all include directories                                               ##
################################################################################
find_include_dirs(RS_INCLUDE_DIRS_LIST)
catkin_package(
   CFG_EXTRAS ${PROJECT_NAME}_config.cmake
   )
################################################################################
## Package dependencies                                                       ##
################################################################################
#Add required catkin packages here
#find_package(catkin REQUIRED COMPONENTS xxx)
#Add required system packages here
#find_package(xxx)
################################################################################
## Build                                                                      ##
################################################################################
## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
        ${RS_INCLUDE_DIRS_LIST}
        ${catkin_INCLUDE_DIRS}
        )

################################################################################
## Update analysis engines, typesystem and include all relevant files         ##
################################################################################
##Update xml list of annotators inside analysis engines
update_analysis_engines(robosherlock)
## generate classes from the typesystem xml files
generate_type_system(robosherlock)
#find all relevant files
find_additional_files()
################################################################################
## Sub Projects                                                               ##
################################################################################
#If you want to divide your projects into subprojects include the subdirectories
#each containing a CMakeLists.txt here
#add_subdirectory(src/xxx)

rs_add_library(rs_lineAnnotator src/LineAnnotator.cpp)
target_link_libraries(rs_lineAnnotator ${CATKIN_LIBRARIES})

rs_add_library(rs_lineAnnotator3D src/LineAnnotator3D.cpp)
target_link_libraries(rs_lineAnnotator3D ${CATKIN_LIBRARIES})

rs_add_library(rs_onlinePointCloudFilter src/OnlinePointCloudFilter.cpp)
target_link_libraries(rs_onlinePointCloudFilter ${CATKIN_LIBRARIES})

rs_add_library(rs_planeExtraction src/PlaneExtraction.cpp)
target_link_libraries(rs_planeExtraction ${CATKIN_LIBRARIES})
