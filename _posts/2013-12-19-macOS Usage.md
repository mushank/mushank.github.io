---
layout: post
title: "macOS Usage"
subtitle: ""
author: "Jack"
date: 2013-12-19
tag: [macOS, Usage]
header-img: "img/post-img/post-bg-ios.jpg"
---

## Display & hidden files
- Display hidden files  

  `$ defaults write com.apple.finder AppleShowAllFiles -bool true; killall Finder`

- Don't display hideen files  

  `$ defaults write com.apple.finder AppleShowAllFiles -bool false; killall Finder`

## Disable & enable Rootless feature
- `$ csrutil disable`	// Disable rootless


- `$ csrutil enable`	// Enable rootless
