---
title: "ZKUDIDManager"
date: 2016-03-24 19:30:00
tag: [iOS, UDID, IDFV, Keychain]
projects: true
layout: post

---

**Generate and manage persistent UDID(Unique Device Identifier) in iOS device.**

*Use `IDFV(identifierForVendor)` + `keychain` to make sure UDID consistency, even if the App has been removed or reinstalled.*

*A replacement for the deprecated mean of `OpenUDID`.*

## 1. Install

```
pod 'ZKUDIDManager', '~> 1.0.2'
```

*Noti: Requires iOS6.0 and later*

## 2. Usage
It's so simple, just two lines of code:

```
#include "ZKUDIDManager.h"
NSString* UIDIString = [ZKUDIDManager value];
```

## 3. Source files
They are in the `ZKUDIDManager` folder:   

- `ZKUDIDManager.h`  
- `ZKUDIDManager.m`  

Now, enjoy yourself!

***[Github Link: ZKUDIDManager.git](https://github.com/mushank/ZKUDIDManager)***
