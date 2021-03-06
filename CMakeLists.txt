#
# This is the toplevel CMakeLists.txt that is used to build Couchbase
#
CMAKE_MINIMUM_REQUIRED(VERSION 2.8.10)
PROJECT("Couchbase")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/tlm/cmake/Modules/")

# Have to add deps first, before any other part of the build tries to
# FIND anything
ADD_SUBDIRECTORY(tlm/deps)

INCLUDE(CouchbaseSingleModuleBuild)

SET(ASTYLE_CFLAGS --recursive
                  --mode=c
                  --quiet
                  --style=1tbs
                  --indent=spaces=4
                  --indent-namespaces
                  --indent-col1-comments
                  --max-instatement-indent=78
                  --pad-oper
                  --pad-header
                  --add-brackets
                  --unpad-paren
                  --align-pointer=name
                  "*.h" "*.c")

# Add a quick (and safe) way to nuke everything
ADD_CUSTOM_TARGET(reset
                  COMMAND repo forall -c "git clean -dfx"
                  VERBATIM)

ADD_SUBDIRECTORY(platform)
ADD_SUBDIRECTORY(memcached)
ADD_SUBDIRECTORY(couchstore)
ADD_SUBDIRECTORY(ep-engine)
ADD_SUBDIRECTORY(couchbase-cli)
ADD_SUBDIRECTORY(couchbase-examples)
ADD_SUBDIRECTORY(healthchecker)
ADD_SUBDIRECTORY(couchbase-python-client)
ADD_SUBDIRECTORY(sigar)
ADD_SUBDIRECTORY(moxi)
ADD_SUBDIRECTORY(couchdb)
ADD_SUBDIRECTORY(geocouch)
ADD_SUBDIRECTORY(ns_server ${CMAKE_CURRENT_SOURCE_DIR}/ns_server/build)
ADD_SUBDIRECTORY(indexing-build-scripts)

