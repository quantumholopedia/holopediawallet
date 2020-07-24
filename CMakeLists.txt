cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    holopedia/external
    holopedia/include
    holopedia/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("holopedia/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  holopedia/external/miniupnpc/connecthostport.c
  holopedia/external/miniupnpc/igd_desc_parse.c
  holopedia/external/miniupnpc/minisoap.c
  holopedia/external/miniupnpc/miniupnpc.c
  holopedia/external/miniupnpc/miniwget.c
  holopedia/external/miniupnpc/minixml.c
  holopedia/external/miniupnpc/portlistingparse.c
  holopedia/external/miniupnpc/receivedata.c
  holopedia/external/miniupnpc/upnpcommands.c
  holopedia/external/miniupnpc/upnpreplyparse.c
  holopedia/src/Common/Base58.cpp
  holopedia/src/Common/CommandLine.cpp
  holopedia/src/Common/Util.cpp
  holopedia/src/Common/StringTools.cpp
  holopedia/src/Common/JsonValue.cpp
  holopedia/src/Common/ConsoleTools.cpp
  holopedia/src/Common/MemoryInputStream.cpp
  holopedia/src/Common/PathTools.cpp
  holopedia/src/Common/StdInputStream.cpp
  holopedia/src/Common/StdOutputStream.cpp
  holopedia/src/Common/StreamTools.cpp
  holopedia/src/Common/StringOutputStream.cpp
  holopedia/src/Common/StringView.cpp
  holopedia/src/Common/VectorOutputStream.cpp
  holopedia/src/crypto/blake256.c
  holopedia/src/crypto/chacha8.c
  holopedia/src/crypto/crypto-ops-data.c
  holopedia/src/crypto/crypto-ops.c
  holopedia/src/crypto/crypto.cpp
  holopedia/src/crypto/groestl.c
  holopedia/src/crypto/hash-extra-blake.c
  holopedia/src/crypto/hash-extra-groestl.c
  holopedia/src/crypto/hash-extra-jh.c
  holopedia/src/crypto/hash-extra-skein.c
  holopedia/src/crypto/hash.c
  holopedia/src/crypto/jh.c
  holopedia/src/crypto/keccak.c
  holopedia/src/crypto/oaes_lib.c
  holopedia/src/crypto/random.c
  holopedia/src/crypto/skein.c
  holopedia/src/crypto/slow-hash.c
  holopedia/src/crypto/slow-hash.cpp
  holopedia/src/crypto/tree-hash.c
  holopedia/src/CryptoNoteCore/BlockchainIndices.cpp
  holopedia/src/CryptoNoteCore/BlockchainMessages.cpp
  holopedia/src/CryptoNoteCore/BlockIndex.cpp
  holopedia/src/CryptoNoteCore/CoreConfig.cpp
  holopedia/src/CryptoNoteCore/CryptoNoteBasic.cpp
  holopedia/src/CryptoNoteCore/CryptoNoteTools.cpp
  holopedia/src/CryptoNoteCore/Currency.cpp
  holopedia/src/CryptoNoteCore/MinerConfig.cpp
  holopedia/src/CryptoNoteCore/Transaction.cpp
  holopedia/src/CryptoNoteCore/Account.cpp
  holopedia/src/CryptoNoteCore/Blockchain.cpp
  holopedia/src/CryptoNoteCore/Checkpoints.cpp
  holopedia/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  holopedia/src/CryptoNoteCore/Core.cpp
  holopedia/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  holopedia/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  holopedia/src/CryptoNoteCore/Difficulty.cpp
  holopedia/src/CryptoNoteCore/IBlock.cpp
  holopedia/src/CryptoNoteCore/Miner.cpp
  holopedia/src/CryptoNoteCore/TransactionExtra.cpp
  holopedia/src/CryptoNoteCore/TransactionPool.cpp
  holopedia/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  holopedia/src/CryptoNoteCore/TransactionUtils.cpp
  holopedia/src/InProcessNode/InProcessNode.cpp
  holopedia/src/InProcessNode/InProcessNodeErrors.cpp
  holopedia/src/NodeRpcProxy/NodeErrors.cpp
  holopedia/src/NodeRpcProxy/NodeRpcProxy.cpp
  holopedia/src/P2p/NetNodeConfig.cpp
  holopedia/src/Serialization/BinaryInputStreamSerializer.cpp
  holopedia/src/Serialization/BinaryOutputStreamSerializer.cpp
  holopedia/src/Serialization/JsonInputValueSerializer.cpp
  holopedia/src/Serialization/JsonOutputStreamSerializer.cpp
  holopedia/src/Serialization/KVBinaryInputStreamSerializer.cpp
  holopedia/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  holopedia/src/Serialization/SerializationOverloads.cpp
  holopedia/src/Transfers/BlockchainSynchronizer.cpp
  holopedia/src/Transfers/SynchronizationState.cpp
  holopedia/src/Transfers/TransfersConsumer.cpp
  holopedia/src/Transfers/TransfersContainer.cpp
  holopedia/src/Transfers/TransfersSubscription.cpp
  holopedia/src/Transfers/TransfersSynchronizer.cpp
  holopedia/src/Wallet/LegacyKeysImporter.cpp
  holopedia/src/Wallet/WalletAsyncContextCounter.cpp
  holopedia/src/Wallet/WalletErrors.cpp
  holopedia/src/WalletLegacy/KeysStorage.cpp
  holopedia/src/WalletLegacy/WalletLegacy.cpp
  holopedia/src/WalletLegacy/WalletHelper.cpp
  holopedia/src/WalletLegacy/WalletLegacySerializer.cpp
  holopedia/src/WalletLegacy/WalletLegacySerialization.cpp
  holopedia/src/WalletLegacy/WalletTransactionSender.cpp
  holopedia/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  holopedia/src/WalletLegacy/WalletUserTransactionsCache.cpp
  holopedia/src/System/ContextGroup.cpp
  holopedia/src/System/Event.cpp
  holopedia/src/System/EventLock.cpp
  holopedia/src/System/InterruptedException.cpp
  holopedia/src/System/Ipv4Address.cpp
  holopedia/src/System/TcpStream.cpp
  holopedia/src/HTTP/HttpRequest.cpp
  holopedia/src/HTTP/HttpParser.cpp
  holopedia/src/HTTP/HttpParserErrorCodes.cpp
  holopedia/src/HTTP/HttpResponse.cpp
  holopedia/src/Rpc/HttpClient.cpp
  holopedia/src/Rpc/JsonRpc.cpp
  holopedia/src/P2p/NetNode.cpp
  holopedia/src/P2p/LevinProtocol.cpp
  holopedia/src/P2p/NetNodeConfig.cpp
  holopedia/src/P2p/PeerListManager.cpp
  holopedia/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  holopedia/src/Logging/ILogger.cpp
  holopedia/src/Logging/LoggerGroup.cpp
  holopedia/src/Logging/CommonLogger.cpp
  holopedia/src/Logging/LoggerManager.cpp
  holopedia/src/Logging/FileLogger.cpp
  holopedia/src/Logging/StreamLogger.cpp
  holopedia/src/Logging/ConsoleLogger.cpp
  holopedia/src/Logging/LoggerMessage.cpp
  holopedia/src/Logging/LoggerRef.cpp
  holopedia/src/BlockchainExplorer/BlockchainExplorer.cpp
  holopedia/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  holopedia/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(holopedia/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} holopedia/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(holopedia/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES holopedia/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
