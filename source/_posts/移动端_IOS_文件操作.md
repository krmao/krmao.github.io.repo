---
title: 移动端_IOS_文件操作
date: 2018-11-19 18:29:49
categories: [技术]
tags: [移动端,IOS]
---

##### 1： 拖拽需要打包的文件至工程为蓝色，勾选 Copy items if needed and Create folder references
##### 2：TARGETS -> Build Phases -> Copy Bundle Resources -> add 第一步的文件
##### 3：案例
```
// 1、获取沙盒根目录let homeDir = NSHomeDirectory() as String
let homeDir = NSHomeDirectory() as String
print("[hybird]", "homeDir", homeDir)

// 2、获取 Documents 目录
let docDirs = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true) as NSArray
let docDir = docDirs[0] as! String
print("[hybird]","docDirs",docDirs)
print("[hybird]","docDir",docDir)
// 3、获取 Caches 目录
let cachesDirs = NSSearchPathForDirectoriesInDomains(.cachesDirectory, .userDomainMask, true) as NSArray
let cachesDir = cachesDirs[0] as! String
print("[hybird]","cachesDirs",cachesDirs)
print("[hybird]","cachesDir",cachesDir)


// 4、获取 Library 目录
let libDirs = NSSearchPathForDirectoriesInDomains(.libraryDirectory, .userDomainMask, true) as NSArray
let libDir = libDirs[0] as! String
print("[hybird]","libDirs",libDirs)
print("[hybird]","libDir",libDir)

// 5、获取 tmp 目录

let tmpDir = NSTemporaryDirectory() as String
print("[hybird]","tmpDir",tmpDir)

// 6、初始化 fileManager
let fileManager = FileManager.default

// 7、使用 fileManager 创建目录
let testDir = docDir + "/test"

do {
    try  fileManager.createDirectory(atPath: testDir, withIntermediateDirectories: true, attributes: nil)
} catch {
    print("[hybird]","createDirectory failure")
}

// 8、使用 fileManager 创建文件
let testFile1 = testDir + "/test1.txt"
let testFile2 = testDir + "/test2.txt"
fileManager.createFile(atPath: testFile1, contents: nil, attributes: nil)
fileManager.createFile(atPath: testFile2, contents: nil, attributes: nil)

// 9、使用 fileManager 获取目录下的文件名
var files = fileManager.subpaths(atPath: testDir)
print("[hybird]","files:",files ?? "")
// 10、使用 fileManager 删除文件
do {
    try fileManager.removeItem(atPath: testFile1)
} catch  {
    print("[hybird]","removeItem failure")
}

print("[hybird]","Bundle.main.resourcePath",Bundle.main.resourcePath ?? "null")
print("[hybird]","Bundle.main.bundlePath",Bundle.main.bundlePath)

files = fileManager.subpaths(atPath: Bundle.main.bundlePath)

print("[hybird]","============================================")
for file in files! {
    print("[hybird]","file:",file)
}
print("[hybird]","============================================")

let bundleZipPath = Bundle.main.path(forResource: "bundle", ofType: "zip", inDirectory: "assets") ?? ""
print("[hybird]","bundleZipPath:",bundleZipPath)
```

##### 4：输出


```
[hybird] homeDir /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630
[hybird] docDirs (
    "/Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Documents"
)
[hybird] docDir /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Documents
[hybird] cachesDirs (
    "/Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Library/Caches"
)
[hybird] cachesDir /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Library/Caches
[hybird] libDirs (
    "/Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Library"
)
[hybird] libDir /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/Library
[hybird] tmpDir /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Data/Application/26A9A63E-FB26-4975-B2C9-CD71B0ECE630/tmp/
[hybird] files: ["test1.txt", "test2.txt"]
[hybird] Bundle.main.resourcePath /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Bundle/Application/A4E96998-21EA-4EFB-9B13-B4FDBC16D9F0/fruit-shop-ios.app
[hybird] Bundle.main.bundlePath /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Bundle/Application/A4E96998-21EA-4EFB-9B13-B4FDBC16D9F0/fruit-shop-ios.app
[hybird] ============================================
[hybird] file: _CodeSignature
[hybird] file: _CodeSignature/CodeResources
[hybird] file: assets
[hybird] file: assets/bundle.zip
[hybird] file: Assets.car
[hybird] file: Base.lproj
[hybird] file: Base.lproj/LaunchScreen.storyboardc
[hybird] file: Base.lproj/LaunchScreen.storyboardc/01J-lp-oVM-view-Ze5-6b-2t3.nib
[hybird] file: Base.lproj/LaunchScreen.storyboardc/Info.plist
[hybird] file: Base.lproj/LaunchScreen.storyboardc/UIViewController-01J-lp-oVM.nib
[hybird] file: Base.lproj/Main.storyboardc
[hybird] file: Base.lproj/Main.storyboardc/BYZ-38-t0r-view-8bC-Xf-vdC.nib
[hybird] file: Base.lproj/Main.storyboardc/Info.plist
[hybird] file: Base.lproj/Main.storyboardc/UIViewController-BYZ-38-t0r.nib
[hybird] file: Frameworks
[hybird] file: Frameworks/libswiftCore.dylib
[hybird] file: Frameworks/libswiftCoreFoundation.dylib
[hybird] file: Frameworks/libswiftCoreGraphics.dylib
[hybird] file: Frameworks/libswiftCoreImage.dylib
[hybird] file: Frameworks/libswiftDarwin.dylib
[hybird] file: Frameworks/libswiftDispatch.dylib
[hybird] file: Frameworks/libswiftFoundation.dylib
[hybird] file: Frameworks/libswiftMetal.dylib
[hybird] file: Frameworks/libswiftObjectiveC.dylib
[hybird] file: Frameworks/libswiftos.dylib
[hybird] file: Frameworks/libswiftQuartzCore.dylib
[hybird] file: Frameworks/libswiftSwiftOnoneSupport.dylib
[hybird] file: Frameworks/libswiftUIKit.dylib
[hybird] file: Frameworks/SnapKit.framework
[hybird] file: Frameworks/SnapKit.framework/_CodeSignature
[hybird] file: Frameworks/SnapKit.framework/_CodeSignature/CodeResources
[hybird] file: Frameworks/SnapKit.framework/Info.plist
[hybird] file: Frameworks/SnapKit.framework/SnapKit
[hybird] file: fruit-shop-ios
[hybird] file: Info.plist
[hybird] file: libswiftRemoteMirror.dylib
[hybird] file: PkgInfo
[hybird] file: Podfile
[hybird] ============================================
[hybird] bundleZipPath: /Users/maokangren/Library/Developer/CoreSimulator/Devices/4001EB70-F821-4801-905D-49DA2DAF52C2/data/Containers/Bundle/Application/A4E96998-21EA-4EFB-9B13-B4FDBC16D9F0/fruit-shop-ios.app/assets/bundle.zip
```
