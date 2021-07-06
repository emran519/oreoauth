# 概要

## Oreo授权系统(守护您的原创代码) 1.1 使用说明

作者：oreo
QQ：609451870
交流QQ群：995322283
编辑时间:2021/6/26

------

> **本文中将介绍Oreo授权系统 V1.1 主站的安装方法以及详细的介绍，OreoAuth类的对接方法，引导你更快速的对接到自己的项目当中。**

**摘  要：**

软件程序的授权是软件程序保护原创性概念的延伸以及发展。软件系统的授权目的是允许软件使用用户按照购买的开发者允许的许可范围内使用软件程序。系统主要由以下几个模块组成：授权设置模块，授权验证规则模块，客户模块，授权管理模块，发布授权用户站点中通知模块，版本管理模块，API设置模块以及其他设置。其中，授权设置模块模块作为授权的重点，验证规则围绕以授权的程序进行规则验证和版本控制，客户模块作为用户绑定所购买的授权产品，授权管理模块则是管理员可以随时可以根据情况来切换当前客户的授权信息，发布通知则是支持给目标程序发送消息。验证器则采用了中间件形式开发，当收到验证信息则优先走中间件进行相关规则的判断，若有验证失败则及时中断后续操作，方可避免额外的性能支出，系统体验也得到了想当的优化。系统还提供了一套前后端分离式开发的用户端界面，管理员可以安装部署到任何一个服务器当中，简单配置跨域设置即可连接至后端系统。用户界面方便用户快速注册和在线购买开发者设定好的程序以及API接口。除了在线购买功能之外还提供了订单管理和在线管理授权程序和API接口等信息。基于B/S架构的域名IP授权系统主要应用了PHP技术，采用了B/S结构，MySQL8.0以及Redis为后台数据库，用户端采用Bootstrap以及JavaScript作为开发框架。

# 一、系统介绍

## 1.概述

### 1.1 开发背景
拒绝盗版软件、保护程序的原创性是基于B/S架构的域名IP授权系统的开发初衷。软件程序的授权是软件程序保护原创性概念的延伸以及发展。软件系统的授权目的是允许软件使用用户按照购买的开发者允许的许可范围内使用软件程序。它涉及到了软件程序安装的数量、使用时间，应用范围和软件程序的在线升级以及功能模块等内容。对于软件系统保护来说，核心的概念是防止软件程序被盗版以及恶意泛滥。

软件程序保护的概念是从开发者的角度出发的，它强调使用基于软件程序的加密的技术手段来保护软件不易被破解或增加代码阅读能力以及降低被恶意二次开发等。理论上，只要你有足够的能力和时间以及资源，所有的软件程序保护技术都可以被破解。然而，如果有一种软件程序保护技术的安全强度达到了让软件程序破解者付出了比购买软件程序还要高的成本，这种软件程序保护技术就是成功的，值得使用的方法。

### 1.2 开发意义
该系统也考虑到程序语言的多样性，简易了对接的难度，任何一种语言开发的程序也能简单的post请求来完成与本系统的对接服务，大大减少程序员为多种程序的管理和升级烦恼。
再也不用为多套程序定制多个授权系统的设计而烦恼，该系统可消除开发者和企业为多套程序设置多种授权系统的设计而增加成本，现在可以一套授权系统来完成全部程序的一站式控制操作。
该系统拥有完整的对客户域名和服务器IP的监听和与本系统的记录校验功能，如有发现盗版系统，能及时提示设定的提示语，且阻断程序后续操作并且即使对本系统反馈盗版程序安装的域名和服务器ip信息，同时保证对正版用户的售后服务，正版用户将能得到云端获取最新版本。
本系统是为了方便开发者和企业程序上线后，控制盗版泛滥以及保护程序原创，作出了一定的控制作用，并且对正版用户便利了在线升级的功能，很大的方便了开发者和企业额外研发开销，从而创造低成本高回报的经济效益。

## 2.系统分析
在软件程序需求分析过程中，对规划阶段初步确定了软件使用范围进行分析和细化，并对各软件程序模块功能进行初步的规划。一个完整的软件需求描述的是一个软件程序开发项目得以成功的基础。无论设计多么精细，编码多么巧妙，如果软件需求没有明确定义，可能会让用户感到不好用或者失望的感觉，这样对于开发者来说是很糟糕的。

### **2.1系统实现的功能**

基于B/S架构的域名IP授权系统主要实现了对于用户的授权管理(授权域名，IP，授权的API)管理，精确获取用户的域名和IP地址等信息，根据管理员的设定验证域名IP，完成管理员预设好的规则进行相关验证以及授予更新权限，达到一个产品保护和产品迭代效应。
基于B/S架构的域名IP授权系统的设计实现了更多个性化设置，管理员全程通过面板来管理，控制，监控等方式获取授权用户的相关信息，本系统还额外提供了便捷的SDK对接包，系统管理员可以快速的通过SDK包来对接自己想要授权销售的系统，对接完成可以通过测试工具验证对接效果。本系统还拥有API销售模块，系统管理员可以额外对API接口进行销售和统计。

## 3.系统的功能需求

基于B/S架构的域名IP授权系统可以分为四大部分：一是授权设置模块，包含授权程序设置，授权系统升级，授权系统规则设置；二是授权API设置模块，包含授权API设置，授权API套餐设置；三是系统设置模块，包含网站信息，文件设置，短信/邮件设置，授权错误提示设置，四是授权域名IP管理，用户管理，API授权管理；五是其他相关安全设置模块，包含管理员设置以及谷歌二步验证。
授权设置模块：授权程序设置、授权系统升级、授权系统规则设置三个子模块。
授权程序设置子模块：该子模块主要用于添加被授权的系统信息。
授权系统升级子模块：该子模块主要用于对应的系统发布新版本。
授权系统规则子模块：该子模块主要用于对应被授权系统的验证规则的设置，包括(双重验证，域名验证方式以及到期时间的验证)。
授权API设置模块包括：授权API设置、授权API套餐设置两个子模块。
授权API设置子模块：该子模块主要用于添加新的API，可以设置请求方式(post/get)。
授权API套餐子模块：该子模块主要用于套餐类型的设置(免费/收费/永久许可)和接口频率限制以及套餐费用的设置。
授权API设置模块包括：网站信息，文件设置，短信/邮件设置，授权错误提示设置四个个子模块。
网站信息子模块：该子模块主要用于系统的基本配置信息。
文件设置子模块：该子模块主要用于更新包的存储方式的设置，这包括可以选择本地存储和腾讯云OCS存储。
短信/邮件设置子模块：该子模块主要用于用户安全中绑定手机或邮箱来获取验证码，该子模块支持了发邮件，腾讯云短信，阿里云短信。
授权错误提示设置子模块：该子模块主要用于如果用户当前域名IP未授权则出现授权失败和提示，提示来自此模块，其中包含(未授权提示，IP不正确错误提示，授权到期提示，不是授权的系统提示)等设置。
其他相关安全设置模块包括：管理员设置、谷歌二步验证设置模块。
管理员设置子模块：该子模块主要用于当前后台管理员帐号密码等相关信息的配置。
谷歌二步验证子模块：该子模块主要用于后台安全认证。
![](https://git.kancloud.cn/repos/emran519/oreo_auth/raw/a28615ebfd2f32558f797e56b17183de1d277d88/images/%E5%9B%BE%E7%89%871.png?access-token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MjU1NTA4ODIsImlhdCI6MTYyNTUwNzY4MiwicmVwb3NpdG9yeSI6ImVtcmFuNTE5XC9vcmVvX2F1dGgiLCJ1c2VyIjp7InVzZXJuYW1lIjoiZW1yYW41MTkiLCJuYW1lIjoiT3JlbyIsImVtYWlsIjoiNjA5NDUxODcwQHFxLmNvbSIsInRva2VuIjoiZWQ0ZjVhZGFjZDk1ZTQxMzJlMTU0MTVlNmI3ODkzMGQiLCJhdXRob3JpemUiOnsicHVsbCI6dHJ1ZSwicHVzaCI6dHJ1ZSwiYWRtaW4iOnRydWV9fX0.9Ydq9fuLZBu3iLUAyO6kmH5klyXk5RJJICuzX7YdtCA)

图3-0 系统基本架构图

## 4.系统特色

灵活的系统授权设置，可无限次添加被授权系统且不会因开发语言不同而受限制，满足绝大部分的授权需求，

完善的授权验证模块，可以自定义各种验证场景，其中双重验证包括：域名验证，IP验证以及双重验证功能。域名验证方式包括：单域名验证，WWW和根域名验证以及泛域名验证，还能额外设置授权到期时间，大幅度满足了企业和开发者的需求。

巧妙设置了API授权机制，平台除了能完成系统的授权以外还可以自定义销售API接口，并且提供了多种销售模式，其中包了免费，按量收费和永久许可，当然这些都可以单独设置销售价格并且提供了接口压力缓解加入了调用频率检测功能。

授权产品迭代方便又高效，系统提供更新包的本地上传和云OSS上传，企业和开发者可以灵活选择使用。

功能丰富的用户管理模块提供用户基本信息操作之外还能设置用户的内测权限。

本系统注重程序的安全，在系统后台增加了谷歌二步验证功能，使用者可以通过扫二维码绑定到个人二步验证器，可以达到步步操作皆可得到认证。

## 5.数据流图

数据流程图简称DFD，它普遍应用于企业的管理系统中，是一种结构化系统分析工具。它从数据传递和加工角度，以图形方式来表达系统的逻辑功能、数据在系统内部的逻辑流向和逻辑变换过程，是结构化系统分析方法的主要表达工具及用于表示软件模型的一种图示方法

通过对基于B/S架构的域名IP授权系统数据流图的描述,可以进一步明确业务流程及相关数据的流动转换,同时为下一步进行系统设计奠定基础。

基于B/S架构的域名IP授权系统的主要系统模型（图2-1 授权系统0层数据流图），它初步描述了这个系统的数据变换过程。总管理员拥有最高权限以及管理和设置授权系统(API)和用户授权信息(API)、D2则是普通用户，即表示用户的授权过的程序(API),D2在此过程中会通过验证器进行基于B/S架构的域名IP授权系统的一系列相关验证并且得到响应。

整个过程是一个严格控制权限的封闭式环境，验证器会严格按照规则来判断合法性，验证不通过则会根据系统设置直接返回对应的响应，通过则允许用户正常使用被授权的系统以及提供在线更新系统的服务。管理员可以随时更改系统的一切相关配置和用户的信息，授权API以上同理但免费API接口(对外开放)不受验证器限制。

![](https://git.kancloud.cn/repos/emran519/oreo_auth/raw/a28615ebfd2f32558f797e56b17183de1d277d88/images/%E5%9B%BE%E7%89%872.png?access-token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MjU1NTA4ODIsImlhdCI6MTYyNTUwNzY4MiwicmVwb3NpdG9yeSI6ImVtcmFuNTE5XC9vcmVvX2F1dGgiLCJ1c2VyIjp7InVzZXJuYW1lIjoiZW1yYW41MTkiLCJuYW1lIjoiT3JlbyIsImVtYWlsIjoiNjA5NDUxODcwQHFxLmNvbSIsInRva2VuIjoiZWQ0ZjVhZGFjZDk1ZTQxMzJlMTU0MTVlNmI3ODkzMGQiLCJhdXRob3JpemUiOnsicHVsbCI6dHJ1ZSwicHVzaCI6dHJ1ZSwiYWRtaW4iOnRydWV9fX0.9Ydq9fuLZBu3iLUAyO6kmH5klyXk5RJJICuzX7YdtCA)

 图5-1 授权系统0层数据流图

![](https://git.kancloud.cn/repos/emran519/oreo_auth/raw/a28615ebfd2f32558f797e56b17183de1d277d88/images/%E5%9B%BE%E7%89%873.png?access-token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MjU1NTA4ODIsImlhdCI6MTYyNTUwNzY4MiwicmVwb3NpdG9yeSI6ImVtcmFuNTE5XC9vcmVvX2F1dGgiLCJ1c2VyIjp7InVzZXJuYW1lIjoiZW1yYW41MTkiLCJuYW1lIjoiT3JlbyIsImVtYWlsIjoiNjA5NDUxODcwQHFxLmNvbSIsInRva2VuIjoiZWQ0ZjVhZGFjZDk1ZTQxMzJlMTU0MTVlNmI3ODkzMGQiLCJhdXRob3JpemUiOnsicHVsbCI6dHJ1ZSwicHVzaCI6dHJ1ZSwiYWRtaW4iOnRydWV9fX0.9Ydq9fuLZBu3iLUAyO6kmH5klyXk5RJJICuzX7YdtCA)

 图5-2 细化后的的管理员用户数据流图

![](https://git.kancloud.cn/repos/emran519/oreo_auth/raw/a28615ebfd2f32558f797e56b17183de1d277d88/images/%E5%9B%BE%E7%89%874.png?access-token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MjU1NTA4ODIsImlhdCI6MTYyNTUwNzY4MiwicmVwb3NpdG9yeSI6ImVtcmFuNTE5XC9vcmVvX2F1dGgiLCJ1c2VyIjp7InVzZXJuYW1lIjoiZW1yYW41MTkiLCJuYW1lIjoiT3JlbyIsImVtYWlsIjoiNjA5NDUxODcwQHFxLmNvbSIsInRva2VuIjoiZWQ0ZjVhZGFjZDk1ZTQxMzJlMTU0MTVlNmI3ODkzMGQiLCJhdXRob3JpemUiOnsicHVsbCI6dHJ1ZSwicHVzaCI6dHJ1ZSwiYWRtaW4iOnRydWV9fX0.9Ydq9fuLZBu3iLUAyO6kmH5klyXk5RJJICuzX7YdtCA)

 图5-3 细化的用户授权认证数据流图
# 二、关于系统

**本系统分为两个端：**

**1.系统后端**

**2.用户端**

## 1.管理员端

### 开发环境

```
- PHP 7.4 （基于高性能的ThinkPHP 6.0.4框架 , PHP扩展fileinfo,redis,sg11）
- MYSQL 8.0
- Redis
- Nginx
*注: PHP版本一定是7.4.X ,必须安装SG11扩展, Mysql版本不能低于5.7，建议上8.0
```

### 安装过程

1.从Oreo官方下载 **Oreo授权系统 V1.0.zip** 的压缩包，上传至你的项目根目录解压，配置你的Apache或Nginx，运行目录设置为 /public。

配置伪静态规则（Nginx）：

```nginx
location / {
	if (!-e $request_filename){
		rewrite  ^(.*)$  /index.php?s=$1  last;   break;
	}
}
```

配置伪静态规则（Apache）：

```Apache
<IfModule mod_rewrite.c>
  Options +FollowSymlinks -Multiviews
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]
</IfModule>
```

2.完成以上配置后你可以浏览器中直接打开项目的域名，如果是首次安装将会自动跳转至安装向导页面，如果没有跳转到安装页面请自行在 域名/install 来打开安装向导。

> 如果项目域名未能打开，请先考虑存在的问题，如: 域名是否已经解析，环境是否配置成功等

------

## 2.用户端


用户端采用前后端分离方式开发，仅需部署HTML代码即可完成打开用户端。

配置伪静态规则（Nginx）：

```nginx
ssi on;
ssi_silent_errors on;
ssi_types text/shtml;
```

配置 JS文件，位于：assets/js/oreo.js 第一行：

```javascript
let domain = '你的管理员端域名';
如: 
let domain = 'https://auth.oreo.com/';
```
## 3.其他

我们强烈建议 前后端都强制设置SSL证书验证。开启HTTPS服务。

前后端分离产生CORS跨域情况。请确保在后端系统参数中的**【后台地址】**和**【用户端地址】**填写正确的域名。请注意域名协议。
> 后台地址填写格式为：协议+域名
> 如：https://auth.2free.cn （注意 最后面不要写入 斜杠 /）

如果您使用了CDN加速服务，则可能需要配置CDN服务商提供的HTTP响应头配置。注意：此功能仅限于您配置后端CDN时使用，如果仅设置了用户端CDN则无需配置。

# 三、关于SDK包

> 请前往Oreo官网下载 SDK1.0.zip 包，官网提供原生PHP对接包和ThinkPHP对接包。

## 1.原生PHP对接包

**目录结构**

```
oreosoft             （授权验证目录）
├─demo                实例文件夹(参考作用，可删除)
│  ├─api              授权接口演示文件
│  ├─complete-demo    基于原生开发的简单的全方面对接演示程序（包括授权检测和升级模块以及一个简单的SQL文件）
│  ├─md5              获取文件MD5值
│  ├─auth             授权检测类实列文件夹   
│  ├─update           更新检测类实列文件夹      
│  ├─.oreo            oreo配置文件（主要配置版本和版权信息,不可删除）
├─src                 授权核心文件夹(类文件) 
│  ├─Env.php         
│  ├─OreoAuth.php     需要引入的OreoAuth类
├─.oreo               oreo配置文件（主要配置版本和版权信息,不可删除）
```

**.oreo文件结构**

```env
[APP]
DEFAULT_TIMEZONE = Asia/Shanghai //时区
COPYRIGHT = OreoAuth Server authorization system
[VERSION]
NUM = 1.0.0 //版本号,每次升级需要更改此处发布即可
```

> **请把.oreo文件放入项目根目录下**

## 2.ThinkPHP对接包

**目录结构**

```
OreoSoft             （授权验证目录）
├─demo                实例文件夹(参考作用，可删除)
│  ├─complete-demo    基于原生开发的简单的全方面对接演示程序（包括授权检测和升级模块以及一个简单的SQL文件）
│  ├─md5              获取文件MD5值
│  ├─auth             授权检测类实列文件夹   
│  ├─update           更新检测类实列文件夹      
│  ├─.oreo            oreo配置文件（主要配置版本和版权信息,不可删除）
├─src                 核心文件夹(不可删除) 
│  ├─auth
│  │  ├─OreoAuth.sql  需要引入的OreoAuth类
│  ├─Base.php          
├─.env               env配置文件（主要配置版本和版权信息,不可删除）
```

------

# 四、接口与返回结果

> 对接相应接口之前，你需要先了解接口返回的数据。

## 接口说明

|         接口地址         |     说明     | 返回类型 |                         备注                          |
| :----------------------: | :----------: | :------: | :---------------------------------------------------: |
|  /oreo/api/checkDomain   |   授权检测   |   json   |                                                       |
|  /oreo/api/checkUpdate   | 最新版本查询 |   json   | data内返回:最新版本号，更新内容，更新时间，包地址信息 |
| /oreo/api/checkUpdateLog | 上报更新结果 |   json   |                   一般无需单独处理                    |

## 返回数据说明

| 键名 | 名称 |  类型  |                     说明                      |
| :--: | :--: | :----: | :-------------------------------------------: |
| code | 编码 |  int   | 4001=>授权错误;200=>授权正常;4002=>暂无新版本 |
| msg  | 内容 | string |                返回的内容提示                 |
| data | 数据 | array  |                相应接口的数据                 |

------

# 五、代码引入指南

## 授权验证

在你的项目全局文件中头部引入

```php
include_once('oreosoft/src/OreoAuth.php');
```

在代码开始前你可以选择做该文件的MD5校验（MD5可以在Oreo授权官方可以在线获得，SDK包内也包含）

```PHP
if(md5_file('../src/OreoAuth.php')!='该文件的MD5值')exit('安全校验失败');
```

数据库获取方法代码段下方开始初始化Oreo类

```php
//实例化OreoAuth类
$oreoAuth = new OreoAuth();
$oreoAuth->loadFile('../.oreo');//请把.oreo放入项目根目录
$authParam = array(
    'domain'   => '', //当前域名 (必)
    'sysKey'   => '',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    'version' => $oreoAuth->get('version.num'), //系统当前版本,.oreo文件中获取 (必填)
    'authKey' =>  '', //填写域名授权后生成的授权码,我们建议从数据库中获取，您可以在用户安装的时候写进数据库(必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => '',//数据库地址,如果不需要请填写2
    'isSqlDataBase' => '',//数据库库名,如果不需要请填写2
    'isSqlUserName' => '',//数据库账号,如果不需要请填写2
    'isSqlPassword' => '',//数据库密码,如果不需要请填写2
    'isSqlHostPort' => '',//数据库端口,如果不需要请填写2
);
//版本号获取方法
//$oreoAuth->get('version.no');
```

可在全局文件中直接进行授权验证(也可以在每个文件设置验证,这样安全性也会得到提升)

```php
$oreoAuth->post($authParam)->url('http://你的域名/oreo/api/checkDomain'); //这里必须要设置正确的协议头，http://或https:// 
```

> 这里的 /oreo/api/checkDomain 是检查授权的接口
>
> 文档前面部分我们已经说明了所有接口，你可以根据需求来定义接口

返回结果

```php
if (!$oreoAuth->error()) { //如果没有发生错误
    $oreoContent = $oreoAuth->data();//返回结果
}else{ //则
    exit($oreoAuth->error());//输出错误
}
```

如需检测的文件或全局文件中

```php
//在我需要验证的页面或者全局验证(授权验证)
if(empty($oreoContent)){ //如果返回结果为Null
    exit('授权检测失败，请联系作者');//输出本地错误,可以自定义
}else if ($oreoContent['code'] == 4001) { //如果返回结果为未授权
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出授权站设置的错误内容
}
```
## 更新检测

> 如果你已经把OreoAuth验证类加载到你的全局（核心）文件当中，那么你只需要在相应需要检测更新的页面文件中加入一下代码即可完成更新检测和更新下载的功能了。

```php
$oreoAuth->post($authParam)->url('http://你的域名/oreo/api/checkUpdate'); //这里必须要设置正确的协议头，http://或https:// 
```

```php
if (!$oreoAuth->error()) { //如果没有发生错误
    $oreoContent = $oreoAuth->data();//返回结果
}else{ //则
    exit($oreoAuth->error());//输出错误
}
```

```php
if(empty($oreoContent)){ //如果返回结果为Null
    exit('授权检测失败，请联系作者');//输出本地错误,可以自定义
}else if ($oreoContent['code'] == 4001) { //如果返回结果为未授权
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出授权站设置的错误内容
}
```

请求成功，你可以打印返回的数据，了解返回的内容。

```php
var_dump($oreoContent);
//返回内容
//如果没有新的版本
array(2) { 
    ["code"]=> int(4002) 
    ["msg"]=> string(18) "暂无最新版本" 
} 
//如果有新的版本
array(3) { 
    ["code"]=> int(200) 
    ["msg"]=> string(15) "有最新版本" 
    ["data"]=> array(4) { 
        ["verNo"]=> string(4) "1.01" 
        ["verAlpha"]=> int(1) "1" 
        ["verText"]=> string(10) "测试1.01" 
        ["verDate"]=> string(19) "2020-10-20 14:35:41"
        ["verUrl"]=> string(87) "http://你的域名/storage/update/zip/20201020/6f3914c856f6d3733ad74e45f17823d2.zip"
    } 
} 
```

那么，你该怎么处理这些信息，下面简单的几行代码就可以解决你的难题

```php
if($oreoContent['code']==4002){
    //这里是没有新版本的情况下的逻辑代码
}else{
    //这里是有新版本的逻辑代码
    $verNo    = $oreoContent['data']['verNo']; //最新版本
    $verAlpha = $oreoContent['data']['verAlpha'];//1=>正式版;2=>内测版
    $verText  = $oreoContent['data']['verText'];//更新内容
    $verDate  = $oreoContent['data']['verDate'];//发布时间
    $verUrl   = $oreoContent['data']['verUrl'];//包地址,此变量不要输出到你的html页面中
}
//而后你可以把以上代码直接引用到你的php代码或html代码当中
```

发现有新版本提示了，那么你该如何下载和安装更新的zip包，在说明这个问题之前，你需要了解更新包的制作过程

## 更新包的结构

```
//更新包不要再嵌套其他文件夹中(选中所有文件再点击压缩而不是返回上一个目录压缩)
├─temp                更新包默认下载地址
│  ├─update           更新包默认下载地址
│  ├─sql              数据库文件夹 （如果本次更新无需升级数据库，无需创建此文件夹）
│  │  ├─update.sql    更新SQL文件       
├─xx1                 你的需要更新的文件夹 （根据更新需求对应目录创建文件夹）
│  ├─xx.php           你的需要更新的代码    
├─xx2                 你的需要更新的文件夹 
│  ├─xx.html          你的需要更新的代码    
├─xx3                 以此类推
├─.oreo               (配置文件，不可删除，每次更新请修改里面的NUM值为更新的版本)
```

好了，你已经学会了更新包的结构了，回到话题怎么下载更新包，我们也提供了下载更新包的接口，使用方法如下

```php
//当有新的版本提示，原则上你的网站会有一个（更新或下载）按钮，点击按钮系统自动下载并且安装好后返回结果
//一般情况下你的更新页面需要通过url的get方法来检测加载相应的页面操作，具体的方法我们会在实例Demo中展示，你也可以直接参考Demo文件的update
$outVersion = $authParam['version']; //之前版本
$updatedir = '../temp/update/';//设置下载目录，建议直接在全局（核心）文件中设置;请不要设置在当前目录，应当在根目录下创建文件夹最合适
$fileName = $oreoAuth->save($oreoContent['data']['verUrl'],$updatedir);//下载文件
$updateZip = $updatedir.$fileName;//下载的文件目录和名称
//初始化PHPZIP
$zip = new ZipArchive;
$res = $zip->open($updateZip);
if ($res === true) {
    $zip->extractTo('../');
    $zip->close();
    $sqlfile = '../temp/sql/update.sql';
    $sql = @file_get_contents($sqlfile);
    if ($sql) {
        error_reporting(0);
        foreach (explode(";[\r\n]+", $sql) as $v) {
            //@mysql_query($v);
            $DB->query($v)->fetch(); //这个Sql导入语句根据您的代码来定义,当然我们也会为你提供mysqli的导入方法
         }
        $type = 1;
        $oreoAuth->delDirAndFile('../temp/sql/'); //删除sql文件
     }
    $oreoAuth->delDirAndFile($updatedir); //删除更新包
    if(!empty($type)){ $type=1;}else{$type=2;}
    $oreoAuth->post($authParam)->updateLog('updateSqlType',$type)->url('http://你的域名/oreo/api/checkUpdateLog');//上报更新结果
    echo '升级成功';
}else{
    echo '升级失败';
}
```

```php
//mysqli导入SQL的方法(根据需要选择使用)（参考）
$_mysqli = new \mysqli($hostname,$username,$password,$database,$port);//地址，用户名，密码，库名，端口
if (mysqli_connect_errno()) {
    exit("连接数据库出错");
}
foreach (explode(";[\r\n]+", $sql) as $v) {
          $_mysqli->multi_query($v); //导入
}
          $_mysqli->close();//关闭连接
          unset($hostname,$username,$password,$database,$port,$_mysqli);//注销变量
```

------

# 六、授权接口的设置方法

> Oreo授权系统除了能授权域名和IP之外，还可以授权api接口，类似于阿里云/腾讯云收费接口，你可以在授权系统中添加api接口，api套餐等，简单配置后用户可以正常购买调用该接口。
>
> 当然Oreo授权系统的API接口授权功能也是很完美的，你可以设置API接口的套餐之外你也可以设置接口费用，调用次数计算，调用频率限制等操作。
>
> 具体配置方法几乎都在Oreo授权系统的管理员面板，你的源API接口文件不会有太多的更改对接等操作。


## 首次设置方法

登录 **Oreo授权系统** 管理员面板，左侧导航栏选择 【授权API设置】->【授权API列表】->【添加】

这里需要添加 :

1.【接口名称】（可中文）

2.【原接口链接】（也就是你已经写好的API接口的具体链接地址，如：https://www.xx.com/api/test.php）

3.【请求地址】(必须英文，这里将会生产一个对应的用户请求链接，如：http://当前域名/oreo/Interface/设置的请求地址)

## API套餐的设置方法
登录 **Oreo授权系统** 管理员面板，左侧导航栏选择 【授权API设置】->【API套餐列表】->【添加】

这里需要添加 :

1.【选择接口】（请选择对应的接口）

2.【套餐类型】（1.免费接口，2.按量收费，3.永久许可）

> ps：如果为免费接口则不需要设置接口价格，调用次数等

3.【接口频率限制/分钟】(表示每分钟单个用户最多能请求的数量，这会防止接口盗刷，写入0表示不限制)

4.【调用次数】(按量收费下生效)

5.【套餐费用】(免费接口模式下无效)

## 开发者需要注意
当然你的api需要简单的修改下即可对接到本程序，首先你的接口必须是能接受**POST**数据，你的接口必须返回 **JSON** 数据，最后你需要输出【oreo_code】结果代码，【oreo_msg】结果内容即可完成对接。

以下是用户对接后请求接口的实例

```json
{
	"token": "24c2fd22-7831-63ab-9283-c558436", //用户的Token
	"param":{
	    "number" : "123456",
	    "name" : "test",
	    "url" : "http://www.xx.com/test",
	    "money": "18.58"
	}
}
//token为用户购买接口后产生的用户Token，这不能为空
//param的内容根据你的接口而定
```

以下是一个简单的对接实例

```php
<?php
header('content-type:application/json'); //json类型
//这里需要填写接口Token,获取方法为,【授权API设置】->【授权API列表】页面对应接口的Token信息
$apiToken = '35386d30-b432-de23-e6cb-39855fe';//接口token
//我们已经做了很好的安全防护，但我们还是希望您能加上一道token验证，这将会更好的保护您的api
if(empty($_POST['api_token'])){
    $arr['oreo_code'] = -1;
    $arr['oreo_msg'] = "Token is Not Null";
    exit(json_encode($arr));
} //如果token为空提示错误

if($_POST['api_token'] != $apiToken){
    $arr['oreo_code'] = -1;
    $arr['oreo_msg'] = "Token Error";
    exit(json_encode($arr));
}//如果token不相符提示错误
     
/*
 * $_POST['param']; //这是接口需要参数数组
 */
//这里是您的其他业务逻辑代码

//....业务结束 返回数据一定要Json数据类型并且数组内增加一个oreo键名并且值为200，这即表示授权站可以根据用户设置扣除余量

//以下是演示代码
//我要获取 param数组的 name 并且 返回该值
exit('{"oreo_code":200,"oreo_msg":"'.$_POST['param']['name'].'"}');

//返回类型说明，您的每次json返回必须携带 oreo_code，和oreo_msg的键名，
//oreo_code 等于 200表示该api接口已经成功执行，此时授权站会处理相关的后续操作，如果oreo_code 不等于 200（比如 -1） 则可以返回oreo_msg （错误内容）
```
# 七、其他模块的对接
## 在网状态

关于【授权管理】->【授权列表】内的**在网状态**功能的说明如下：

> 我们在开发授权的时候就了解整个授权的过程，因此我们发现，在验证机制中，如果开发者**过多使用验证授权状态的查询**，可能会导致**授权的系统**（也就是你要销售的系统）**加载速度偏慢**的情况，当然导致这个的因素很多，这其中包括**用户本身服务器的情况很糟糕**，或许**网络因素(服务器在境外)**，也或许是**单方面访问者的网络波动**，当然我们**也不排除你授权验证服务器的上述问题的存在情况**。
>
> 因此，我们开发了一种**初次验证的机制**，原理很简单，我们**只需要在首次访问的时候进行验证**，如果**验证不通过**，那么，可以**继续验证**，否则，**验证通过**接下来的其他验证**都基于通过，不经过再次进行远程验证**，这个过程你可以写入到**Session**，**redis**，**Cache**。
>
> 不过，我们又发现了**新的问题**，那就是，如果我**验证通过了**，也给**该用户放行一段时间**（可能是一天，七天或者更久），**期间发生授权变动**，导致**授权验证不及时**(当然这只是个**对于已经有缓存的用户而言**，**其他用户**访问**依然**是会有**最新的授权状态展示**)，不过，**在网状态**功能下你可以**判断该授权是否还在用你的系统**之外还可以给该网站**清理缓存**而达到验证的及时性。

## 验证方法(在网)
这个过程很简单，我们只需要在授权的系统（就是你销售的系统）内返回一个json数据即可，当然这个过程只是一个检测是否在网状态的验证，因此不会有安全隐患的存在，我们只需要查看返回的code状态。

访问此链接可以正常返回一个json数据，内容为：

```
$arr['code'] = 200; // code=>200 保持不变
$arr['msg'] = "OREO IS OK";
exit(json_encode($arr));
```
## 清理缓存方法
这个过程也是相当的简单，你可以在获取在网状态之后，可以选择面板内的**清除缓存**来进行操作，当然这个需要你在你的授权程序（销售的程序）配置一些简单的代码，这个过程可能有些敏感操作，因此我们已经做出了一些安全验证（授权站主动POST该站点**系统码**以及**授权码**）参考代码如下：

```
$systemKey = $_POST['systemKey'];//远程系统码
$authKey = $_POST['authKey'];//远程授权码
if($systemKey != 这里是你在核心加载过的系统码变量 )$arr['code'] = -1;
if($authKey!= 这里是你在之前指定过的用户授权码变量 )$arr['code'] = -1;
$arr['msg'] = "安全校验失败";
//清除缓存
if (delete_dir_file( '这里是指定自己项目缓存目录，包括session目录或cache目录')) {  //这里我引用一个删除的函数，当然这个函数在下方会有详细列出
    $arr['code'] = 200;
    $arr['msg'] = "清除成功";
} else {
    $arr['code'] = -1;
    $arr['msg'] = "清除失败";
}
exit(json_encode($arr));

//通过上面两个安全验证之后你可以自由发挥，根据你的项目自定义清理的方式，如果你的缓存方式不同则代码可能也不同，凡是清理成功你需要返回json类型，code=200表示成功，code=-1表示失败即可
```

我用的删除文件的函数

```
/**
 * 循环删除目录和文件
 * @param string $dir_name
 * @return bool
 */
function delete_dir_file($dir_name) {
    $result = false;
    if(is_dir($dir_name)){
        if ($handle = opendir($dir_name)) {
            while (false !== ($item = readdir($handle))) {
                if ($item != '.' && $item != '..') {
                    if (is_dir($dir_name . DIRECTORY_SEPARATOR . $item)) {
                        delete_dir_file($dir_name . DIRECTORY_SEPARATOR . $item);
                    } else {
                        unlink($dir_name . DIRECTORY_SEPARATOR . $item);
                    }
                }
            }
            closedir($handle);
            if (rmdir($dir_name)) {
                $result = true;
            }
        }
    }
    return $result;
}
```

# 实例Demo
> 为了更好的掌握使用方法，我们也做了一些列的Demo，你可以参考官方Demo文件快速学习对接。（ SDK1.0.zip）包中包含了以下代码。
>
> 我们还为你提供更完整的Demo文件，在SDK包/demo/complete-demo，你完全可以参考该目录下的文件来对接你的项目。同样你也可以
>
> 把这目录上传到你的测试网站来对接调试。

## 授权验证实例
```php
<?php
include_once ('../../src/lib/OreoAuth.php');//引入OreoAuth类
//...
//我的其他业务代码
//...
//安全校验
if (md5_file('../../src/lib/OreoAuth.php') != '29d9c5bb40d839968d398d2285fd8860') exit('安全校验失败');
//实例化OreoAuth类
$oreoAuth = new OreoAuth();
$oreoAuth->loadFile('../.oreo');//请把.oreo放入项目根目录
$authParam = array(
    'domain' => $_SERVER['HTTP_HOST'], //当前域名 (必填)
    'sysKey' => '4zzd278-21be-06e5-be32-4fbd248',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    'version' => $oreoAuth->get('version.num'), //系统当前版本,从env文件中获取 (必填)
    'authKey' =>  '3dd73123-0dda-0114-33d6-s123a1e3', //填写域名授权后生成的授权码,我们建议从数据库中获取，您可以在用户安装的时候写进数据库(必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => '127.0.0.1',//数据库地址,如果不需要请填写2
    'isSqlDataBase' => 'root',//数据库库名,如果不需要请填写2
    'isSqlUserName' => 'root',//数据库账号,如果不需要请填写2
    'isSqlPassword' => '123456',//数据库密码,如果不需要请填写2
    'isSqlHostPort' => '3306'//数据库端口,如果不需要请填写2
);
$oreoAuth->post($authParam)->url('https://www.oreopay.com/oreo/api/checkDomain'); //这里必须要设置正确的协议头，http://或https://
if (!$oreoAuth->error()) { //如果没有发生错误
    $oreoContent = $oreoAuth->data();//返回结果
}else{ //则
    exit($oreoAuth->error());//输出错误
}
//...
//我的其他业务代码
//....
//在我需要验证的页面或者全局验证(授权验证)
if(empty($oreoContent)){ //如果返回结果为Null
    exit('授权检测失败，请联系作者');//输出本地错误,可以自定义
}else if ($oreoContent['code'] == 4001) { //如果返回结果为未授权
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出授权站设置的错误内容
}
//我们可以打印看一下授权成功的返回信息
//var_dump($oreoContent);
//exit();


//如果没有执行以上步骤则表示授权成功
//...
//我的其他业务代码
//....
```

## 更新检测实例

```php
<?php
include_once ('../../src/lib/OreoAuth.php');//引入OreoAuth类
//...
//我的其他业务代码
//...
//安全校验
if (md5_file('../../src/lib/OreoAuth.php') != '29d9c5bb40d839968d398d2285fd8860') exit('安全校验失败');
//实例化OreoAuth类
$oreoAuth = new OreoAuth(); //如果在全局(核心文件)已经引入则不需要再引入了
$oreoAuth->loadFile('../.oreo');//请把.oreo放入项目根目录,如果在全局(核心文件)已经引入则不需要再引入了
$authParam = array(
    'domain' => $_SERVER['HTTP_HOST'], //当前域名 (必填)
    'sysKey' => '4zzd278-21be-06e5-be32-4fbd248',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    'version' => $oreoAuth->get('version.num'), //系统当前版本,从env文件中获取 (必填)
    'authKey' =>  '3dd73123-0dda-0114-33d6-s123a1e3', //填写域名授权后生成的授权码,我们建议从数据库中获取，您可以在用户安装的时候写进数据库(必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => '127.0.0.1',//数据库地址,如果不需要请填写2
    'isSqlDataBase' => 'root',//数据库库名,如果不需要请填写2
    'isSqlUserName' => 'root',//数据库账号,如果不需要请填写2
    'isSqlPassword' => '123456',//数据库密码,如果不需要请填写2
    'isSqlHostPort' => '3306'//数据库端口,如果不需要请填写2
);

//以下是可以在需要的页面当中配置下可以获取更新版本的信息
$oreoAuth->post($authParam)->url('https://www.oreopay.com/oreo/api/checkUpdate'); //这里必须要设置正确的协议头，http://或https://
if (!$oreoAuth->error()) { //如果没有发生错误
    $oreoContent = $oreoAuth->data();//返回结果
}else{ //则
    exit($oreoAuth->error());//输出错误
}
//...
//我的其他业务代码
//....
//在我需要检测更新的页面或者全局验证(更新检测)
if(empty($oreoContent)){ //如果返回结果为Null
    exit('授权检测失败，请联系作者');//输出本地错误,可以自定义
}else if ($oreoContent['code'] == 4001) { //如果返回结果为未授权
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出授权站设置的错误内容
}
//以上代码是授权检测代码,可根据需要来自定义，如果您加入了核心代码当中则可以不写

//检测最新版本
$updatedir = '../temp/update/';//设置下载目录;请不要设置在当前目录，应当在根目录下创建文件夹最合适,请给予0755权限
$fileName = $oreoAuth->save($oreoContent['data']['verUrl'],$updatedir); //下载包
$updateZip = $updatedir.$fileName; //拼接
//初始化PHPZIP
$zip = new ZipArchive;
$res = $zip->open($updateZip);
if ($res === true) {
    $zip->extractTo('../');
    $zip->close();
    $sqlfile = '../temp/sql/update.sql';
    $sql = @file_get_contents($sqlfile);
    if ($sql) {
        error_reporting(0);
        foreach (explode(";[\r\n]+", $sql) as $v) {
            //@mysql_query($v);
            $DB->query($v)->fetch(); //这个Sql导入语句根据您的代码来定义
        }
        $type = 1;
    }
    $oreoAuth->delDirAndFile($updatedir); //删除更新包
    if(!empty($type)){ $type=1;}else{$type=2;}
    $oreoAuth->post($authParam)->updateLog('updateSqlType',$type)->url('https://www.oreopay.com/oreo/api/checkUpdateLog');//上报结果//这里必须要设置正确的协议头，http://或https://
    echo '升级成功';
}else{
    echo '升级失败';
}
```
