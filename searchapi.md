# amway_search_qa
---------------------------------------
<br />



## API列表

<div>
    <table>
        <tr>
            <th >API名称</th>
            <th >认证方式</th>
            <th >描述</th>
        </tr>
            <tr>
                <td>search搜索_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
            <tr>
                <td>授权api_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
            <tr>
                <td>搜索下拉推荐_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
            <tr>
                <td>每天批量更新热词_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
            <tr>
                <td>热词推荐_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
            <tr>
                <td>索引数据_Cloned</td>
                <td>OPENID</td>
                <td></td>
            </tr>
    </table>
</div>

## API调用
### 公共入参
公共请求参数是指每个接口都需要使用到的请求参数。

参数名称 | 位置 |必须 | 描述
-------|------|--------|----
X-Ca-Key |Header| 是  | Appkey，调用API的身份标识，可以到阿里云[API网关控制台](https://apigateway.console.aliyun.com/#/apps/list)申请
X-Ca-Signature | Header| 是  | 通过签名计算规则计算的请求签名串，参照：<a href="#Signature">签名计算规则</a>
X-Ca-Timestamp | Header| 否  | API 调用者传递时间戳，值为当前时间的毫秒数，也就是从1970年1月1日起至今的时间转换为毫秒，时间戳有效时间为15分钟
X-Ca-Nonce|Header| 否  |API请求的唯一标识符，15分钟内同一X-Ca-Nonce不能重复使用，建议使用 UUID，结合时间戳防重放
Content-MD5|Header| 否  |当请求 Body 非 Form 表单时，需要计算 Body 的 MD5 值传递给云网关进行 Body MD5 校验
X-Ca-Signature-Headers|Header|否|指定哪些Header参与签名，支持多值以","分割，默认只有X-Ca-Key参与签名，为安全需要也请将X-Ca-Timestamp、X-Ca-Nonce进行签名，例如：X-X-Ca-Signature-Headers:Ca-Timestamp,X-Ca-Nonce

### <span id="Signature">签名计算规则</span>
请求签名，是基于请求内容计算的数字签名，用于API识别用户身份。客户端调用API时，需要在请求中添加计算的签名（X-Ca-Signature）。
#### 签名计算流程
_________________________________________________________
准备APPkey → 构造待签名字符串stringToSign → 使用Secret计算签名
_________________________________________________________

##### 1.准备APPKey
Appkey，调用API的身份标识，可以到阿里云[API网关控制台](https://apigateway.console.aliyun.com/#/apps/list)申请

##### 2.构造待签名字符串stringToSign

````
String stringToSign=
HTTPMethod + "\n" +
Accept + "\n" +                //建议显示设置 Accept Header。当 Accept 为空时，部分 Http 客户端会给 Accept 设置默认值为 */*，导致签名校验失败。
Content-MD5 + "\n"
Content-Type + "\n" +
Date + "\n" +
Headers +
Url
````

###### HTTPMethod
为全大写，如 POST。

````
Accept、Content-MD5、Content-Type、Date 如果为空也需要添加换行符”\n”，Headers如果为空不需要添加”\n”。
````

###### Content-MD5

Content-MD5 是指 Body 的 MD5 值，只有当 Body 非 Form 表单时才计算 MD5，计算方式为：

String content-MD5 = Base64.encodeBase64(MD5(bodyStream.getbytes("UTF-8")));
bodyStream 为字节数组。

###### Headers

Headers 是指参与 Headers 签名计算的 Header 的 Key、Value 拼接的字符串，建议对 X-Ca 开头以及自定义 Header 计算签名，注意如下参数不参与 Headers 签名计算：X-Ca-Signature、X-Ca-Signature-Headers、Accept、Content-MD5、Content-Type、Date。

###### Headers 组织方法：
先对参与 Headers 签名计算的 Header的Key 按照字典排序后使用如下方式拼接，如果某个 Header 的 Value 为空，则使用 HeaderKey + “:” + “\n”参与签名，需要保留 Key 和英文冒号。

````
String headers =
HeaderKey1 + ":" + HeaderValue1 + "\n"\+
HeaderKey2 + ":" + HeaderValue2 + "\n"\+
...
HeaderKeyN + ":" + HeaderValueN + "\n"
````

将 Headers 签名中 Header 的 Key 使用英文逗号分割放到 Request 的 Header 中，Key为：X-Ca-Signature-Headers。

###### Url

Url 指 Path + Query + Body 中 Form 参数，组织方法：对 Query+Form 参数按照字典对 Key 进行排序后按照如下方法拼接，如果 Query 或 Form 参数为空，则 Url = Path，不需要添加 ？，如果某个参数的 Value 为空只保留 Key 参与签名，等号不需要再加入签名。

````
String url =
Path +
"?" +
Key1 + "=" + Value1 +
"&" + Key2 + "=" + Value2 +
...
"&" + KeyN + "=" + ValueN
````

注意这里 Query 或 Form 参数的 Value 可能有多个，多个的时候只取第一个 Value 参与签名计算。

##### 3.使用Secret计算签名

````
Mac hmacSha256 = Mac.getInstance("HmacSHA256");
byte[] keyBytes = secret.getBytes("UTF-8");
hmacSha256.init(new SecretKeySpec(keyBytes, 0, keyBytes.length, "HmacSHA256"));
String sign = new String(Base64.encodeBase64(Sha256.doFinal(stringToSign.getBytes("UTF-8")),"UTF-8"));
````

Secret 为 APP 的密钥，请在[应用管理](https://apigateway.console.aliyun.com/#/apps/list)中获取。



## API名称：search搜索_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/amway-search/v1/search/[appName]

方法：GET

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>appName</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td>opensearch示例应用名称</td>
</tr>
<tr>
<td>token</td>
<td>HEAD</td>
<td>STRING</td>
<td>是</td>
<td>token</td>
</tr>
<tr>
<td>query</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>搜索参数 需要urlencode</td>
</tr>
</table>
</div>

<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````
{
    "timeStamp": 1575859693393,
    "status": 0,
    "results": {
        "status": "OK",
        "request_id": "157585969319189455029268",
        "result": {
            "searchtime": 0.087178,
            "total": 106,
            "num": 5,
            "viewtotal": 106,
            "compute_cost": [
                {
                    "index_name": "t_content",
                    "value": 0.753
                }
            ],
            "items": [
                {
                    "fields": {
                        "aemtags": "",
                        "aemtagspath": "",
                        "allowidentities": "",
                        "author": "",
                        "baseid": "",
                        "brands": "",
                        "channel": "",
                        "classes": "",
                        "contenttag": "",
                        "description": "",
                        "favoritetags": "",
                        "host": "bizqa.amwaynet.com.cn",
                        "id": "http://bizqa.amwaynet.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/index.html",
                        "keywords": "纽崔莱, 纽崔莱品牌信息, 纽崔莱品牌信息-置顶文章",
                        "labels": "",
                        "name": "",
                        "people": "",
                        "searchtype": "",
                        "sentiments": "",
                        "source": "",
                        "strippedcontent": "关于纽崔莱 1 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 1 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 1 首页 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 关于纽崔莱 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 首页 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 返回 品牌起源 中国是纽崔莱的起源地 神奇植物-紫花苜蓿 营养补充食品初尝试 探索营养的奥秘 纽崔莱品牌的来历 梦想终于变成现实 品牌理念 纽崔莱品牌宣言 健康是美好生活的前提 营养是生命的源泉 现代生活方式影响营养吸收 补充营养维护身体健康 大事记 有机种植 自有有机农场 鲑鱼湖农场 墨西哥农场 巴西农场 有机认证标准 植物营养素 植物蕴藏的珍宝 善用植物营养素的力量 膳食的彩虹摄取规则 部分植物原料 生产步骤 科研成果 领先优势 关于营养基因学 植物营养素专利 强大后盾 纽崔莱营养与健康研究中心 安利（中国）研发中心 安利（中国）植物研发中心 纽崔莱科学顾问委员会 荟萃国际精英的全球研发 从种子到成品 优选种子 采集精华 提取调配 优质成品 超高标准 权威认证 信 誉 萃 炼 纽崔莱见证自然的力量 坚信自然的力量 五大热剧创意中插 纽崔莱携手夏至未至开启自然课堂 纽崔莱携手鬼吹灯开启脑洞时间 那年花开月正圆 轩辕剑之汉之云 将军在上 敬请期待 山姆·宏邦中国巡讲 媒体新闻列表 精彩回顾 用心萃炼 只为你赢 热点推荐 纽崔莱与奥运 自然之力 始终助力 热点推荐 首页 早餐其实很重要 专家说：早餐现状面面观 数据看：我国居民早餐状况 不吃早餐危害多 如何科学地加入“早餐党” 营养从早开始专家片 营养资料库 营养从早开始，就这么简单 精彩看点 早餐吃这么多才达标 首页 关于健康生活实验室 健康生活实验室项目起源 常见问题 最新动态 斯坦福大学健康生活实验室 乐纤体重管理-首页 体重管理 刻不容缓 肥胖带来的危害 管理体重 为何不简单 造成肥胖的原因 肥胖的自测 营养中国行 轻松3步 管理体重 4个少 八杯水 半小时 乐纤运动建议 4321操入门版 核心训练进阶版 燃脂魔性操 乐纤资料库 纽崔莱益生菌登陆微购 纽崔莱 自然精华 &gt; &nbsp; 拥有 可持续的有机耕种体系 善用 植物蕴藏的珍宝 科学精粹 &gt; &nbsp; 科学的 产品提炼生产步骤 前沿的 营养基因学探索 了解详情 &gt; 了解详情 &gt; 了解详情 &gt;",
                        "tags": "纽崔莱\t纽崔莱品牌信息\t纽崔莱品牌信息-置顶文章",
                        "tagspath": "",
                        "thumbnail": "",
                        "title": "纽崔莱",
                        "url": "http://bizqa.amwaynet.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite.html",
                        "videoid": "",
                        "videoimgsurl": "",
                        "videopath": "",
                        "videoposter": "",
                        "aboreadonly": "0",
                        "allowshare": "0",
                        "displaypublishtime": "0",
                        "modifieddate": "1574899228000",
                        "nosearch": "0",
                        "promotion": "0",
                        "publishdate": "1548462996000",
                        "videolen": "0",
                        "index_name": "t_content"
                    },
                    "property": {},
                    "attribute": {},
                    "variableValue": {},
                    "sortExprValues": [
                        "10000.4586862028"
                    ]
                },
                {
                    "fields": {
                        "aemtags": "",
                        "aemtagspath": "",
                        "allowidentities": "",
                        "author": "",
                        "baseid": "",
                        "brands": "",
                        "channel": "",
                        "classes": "",
                        "contenttag": "",
                        "description": "",
                        "favoritetags": "",
                        "host": "qa.amway.com.cn",
                        "id": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/about/index.html",
                        "keywords": "纽崔莱品牌 , 纽崔莱产品 , 健康 ",
                        "labels": "",
                        "name": "",
                        "people": "",
                        "searchtype": "",
                        "sentiments": "",
                        "source": "",
                        "strippedcontent": "关于纽崔莱 1 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 1 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 1 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 关于纽崔莱 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 返回 品牌起源 中国是纽崔莱的起源地 神奇植物-紫花苜蓿 营养补充食品初尝试 探索营养的奥秘 纽崔莱品牌的来历 梦想终于变成现实 品牌理念 纽崔莱品牌宣言 健康是美好生活的前提 营养是生命的源泉 现代生活方式影响营养吸收 补充营养维护身体健康 大事记 有机种植 自有有机农场 鲑鱼湖农场 墨西哥农场 巴西农场 有机认证标准 植物营养素 植物蕴藏的珍宝 善用植物营养素的力量 膳食的彩虹摄取规则 部分植物原料 生产步骤 科研成果 领先优势 关于营养基因学 植物营养素专利 强大后盾 纽崔莱营养与健康研究中心 安利（中国）研发中心 安利（中国）植物研发中心 纽崔莱科学顾问委员会 荟萃国际精英的全球研发 从种子到成品 优选种子 采集精华 提取调配 优质成品 超高标准 权威认证 信 誉 萃 炼 纽崔莱见证自然的力量 坚信自然的力量 五大热剧创意中插 纽崔莱携手夏至未至开启自然课堂 纽崔莱携手鬼吹灯开启脑洞时间 那年花开月正圆 轩辕剑之汉之云 将军在上 敬请期待 山姆·宏邦中国巡讲 媒体新闻列表 精彩回顾 用心萃炼 只为你赢 热点推荐 纽崔莱与奥运 自然之力 始终助力 热点推荐 首页 早餐其实很重要 专家说：早餐现状面面观 数据看：我国居民早餐状况 不吃早餐危害多 如何科学地加入“早餐党” 营养从早开始专家片 营养资料库 营养从早开始，就这么简单 精彩看点 早餐吃这么多才达标 首页 关于健康生活实验室 健康生活实验室项目起源 常见问题 最新动态 斯坦福大学健康生活实验室 乐纤体重管理-首页 体重管理 刻不容缓 肥胖带来的危害 管理体重 为何不简单 造成肥胖的原因 肥胖的自测 营养中国行 轻松3步 管理体重 4个少 八杯水 半小时 乐纤运动建议 4321操入门版 核心训练进阶版 燃脂魔性操 乐纤资料库 纽崔莱益生菌登陆微购 关于纽崔莱 品牌历史 &gt; &nbsp; 自然精华 &gt; &nbsp; 科学精粹 &gt; &nbsp; 质量监控 &gt; &nbsp;",
                        "tags": "纽崔莱品牌\t纽崔莱产品\t健康",
                        "tagspath": "",
                        "thumbnail": "",
                        "title": "关于纽崔莱",
                        "url": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/about/index.html",
                        "videoid": "",
                        "videoimgsurl": "",
                        "videopath": "",
                        "videoposter": "",
                        "aboreadonly": "0",
                        "allowshare": "0",
                        "displaypublishtime": "0",
                        "modifieddate": "1574303774000",
                        "nosearch": "0",
                        "promotion": "0",
                        "publishdate": "1545067320000",
                        "videolen": "0",
                        "index_name": "t_content"
                    },
                    "property": {},
                    "attribute": {},
                    "variableValue": {},
                    "sortExprValues": [
                        "10000.458670944"
                    ]
                },
                {
                    "fields": {
                        "aemtags": "",
                        "aemtagspath": "",
                        "allowidentities": "",
                        "author": "",
                        "baseid": "",
                        "brands": "",
                        "channel": "",
                        "classes": "",
                        "contenttag": "",
                        "description": "",
                        "favoritetags": "",
                        "host": "bizqa.amwaynet.com.cn",
                        "id": "https://bizqa.amwaynet.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/about/index.html",
                        "keywords": "纽崔莱品牌 , 纽崔莱产品 , 健康 ",
                        "labels": "",
                        "name": "",
                        "people": "",
                        "searchtype": "",
                        "sentiments": "",
                        "source": "",
                        "strippedcontent": "关于纽崔莱 1 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 1 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 1 首页 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 关于纽崔莱 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 首页 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 返回 品牌起源 中国是纽崔莱的起源地 神奇植物-紫花苜蓿 营养补充食品初尝试 探索营养的奥秘 纽崔莱品牌的来历 梦想终于变成现实 品牌理念 纽崔莱品牌宣言 健康是美好生活的前提 营养是生命的源泉 现代生活方式影响营养吸收 补充营养维护身体健康 大事记 有机种植 自有有机农场 鲑鱼湖农场 墨西哥农场 巴西农场 有机认证标准 植物营养素 植物蕴藏的珍宝 善用植物营养素的力量 膳食的彩虹摄取规则 部分植物原料 生产步骤 科研成果 领先优势 关于营养基因学 植物营养素专利 强大后盾 纽崔莱营养与健康研究中心 安利（中国）研发中心 安利（中国）植物研发中心 纽崔莱科学顾问委员会 荟萃国际精英的全球研发 从种子到成品 优选种子 采集精华 提取调配 优质成品 超高标准 权威认证 信 誉 萃 炼 纽崔莱见证自然的力量 坚信自然的力量 五大热剧创意中插 纽崔莱携手夏至未至开启自然课堂 纽崔莱携手鬼吹灯开启脑洞时间 那年花开月正圆 轩辕剑之汉之云 将军在上 敬请期待 山姆·宏邦中国巡讲 媒体新闻列表 精彩回顾 用心萃炼 只为你赢 热点推荐 纽崔莱与奥运 自然之力 始终助力 热点推荐 首页 早餐其实很重要 专家说：早餐现状面面观 数据看：我国居民早餐状况 不吃早餐危害多 如何科学地加入“早餐党” 营养从早开始专家片 营养资料库 营养从早开始，就这么简单 精彩看点 早餐吃这么多才达标 首页 关于健康生活实验室 健康生活实验室项目起源 常见问题 最新动态 斯坦福大学健康生活实验室 乐纤体重管理-首页 体重管理 刻不容缓 肥胖带来的危害 管理体重 为何不简单 造成肥胖的原因 肥胖的自测 营养中国行 轻松3步 管理体重 4个少 八杯水 半小时 乐纤运动建议 4321操入门版 核心训练进阶版 燃脂魔性操 乐纤资料库 纽崔莱益生菌登陆微购 关于纽崔莱 品牌历史 &gt; &nbsp; 自然精华 &gt; &nbsp; 科学精粹 &gt; &nbsp; 质量监控 &gt; &nbsp;",
                        "tags": "纽崔莱品牌\t纽崔莱产品\t健康",
                        "tagspath": "",
                        "thumbnail": "",
                        "title": "关于纽崔莱",
                        "url": "https://bizqa.amwaynet.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/about/index.html",
                        "videoid": "",
                        "videoimgsurl": "",
                        "videopath": "",
                        "videoposter": "",
                        "aboreadonly": "0",
                        "allowshare": "0",
                        "displaypublishtime": "0",
                        "modifieddate": "1574303783000",
                        "nosearch": "0",
                        "promotion": "0",
                        "publishdate": "1545067320000",
                        "videolen": "0",
                        "index_name": "t_content"
                    },
                    "property": {},
                    "attribute": {},
                    "variableValue": {},
                    "sortExprValues": [
                        "10000.4585336149"
                    ]
                },
                {
                    "fields": {
                        "aemtags": "",
                        "aemtagspath": "",
                        "allowidentities": "",
                        "author": "",
                        "baseid": "",
                        "brands": "",
                        "channel": "",
                        "classes": "",
                        "contenttag": "",
                        "description": "",
                        "favoritetags": "",
                        "host": "qa.amway.com.cn",
                        "id": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/index.html",
                        "keywords": "纽崔莱, 纽崔莱品牌信息, 纽崔莱品牌信息-置顶文章",
                        "labels": "",
                        "name": "",
                        "people": "",
                        "searchtype": "",
                        "sentiments": "",
                        "source": "",
                        "strippedcontent": "关于纽崔莱 1 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 1 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 1 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 关于纽崔莱 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 返回 品牌起源 中国是纽崔莱的起源地 神奇植物-紫花苜蓿 营养补充食品初尝试 探索营养的奥秘 纽崔莱品牌的来历 梦想终于变成现实 品牌理念 纽崔莱品牌宣言 健康是美好生活的前提 营养是生命的源泉 现代生活方式影响营养吸收 补充营养维护身体健康 大事记 有机种植 自有有机农场 鲑鱼湖农场 墨西哥农场 巴西农场 有机认证标准 植物营养素 植物蕴藏的珍宝 善用植物营养素的力量 膳食的彩虹摄取规则 部分植物原料 生产步骤 科研成果 领先优势 关于营养基因学 植物营养素专利 强大后盾 纽崔莱营养与健康研究中心 安利（中国）研发中心 安利（中国）植物研发中心 纽崔莱科学顾问委员会 荟萃国际精英的全球研发 从种子到成品 优选种子 采集精华 提取调配 优质成品 超高标准 权威认证 信 誉 萃 炼 纽崔莱见证自然的力量 坚信自然的力量 五大热剧创意中插 纽崔莱携手夏至未至开启自然课堂 纽崔莱携手鬼吹灯开启脑洞时间 那年花开月正圆 轩辕剑之汉之云 将军在上 敬请期待 山姆·宏邦中国巡讲 媒体新闻列表 精彩回顾 用心萃炼 只为你赢 热点推荐 纽崔莱与奥运 自然之力 始终助力 热点推荐 首页 早餐其实很重要 专家说：早餐现状面面观 数据看：我国居民早餐状况 不吃早餐危害多 如何科学地加入“早餐党” 营养从早开始专家片 营养资料库 营养从早开始，就这么简单 精彩看点 早餐吃这么多才达标 首页 关于健康生活实验室 健康生活实验室项目起源 常见问题 最新动态 斯坦福大学健康生活实验室 乐纤体重管理-首页 体重管理 刻不容缓 肥胖带来的危害 管理体重 为何不简单 造成肥胖的原因 肥胖的自测 营养中国行 轻松3步 管理体重 4个少 八杯水 半小时 乐纤运动建议 4321操入门版 核心训练进阶版 燃脂魔性操 乐纤资料库 纽崔莱益生菌登陆微购 纽崔莱 即刻体验 即刻体验 即刻体验 自然精华 &gt; &nbsp; 拥有 可持续的有机耕种体系 善用 植物蕴藏的珍宝 科学精粹 &gt; &nbsp; 科学的 产品提炼生产步骤 前沿的 营养基因学探索 了解详情 &gt; 了解详情 &gt; 了解详情 &gt;",
                        "tags": "纽崔莱\t纽崔莱品牌信息\t纽崔莱品牌信息-置顶文章",
                        "tagspath": "",
                        "thumbnail": "",
                        "title": "纽崔莱",
                        "url": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite.html",
                        "videoid": "",
                        "videoimgsurl": "",
                        "videopath": "",
                        "videoposter": "",
                        "aboreadonly": "0",
                        "allowshare": "0",
                        "displaypublishtime": "0",
                        "modifieddate": "1574303774000",
                        "nosearch": "0",
                        "promotion": "0",
                        "publishdate": "1548462996000",
                        "videolen": "0",
                        "index_name": "t_content"
                    },
                    "property": {},
                    "attribute": {},
                    "variableValue": {},
                    "sortExprValues": [
                        "10000.4584268034"
                    ]
                },
                {
                    "fields": {
                        "aemtags": "",
                        "aemtagspath": "",
                        "allowidentities": "",
                        "author": "",
                        "baseid": "",
                        "brands": "",
                        "channel": "",
                        "classes": "",
                        "contenttag": "",
                        "description": "",
                        "favoritetags": "",
                        "host": "qa.amway.com.cn",
                        "id": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/campaign/probiotics/index.html",
                        "keywords": "",
                        "labels": "",
                        "name": "",
                        "people": "",
                        "searchtype": "",
                        "sentiments": "",
                        "source": "",
                        "strippedcontent": "关于纽崔莱 1 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 1 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 1 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 关于纽崔莱 关于纽崔莱 品牌历史 自然精华 科学精粹 严格品控 权威认证 产品系列 纽崔莱的特别之处 全部产品 营养素补充系列 功能性保健系列 乐纤体重管理系列 儿童成长系列 海外购系列 最新活动和广告 见证自然的力量 纽崔莱追溯网站 坚信自然的力量 纽崔莱与奥运 纽崔莱营养早餐 健康生活实验室 乐纤体重管理 纽崔莱益生菌登陆微购 返回 品牌起源 中国是纽崔莱的起源地 神奇植物-紫花苜蓿 营养补充食品初尝试 探索营养的奥秘 纽崔莱品牌的来历 梦想终于变成现实 品牌理念 纽崔莱品牌宣言 健康是美好生活的前提 营养是生命的源泉 现代生活方式影响营养吸收 补充营养维护身体健康 大事记 有机种植 自有有机农场 鲑鱼湖农场 墨西哥农场 巴西农场 有机认证标准 植物营养素 植物蕴藏的珍宝 善用植物营养素的力量 膳食的彩虹摄取规则 部分植物原料 生产步骤 科研成果 领先优势 关于营养基因学 植物营养素专利 强大后盾 纽崔莱营养与健康研究中心 安利（中国）研发中心 安利（中国）植物研发中心 纽崔莱科学顾问委员会 荟萃国际精英的全球研发 从种子到成品 优选种子 采集精华 提取调配 优质成品 超高标准 权威认证 信 誉 萃 炼 纽崔莱见证自然的力量 坚信自然的力量 五大热剧创意中插 纽崔莱携手夏至未至开启自然课堂 纽崔莱携手鬼吹灯开启脑洞时间 那年花开月正圆 轩辕剑之汉之云 将军在上 敬请期待 山姆·宏邦中国巡讲 媒体新闻列表 精彩回顾 用心萃炼 只为你赢 热点推荐 纽崔莱与奥运 自然之力 始终助力 热点推荐 首页 早餐其实很重要 专家说：早餐现状面面观 数据看：我国居民早餐状况 不吃早餐危害多 如何科学地加入“早餐党” 营养从早开始专家片 营养资料库 营养从早开始，就这么简单 精彩看点 早餐吃这么多才达标 首页 关于健康生活实验室 健康生活实验室项目起源 常见问题 最新动态 斯坦福大学健康生活实验室 乐纤体重管理-首页 体重管理 刻不容缓 肥胖带来的危害 管理体重 为何不简单 造成肥胖的原因 肥胖的自测 营养中国行 轻松3步 管理体重 4个少 八杯水 半小时 乐纤运动建议 4321操入门版 核心训练进阶版 燃脂魔性操 乐纤资料库 纽崔莱益生菌登陆微购 纽崔莱益生菌登陆微购",
                        "tags": "",
                        "tagspath": "",
                        "thumbnail": "/content/china/accl/amwaychina/zh_cn/news/report/report2017/html/images/image.jpg",
                        "title": "纽崔莱益生菌登陆微购",
                        "url": "http://qa.amway.com.cn/content/china/accl/amwaychina/zh_cn/product/nutrilite/campaign/probiotics/index.html",
                        "videoid": "",
                        "videoimgsurl": "",
                        "videopath": "",
                        "videoposter": "",
                        "aboreadonly": "0",
                        "allowshare": "0",
                        "displaypublishtime": "0",
                        "modifieddate": "1574303774000",
                        "nosearch": "0",
                        "promotion": "0",
                        "publishdate": "1537896114000",
                        "videolen": "0",
                        "index_name": "t_content"
                    },
                    "property": {},
                    "attribute": {},
                    "variableValue": {},
                    "sortExprValues": [
                        "10000.456946671"
                    ]
                }
            ],
            "facet": []
        },
        "errors": [],
        "tracer": "",
        "ops_request_misc": "%7B%22request%5Fid%22%3A%22157585969319189455029268%22%2C%22scm%22%3A%2220140713.190013569..%22%7D",
        "topContent": []
    },
    "errorCode": null,
    "errorMessage": null
}
````

#### 异常返回示例

````
{
    "timeStamp": 1575859675818,
    "status": 0,
    "results": {
        "status": "FAIL",
        "request_id": "157585967519733965401318",
        "result": {
            "searchtime": 8.4E-5,
            "total": 0,
            "num": 0,
            "viewtotal": 0,
            "items": [],
            "facet": []
        },
        "errors": [
            {
                "code": 6008,
                "message": "Query syntax error."
            }
        ],
        "tracer": "",
        "topContent": []
    },
    "errorCode": null,
    "errorMessage": null
}
````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>



## API名称：授权api_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/gateway/jwt

方法：GET

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>sdata</td>
<td>HEAD</td>
<td>STRING</td>
<td>是</td>
<td>sign</td>
</tr>
<tr>
<td>audOrganization</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>组织</td>
</tr>
<tr>
<td>audProject</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>项目</td>
</tr>
<tr>
<td>audService</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>服务</td>
</tr>
<tr>
<td>audEnvironment</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>环境</td>
</tr>
<tr>
<td>sendTime</td>
<td>QUERY</td>
<td>STRING</td>
<td>是</td>
<td>时间戳 当前时间</td>
</tr>
</table>
</div>

<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````

````

#### 异常返回示例

````

````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>



## API名称：搜索下拉推荐_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/amway-search/v1/search/[appName]/[suggestName]

方法：GET

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>suggestName</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td></td>
</tr>
<tr>
<td>appName</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td></td>
</tr>
<tr>
<td>token</td>
<td>HEAD</td>
<td>STRING</td>
<td>否</td>
<td></td>
</tr>
<tr>
<td>query</td>
<td>QUERY</td>
<td>STRING</td>
<td>否</td>
<td></td>
</tr>
</table>
</div>

<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````

````

#### 异常返回示例

````

````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>



## API名称：每天批量更新热词_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/amway-search/v1/hot-word/batchAdd

方法：POST

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>token</td>
<td>HEAD</td>
<td>STRING</td>
<td>否</td>
<td>token</td>
</tr>
</table>
</div>

<br />
### *请求Body描述(非Form表单数据)*
[
{ 	
"word":"纽崔莱", 	
"type":"search", 	
"link":"h", 	
"icon":"", 	
"rate":"10", 	
"platform":"aem" 
},
{ 	
"word":"面膜", 	
"type":"search", 	
"link":"h", 	
"icon":"", 	
"rate":"1", 	
"platform":"aem" 
}
]
<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````
{
    "timeStamp": 1574303491515,
    "status": 0,
    "results": true,
    "errorCode": null,
    "errorMessage": null
}
````

#### 异常返回示例

````
{
    "timeStamp": 1574303491515,
    "status": 1,
    "results": null,
    "errorCode": 1000,
    "errorMessage": "系统异常"
}
````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>



## API名称：热词推荐_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/amway-search/v1/hot-word/recommend/[platform]

方法：GET

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>platform</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td>平台名称</td>
</tr>
<tr>
<td>token</td>
<td>HEAD</td>
<td>STRING</td>
<td>否</td>
<td>token</td>
</tr>
</table>
</div>

<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````

````

#### 异常返回示例

````

````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>



## API名称：索引数据_Cloned

### *描述*



### *请求信息*

HTTP协议：HTTP

调用地址：0581542131d34d26ad25b88564493834-cn-shenzhen.alicloudapi.com/amway-search/v1/index/[appName]/[tableName]

方法：POST

<br />
### *请求参数*

<div>
<table>
<tr>
<th style="width: 20%;">名称</th>
<th style="width: 10%;">位置</th>
<th style="width: 10%;">类型</th>
<th style="width: 10%;">必填</th>
<th style="width: 30%;">描述</th>
</tr>
<tr>
<td>tableName</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td>OpenSearch 主表名称</td>
</tr>
<tr>
<td>appName</td>
<td>PATH</td>
<td>STRING</td>
<td>是</td>
<td>OpenSearch 实例名称</td>
</tr>
<tr>
<td>token</td>
<td>HEAD</td>
<td>STRING</td>
<td>否</td>
<td>token</td>
</tr>
</table>
</div>

<br />
### *请求Body描述(非Form表单数据)*
[{
	"cmd":"add",
	"timestamp":{{timestamp}},
	"fields":{
	"id":"https://ch.amwaynet.com.cn/content/china/accl/ch/artistry/brand/2018/0409050.html"   ,
	"title":"暖春出游必备小三件，你知道吗？",
	"description":"这些“法宝”，助你春日出游美美美！",
	"strippedcontent":"工作室名称 工作室欢迎语 亲，您的好友分享了这款产品 点击查看详情 亲，您的好友分享了该系列产品 点击查看详情 亲，您的好友分享了这款产品组合 点击查看详情 相关推荐 工作室名称 工作室欢迎语 亲，您的好友分享了这款产品 点击查看详情 亲，您的好友分享了该系列产品 点击查看详情 亲，您的好友分享了这款产品组合 点击查看详情",
	"searchtype":["ABO","PC"],
	"channel":"",
	"author":"",
	"thumbnail":"/content/dam/china/accl/content_hub/artistry/brand/2018/0409050/0.jpg",
	"classes":"",
	"tags":["小白成长","企业文化","音频"],
	"tagspath":["knowledge-management : KM / KM-A / 企业文化","knowledge-management : KM / KM-D / KM-D-B / 小白成长"," Content Hub : 通用标签 / 内容格式 / 音频"],
	"favoritetags":"",
	"aemtags":["小白成长","企业文化","音频"],
	"aemtagspath":"Content Hub : 通用标签 / 内容格式 / 音频",
	"publishdate":"1574060197381",
	"modifieddate":"1574060197381",
	"nosearch":"1",
	"aboreadonly":"1",
	"allowidentities":["NewInfo","PA_A","PA_B","PA_C","PA_D","PA_E","PA_F","PA_N","PA_P","SA_A","SA_B","SA_C","SA_D","SA_E","SA_F","SA_N","SA_P","SA_G","SA_H","SA_I","SA_J","SA_K","SA_L","SA_M","SA_O","SS_A","SS_B","SS_C","SS_D","SS_E","SS_F","SS_N","SS_P","SS_G","SS_H","SS_I","SS_J","SS_K","SS_L","SS_M","SS_O","PC","SR"],
	"allowshare":"1",
	"promotion":"1",
	"source":"category",
	"host":"ch.amwaynet.com.cn",
	"url":"https://ch.amwaynet.com.cn/content/china/accl/ch/training/cloud-school/ACTI/006.html",
	"contenttag":"培训",
	"keywords":"小白成长, 企业文化, 音频",
	"videoPoster":"/content/dam/china/accl/content_hub/training/cloud-classroom/business-skills/development/0389/1.jpg",
	"videoPath":"https://ch.amway.com.cn/ch/03-YYSZ-FL-07.mp4",
	"videoLen":"60",
	"displayPublishTime":"1"
}
},{
	"cmd": "add",
	"timestamp": 1574846366461,
	"fields": {
		"id": "c581fe3481",
		"title": "TestAmway01",
		"description": null,
		"strippedcontent": null,
		"searchtype": null,
		"channel": null,
		"author": null,
		"thumbnail": null,
		"classes": null,
		"tags": [
			"人",
			"室内",
			"表",
			"墙",
			"树",
			"户外",
			"书",
			"货架",
			"笔记本 电脑",
			"计算机",
			"坐",
			"图书馆",
			"电子",
			"屏幕",
			"适合",
			"文本",
			"白板",
			"姿势",
			"Unknown #2",
			"Unknown #1",
			"Unknown #3",
			"Unknown #4",
			"Neutral",
			"Positive",
			"Baidu",
			"Amway"
		],
		"tagspath": null,
		"favoritetags": null,
		"aemtags": null,
		"aemtagspath": null,
		"publishdate": "1574766955026",
		"modifieddate": "1574766955026",
		"nosearch": null,
		"aboreadonly": "1",
		"allowidentities": null,
		"allowshare": null,
		"promotion": null,
		"source": null,
		"host": "videoindexerexample01.azurewebsites.net",
		"url": "https://videoindexerexample01.azurewebsites.net/Home/Edit/c581fe3481",
		"contenttag": null,
		"keywords": "人,室内,表,墙,树,户外,书,货架,笔记本 电脑,计算机,坐,图书馆,电子,屏幕,适合,文本,白板,姿势,Unknown #2,Unknown #1,Unknown #3,Unknown #4,Neutral,Positive,Baidu,Amway",
		"videoPoster": null,
		"videoPath": null,
		"videoLen": null,
		"displayPublishTime": null,
		"link": "product",
		"lables": [
			"人",
			"室内",
			"表",
			"墙",
			"树",
			"户外",
			"书",
			"货架",
			"笔记本 电脑",
			"计算机",
			"坐",
			"图书馆",
			"电子",
			"屏幕",
			"适合",
			"文本",
			"白板",
			"姿势"
		],
		"peoples": [
			"Unknown #2",
			"Unknown #1",
			"Unknown #3",
			"Unknown #4"
		],
		"sentiment": [
			"Neutral",
			"Positive"
		],
		"brands": [
			"Baidu",
			"Amway"
		],
		"baseid": null
	}
}]
<br />
### *返回信息*

#### 返回参数类型

JSON

#### 返回结果示例

````
{
    "timeStamp": 1574234929331,
    "status": 0,
    "results": {
        "result": "true",
        "traceInfo": {
            "requestId": "157423492919189454232557",
            "tracer": null,
            "setTracer": false,
            "setRequestId": true
        },
        "setTraceInfo": true,
        "setResult": true
    },
    "errorCode": null,
    "errorMessage": null
}
````

#### 异常返回示例

````

````

<br />
### *错误码*

<div>
<table>
<tr>
<th style="width: 15%;">错误码</th>
<th style="width: 20%;">错误信息</th>
<th style="width: 25%;">描述</th>
</tr>
<tr>
<td>公共错误码</td>
<td>--</td>
<td>所有API公用的错误码，请参照《 <a href="#pubError">公共错误码</a> 》</td>
</tr>
</table>
</div>




## <span id='pubError'>公共错误</span>
### 如何获取公共错误
所有的 API 请求只要到达了网关，网关就会返回请求结果信息。

用户需要查看返回结果的头部，即 Header 部分。返回参数如示例：

	//请求唯一ID，请求一旦进入API网关应用后，API网关就会生成请求ID并通过响应头返回给客户端，建议客户端与后端服务都记录此请求ID，可用于问题排查与跟踪
	X-Ca-Request-Id: 7AD052CB-EE8B-4DFD-BBAF-EFB340E0A5AF

	//API网关返回的错误消息，当请求出现错误时API网关会通过响应头将错误消息返回给客户端
	X-Ca-Error-Message: Invalid Url

	//当打开Debug模式后会返回Debug信息，此信息后期可能会有变更，仅用做联调阶段参考
	X-Ca-Debug-Info: {"ServiceLatency":0,"TotalLatency":2}

在 Header 中获得 X-Ca-Error-Message 可以基本明确报错原因，而 X-Ca-Request-Id 可以用于提供给这边的支持人员，供支持人员搜索日志。
### 公共错误码
#### 客户端错误

错误代码|Http 状态码|语义|解决方案
------|-----------|---|------
Throttled by USER Flow Control|403|因用户流控被限制|调用频率过高导致被流控，可以联系 API 服务商协商放宽限制。
Throttled by APP Flow Control|403|因APP流控被限制|调用频率过高导致被流控，可以联系 API 服务商协商放宽限制。
Throttled by API Flow Control|403	|因 API 流控被限制|调用频率过高导致被流控，可以联系 API 服务商协商放宽限制。
Throttled by DOMAIN Flow Control	|403|	因二级域名流控被限制|直接访问二级域名调用 API，每天被访问次数上限1000次。
TThrottled by GROUP Flow Control|403|因分组流控被限制|调用频率过高导致被流控，可以联系 API 服务商协商放宽限制。
Quota Exhausted	|403|	调用次数已用完	|购买的次数已用完。
Quota Expired	|403|	购买次数已过期	|购买的次数已经过期。
User Arrears	|403|	用户已欠费	|请尽快充值续费。
Empty Request Body	|400|	body 为空|	请检查请求 Body 内容。
Invalid Request Body	|400	|body 无效	|请检查请求 Body。
Invalid Param Location	|400|	参数位置错误|请求参数位置错误。
Unsupported Multipart	|400|	不支持上传|不支持上传文件。
Invalid Url	|400	|Url 无效	|请求的 Method、Path 或者环境不对。请参照错误说明 Invalid Url。
Invalid Domain	|400|	域名无效	|请求域名无效，根据域名找不到 API。请联系 API 服务商。
Invalid HttpMethod	|400	|HttpMethod 无效|输入的 Method 不合法。
Invalid AppKey|400|AppKey 无效或不存在	|请检查传入的 AppKey。注意左右空格的影响。
Invalid AppSecret	|400	|APP 的Secret 错误|	检查传入的 AppSecret。注意左右空格的影响。
Timestamp Expired|400|时间戳过时|请核对请求系统时间是否为标准时间。
Invalid Timestamp	|400|	时间戳不合法|请参照 请求签名说明文档。
Empty Signature	|404|签名为空|请传入签名字符串，请参照 请求签名说明文档。
Invalid Signature, Server StringToSign:%s|400|签名无效|签名无效，参照 Invalid Signature 错误说明
Invalid Content-MD5|400|	Content-MD5 值不合法|请求 Body 为空，但传入了 MD5 值，或 MD5 值计算错误。请参照 请求签名说明文档。
Unauthorized	|403|	未被授权|	APP 未获得要调用的 API 的授权。请参照错误说明 Unauthorized。
Nonce Used|400|	SignatureNonce| 已被使用|SignatureNonce 不能被重复使用。
API Not Found|	400	|找不到 API|传入的APIdi地址或者HttpMethod不正确，或已下线。

#### 服务器端错误（调用 API）
以下为API服务端错误，如果频繁错误，可联系服务商。

错误代码|Http状态码|语义|解决方案
------|----------|---|----
Internal Error	|500	|内部错误|建议重试,或者联系服务商
Failed To Invoke Backend Service	|500|	底层服务错误|API 提供者底层服务错误，建议重试，如果重试多次仍然不可用，可联系 API 服务商解决
Service Unavailable|503|	服务不可用	|建议重试,或者联系服务商
Async Service	|504	|后端服务超时|建议重试,或者联系服务商

