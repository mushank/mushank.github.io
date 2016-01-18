# webRTC Dev Notes

## iOS Plateform

### Development Environment
---
An OS X machine is required for iOS development. While it’s possible to develop purely from the command line with text editors, it’s easiest to use Xcode. Both methods will be illustrated here.

### Getting the Code
---

#### Install prerequisite software

#### Set the target OS in your environment:
` export GYP_DEFINES="OS=ios" `
#### Create a working directory, enter it, and run
` fetch --nohooks webrtc_ios `  
` gclient sync `  
This will fetch a regular WebRTC checkout with the iOS-specific parts added. The same checkout can be used for both Mac and iOS development, depending on the OS you set in GYP_DEFINES (see above).

### Compiling the Code
---
