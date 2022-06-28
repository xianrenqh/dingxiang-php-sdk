# 顶象PHP SKD

因为要使用Thinkphp和Webman框架，而官方没有提供Composer安装，故自己弄一个集成。

## 依赖项
请在安装之前确保以下模块已安装且可用，否则运行的时候会报错：
* PHP JSON模块
* PHP CURL模块

## 安装
在项目的根目录执行安装指令：
> composer require xianrenqh/dingxiang-php-sdk

## 使用
目前仅支持以下服务：
* 设备指纹
* 无感验证
* 实时风险决策

## 案例
### php服务端使用
```php
use xianrenqh\Dingxiang\CaptchaClient;
$appId     = "appId";   
$appSecret = "appSecret";
$client    = new CaptchaClient($appId, $appSecret);
$client->setTimeOut(2);

//指定服务器地址，saas可在控制台，应用管理页面最上方获取
$client->setCaptchaUrl("https://xxx.dingxiang-inc.com/api/tokenVerify");

$response = $client->verifyToken($param['captcha_token']);
var_dump($response->serverStatus);
//确保验证状态是SERVER_SUCCESS，SDK中有容错机制，在网络出现异常的情况会返回通过

if($response->result){
    echo "true";
    /**token验证通过，继续其他流程**/
}else{
    echo "false";
    /**token验证失败**/
}

```

### web端接入
获取appId

请先进入顶象控制台（或点击右上角登陆按钮）中的“应用管理”或“应用配置”模块，并找到appId。

引入 JS
~~~
<script src="https://cdn.dingxiang-inc.com/ctu-group/captcha-ui/index.js" crossorigin="anonymous" id="dx-captcha-script"></script>
~~~
> 注意：由于验证码 JS 会不定期升级更新，请直接引用顶象 CDN 上的资源，以便及时获得最新安全防护。不要将 JS 文件下载到自己服务器使用。

初始化
以下分别列举Javascript、React、Vue的初始化接入示例代码：

#### Javascript 示例
假设页面上有一个 
~~~
<div id="c1"></div>
~~~
，则可以像下面这样初始化验证码。
```js
var myCaptcha = _dx.Captcha(document.getElementById('c1'), {
  appId: 'appId', //appId，在控制台应用管理或应用配置模块获取
  apiServer: 'https://xxx.dingxiang-inc.com',  
 // apiServer域名地址在控制台页面->无感验证->应用管页面左上角获取，必须填写完整包括https://。
  success: function (token) {
    // console.log('token:', token) 
    // 获取验证码token，用于后端校验，注意获取token若是sl开头的字符串，则是前端网络不通生成的降级token,请检查前端网络及apiServer地址。
  }
})
```
初始化完成后，验证码组件会被插入到 <div id="c1"></div> 中。

#### React 示例
假设页面上有一个 <div id="demo"></div>，则可以像下面这样初始化验证码：

```js
// 类组件使用componentDidMount  
useEffect(() => {  
  _dx.Captcha(document.getElementById('demo'), {
    appId: 'appId', //appId，在控制台应用管理或应用配置模块获取
    apiServer: 'https://xxx.dingxiang-inc.com',  
    // apiServer域名地址在控制台页面->无感验证->应用管页面左上角获取，必须填写完整包括https://。
    success: token => {
      // 获取验证码token，用于后端校验，注意获取token若是sl开头的字符串，则是前端网络不通生成的降级token,请检查前端网络及apiServer地址。
      console.log(token);
    }
  });
}, [])
```
可点击查看 React完整示例代码
初始化完成后，验证码组件会被插入到 <div id="demo"</div>中。

#### Vue 示例
假设页面上有一个 <div ref="demo"></div>，则可以像下面这样初始化验证码：
```vue
mounted() {
  _dx.Captcha(this.$refs.demo, {
    // appId, 在控制台应用管理或应用配置模块获取
    appId: "appId",
    apiServer: 'https://xxx.dingxiang-inc.com',  
    // apiServer域名地址在控制台页面->无感验证->应用管页面左上角获取，必须填写完整包括https://。
    success: token => {
      // 获取验证码token，用于后端校验，注意获取token若是sl开头的字符串，则是前端网络不通生成的降级token,请检查前端网络及apiServer地址。
      console.log(token);
    }
  });
}
```
可点击查看 Vue完整示例代码
初始化完成后，验证码组件会被插入到 <div ref="demo"</div>中。


其他具体使用方法请参考
https://www.dingxiang-inc.com/docs
