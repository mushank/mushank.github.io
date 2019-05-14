---
layout: post
title: "iOS开发｜WWDC18 Automating App Store Connect"
subtitle: "WWDC2018 - App Store Connect API"
author: "Jack"
date: 2018-10-21
tag: [iOS, WWDC2018, App Store Connect API]
header-img: "img/post-img/post-bg-ios.jpg"
---

[官方视频请点此处](https://developer.apple.com/videos/play/wwdc2018/303/)

### 概述

目前的自动化进度

- Xcode上传构建版本
- 下载崩溃报告
- Transporter，命令行工具，自动上传metadata.xml、构建版本
- Reporter，命令行工具，下载销售和财务报告

现在推出更全面的App Store Connect API，让你更自由的组合工作流。一套标准的REST API，附带JSON响应，JWT确保安全。

### API包含的内容

- TestFlight
  - 管理测试人员和群组
  - 提交审核
  - 管理公共链接

- 用户和职能
  - 增加和删除用户
  - 授权职能
  - 管理App权限

- 服务配置
  - 添加开发设备
  - 注册Bundle ID
  - 创建证书
  - 管理描述文件

- 报告
  - 下载销售和财务报告

- Transporter更新
  - 支持Linux
  - 支持API Token认证

### API使用介绍

- 获取数据（获取源：GET）

```ruby
# 举例：获取用户信息

# 1. API基地址
api.appstoreconnect.apple.com

# 2. 添加API版本号（当前为v1）
api.appstoreconnect.apple.com/v1

# 3. 添加源类型名称（resource type name，以user来举例）
# 代表团队中的所有用户，返回一个JSON对象
> GET api.appstoreconnect.apple.com/v1/users 

HTTP/1.1 200 OK
{
    "data": [{
        "type": "users",
        "id": "17cbd794-94a3-c7b0-1051",
        "attributes"; {
            "firstName": "Kate",
            "lastName": "Bell",
            "email": "kate-bell@apple.com",
            ...
        },
        "relationships": {...},
        "links": {
            "self": "https://api.appstoreconnect.apple.com/v1/users/17cbd794-94a3-c7b0-1051"
        }
    },
    {
        ...
    }]
}
# 其中links字段内的self字段，标示当前用户数据的地址（即：源），可通过该源获取该用户的数据。
```

- 更新数据（创建源：POST，更新源：PATCH，删除源：DELETE）

```
# 举例：添加一个新用户

# 首先创建一个邀请源
> POST /v1/userInvitations
{
    "data": {
		"type": "userInvitations",
		"attributes": {
            "firstName": "John",
            "lastName": "Appleseed",
            "email": "john-appleseed@apple.com",
            "roles": ["DEVELOPER"],
            "allAppsVisible": "true"
		}
    }
}

HTTP/1.1 201 CREATED
{
    "data": {
        "type": "userInvitations",
        "id": "24cbd794-94a3-c7b0-1051",
        "attributes"; {
            "firstName": "John",
            "lastName": "Appleseed",
            "email": "john-appleseed@apple.com",
			"roles": ["DEVELOPER"],
			"allAppsVisible": true,
			"expirationDate"; "2019-05-21T13:13:00"
        },
        "links": {
            "self": "https://api.appstoreconnect.apple.com/v1/userInvitations/24cbd794-94a3-c7b0-1051"
        }
    }
}

# 举例：更新一个用户，添加市场职能

# 修改源自属性
> PATCH /vi/users/24cbd794-94a3-c7b0-1051
{
    "data": {
        "type": "users",
        "id": "24cbd794-94a3-c7b0-1051",
        "attributes"; {
			"roles": ["DEVELOPER", "MARKETING"],
        }
    }
}

HTTP/1.1 200 OK
{
    "data": {
        "type": "users",
        "id": "24cbd794-94a3-c7b0-1051",
        "attributes"; {
            "firstName": "John",
            "lastName": "Appleseed",
            "email": "john-appleseed@apple.com",
			"roles": ["DEVELOPER", "MARKETING"],
			"allAppsVisible": true,
        },
        "links": {
            "self": "https://api.appstoreconnect.apple.com/v1/userInvitations/24cbd794-94a3-c7b0-1051"
        }
    }
}

# 举例：删除一个用户
> DELETE /vi/users/24cbd794-94a3-c7b0-1051
HTTP/1.1 204 NO CONTENT
```

**示例场景：某位员工离职，需要删除该用户**

```ruby
# 通过GET请求访问用户源（users），获取全部用户
GET /v1/users
...

# 通过邮箱筛选该离职用户
GET /v1/users?filter[email]=john-appleseed@apple.com
...

# 使用该用户ID获取用户实例（返回用户实例，并附带ID）
GET /v1/users/24cbd794-94a3-c7b0-1051
...

# 删除用户
DELETE /v1/user/24cbd794-94a3-c7b0-1051
HTTP/1.1 204 NO CONTENT

# 验证是否被删除，继续向该用户源发送GET请求
GET /v1/users/24cbd794-94a3-c7b0-1051
HTTP/1.1 404 NOT FOUND
```

- 各API之间的关系（如何协同合作）

```ruby
BetaGroups # 测试小组
BetaTesters # 测试人员

# 举例：将测试人员加入测试小组

# 获取所有测试小组
> GET /v1/betaGroups
HTTP/1.1 200 OK
{
    "data": [{
        "type":"betaGroups",
        "id": "55555555-1111-2323-1231",
        "attributes": {...}
        "relationships": {
            "app": {...},
            "betaTesters": {
                "links": {
                    # ⚠️：关系自链接，增加关系时被使用
                    "self": "/v1/betaGroups/55555555-1111-2323-1231/relationships/betaTesters", 
                    # ⚠️：相关链接，实际相关数据
                    "related": "/v1/betaGroups/55555555-1111-2323-1231/beteTesters" 
                }
            },
            "builds": {...}
        },
        "links": {
            "self": "/v1/betaGroups/55555555-1111-2323-1231"
        }
    },
    {
       ... 
    }]
}

# 添加关系（⚠️：使用关系自链接）
> POST /v1/betaGroups/55555555-1111-2323-1231/relationships/betaTesters
{
    "data": [
    	{
            "type": "betaTesters",
            "id": "41234123-4354-1235-1233",
    	},
    	{
            "type": "betaTesters",
            "id": "61234124-4354-1235-1233",
    	}
    ]
}

# 查看实际关联数据，此处指本测试小组内关联的测试人员（⚠️：使用相关链接）
> GET /v1/betaGroups/55555555-1111-2323-1231/betaTesters
...


# 举例：批量获取不同小组中的测试人员

# 使用包含参数（include）
> GET v1/betaGroups?include=betaTesters
...
# 此处返回的betaTesters字段中含有data字段，包含小组内测试人员信息
# respose底部有include字段部分（与最外层data字段平级），包含所有测试人员的详细信息（将测试人员的详细信息单独放于一个地方是处于节约数据传输）
```

**示例场景**

```ruby
# 添加测试小组
POST /v1/betaGroups
{
	"data": "betaGroups",
	"attributes": {
        # 创建测试小组时所需名称信息
        "name": "Test Group"
	},
    # 如果不加relationships进行post请求，返回 HTTP/1.1 409 CONFLICT
	"relationships": { 
        "app": {
            "data": {
                "type": "apps",
                # 创建测试小组，并且关联到id为1002020023的app
                "id": "1002020023" 
            }
        }
	}
}

HTTP/1.1 201 CREATED
{
    "data": {
        "type": "betaGroups",
        "id": "asdjfajwer-aefa-adfa-rqer",
        "attributes": {
            ...
        },
        "relationships": {
            ...
        }
    }
}

# 修改小组信息
PATCH /v1/betaGroups/asdjfajwer-aefa-adfa-rqer
{
    "data": {
		"type": "betaGroups",
		"id": "asdjfajwer-aefa-adfa-rqer",
		"attribute": {
            # 需要修改的名称
            "name": "WWDC Group" 
		}
    }
}

HTTP/1.1 200 OK
{
	...
}

# 添加测试人员
POST v1/betaTester
{
    "data": "betaTester",
    "attributes": {
        "firstName": "Kate",
        "lastName": "Bell",
        "email": "kate-bell@apple.com"
    }
	"relationships": {
        "betaGroups": {
            "data": [
                {
                    "type": "betaGroups",
                    "id": "asdjfajwer-aefa-adfa-rqer"
                }
            ]
        }
	}
}

HTTP/1.1 201 CREATED
{
    ...
}

# 查看测试小组中的测试人员
GET /v1/betaGroups/asdjfajwer-aefa-adfa-rqer/betaTesters
HTTP/1.1 200 OK

# 筛选查看字段，指定查看emai
GET /v1/betaGroups/asdjfajwer-aefa-adfa-rqer/betaTesters?fields[betaTesters]=email
HTTP/1.1 200 OK
```

- 处理错误

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

- 授权与认证

1. 创建API Key
2. 创建Token
   1. Issuer ID：发型商ID
   2. Key ID：API Key对应的ID
   3. 过期时间：20min过期
   4. Audience，常量，固定为：appstoreconnect-v1
   5. algorithm，算法，ES256

JWT提供了实现该令牌算法的函数库，Ruby举例：

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

3. 请求中携带Token

```ruby
# 放入Header "Authorization: Bearer <token_value>"
$ curl  https://api.appstoreconnect.apple.com/v1/users --Header "Authorization: Bearer lOOOOOOOOOOOONG_GENERATED_TOKEN"
```

- 最佳实践

1. 保护好你的private key，如果发生泄漏，立即revoke
2. 重用token（官方建议18min更换一次）
3. 使用links（自链接，多步骤操作时，下一步骤中尽可能提取上一步骤中返回的link）
4. 使用API Doc











