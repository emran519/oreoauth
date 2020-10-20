# Oreo授权系统(守护您的原创代码) 1.0 使用说明

作者:oreo

QQ:609451870

编辑时间:2020/10/19

------

> **本文中将介绍Oreo授权系统 V1.0 主站的安装方法以及详细的介绍，OreoAuth类的对接方法，引导你更快速的对接到自己的项目当中。**



## 一、关于系统

### 开发环境

```
- PHP 7.4 （基于高性能的ThinkPHP 6.0.4框架 , PHP扩展fileinfo,redis,sg11）
- MYSQL 8.0
- Redis
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

2.完成以上配置后你可以浏览器中直接打开项目的域名，如果是首次安装将会自动跳转至安装向导页面。

> 如果项目域名未能打开，请先考虑存在的问题，如: 域名是否已经解析，环境是否配置成功等

------

##  二、关于SDK包

请前往Oreo官网下载 SDK1.0.zip 包，初步计划先提供原生SDK对接包，后期将增加ThinkPHP和Laravel的Composer包。

### 目录结构

```
oreosoft     （授权验证目录）
├─demo        实例文件夹(参考作用，可删除) 
│  ├─auth     授权检测类实列文件夹   
│  ├─update   更新检测类实列文件夹      
├─src         核心文件夹(不可删除) 
│  ├─Env.php      
│  ├─OreoAuth.php
├─.env        (配置文件，不可删除)
```

### env文件结构

```env
[APP]
DEFAULT_TIMEZONE = Asia/Shanghai //时区
[VERSION]
NUM = 1.01  //版本号
[AUTH]
KEY = acf5154c-b7a5-6e17-c12a-e6a6cac  //用户授权后生成的授权秘钥,你也可以选择存入数据库
```

> **请把.env文件放入项目根目录下**

------

## 三、返回结果

对接相应接口之前，你需要先了解接口返回的数据。

| 键名 | 名称 |  类型  |                     说明                     |
| :--: | :--: | :----: | :------------------------------------------: |
| code | 编码 |  int   | -1=>授权错误;4001=>授权正常;4002=>暂无新版本 |
| msg  | 内容 | string |                      /                       |
| data | 数据 | array  |                相应接口的数据                |

------

## 四、代码引入方式（原生）

### 使用方法

#### 授权验证

在你的项目全局文件中头部引入

```php
include_once('oreosoft/src/OreoAuth.php');
```

在代码开始前你可以选择做该文件的MD5校验（MD5可以在Oreo授权官方可以在线获得）

```PHP
if(md5_file('../src/OreoAuth.php')!='该文件的MD5值')exit('安全校验失败');
```

数据库获取方法代码段下方开始初始化Oreo类

```php
//实例化OreoAuth类
$oreoAuth = new OreoAuth();
$oreoAuth->loadFile('../.env');//请把.env放入项目根目录
$authParam = array(
    'authSafe' => '', //1=>无需盗版入库,2=>需盗版入库即吧数据库信息录入到授权站内 (必填)
    'domain'   => '', //当前域名 (必)
    'sysKey'   => '',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => '',//数据库地址 （选填）
    'isSqlDataBase' => '',//数据库库名 （选填）
    'isSqlUserName' => '',//数据库账号 （选填）
    'isSqlPassword' => '',//数据库密码 （选填）
    'isSqlHostPort' => '',//数据库端口 （选填）
);
$oreoAuth->oreoDomainKey = ''; //填写域名授权后生成的授权码,此项可以直接从env文件中配置和获取，也可以根据情况从数据库中获取 (必填)
//授权码Env获取方法
//$oreoAuth->get('system.key');
```

可在全局文件中直接进行授权验证

```php
$oreoAuth->post($authParam)->url('http://你的域名/oreo/api/checkDomain'); //这里必须要设置正确的协议头，http://或https:// 
```

> 这里的 /oreo/api/checkDomain 是检查授权的接口
>
> 文档后部将列出所有接口，你可以根据需求来定义接口

返回结果

```php
if (!$oreoAuth->error()) {
    $oreoContent = $oreoAuth->data();//返回
}
```

如需检测的文件或全局文件中

```php
if ($oreoContent['code'] == 4001) {
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出错误内容
}
```

#### 更新检测

如果你已经把OreoAuth验证类加载到你的全局（核心）文件当中，那么你只需要在相应需要检测更新的页面文件中加入一下代码即可完成更新检测和更新下载的功能了。

```php
$oreoAuth->post($authParam)->url('http://你的域名/oreo/api/checkUpdate'); //这里必须要设置正确的协议头，http://或https:// 
```

```php
//以下代码如果在全局（核心）文件中使用了则不需要再次加入其他页面了
if ($oreoContent['code'] == 4001) {
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出错误内容
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
        ["verText"]=> string(10) "测试1.01" 
        ["verDate"]=> string(19) "2020-10-20 14:35:41"
        ["verUrl"]=> string(87) "http://你的域名/storage/update/zip/20201020/6f3914c856f6d3733ad74e45f17823d2.zip"
    } 
} 
```

那么，你该怎么处理这些信息，下面简单的几行代码就可以解决你的难题

```php
if($oreoContent['code']!=4002){
    //这里是没有新版本的情况下的逻辑代码
}else{
    //这里是有新版本的逻辑代码
    $verNo   = $oreoContent['data']['verNo']; //最新版本
    $verText = $oreoContent['data']['verText'];//更新内容
    $verDate = $oreoContent['data']['verDate'];//发布时间
    $verUrl = $oreoContent['data']['verUrl'];//包地址,此变量不要输出到你的html页面中
}
//而后你可以把以上代码直接引用到你的php代码或html代码当中
```

发现有新版本提示了，那么你该如何下载和安装更新的zip包，在说明这个问题之前，你需要了解更新包的制作过程

#### 更新包的结构

```
//更新包不要再嵌套其他文件夹中(选中所有文件再点击压缩而不是返回上一个目录压缩)
├─oreosql        数据库文件夹 （如果本次更新无需升级数据库，无需创建此文件夹）
│  ├─update.sql  更新SQL文件    
├─xx1            你的需要更新的文件夹 （根据更新需求对应目录创建文件夹）
│  ├─xx.php      你的需要更新的代码    
├─xx2            你的需要更新的文件夹
│  ├─xx.html     你的需要更新的代码    
├─xx3            以此类推
├─.env           (配置文件，不可删除，每次更新请修改里面的NUM值为更新的版本)
```

好了，你已经学会了更新包的结构了，回到话题怎么下载更新包，我们也提供了下载更新包的接口，使用方法如下

```php
//当有新的版本提示，原则上你的网站会有一个（更新或下载）按钮，点击按钮系统自动下载并且安装好后返回结果
//一般情况下你的更新页面需要通过url的get方法来检测加载相应的页面操作，具体的方法我们会在实例Demo中展示，你也可以直接参考Demo文件的update
$outVersion = $authParam['version']; //之前版本
$updatedir = '../temp/update/';//设置下载目录，建议直接在全局（核心）文件中设置;请不要设置在当前目录，应当在根目录下创建文件夹最合适
$fileName = $oreoAuth->save($oreoContent['data']['verUrl'],$updatedir);//下载文件
$updatezip = $updatedir.$fileName;//下载的文件目录和名称
//初始化PHPZIP
$zip = new ZipArchive;
$res = $zip->open($updateZip);
if ($res === true) {
    $zip->extractTo('../');
    $zip->close();
    $sqlfile = '../oreosql/update.sql';
    if (file_get_contents($sqlfile)) {
        error_reporting(0);
        foreach (explode(";[\r\n]+", $sql) as $v) {
            //@mysql_query($v);
            $DB->query($v)->fetch(); //这个Sql导入语句根据您的代码来定义
         }
        $sqlResult = 1;
     }
    $this->delDirAndFile($updateZip);
    $oreoAuth->post($authParam)->url('http://你的域名/oreo/api/checkUpdateLog?type='.$sqlResult);//上报更新结果
    echo '升级成功';
}else{
    echo '升级失败';
}
```

### 实例Demo

为了更好的掌握使用方法，我们也做了一些列的Demo，你可以参考官方Demo文件快速学习对接。（ SDK1.0.zip）包中包含了以下代码。

#### 授权验证

```php
<?php
include_once('oreosoft/src/OreoAuth.php');//引入OreoAuth类
//...
//我的其他业务代码
//...
//安全校验
if(md5_file('../src/OreoAuth.php')!='该文件的MD5值')exit('安全校验失败');
//实例化OreoAuth类
$oreoAuth = new OreoAuth();
$oreoAuth->loadFile('../.env');//请把.env放入项目根目录
$authParam = array(
    'authSafe' => '2', //1=>无需盗版入库,2=>需盗版入库即吧数据库信息录入到授权站内 (必填)
    'domain'   => $_SERVER['HTTP_HOST'], //当前域名 (必填)
    'sysKey'   => 'acf5154c-b7a5-6e17-c12a-e6a6cac',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    'version' => $oreoAuth->get('version.num'), //系统当前版本,从env文件中获取 (必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => $sqlConf['host'],//数据库地址 （选填）
    'isSqlDataBase' => $sqlConf['database'],//数据库库名 （选填）
    'isSqlUserName' => $sqlConf['username'],//数据库账号 （选填）
    'isSqlPassword' => $sqlConf['password'],//数据库密码 （选填）
    'isSqlHostPort' => $sqlConf['port'],//数据库端口 （选填）
);
$oreoAuth->oreoDomainKey = $oreoAuth->get('system.key');//填写域名授权后生成的授权码,此项可以直接从env文件中配置和获取，也可以根据情况从数据库中获取 ,我在这选择了从env文件中获取(必填)
$oreoAuth->post($authParam)->url('https://auth.test.com/oreo/api/checkDomain'); //这里必须要设置正确的协议头，http://或https://
if (!$oreoAuth->error()) {
    $oreoContent = $oreoAuth->data();
}
//...
//我的其他业务代码
//....
//在我需要验证的页面或者全局验证
if ($oreoContent['code'] == 4001) {
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出错误内容
}
```

#### 更新检测

```php
<?php
include_once('oreosoft/src/OreoAuth.php');//引入OreoAuth类
//...
//我的其他业务代码
//...
//安全校验
if(md5_file('../src/OreoAuth.php')!='该文件的MD5值')exit('安全校验失败');
//实例化Env
Env::loadFile('env文件路径'); //请把.env放入项目根目录
//实例化OreoAuth类
$oreoAuth = new OreoAuth();
$authParam = array(
    'authSafe' => '2', //1=>无需盗版入库,2=>需盗版入库即吧数据库信息录入到授权站内 (必填)
    'domain'   => $_SERVER['HTTP_HOST'], //当前域名 (必填)
    'sysKey'   => 'acf5154c-b7a5-6e17-c12a-e6a6cac',//程序KEY,填写后台【授权程序设置】->【授权程序列表】生成的【程序验证码】 (必填)
    'version' => $oreoAuth->get('version.num'), //系统当前版本,从env文件中获取 (必填)
    //如需想盗版入库还可以配置数据库参数
    'isSqlHostName' => $sqlConf['host'],//数据库地址 （选填）
    'isSqlDataBase' => $sqlConf['database'],//数据库库名 （选填）
    'isSqlUserName' => $sqlConf['username'],//数据库账号 （选填）
    'isSqlPassword' => $sqlConf['password'],//数据库密码 （选填）
    'isSqlHostPort' => $sqlConf['port'],//数据库端口 （选填）
);
$oreoAuth->oreoDomainKey = Env::get('system.key');//填写域名授权后生成的授权码,此项可以直接从env文件中配置和获取，也可以根据情况从数据库中获取 ,我在这选择了从env文件中获取(必填)
$oreoAuth->post($authParam)->url('https://auth.test.com/oreo/api'); //这里必须要设置正确的协议头，http://或https://
if (!$oreoAuth->error()) {
    $oreoContent = $oreoAuth->data();
}
//...
//我的其他业务代码
//....
//在我需要验证的页面或者全局验证
if ($oreoContent['code'] == 4001) {
    exit("{$oreoContent['msg']}");//直接终止其余操作，输出错误内容
}
```
