Bowser
======

A WebRTC browser for iOS developed in the open. Bowser is built on top of [OpenWebRTC](https://github.com/EricssonResearch/openwebrtc).

![Bowser logo](http://static.squarespace.com/static/53f1eedee4b0439bf8d480c5/t/53f25022e4b0cca46a383183/1408389154850/?format=500w "Bowser logo")

## ISI Install Instructions

This assumes you have checked out and built [OpenWebRTC](www.github.com/isi-research/openwebrtc) for iOS.

* Clone this repo somewhere on your Mac and open Bowser in XCode.
* Drag and drop the OpenWebRTC.framework that you built into the Frameworks box on the left panel in XCode. You will find OpenWebRTC.framework in ~/Library/Developer/OpenWebRTC/iPhone.sdk. If you don't find it here, it's possible that it's in /Library/Developer/OpenWebRTC/iPhone.skd but this means you didn't install the iOS package for only your user.
* Update the include paths in Xcode to include all the header files in the repository. Go to build settings and edit Header Search Paths to add the directory where you checked out Bowser. Recursively add this directory. 
* Go to Product -> Scheme -> Edit Scheme and ensure that you have the release build set for 'Run' if you want to build the optimized version. Else you need to ensure you build for profiling.

## App Store
Bowser is not only Open Source, it is also available as a [free download](https://itunes.apple.com/app/bowser/id560478358?mt=8) on the Apple App Store. When improvements have been made to Bowser or OpenWebRTC new versions for the App Store are published by Ericsson Research.

<a href="https://itunes.apple.com/app/bowser/id560478358?mt=8"><img src="http://static.squarespace.com/static/53f1eedee4b0439bf8d480c5/t/545343aee4b0c4d5c0fdb7be/1414742958599/Download_on_the_App_Store_Badge_US-UK_135x40_0801.png?format=300w"></a>

[![Bowser video](http://img.youtube.com/vi/mR_-2trCjzE/0.jpg)](http://www.youtube.com/watch?v=mR_-2trCjzE)

## Developing for Bowser
Tips and other resources can be found on the wiki page [Developing for Bowser](https://github.com/EricssonResearch/bowser/wiki/Developing-for-Bowser).

## Extension of UIWebView
Bowser is based on the official `UIWebView` provided by the platform and the [WebRTC](http://www.w3.org/2011/04/webrtc/) API's are implemented with JavaScript that is injected into web pages as they load, the injected JavaScript code is using remote procedure calls to control the [OpenWebRTC](https://github.com/EricssonResearch/openwebrtc) backend.

The [plan](https://github.com/EricssonResearch/bowser/issues/1) is to move to the `WKWebView`, introduced in iOS 8, as soon as possible.  

## Video rendering
Mobile Safari on iPhone displays `<video>` elements only in fullscreen. This severely limits the UI of your apps, especially when designing video communication apps using WebRTC. Bowser goes beyond that and allows you to fully customise and manipulate `<video>` elements using CSS and JavaScript.

## Building
To build Bowser you need a completed build of [OpenWebRTC](https://github.com/EricssonResearch/openwebrtc), Xcode and the iOS SDK. The build itself is pretty simple as we have set up the header/library search paths to expect that you cloned Bowser alongside OpenWebRTC. As long as you have:
```
/path/to/bowser
/path/to/openwebrtc
```
...then you shouldn't have any problems with just opening the Xcode project and building.

## Debugging WebRTC scripts
As Bowser is using the official `UIWebView` that is provided by the iOS platform you can use [Apple's Web Developer Tools](https://developer.apple.com/safari/tools/) to debug your WebRTC scripts. It is also possible to use Safari on OS X to debug Bowser. The details are described in this [blog post](http://www.openwebrtc.org/blog/2014/10/31/webrtc-in-safari-using-openwebrtc).

## Background
Bowser was originally developed by Ericsson Research and released in October of 2012, for both iOS and Android devices. Back then Bowser was the world's first WebRTC-enabled browser for mobile devices. Bowser was later removed from the Apple App Store and Google Play but was resurrected and released as Open Source together with OpenWebRTC.

## License
Bowser is released under BSD-2 clause. See LICENSE for details.

## Building for MARS
Building Bowser for iOS devices on macOS is a pain because the Bowser documentation is very messy. If you just want to run Bowser, you can either use the iTunes store version or download directly from the Ericsson Research Github. The Github version uses CocoaPods to download a version of OpenWebRTC without any debugging symbols so it's impossible to do any proper development/profiling. 
If you want to get a look at the debugging info in Bowser, you need to get OpenWebRTC by yourself and add this to the XCode project. The following guide is a bit hacky and the result of a trial and error process method of getting everything building in XCode. There may be a much better method of achieving the same results. 

### Step-by-step guide for macOS host building openwebrtc for iOS

```
1. cd ~ # this is important! cerbero requires you to checkout in home.
2. git clone git@github.com:isi-research/cerbero.git
3. cd cerbero
4. sudo mkdir -p /Library/Frameworks/OpenWebRTC.framework
5. sudo chown -R $UID /Library/Frameworks/OpenWebRTC.framework/
6. mkdir -p dist
7. mkdir -p /Library/Frameworks/OpenWebRTC.framework/Versions/0.3
8. ln -sf /Library/Frameworks/OpenWebRTC.framework/Versions/0.3 dist/darwin_x86_64
9. ./cerbero-uninstalled -c config/osx-x86-64.cbc fetch-package --full-reset --reset-rdeps openwebrtc && ./cerbero-uninstalled -c config/osx-x86-64.cbc bootstrap && ./cerbero-uninstalled -c config/osx-x86-64.cbc package -f openwebrtc #builds for macOS
10. ./cerbero-uninstalled -c config/cross-ios-universal.cbc fetch-package --full-reset --reset-rdeps openwebrtc && ./cerbero-uninstalled -c config/cross-ios-universal.cbc bootstrap && ./cerbero-uninstalled -c config/cross-ios-universal.cbc package -f openwebrtc #builds for iOS
11. sudo mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.build
12. open openwebrtc-0.3.0-x86_64.pkg #follow pkg instructions to install
13. open openwebrtc-devel-0.3.0-x86_64.pkg #follow pkg instructions to install
14. open openwebrtc-devel-0.3.0-ios-universal.pkg #select 'install for me only'
```

### Step-by-step guide for macOS host building Bowser for iOS

1. Go do directory where you want to download/build Bowser
```
2. git clone git@github.com:isi-research/bowser.git
```
3. Drag and drop the OpenWebRTC.framework into the Frameworks box on the left panel in XCode. You will find OpenWebRTC.framework in ~/Library/Developer/OpenWebRTC/iPhone.sdk.
4. Go to Product -> Scheme -> Edit Scheme and ensure that you have the release build set for 'Run' if you want to build the optimized version. Else you need to ensure you build for profiling. 

## Updating code in OpenWebRTC

Updating code in Bowser is trivial as you just update and rebuild the Xcode project. However, when you want to update the code in openwebrtc, you need to take a more roundabout route. 

1. Check out the code for the dependency from our fork in the isi-research Github.
2. Edit the code in the checked out repository.
3. Commit and push the changes.
```
4. sudo mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.package # basically we are just 'backing up' the build. you could delete it too.
5. mv /Library/Frameworks/OpenWebRTC.framework.build /Library/Frameworks/OpenWebRTC.framework
6. ./cerbero-uninstalled -c config/osx-x86-64.cbc fetch-package --full-reset --reset-rdeps openwebrtc && ./cerbero-uninstalled -c config/osx-x86-64.cbc bootstrap && ./cerbero-uninstalled -c config/osx-x86-64.cbc package -f openwebrtc #builds for macOS
7. ./cerbero-uninstalled -c config/cross-ios-universal.cbc fetch-package --full-reset --reset-rdeps openwebrtc && ./cerbero-uninstalled -c config/cross-ios-universal.cbc bootstrap && ./cerbero-uninstalled -c config/cross-ios-universal.cbc package -f openwebrtc #builds for iOS
8. mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.build
9. open openwebrtc-0.3.0-x86_64.pkg #follow pkg instructions to install
10. open openwebrtc-devel-0.3.0-x86_64.pkg #follow pkg instructions to install
11. open openwebrtc-devel-0.3.0-ios-universal.pkg #select 'install for me only'
```
12. Rebuild the project in Xcode. 
