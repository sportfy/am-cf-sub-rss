# 定制聚合订阅 am-cf-sub-rss
### 通过Cloudflare Workers &amp; Pages 搭建，将你任意节点与多个订阅聚合成专属于你的订阅链接,完成定制汇聚订阅

- 官网教程：[AM科技](https://am.809098.xyz)
- YouTube频道：[@AM_CLUB](https://youtube.com/@AM_CLUB)
- Telegram交流群：[@AM_CLUBS](https://t.me/AM_CLUBS)
- 免费订阅：[进群发送关键字: 订阅](https://t.me/AM_CLUBS)

## Pages 部署方法 [视频教程](https://youtube.com/watch?v=f9hDJCqAEGA)
### 1. 部署 Cloudflare Pages：
   - 在 Github 上先 Fork 本项目，并点上 Star !!!
   - 在 Cloudflare Pages 控制台中选择 `连接到 Git`后，选中 `am-cf-sub-rss`项目后点击 `开始设置`。

### 2. 给 Pages绑定 自定义域：
   - 在 Pages控制台的 `自定义域`选项卡，下方点击 `设置自定义域`。
   - 填入你的自定义次级域名，注意不要使用你的根域名，例如：
     您分配到的域名是 `fuck.google.com`，则添加自定义域填入 `sub.fuck.google.com`即可；
   - 按照 Cloudflare 的要求将返回你的域名DNS服务商，添加 该自定义域 `sub`的 CNAME记录 `am-cf-sub-rss.pages.dev` 后，点击 `激活域`即可。

### 3. 修改 快速订阅入口 ：

  例如您的pages项目域名为：`sub.fuck.google.com`；
   - 添加 `TOKEN` 变量，快速订阅访问入口，默认值为: `auto` ，获取订阅器默认节点订阅地址即 `/auto` ，例如 `https://sub.fuck.google.com/auto`

### 4. 添加你的节点和订阅链接：
   - 添加 `LINK` 变量，参数添加你的自建节点链接和机场订阅链接，确保每行一个链接，例如：
   ```
   vless://866853eb-5293-4f09-bf00-e13eb237c655@icook.tw:443?encryption=none&security=tls&sni=worker.amcloud.filegear-sg.me&fp=random&type=ws&host=worker.amcloud.filegear-sg.me#t.me%2FAM_CLUBS vless://866853eb-5293-4f09-bf00-e13eb237c655@visa.com:443?encryption=none&security=tls&sni=worker.amcloud.filegear-sg.me&fp=random&type=ws&host=worker.amcloud.filegear-sg.me#youtube.com%2F%40AM_CLUB
   https://trojan.amcloud.filegear-sg.me/auto
   https://worker.amcloud.filegear-sg.me/bestip/866853eb-5293-4f09-bf00-e13eb237c655?uuid=866853eb-5293-4f09-bf00-e13eb237c655
   ```

## Workers 部署方法
### 1. 部署 Cloudflare Worker：

   - 在 Cloudflare Worker 控制台中创建一个新的 Worker。
   - 将 [worker.js](https://github.com/ansoncloud8/am-cf-sub-rss/blob/main/_worker.js)  的内容粘贴到 Worker 编辑器中。


### 2. 修改 订阅入口 ：

  例如您的workers项目域名为：`sub.am.workers.dev`；
   - 通过修改 `mytoken` 赋值内容，达到修改你专属订阅的入口，避免订阅泄漏。
     ```
     let mytoken = 'auto';
     ```
     如上所示，你的订阅地址则如下：
     ```url
     https://sub.am.workers.dev/auto
     或
     https://sub.am.workers.dev/?token=auto
     ```


### 3. 添加你的节点或订阅链接：

**3.1 修改 MainData 参数示例**

 - 修改 `MainData` 参数添加你的自建节点，例如：
   
	```js
	const MainData = `
	vless://866853eb-5293-4f09-bf00-e13eb237c655@icook.tw:443?encryption=none&security=tls&sni=worker.amcloud.filegear-sg.me&fp=random&type=ws&host=worker.amcloud.filegear-sg.me#t.me%2FAM_CLUBS 
	vless://866853eb-5293-4f09-bf00-e13eb237c655@visa.com:443?encryption=none&security=tls&sni=worker.amcloud.filegear-sg.me&fp=random&type=ws&host=worker.amcloud.filegear-sg.me#youtube.com%2F%40AM_CLUB
	`
	```
注意！`MainData`参数的特殊引号必须保留，否则代码异常。



 **3.2 修改 urls 参数示例**
 
 - 修改 `urls` 参数，在脚本中设置 `urls` 变量为 你的订阅链接 的 URL。例如：

	```js
	const urls = [
		'https://trojan.amcloud.filegear-sg.me/auto',
 		'https://hy2sub.pages.dev',
	];
	```
注意！订阅链接内容必须为`base64`格式。

## 变量说明
| 变量名 | 示例 | 备注 | 
|--------|---------|-----|
| LINK | vless://b7a39... vmess://ew0K... https://sub...  | 可同时放入多个节点链接与多个订阅链接, 链接之间用换行做间隔 | 
| TOKEN | auto | 快速订阅内置节点的订阅路径地址 /auto | 
| TGTOKEN | 6894123456:XXXXXXXXXX0qExVsBPUhHDAbXXXXXqWXgBA | 发送TG通知的机器人token | 
| TGID | 6946912345 | 接收TG通知的账户数字ID | 
| SUBAPI | subapi.xx.xx.io | clash、singbox等 订阅转换后端 | 
| SUBCONFIG | [https://raw.github.../ACL4SSR_Online_MultiCountry.ini](https://raw.githubusercontent.com/ansoncloud8/ACL4SSR/main/Clash/config/ACL4SSR_Online_Full_MultiMode.ini) | clash、singbox等 订阅转换配置文件 | 


## 注意事项
项目中，TGTOKEN和TGID在使用时需要先到Telegram注册并获取。其中，TGTOKEN是telegram bot的凭证，TGID是用来接收通知的telegram用户或者组的id。


## Star 星星走起
[![Stargazers over time](https://starchart.cc/ansoncloud8/am-cf-sub-rss.svg?variant=adaptive)](https://starchart.cc/ansoncloud8/am-cf-sub-rss)


 # 
<details><summary><strong> [点击展开] 赞赏支持 ~🧧</strong></summary>
*我非常感谢您的赞赏和支持，它们将极大地激励我继续创新，持续产生有价值的工作。*
  
- **TRC20:** `TWTxUyay6QJN3K4fs4kvJTT8Zfa2mWTwDD`
  
</details>
