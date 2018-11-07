---
layout: post
title: "iOS开发｜CocoaPods Usage"
subtitle: ""
author: "Jack"
date: 2014-04-05
tag: [iOS, CocoaPods, Usage]
header-img: "img/post-img/post-bg-ios.jpg"
---

## 1. Install

1. MAC OS默认已有Ruby环境，所以可直接执行安装命令（升级CocoaPods也使用此命令）：  
   `$ sudo gem install cocoapods`

2. 安装结束后，执行命令更新本地Pods依赖库Tree：  
   `$ pod setup`

### 1.1 Problems may be occured
- 执行install命令无反应  
  这是因为Ruby默认源使用的是cocoapod.org，国内访问这个网址偶尔会有困难，可通过将Ruby源替换成`https://gems.ruby-china.org/`来解决。替换命令如下：  
  `$ gem sources --remove https://rubygems.org/`  
  `$ gem sources -add https://gems.ruby-china.org/`  
  执行命令`gem sources -l`来查看是否替换成功，正常输出如下：  

  >	*** CURRENT SOURCES ***  
  	https://gems.ruby-china.org/ 	

- gem版本过于陈旧  
  如果gem版本过于陈旧，也有可能会导致安装CocoaPods失败，可通过如下命令升级gem：  
  `$ sudo gem update --system`  

## 2. Usage
1. 切换至工程目录下，执行命令创建Podfile文件：  
   `$ touch Podfile`  

2. 编辑Podfile文件，写入需要的第三方库信息，示例如下：  

   >	platform :ios, '7.0'  
   	inhibit_all_warnings!  
   	pod 'AFNetworking', '~> 2.6.0'  
   	pod 'Reachability', '~> 3.2'  
   	pod 'Masonry', '~> 0.6.3'  
   	pod 'SDWebImage', '~> 3.7.3'  
   	pod 'MBProgressHUD', '~> 0.9.1'  
   	pod 'IQKeyboardManager', '~> 3.3.2'  

3. 导入第三方库，命令如下：  
   `$ pod install`  

### 2.1 About Podfile
-  Podfile文件并非必须放置在当前工程文件中，可以放置在任意一个目录下，想要这么做只需在Podfile文件中指定工程文件的路劲即可，在Podfile文件最开始增加一行内容：  
   `xcodeproj "/Users/xxx/xxx/Project/Project.xcodeproj"`  
   然后进入Podfile文件所在路劲，执行`$ pod install`命令下载指定第三方库。  

-  如果不显式指定Podfile对应的Target，CocoaPods默认指向我们工程中的第一个Target。换句话说，默认只有工程中的第一个Target能够使用Podfile中描述的Pod依赖库。如果想让一个Podfile文件同时描述工程中多个Target，可使用如下方法：  
   * 多个Target使用相同Pods依赖库，可通过使用`link_with`关键字实现，在Podfile文件首行添加如下内容：  
     `link_with 'Target_1', 'Target_2'`  
   * 不同Target使用不同Pods依赖库，此时可通过使用`target`关键字实现，Podfile文件描述如下(其中do/end作为开始和结束的标识符)：  

     >	target :'Target_1' do  
     	platform :ios  
     	pod 'AFNetworking', '~> 2.6.0'  
     	pod 'Reachability', '~> 3.2'  
     	platform :ios, '7.0'  
     	pod 'SDWebImage', '~> 3.7.3'  
     	end  
     	target :'Target_2' do  
     	pod 'Masonry', '~> 0.6.3'  
     	end  

-  Podfile中指定依赖库版本，具体写法和标识含义如下：  

   >	pod 'AFNetworking'	//不显式指定依赖库版本，表示每次都获取最新版本  
   	pod 'AFNetworking', '2.0'	//只使用2.0版本  
   	pod 'AFNetworking', '> 2.0'	//使用高于2.0的版本  
   	pod 'AFNetworking', '>= 2.0'	//使用大于或等于2.0的版本  
   	pod 'AFNetworking', '< 2.0'	//使用小于2.0的版本  
   	pod 'AFNetworking', '<= 2.0'	//使用小于或等于2.0的版本  
   	pod 'AFNetworking', '~> 0.1.2'	//使用大于等于0.1.2但小于0.2的版本  
   	pod 'AFNetworking', '~>0.1'	//使用大于等于0.1但小于1.0的版本  
   	pod 'AFNetworking', '~>0'	//高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本  

### 2.2 Key Orders
- **$ pod install**  
  根据Podfile文件指定的内容，安装依赖库，如果有Podfile.lock文件而且对应的Podfile文件未被修改，则会根据Podfile.lock文件指定的版本安装。 
  每次更新了Podfile文件时，都需要重新执行该命令，以便重新安装Pods依赖库。   
- **$ pod update**  
  若果Podfile中指定的依赖库版本不是写死的，当对应的依赖库有了更新，无论有没有Podfile.lock文件都会去获取Podfile文件描述的允许获取到的最新依赖库版本。  
- **$ pod search**  
  该命令是用来按名称搜索可用的Pods依赖库  
- **$ pod setup**  
  该命令用于更新本地电脑上的保存的Pods依赖库Tree。  
  由于每天有很多人会创建或者更新Pods依赖库，这条命令执行的时候会异常耗时。我们需要经常执行这条命令，否则有新的Pods依赖库时执行pod search命令是搜不出来的。   

## 3. Publish
1. 为了发布自己的CocoaPods依赖库，首先要向对应的Git仓库添加相关文件，所需文件如下：  
   - **podspec文件**：  
     该文件为Pods依赖库的描述文件，每个Pods依赖库都必须有且仅能有这么一个描述文件。文件名称需要和创建的依赖库名称保持一致，即example.podspec。创建命令：`$ pod spec create example.podspec`  
   - **LICENSE文件**  
     CocoaPods强制要求所有的Pods依赖库都必须要有license文件，否则通不过验证。  
   - **类文件**  
     将类文件放入与依赖库名称相同的文件夹中。  
   - **Demo**  
     为了使用户能够快速掌握我们的依赖库，通常需要提供一个Demo工程。将创建的Demo工程放入名为exampleDemo文件夹中。  
   - **README.md**  
     一个成功的GitHub仓库必不可少的一部分，用于描述对仓库的详细说明。  
     ***注意：其中1. 2. 3. 是必需的，4. 5. 是可选的，不过建议全部创建***  

2. 提交本地仓库至GitHub

3. 发布至CocoaPods（Trunk方式）
   1. 注册Trunk  
      `$ pod trunk register example@email.com 'yourName'  --verbose` // --verbose参数是为了便于输出注册过程中的调试信息  
      执行完命令，注册邮箱会收到一封带有验证链接的邮件，请点击确认。

   2. 查询注册信息  
      `$ pod trunk me`  

   3. 提交podspec文件  
      `$ pod trunk push example.podspec` // 此命令会首先验证podspec文件是否合法，检测合法后上传至trunk服务器

   **注：以前CocoaPods团队的审核时间一般需要一到两个工作日，而现在几秒钟内即可完成，并且会自动更新本地Pods依赖库Tree，成功提交后使用`$ pod search`命令即可立即查询到自己提交的Pods库**


4. ~~发布至CocoaPods(Pull Request方式)~~  
   *(此种方式已被CocoaPods官方废弃，请使用`Trunk`方式进行发布）*

   1. 执行如下命令,为Pods依赖库添加版本号和标签号：  
      `$ set the new version to 1.0.0`  
      `$ set the new tag to 1.0.0`  
   2. 执行Pod验证命令：  
      `$ pod lib lint`  
      如果一切正常，此命令执行完后会出现如下输出：  

      >	-> example (1.0.0)  
      	example passed validation.  

      至此，Pod验证成功。  
      **注：在执行Pod验证命令时，log信息中出现任何warning/error信息，验证都会失败！可根据log信息做出响应修改后再次验证**  

   3. fork一份CocoaPods官方的Specs仓库  

   4. clone刚才fork的仓库到本地   

   5. 将自己的podspec文件添加到本地Specs仓库中  
      **注：podspec文件在Specs仓库中的保存原则是：Pods依赖库同名文件夹--->版本号同名文件夹--->podspec文件**  
   6. 提交本地Specs仓库中的修改到GitHub仓库  

   7. 将自己fork的Specs仓库所做的修改pull给CocoaPods官方的Specs仓库  

   8. 等待CocoaPods维护人员审核通过，并将我们的pull请求合并到官方Specs仓库中。  
      **注：有任何消息，CocoaPods都会发邮件通知**  
   9. 通过审核后，执行`$ pod set`命令更新本地依赖库Tree，随后使用搜索命令`$ pod search`搜索，就能找到我们自己发布的Pods库了！








