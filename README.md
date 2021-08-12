## Prebuilt ANGLE binary for iOS using Metal (arm64, armv7, x86_64)

Based on ANGLE master branch, current version is [8ef9aba7a42f015a35e1e318affe188c3806f539](https://github.com/kakashidinho/metalangle/).

### Compiling instructions

In file src/libANGLE/renderer/gl/eagl/DisplayEAGL.mm
change `kEAGLRenderingAPIOpenGLES3` to `kEAGLRenderingAPIOpenGLES2`.
This updates target OpenGLES version to 2, which is required for old 32-bit iOS devices (iPhone 5).

Set these parameters for both 'MetalANGLE' and 'MetalANGLE_ios' targets:

```
Build Active Architecture Only: Yes
Deployment Postprocessing: Yes
Strip Linked Product: Yes
Strip Style: Non-Global Symbols
Perform Single-Object Prelink: Yes
Preserve Private External Symbols: No
Symbols Hidden By Default: No
Inline Methods Hidden: Yes
```

After changing the targets the static libraries can be built for all targets:

```
cd ios/xcode

./fetchDependencies.sh

xcodebuild -showsdks

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -target MetalANGLE_static -configuration Release -arch arm64 -sdk iphoneos13.2 build
echo Output is in build/Release-iphoneos/libMetalANGLE_static.a

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -target MetalANGLE_static -configuration Release -arch armv7 -sdk iphoneos13.2 build
echo Output is in build/Release-iphoneos/libMetalANGLE_static.a

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -target MetalANGLE_static -configuration Release -arch x86_64 -sdk iphonesimulator13.2 build
echo Output is in build/Release-iphonesimulator/libMetalANGLE_static.a

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -target MetalANGLE_static -configuration Release -arch i386 -sdk iphonesimulator13.2 build
echo Output is in build/Release-iphonesimulator/libMetalANGLE_static.a

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -target MetalANGLE_static -configuration Release -arch arm64 -sdk iphonesimulator13.2 build
echo Output is in build/Release-iphonesimulator/libMetalANGLE_static.a
```

Note: to enable **Bitcode**, add `OTHER_CFLAGS="-fembed-bitcode"` to command line arguments for arm64 and armv7 iOS targets.


In order to build libraries for Mac Catalyst, use the following commands:

```
xcodebuild -project OpenGLES.xcodeproj -scheme MetalANGLE_static -sdk macosx -configuration Release -destination 'platform=macOS,variant=Mac Catalyst' build SUPPORTS_MACCATALYST=YES
lipo <PATH_TO_OUTPUT>/libMetalANGLE_static.a -extract x86_64 -output libMetalANGLE_static_x86_64.a
lipo <PATH_TO_OUTPUT>/libMetalANGLE_static.a -extract arm64 -output libMetalANGLE_static_arm64.a
```
