# clang-as-ios-dylib

__clang-as-ios-dylib__ is a workaround for building iOS dynamic libraries from Xcode.  

## Usage

1. In Xcode, create a new __OS X Cocoa Library__ project.

1. In the project, create a new __Configuration Settings File (.xcconfig)__ file with the following contents:
  ```sh
  // The following can be one of 'latest', 'earliest', or a specific
  // SDK version like '7.0', or '6.1'.
  CAID_BASE_SDK_VERSION = latest
  CAID_IPHONEOS_DEPLOYMENT_TARGET = latest
  
  CAID_LINKS_PATH = $(PROJECT_DIR)/../Vendor/clang-as-ios-dylib/links
  
  LD = $(CAID_LINKS_PATH)/ld-iphonesimulator-$(CAID_BASE_SDK_VERSION)-targeting-$(CAID_IPHONEOS_DEPLOYMENT_TARGET)
  CC = $(CAID_LINKS_PATH)/cc-iphonesimulator-$(CAID_BASE_SDK_VERSION)-targeting-$(CAID_IPHONEOS_DEPLOYMENT_TARGET)
  ```

1. In your project’s __Info__ settings panel, for each build configuration, select the xcconfig file created in #2.

1. Change references to __Cocoa.framework__ to __Foundation.framework__:

  * In your __.pch__ file, change `#import <Cocoa/Cocoa.h>` to `#import <Foundation/Foundation.h>`.
  * In your target's __Build Phases__ panel, under __Link Binary With Libraries__, remove __Cocoa.framework__ and add __Foundation.framework__.

