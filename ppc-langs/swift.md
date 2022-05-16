# Swift

Site: swift.org

Create project:

```
// Package
$ swift package init
$ swift build

// Executable
mkdir swift-test && cd swift-test
swift package init --type executable
swift run swift-test

// Binary will be here: .build/${ARCH}-apple-macosx/${RELEASE_TYPE}/swift-test
```

Default arch & platform are your host arch & platform.

Build for arm via xcode:

```
$ xcrun swift build -c release --arch arm64
```
