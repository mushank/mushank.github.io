---
title: "webRTC Dev Notes"
date: 2016-01-18 19:00:00
tag: [webRTC]
blog: true
layout: post

---


# webRTC Dev Notes

### Development Environment
---
An OS X machine is required for iOS development. While it’s possible to develop purely from the command line with text editors, it’s easiest to use Xcode. Both methods will be illustrated here.

### Getting the Code
---
#### Install prerequisite software/depot_tools
1. Fetch depot_tools:  
` $ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git `

2. Add depot_tools to your PATH:  
` $ export PATH=`pwd`/depot_tools:"$PATH" `  
   * Yes, you want to put depot_tools ahead of everything else, otherwise gcl will refer to the GNU Common Lisp compiler.  
   * You may want to add this to your .bashrc file or your shell's equivalent so that you don’t need to reset your $PATH manually each time you open a new shell.  

3. Xcode 3.0 or higher

#### Set the target OS in your environment:
` export GYP_DEFINES="OS=ios" `
#### Create a working directory, enter it, and run
` fetch --nohooks webrtc_ios `  
` gclient sync `  
This will fetch a regular WebRTC checkout with the iOS-specific parts added. The same checkout can be used for both Mac and iOS development, depending on the OS you set in GYP_DEFINES (see above).

### Compiling the Code
---
