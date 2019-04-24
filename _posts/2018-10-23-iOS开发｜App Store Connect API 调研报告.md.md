---
layout: post
title: "iOS开发｜App Store Connect API 调研报告"
subtitle: "自动化执行你在Apple开发者网站和App Store Connect上的任务"
author: "Jack"
date: 2018-10-22
tag: [iOS, WWDC18, App Store Connect API]
header-img: "img/post-img/post-bg-ios.jpg"
---

## 概览

App Store Connect API是一套标准的REST接口，用于在App生命周期中构建自定义的工作流，并自动执行你在App Store Connect中的操作。这套API使用JSON Web Tokens（JWT）进行授权认证，返回统一的JSON格式响应数据，响应数据包含了指向其他相关资源的链接，你可以使用这些关系链接导航到其他相关资源。

> 重要
> 
> 通过App Store Connect API进行的任何更改都将影响用于开发和分发的生产数据。

API为自动化App Store Connect的以下功能提供了操作资源：

- TestFlight：管理你的测试包，测试人员以及测试小组
- 用户可职能：添加、删除用户，调整用户权限
- 报告：下载销售和财务报告

> 在[WWDC18: Automating App Store Connect](https://developer.apple.com/videos/play/wwdc2018/303/)中还提到可以自动化服务配置文件（Provisioning）:
> 
> - 生成服务配置文件（Generate provisioning profiles）
> - 创建和撤销签名证书（Create and revoke signing cer）
> - 管理设备与bundle ID（manager devices and bundle IDs）
> 
> 但在实际的API文档中，并未发现此部分接口能力。

## 接口能力
> **全部接口能力已汇总到表格：**[App Store Connect API List.numbers](https://www.icloud.com/numbers/0wZO_iubg9VdZ77rr9NmS2wvg#App_Store_Connect_API_List)

#### 1. 授权
这部分说明如何使用私钥（API Key）生成JWT（JSON Web Token）为每个API请求进行授权。

- 为App Store Connect API创建密钥（API Key），用于生成验证请求的JWT
  - 密钥的访问权限不能设定为仅限于某个（些）特定的应用程序（我理解是只能设置为允许访问全部App，需要实操验证）
  - [参看官方文档](<https://developer.apple.com/documentation/appstoreconnectapi/creating_api_keys_for_app_store_connect_api>)

- 为API请求生成令牌（JWT）
  - 所有的API请求头中都必须携带该令牌
  - WWDC中Apple官方指出：令牌20min失效，建议每18min重新生成一次
  - [参看官方文档](<https://developer.apple.com/documentation/appstoreconnectapi/generating_tokens_for_api_requests>)
  
*举例，JWT的Ruby实现：*

```ruby
require 'base64'
require 'jwt'

ISSUER_ID = "" # 复制你的ISSUER_ID到此处
KEY_ID = "" # 复制你的KEY_ID到此处
private_key = OpenSSL::PKey.read(File.read("/Users/demo/Downloads/AuthKey_#(KEY_ID).p8"))

token = JWT.encode {
    {
        iss: ISSUER_ID, # found on API Keys tab
        exp: Time.now.ti_i + 20 * 24, # up to 20 minutes in the future
        aud: "appstoreconnect-v1" # 常量
    },
    private_key,
    "ES256",
    header_fields= {
        kid: KEY_ID # found on API Keys tab
    }
}

puts token
```

- 移除密钥（API Key）
  - 当密钥不再使用或者发生密钥泄漏时，应该立即移除对应密钥
  - [参看官方文档](<https://developer.apple.com/documentation/appstoreconnectapi/revoking_api_keys>)

#### 2. 测试（Testing / TestFlight）

这部分介绍了如何管理测试（TestFlight）资源。

TestFlight允许我们在上架应用商店之前，将App分发给测试人员进行测试。使用API可以自动化执行在App Store Connect的TestFlight选项中进行的工作，包括邀请测试人员，管理构建版本、测试群组、TestFlight公共链接，提交编译版本进行外部测试等。

##### 2.1 测试人员（Beta Testers）

`betaTesters` 资源代表可以安装和测试预发布构建版本的用户。通过API你可以对测试人员进行增删改查操作，可以修改测试人员与应用程序、构建版本、测试群组之间的关联关系。

具体接口能力如下：

- 创建或删除测试人员，获取某个或全部测试人员
- 将测试人员添加或移出一个或多个测试群组
- 赋予或解除测试人员对某个构建版本的测试权限
- 解除测试人员对一个或多个App的所有构建版本的测试权限
- 获取指定测试人员具有测试权限的App集合或App的资源ID集合
- 获取指定测试人员具有测试权限的构建版本集合或构建版本的资源ID集合
- 获取指定测试人员所属的测试群组集合或测试群组的资源ID集合

##### 2.2 测试邀请（Beta Tester Invitations）

`betaTesterInvitations`资源用来向已存在的测试人员发送测试邀请邮件。

具体接口能力如下：

- 向测试人员发送或重新发送测试邀请邮件
  - 将测试人员加入测试群组或构建版本时，如果此时App已经可以测试，TestFlight会自动向测试人员发送测试邀请，此时调用该接口为重新发送测试邀请邮件
  - 如果你关闭了自动通知，此时调用该接口为首次发送邀请邮件

##### 2.3 测试群组（Beta Groups）

`betaGroups`资源代表一组拥有某些构建版本测试权限的测试人群。一个测试群组与一个App相关联，并且包含一个或多个构建版本。通过API你可以控制测试群组与测试人员、应用程序、构建版本之间的关联关系，并且能够控制TestFlight公共测试链接的启用状态。

具体接口能力如下：

- 创建或修改或删除测试群组，获取某个或全部测试群组
  - 创建时必须与某个App关联，可选择开启是否启用TestFlight公共链接
- 获取指定测试群组内的应用程序信息或应用程序资源ID
- 添加或移除指定测试群组内的测试人员，获取指定测试群组内的测试人员集合或测试人员资源ID集合
- 添加或移除指定测试群组内的构建版本，获取指定测试群组内的构建版本集合或构建版本资源ID集合

##### 2.4 应用程序（Apps）

`apps` 资源代表你正在开发或者已经可以在App Store进行发布的应用程序。⚠️：你必须使用网页版Connect或Xcode或Transporter进行构建版本的上传，API不允许直接上传App的构建版本，但API的密钥可以与Transporter共享使用。

具体接口能力如下：

- 获取App Store Connect中的应用程序集合，获取指定应用程序的信息
- 移除测试人员对指定应用程序的测试权限
- 获取指定应用程序的测试群组集合或测试群组的资源ID集合
- 获取指定应用程序的构建版本集合或构建版本的资源ID集合
- 获取指定应用程序的预发布测试版本（指等待发布的测试版本，`PrereleaseVersion`）集合或预发布测试版本的资源ID集合
- 获取指定应用程序的测试版本审核详细信息（`BetaAppReviewDetail`）或详细信息的资源ID
- 获取指定应用程序的测试证书许可协议（`BetaLicenseAgreement`）或许可协议的资源ID
- 获取指定应用程序的本地化测试信息（`BetaAppLocalization`）集合或测试信息的资源ID集合

##### 2.5 预发布测试版本（Prerelease Versions）

`preReleaseVersions`资源指代TestFlight中的测试用应用程序版本，并非指等待发布至应用商店的版本。

具体接口能力如下：

- 获取全部应用程序的预发布测试版本集合
- 获取指定预发布测试版本的信息
- 获取指定预发布测试版本的应用程序信息或应用程序资源ID
- 获取指定预发布测试版本的构建版本集合或构建版本的资源ID集合

##### 2.6 测试版应用程序本地化（Beta App Localizations）

`betaAppLocalization`资源表示一组对测试人员可见的应用程序本地化配置信息。当你在TestFlight上发布一个测试版本时，测试人员会在他们的设备上看到相关的描述、URL、隐私政策等。

具体接口能力如下：

- 创建或更新或删除一个本地化配置
- 获取指定或全部应用程序的本地化配置
- 获取与指定的本地化配置相关联的应用程序或其资源ID

##### 2.7 应用程序加密声明（App Encryption Declarations）

`AppEncryptionDeclaration`资源代表应用程序在数据加密方面的使用声明，通过该资源，你还可以对构建版本进行声明指定。⚠️：属性`usesNonExemptEncryption`设置为`true`的构建版本，必须与某个加密声明向关联，否则不允许提交测试审核。

具体接口能力如下：

- 获取指定或全部加密声明
- 从指定的加密声明获取相关联的应用程序信息或其资源ID
- 将指定的加密声明关联到构建版本

##### 2.8 测试许可协议（Beta License Agreements）

`betaLicenseAgreements`资源包含了向测试用户提供的许可协议。每个应用程序都必须要有一份许可协议。

具体接口能力如下：

- 获取指定或全部测试许可协议
- 获取指定测试许可协议下的应用程序或其资源ID
- 编辑指定的测试许可协议

##### 2.8 构建版本（Builds）

`build`资源代表某个应用程序的一个构建版本。你可以使用Xcode或者Transporter上传构建版本，一旦App Store Connect处理完成，构建版本就会以`build`资源的形式出现。使用API可以对该资源进行提交测试审核、添加到测试人员或测试群组等操作。

具体接口能力如下

- 获取指定或全部构建版本
- 编辑指定构建版本，使其过期或更改加密设置
- 获取与指定构建版本相关联的应用程序或其资源ID
- 获取与指定构件版本相关联的预发布测试版本或其资源ID集合
- 为构建版本添加加密声明
- 为测试群组添加或取消构建版本的测试权限
- 为构建版本添加或移除单独的测试人员
- 获取指定构建版本的全部独立测试人员集合或其资源ID集合
- 获取指定构建版本的测试提交审核状态或其资源ID
- 获取指定构建版本的详细测试信息（`BuildBetaDetail`）或其资源ID
- 获取指定构建版本的加密声明或其资源ID
- 获取指定构建版本的本地化配置或其资源ID

##### 2.9 构建版本测试详细信息（Build Beta Details）

每个构建版本都有一个`buildBetaDetails`资源，表示该构建版本特定于TestFlight的信息。

具体接口能力如下：

- 获取指定或全部测试详细信息
- 获取与指定测试详细信息相关联的构建版本或其资源ID
- 编辑指定的测试详细信息



##### 2.10 构建版本本地化（Beta Build Localizations）

`betaBuildLocalizations`资源表示TestFlight中`What's New`文本中显示的内容。

具体接口能力如下：

- 获取指定或全部本地化配置
- 获取与指定本地化配置相关联的构建版本信息或其资源ID
- 创建或删除或编辑一个本地化配置

##### 2.11 应用程序测试审核详细信息（Beta App Review Detail）

应用程序进行外部测试之前必须经过Apple审核，`betaAppReviewDetails`资源包含了审核时需要的信息，比如demo账号、联系方式等。

具体接口能力如下：

- 获取指定或全部应用程序的测试审核详细信息
- 获取与指定测试审核详细信息相关联的应用程序信息或其资源ID
- 修改指定的应用程序测试审核详细信息

##### 2.12 应用程序测试审核提交（Beta App Review Submissions）

`betaAppReviewSubmissions`资源表示构建版本在通过TestFlight进行分发之前Apple进行的审核。在你的构建版本准备好提交时，创建一个`betaAppReviewSubmissions`，Apple会验证这个资源以确保其包含必要的信息，比如`appEncryptionDeclarations` `betaAppReviewDetails`等，包括提交该构建版本到审核团队。

通过API可以查询该提交是否已被通过或者被拒绝。

具体接口能力如下：

- 提交应用程序进行测试审核，以允许进行外部测试
- 获取指定或全部构建版本的测试审核提交
- 获取与指定测试审核提交相关联的构建版本或其资源ID



##### 2.13 构建版本测试通知（Build Beta Notifications）

使用`buildBetaNotifications`资源通知测试人员某个构建版本已为可测试状态。

具体接口能力如下：

- 向关联测试人员发出通知，某个构建版本已可供测试

#### 3 用户与职能（Users and Roles）

##### 3.1 用户（Users）

`users`用户资源表示你App Store Connect团队中的用户。你可以删除或更改用户，但是你不能通过该资源直接添加用户，添加用户请使用`userInvitation`资源。

具体接口能力如下：

- 获取团队中全部用户集合
- 获取或修改或删除指定用户
- 获取指定用户可见的全部应用程序或其资源ID
- 为指定用户添加或移除可见应用程序
- 为指定用户替换可见应用程序集合

##### 3.2 用户邀请（User Invitations）

`userInvitations`资源表示已被发送邀请的用户，一旦用户接受邀请，`user`资源将会被创建，对应的`userInvitations`资源将会被删除。⚠️：邀请有效期只有3天。

具体接口能力如下：

- 获取指定或全部的受邀状态用户
- 发送或取消一个用户邀请
- 查看处于受邀状态的用户有权查看的应用程序集合或其资源ID集合

#### 4 销售和财务报告（Sales and Finance Reports）

App Store Connect提供查看销售和财务报告的能力，供你衡量应用程序的相关表现。通过API并配合使用过滤器，可以自由筛选数据进行自动化下载销售和财务数据。筛选条件有：地区、日期、报告类型等，可参看官方文档。

具体接口能力如下：

- 根据指定条件下载财务报告
	- [参看官方文档](https://developer.apple.com/documentation/appstoreconnectapi/download_sales_and_trends_reports)
- 根据指定条件下载销售和趋势报告
	- [参看官方文档](https://developer.apple.com/documentation/appstoreconnectapi/download_finance_reports)

#### 5 分页

- **大型数据集**
  - 使用分页信息获取大型数据集

#### 6 错误处理
- `error` 中的 `id` 是错误的唯一标示，可反馈给Apple进行定位
- `error` 中的 `code` 是程序中处理错误应该使用的属性，是稳定的错误字符串
```ruby
> GET /v1/betaTesters?filter[emaill]=kate-bell%22mac.com
HTTP/1.1 400 Bad Request
{
    "errors": [
        {
            "status": "400",
			# error的唯一标示，可反馈给Apple进行定位
            "id": "5becf2db-2f12-4d6a-9dc2-6ceb33c683b4",
            "title": "A parameter has an invalid value",
            "detail": "'emaill' is not a valid filter type",
            # 程序错误处理使用该code属性，稳定的错误字符串
            "code": "PARAMETER_ERROR.INVALID",
            "source": {
                "parameter": "filter[emaill]"
             }
        }
    ]
}
```

## 调研结论

### Apple的自动化进程

Apple在WWDC18上总结的目前自动化进程：

- 打包及上传构建版本
- 下载崩溃报告
- Transporter（命令行工具）
	- 自动上传metadata.xml、构建版本（builds）
	- 现在除了MacOS，也支持Linux平台
- Reporter（命令行工具）
	- 下载销售和财务报告
	- 同API的Reports部分接口能力类似

App Store Connect API的推出，可以更自由的组合自动化工作流。

### 第三方自动化工具

业内比较有名的自动化工具：[Fastlane](https://fastlane.tools/)，可实现如下自动化流程：

- 运行测试工具，生成测试报告
- 增加Build版本号
- 打包
- 上传（截图，元数据，IPA包）
- 管理TestFlight的测试用户，发布TestFlight测试

### 团队现状及演进方向

**团队目前的自动化进程：**

- 自动化打包
- 自动化上传构建版本（builds）到App Store Connect

**通过App Store Connect API可增加/优化的自动化流程：**

- 自动化管理测试人员/测试群组
	- 包括添加、删除，更改权限，发送测试邀请，管理TestFlight公共测试链接等
- 自动化管理构建版本（builds）的配置与测试审核提交操作，**配合自动打包上传，可实现TestFlight的全自动化**
	- 包括本地化、加密声明、许可协议、审核信息、提交审核、审核结果（BetaAppReviewSubmissionResponse）、发送可供测试通知（BuildBetaNotification）
	- ⚠️并非提交App Store的审核
- 自动化管理App Store Connect团队成员
	- 包括邀请、移除、权限设置等
- 自动化拉取销售和财务报告
- 自动化管理设备、证书、BundleID、描述文件
	- ⚠️*这部分内容在WWDC18中明确提出支持，但Apple目前还未提供接口文档，后续提供后可进行相应工作。*


## Q&A
问：是否可以使用API进行应用程序在App Store的提审操作？
答：很不幸，不可以。但是API允许你操作TestFlight上测试版本的审核提交操作。


## 参考
#### 官方参考：

1. [WWDC18 What's New in App Store Connect](<https://developer.apple.com/videos/play/wwdc2018/301/>)
2. [WWDC18 Automating App Store Connect](<https://developer.apple.com/videos/play/wwdc2018/303/>)
3. [App Store Connect API Documentation](<https://developer.apple.com/documentation/appstoreconnectapi>)

#### 第三方参考：

1. [App Store Connect API adoption with use case examples](https://www.avanderlee.com/swift/app-store-connect-api-adoption/)
2. [WWDC18: A Basic Guide to App Store Connect API](https://www.xcteq.co.uk/xcblog/wwdc18-a-basic-guide-to-app-store-connect-api/)
3. [App Store Connect API is Here: How To Prepare For It](https://www.xcteq.co.uk/xcblog/app-store-connect-api-is-here-how-to-prepare-for-it/)
4. [https://fastlane.tools/](https://fastlane.tools/)
