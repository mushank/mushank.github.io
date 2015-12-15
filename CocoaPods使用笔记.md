# CocoaPods使用笔记

## 安装部分

1. MAC OS默认已有Ruby环境，所以可直接执行安装命令（升级CocoaPods也使用此命令）：  
` $ sudo gem install cocoapods `

2. 安装结束后，执行命令更新本地Pods依赖库Tree：  
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

### 关于Podfile
1. Podfile文件并非必须放置在当前工程文件中，可以放置在任意一个目录下，想要这么做只需在Podfile文件中指定工程文件的路劲即可，在Podfile文件最开始增加一行内容：  
` xcodeproj "/Users/xxx/xxx/Project/Project.xcodeproj" `  
然后进入Podfile文件所在路劲，执行pod install命令下载指定第三方库。  

2. 如果不显式指定Podfile对应的Target，CocoaPods默认指向我们工程中的第一个Target。换句话说，默认只有工程中的第一个Target能够使用Podfile中描述的Pod依赖库。如果想让一个Podfile文件同时描述工程中多个Target，可使用如下方法：  
   * 多个Target使用相同Pods依赖库，可通过使用` link_with `关键字实现，在Podfile文件首行添加如下内容：  
   ` link_with 'Target_1', 'Target_2' `  
   * 不同Target使用不同Pods依赖库，此时可通过使用` target `关键字实现，Podfile文件描述如下(其中do/end作为开始和结束的标识符)：  
	target :'Target_1' do  
	platform :ios  
	pod 'AFNetworking', '~> 2.6.0'  
	pod 'Reachability', '~> 3.2'  
	platform :ios, '7.0'  
	pod 'SDWebImage', '~> 3.7.3'  
	end  
	target :'Target_2' do  
	pod 'Masonry', '~> 0.6.3'  
	end  

3. Podfile中指定依赖库版本，具体写法和标识含义如下：  
	pod 'AFNetworking'	//不显式指定依赖库版本，表示每次都获取最新版本  
	pod 'AFNetworking', '2.0'	//只使用2.0版本  
	pod 'AFNetworking', '> 2.0'	//使用高于2.0的版本  
	pod 'AFNetworking', '>= 2.0'	//使用大于或等于2.0的版本  
	pod 'AFNetworking', '< 2.0'	//使用小于2.0的版本  
	pod 'AFNetworking', '<= 2.0'	//使用小于或等于2.0的版本  
	pod 'AFNetworking', '~> 0.1.2'	//使用大于等于0.1.2但小于0.2的版本  
	pod 'AFNetworking', '~>0.1'	//使用大于等于0.1但小于1.0的版本  
	pod 'AFNetworking', '~>0'	//高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本  
	
### 相关命令
1. pod install  
根据Podfile文件指定的内容，安装依赖库，如果有Podfile.lock文件而且对应的Podfile文件未被修改，则会根据Podfile.lock文件指定的版本安装。 
每次更新了Podfile文件时，都需要重新执行该命令，以便重新安装Pods依赖库。   
2. pod update  
若果Podfile中指定的依赖库版本不是写死的，当对应的依赖库有了更新，无论有没有Podfile.lock文件都会去获取Podfile文件描述的允许获取到的最新依赖库版本。  
3. pod search  
该命令是用来按名称搜索可用的Pods依赖库  
4. pod setup  
该命令用于更新本地电脑上的保存的Pods依赖库Tree。  
由于每天有很多人会创建或者更新Pods依赖库，这条命令执行的时候会异常耗时。我们需要经常执行这条命令，否则有新的Pods依赖库时执行pod search命令是搜不出来的。  



