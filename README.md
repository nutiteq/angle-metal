## Prebuilt ANGLE binary for iOS using Metal (arm64, armv7, x86_64)

Based on ANGLE master branch, current version is [8590cd1c74a576bd99c88f46cf07e33c845f57a5](https://github.com/kakashidinho/metalangle/).

### Compiling instructions

In file src/libANGLE/renderer/gl/eagl/DisplayEAGL.mm
change `kEAGLRenderingAPIOpenGLES3` to `kEAGLRenderingAPIOpenGLES2`.
This updates target OpenGLES version to 2, which is required for old 32-bit iOS devices (iPhone 5).

Set these parameters for both 'MetalANGLE' and 'MetalANGLE_ios' targets:

```
Valid Architectures: arm64 armv7 x86_64 i386
Build Active Architecture Only: Yes
Mach-O Type: Static Library
Symbols Hidden by Default: No
```

After changing the targets the static libraries can be built for all targets:

```
cd ios/xcode
./fetchDependencies.sh

xcodebuild -showsdks

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -configuration Release -arch arm64 -sdk iphoneos13.2 build
echo Output is in build/Release-iphoneos/MetalANGLE.framework/MetalANGLE

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -configuration Release -arch armv7 -sdk iphoneos13.2 build
echo Output is in build/Release-iphoneos/MetalANGLE.framework/MetalANGLE

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -configuration Release -arch x86_64 -sdk iphonesimulator13.2 build
echo Output is in build/Release-iphonesimulator/MetalANGLE.framework/MetalANGLE

rm -rf build
xcodebuild -project OpenGLES.xcodeproj -configuration Release -arch i386 -sdk iphonesimulator13.2 build
echo Output is in build/Release-iphonesimulator/MetalANGLE.framework/MetalANGLE
```
