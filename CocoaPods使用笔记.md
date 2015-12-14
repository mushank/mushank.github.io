# CocoaPods使用笔记

## 安装部分

1. MAC OS默认已有Ruby环境，所以可直接执行安装命令（升级CocoaPods也使用此命令）：  
` $ sudo gem install cocoapods `

2. 安装结束后，执行命令：  
` $ pod setup `

### 可能遇到的问题
1. 执行install命令无反应  
这是因为Ruby默认源使用的是cocoapod.org，国内访问这个网址偶尔会有困难，可通过将Ruby源替换成淘宝来解决。替换命令如下：  
` $ gem sources --remove https://rubygems.org/ `  
` $ gem sources -a https://taobao.org/ `  
执行命令` gem sources -l ` 来查看是否替换成功，正常输出如下：  
	\*\*\* CURRENT SOURCES \*\*\*

	https://ruby.taobao.org/  
	
2. gem版本过于陈旧  
如果gem版本过于陈旧，也有可能会导致安装CocoaPods失败，可通过如下命令升级gem：  
` $ sudo gem update --system `  

## 使用CocoaPods
1. 切换至工程目录下，执行命令创建Podfile文件：  
` touch Podfile `  

2. 编辑Podfile文件，写入需要的第三方库信息，示例如下：  
` 
platform :ios, '7.0'
inhibit_all_warnings!
pod 'AFNetworking', '~> 2.6.0'
pod 'Reachability', '~> 3.2'
pod 'Masonry', '~> 0.6.3'
pod 'SDWebImage', '~> 3.7.3'
pod 'MBProgressHUD', '~> 0.9.1'
pod 'IQKeyboardManager', '~> 3.3.2'
`

3. 导入第三方库，命令如下：  
` $ pod install `  

