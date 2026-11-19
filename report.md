# ches2010_tvbox TVBox 源整合报告

- 源项目: https://github.com/ches2010/tvbox (分支 `master`)
- 抓取规则参考: [FongMi/CatVodSpider](https://github.com/FongMi/CatVodSpider)
- 整合文件: `ches2010_tvbox_merged.json` (md5: `b3e0a8d6c8eb602badbf8aed337d7a7f`)

## 一、整合文件使用说明

1. 将 `ches2010_tvbox_merged.json` 放入 TVBox/OK影视 配置目录，或部署为可直链 URL 后在应用内"自定义"添加该 URL。
2. 全局蜘蛛 jar 已内联为绝对地址（见下表），配置自包含，无需额外文件。
3. 部分源使用站点级 `jar` 字段（各自引擎的蜘蛛 jar），已一并改写为绝对地址。
4. 本整合包含 18+ 与直播源，请自行判断合规性，勿用于商业用途。

## 二、抓取规则说明（CatVodSpider 蜘蛛协议）

TVBox(OK影视/FengMi TV) 的蜘蛛抓取协议由 CatVodSpider 定义，本整合中所有 `csp_*` 与 `type=1` 采集源均遵循该协议：

| 项 | 说明 |
| --- | --- |
| `spider` | `<jar地址>;md5;<md5>`，蜘蛛 jar 包（CatVodSpider 打包产物 custom_spider.jar 或第三方兼容 jar） |
| 入口 | `init(extend)` / `homeContent(filter)` / `categoryContent(tid,pg,filter,extend)` / `searchContent(key,quick,pg)` / `detailContent(ids)` / `playerContent(flag,id,vipFlags)` / `proxy(params)` |
| 返回 | 列表: `{class, filters, list[]}`；详情/播放: `VOD` 字段 |
| VOD 字段 | `vod_id, vod_name, vod_pic, vod_remarks, vod_actor, vod_director, vod_class, vod_content, vod_play_from, vod_play_url, vod_play_id, vod_year, vod_area, vod_lang, vod_score, vod_time, vod_from` |
| 播放值 | `线路$$$线路` 分隔，集间 `#`，每集 `名称$播放值` |
| `type` | 1=JSON/XML 采集接口(jar 内置通用采集器)；2=网页抓取(XPath/XBPQ)；3=蜘蛛(csp_ 类 / js / py) |
| `ext` | 蜘蛛扩展参数：JSON 对象、`$$$` 分隔串或过滤文件路径 |
| `sites` 字段 | `key, name, type, api, ext, searchable, quickSearch, changeable, filterable, timeout, jar(可选,站点级) |
| 采集接口 | 标准 v10 格式 `api.php/provide/vod/...`，jar 通用采集器直接转 VOD |

各引擎 jar 覆盖的 `csp_*` 类不同，整合时按源原属配置为其补齐站点级 `jar`，避免类缺失。

## 三、蜘蛛 Jar 清单

| Jar | 大小 | MD5 | 状态 |
| --- | --- | --- | --- |
| `jar/fan.txt` | 1.10MB | `c62c307314178af8637480fcb2fe61f7` | ✅ md5 已校验 ⬅全局 |
| `jar/pg.jar` | 2.56MB | `dffec63fb83a2b31b8606bd4bf6bead9` | ✅ md5 已校验 |
| `jar/XYQ.jar` | 0.38MB | `705ba6e23c4384c37a11dd904727520b` | ✅ md5 已校验 |
| `xiaosa/spider.jar` | 1.84MB | `66bf5c12bf2e384214235e9996e9fee9` | ✅ md5 已校验 |
| `http://我不是.摸鱼儿.top/by/jar/moyu.php` | - | `0a9c6b44a20445a9b3c2a39763ef7cf1` | ⚠️ 配置内 md5（external） |
| `jar/custom_spider.jar` | 0.25MB | `41c87635d7592069884a5dafa12acabe` | ✅ md5 已校验 |
| `jar/pg23a94bb.jar` | 10.96MB | `73b12022e75e5139dc39e9690727021c` | ⚠️ 配置内 md5（too-large） |
| `jar/sx.jar` | 1.74MB | `6186f490eadf878ba5de21a7fa29e594` | ✅ md5 已校验 |
| `jar/Yoursmile.jar` | 1.40MB | `97796a79a781055669f2d5442439dc5f` | ✅ md5 已校验 |
| `jar/XBPQ.jar` | 0.38MB | `4145467d49e0437dc43e8bbb79e3ff7d` | ✅ md5 已校验 |
| `http://我不是.摸鱼儿.top/by/jar/moyuer.php` | - | `b752cad81649bf070c934c423c3a1b86` | ⚠️ 配置内 md5（external） |

全局 spider: `https://raw.githubusercontent.com/ches2010/tvbox/master/jar/fan.txt;md5;c62c307314178af8637480fcb2fe61f7`

## 四、整合统计

- 参与配置（优先级从高到低）: dianshi.json, 0821.json, js.json, 0825.json, XYQ.json, xiaosa/api.json, 367.json, 0827.json, 9918.json, 99188.json, 0826.json, jsm.json
- 站点: 合计 **741** 个（按 key 去重，丢弃重复 639 条）
- 直播: 37 条；解析: 48 条；规则: 49 条；doh: 7；ads: 63；flags: 61

**按引擎**: jar-csp(站点级jar) 337，js-drpy 265，collect 69，jar-csp 51，py 12，other 7

**按来源配置**: js.json 240，dianshi.json 132，xiaosa/api.json 120，9918.json 70，367.json 49，0821.json 43，XYQ.json 40，0825.json 37，0827.json 9，0826.json 1

> 注：XYQ.json 原文件含 `//` 注释或尾逗号（非严格 JSON），已按宽松 JSON 解析后参与整合。

## 五、站点明细

### jar-csp（51）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 👽玩偶哥哥┃4K弹幕 | 玩偶 | `csp_WoGGGuard` ext=json-ext | dianshi.json |
| 2 | 🚀叨观荐影┃预告片 | YGP | `csp_YGPGuard` | dianshi.json |
| 3 | 📽️一直播┃直播 | alllive | `csp_AllliveGuard` | dianshi.json |
| 4 | 💡聚剧剧┃三盘 | seed | `csp_SeedhubGuard` ext=5++kwLhNYm9UrO9wh7Dl7eKamTee4s | dianshi.json |
| 5 | 🐟白白┃秒播 | 白白 | `csp_SbaibaiGuard` | dianshi.json |
| 6 | 📔厂长┃不卡 | 厂长 | `csp_NewCzGuard` | dianshi.json |
| 7 | 🎃南瓜┃不卡 | 南瓜 | `csp_NanGuaGuard` | dianshi.json |
| 8 | ☀原创┃不卡 | 原创 | `csp_YCyzGuard` | dianshi.json |
| 9 | 🍎苹果┃不卡 | 苹果 | `csp_LiteAppleGuard` | dianshi.json |
| 10 | 🍓糯米┃秒播 | 糯米 | `csp_NmyswvGuard` | dianshi.json |
| 11 | 💮文采┃秒播 | 文采 | `csp_JpysGuard` | dianshi.json |
| 12 | 🌟立播┃秒播 | Lib | `csp_LibvioGuard` ext=json-ext | dianshi.json |
| 13 | 🍊在线┃秒播 | zxzj | `csp_ZxzjGuard` ext=https://www.zxzjhd.com/ | dianshi.json |
| 14 | 🍄比特┃手机 | 比特 | `csp_BttwooGuard` | dianshi.json |
| 15 | ⏮️低端┃外剧 | 低端 | `csp_DdrkGuard` | dianshi.json |
| 16 | 👀萌米┃多线 | 萌米 | `csp_AppTTGuard` ext=uqGL1bNENEIVq+dC1p/Y9uWjuA== | dianshi.json |
| 17 | 📺热播┃多线 | 热播 | `csp_AppTTGuard` ext=uqGL1bNENExT7/hGxpSE5qU= | dianshi.json |
| 18 | 👓欢视┃多线 | 欢视 | `csp_AppTTGuard` ext=uqGL1bNENExT9fFAy5mE5qU= | dianshi.json |
| 19 | 🏝奥特┃多线 | 奥特 | `csp_AueteGuard` ext=https://auete.com/ | dianshi.json |
| 20 | 🐭荐片┃P2P | 荐片 | `csp_JPJGuard` | dianshi.json |
| 21 | 🧲新6V┃磁力 | 新6V | `csp_SixVGuard` ext=https://www.xb6v.com/ | dianshi.json |
| 22 | 🚌巴士┃动漫 | Dm84 | `csp_Dm84Guard` | dianshi.json |
| 23 | 🎀异界┃动漫 | Ysj | `csp_YsjGuard` | dianshi.json |
| 24 | 🐾日本┃动漫 | Anime1 | `csp_Anime1Guard` | dianshi.json |
| 25 | ⚽926┃看球 | 926 | `csp_kanqiu926Guard` ext=uqGL1fpJNAUa4uUHi9iMsef1+C/R | dianshi.json |
| 26 | ⚽88┃看球 | 88 | `csp_Sir88Guard` | dianshi.json |
| 27 | ⚽看球┃直播 | 看球 | `csp_KanqiuGuard` | dianshi.json |
| 28 | 🎧明星┃MV | MTV | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 29 | 🚑急救┃教学 | Aid | `csp_FirstAidGuard` | dianshi.json |
| 30 | 🍄抠抠┃搜搜 | 抠搜 | `csp_KkSsGuard` ext=json-ext | dianshi.json |
| 31 | 🌈优汐┃搜搜 | UC | `csp_UuSsGuard` ext=json-ext | dianshi.json |
| 32 | 🛴手机┃推送 | push_agent | `csp_PushGuard` ext=json-ext | dianshi.json |
| 33 | 🅱哔哔合集┃弹幕 | Bili | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 34 | 🅱哔哔演唱会┃弹幕 | Biliych | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 35 | 📚少儿┃教育 | 少儿教育 | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 36 | 📚小学┃课堂 | 小学课堂 | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 37 | 📚初中┃课堂 | 初中课堂 | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 38 | 📚高中┃课堂 | 高中教育 | `csp_BiliGuard` ext=json-ext | dianshi.json |
| 39 | 【许半仙】 | 豆 | `csp_DouDouGuard` | 0821.json |
| 40 | 🐻小熊┃秒播 | 神车 | `csp_AppSKGuard` ext=rfOX1voDIQhH8epBwtCFsq+8syyZ18 | 0821.json |
| 41 | 💡流光┃秒播 | 溢彩 | `csp_AppSxGuard` ext=rfOb1uAWbkRHp7hdxprG9un3+T/f19 | 0821.json |
| 42 | 🍊水星┃多线 | 兄弟 | `csp_AppSxGuard` ext=rfOb1uAWbkRHp7hdxprG9un3+SLP18 | 0821.json |
| 43 | 🐭荐片┃P2P | 贱贱 | `csp_JPJGuard` | 0821.json |
| 44 | 🐟盘她┃三盘 | YpanSo | `csp_YpanSoGuard` ext=json-ext | 0821.json |
| 45 | 👻盘它┃三盘 | xzso | `csp_XzsoGuard` ext=json-ext | 0821.json |
| 46 | 🦋米搜┃夸父 | 米搜 | `csp_MIPanSoGuard` ext=json-ext | 0821.json |
| 47 | 😻夸搜┃夸父 | 夸搜 | `csp_PanSearchGuard` ext=json-ext | 0821.json |
| 48 | 🙀盘搜┃阿狸 | Aliso | `csp_PanSearchGuard` ext=json-ext | 0821.json |
| 49 | 😹易搜┃阿狸 | YiSo | `csp_YiSoGuard` ext=json-ext | 0821.json |
| 50 | AList┃网盘 | csp_AList | `csp_AList` ext=https://raw.githubusercontent. | 0821.json |
| 51 | 导航www.饭太硬.com | fan | `csp_XPathGuard` | 0826.json |

### jar-csp(站点级jar)（337）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 飞宇影院 | fyyy | `csp_XBPQ` ext=json-ext | dianshi.json |
| 2 | 茶杯狐 | cbh | `csp_XBPQ` ext=json-ext | dianshi.json |
| 3 | 热播｜APP | 热播影视 | `csp_AppRJ` ext=json-ext | dianshi.json |
| 4 | 天天｜APP | 天天影视 | `csp_AppRJ` ext=json-ext | dianshi.json |
| 5 | 浪酷｜APP | 浪酷影视 | `csp_AppRJ` ext=json-ext | dianshi.json |
| 6 | 星河｜APP | 星河 | `csp_AppSy` ext=json-ext | dianshi.json |
| 7 | 追忆｜APP | 追忆 | `csp_AppSy` ext=json-ext | dianshi.json |
| 8 | 乐达｜APP | 乐达 | `csp_AppGet` ext=json-ext | dianshi.json |
| 9 | 灵虎｜APP | 灵虎 | `csp_AppGet` ext=json-ext | dianshi.json |
| 10 | 旗星｜APP | 旗星 | `csp_AppGet` ext=json-ext | dianshi.json |
| 11 | 云云｜APP | 云云 | `csp_AppGet2` ext=json-ext | dianshi.json |
| 12 | 光影｜APP | 光影 | `csp_AppGet` ext=json-ext | dianshi.json |
| 13 | 火猫丨APP | 火猫 | `csp_AppGet` ext=json-ext | dianshi.json |
| 14 | 若雪丨APP | 若雪 | `csp_AppGet2` ext=json-ext | dianshi.json |
| 15 | 丫丫｜APP | 丫丫 | `csp_AppGet` ext=json-ext | dianshi.json |
| 16 | 爱看丨APP | 爱看 | `csp_AppGet` ext=json-ext | dianshi.json |
| 17 | 蓝光｜APP | 蓝光 | `csp_AppGet` ext=json-ext | dianshi.json |
| 18 | 豆丁｜APP | 豆丁 | `csp_AppGet` ext=json-ext | dianshi.json |
| 19 | 蓝鹰｜APP | 蓝鹰 | `csp_AppGet2` ext=json-ext | dianshi.json |
| 20 | 仓鼠｜APP | 仓鼠 | `csp_AppGet2` ext=json-ext | dianshi.json |
| 21 | 鲸鱼｜APP | 鲸鱼 | `csp_AppGet2` ext=json-ext | dianshi.json |
| 22 | 米诺｜APP | 米诺 | `csp_AppGet` ext=json-ext | dianshi.json |
| 23 | 云速｜APP | 云速 | `csp_AppGet` ext=json-ext | dianshi.json |
| 24 | 瓜萌｜APP | 瓜萌 | `csp_AppGet` ext=json-ext | dianshi.json |
| 25 | 晴天｜APP | 晴天 | `csp_AppGet` ext=json-ext | dianshi.json |
| 26 | 橘子｜APP | 橘子 | `csp_AppGet` ext=json-ext | dianshi.json |
| 27 | 萝卜｜APP | 萝卜 | `csp_AppGet` ext=json-ext | dianshi.json |
| 28 | 米兔｜APP | 米兔 | `csp_AppGet` ext=json-ext | dianshi.json |
| 29 | 小红｜APP | 小红 | `csp_AppGet` ext=json-ext | dianshi.json |
| 30 | 白蛇｜APP | 白蛇 | `csp_AppGet` ext=json-ext | dianshi.json |
| 31 | 若惜｜APP | 若惜 | `csp_AppGet` ext=json-ext | dianshi.json |
| 32 | 外剧｜APP | 外剧 | `csp_AppGet` ext=json-ext | dianshi.json |
| 33 | 移动｜APP | 移动 | `csp_YD` | dianshi.json |
| 34 | 影视 | ️奇优[直连] | 奇优 | `csp_XBPQ` ext=https://raw.githubusercontent. | dianshi.json |
| 35 | 搜索 | TG[jar] | TGYunPanLocal | `csp_TGYunPanLocal` ext=https://raw.githubusercontent. | js.json |
| 36 | 视频 | Youtube[jar] | Youtube | `csp_Youtube` ext=json-ext | js.json |
| 37 | 科普 | 急救[jar] | FirstAid | `csp_FirstAid` | js.json |
| 38 | 配置 | 中心[jar] | 配置中心 | `csp_Config` ext=https://raw.githubusercontent. | js.json |
| 39 | 音频 | 酷狗[jar] | 酷狗 | `csp_Kugou` | js.json |
| 40 | MV | KTV[jar] | Iktv | `csp_Iktv` | js.json |
| 41 | MV | 音悦台[jar] | Yinyuetai | `csp_Yinyuetai` | js.json |
| 42 | 书籍 | 分享[jar] | AliShareEBook | `csp_AliShare` ext=https://raw.githubusercontent. | js.json |
| 43 | 弹幕 | 荐片[jar] | 荐片弹幕版 | `csp_Jianpian` ext=https://raw.githubusercontent. | js.json |
| 44 | 弹幕 | 哔滴[jar] | Bdys_spider | `csp_Bdys01` ext=https://raw.githubusercontent. | js.json |
| 45 | 弹幕 | 天天[jar] | 天天 | `csp_TTian` ext=http://op.ysdqjs.cn$$$null$$$1 | js.json |
| 46 | 弹幕 | 追剧[jar] | 追剧 | `csp_TTian` ext=http://app.kzjtv.com$$$null$$$ | js.json |
| 47 | 弹幕 | 南瓜[jar] | NanGua | `csp_NanGua` ext=null$$$1 | js.json |
| 48 | 弹幕 | 资源分享[jar] | PushShare | `csp_PushShare` ext=https://raw.githubusercontent. | js.json |
| 49 | 弹幕 | PikPak分享[jar] | PikPakShare | `csp_PikPakShare` ext=https://raw.githubusercontent. | js.json |
| 50 | 弹幕 | 影视分享[jar] | AliShare | `csp_AliShare` ext=https://raw.githubusercontent. | js.json |
| 51 | 弹幕 | 迅雷分享[jar] | ThunderShare | `csp_ThunderShare` ext=https://raw.githubusercontent. | js.json |
| 52 | 弹幕 | 夸克分享[jar] | QuarkShare | `csp_QuarkShare` ext=https://raw.githubusercontent. | js.json |
| 53 | 弹幕 | 115分享[jar] | 115Share | `csp_P115Share` ext=https://raw.githubusercontent. | js.json |
| 54 | 弹幕 | UC分享[jar] | UCShare | `csp_UCShare` ext=https://raw.githubusercontent. | js.json |
| 55 | 弹幕 | Samba分享[jar] | SambaShare | `csp_SambaShare` ext=https://raw.githubusercontent. | js.json |
| 56 | 弹幕 | 玩偶[jar] | Wogg | `csp_Wogg` ext=https://raw.githubusercontent. | js.json |
| 57 | 弹幕 | 表哥[jar] | Wobg | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 58 | 弹幕 | 老哥[jar] | yydsys | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 59 | 弹幕 | 蜡笔[jar] | 蜡笔网盘 | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 60 | 网盘 | 木偶哥哥[jar] | Mogg | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 61 | 网盘 | 土豆[jar] | 土豆 | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 62 | 网盘 | 小米[jar] | 小米网盘 | `csp_Wobg` ext=https://raw.githubusercontent. | js.json |
| 63 | 弹幕 | 新视觉[jar] | Xinshijue | `csp_Xinshijue` ext=https://raw.githubusercontent. | js.json |
| 64 | 弹幕 | HDmoli[jar] | Moli | `csp_Moli` ext=https://raw.githubusercontent. | js.json |
| 65 | 弹幕 | 不太灵[jar] | Bt0 | `csp_Bt0` ext=https://raw.githubusercontent. | js.json |
| 66 | 弹幕 | 低端[jar] | ddys | `csp_Ddys` ext=https://raw.githubusercontent. | js.json |
| 67 | 弹幕 | 美剧迷[jar] | 美剧迷 | `csp_Meijumi` ext=https://raw.githubusercontent. | js.json |
| 68 | 弹幕 | 皮皮虾[jar] | Ppxzy | `csp_Ppxzy` ext=https://raw.githubusercontent. | js.json |
| 69 | 弹幕 | 4KHDR[jar] | 4KHDR | `csp_FourKHDR` ext=https://raw.githubusercontent. | js.json |
| 70 | 弹幕 | 校长[jar] | 校长影视 | `csp_XiaoZhang` ext=https://raw.githubusercontent. | js.json |
| 71 | 弹幕 | 团长[jar] | 团长资源 | `csp_TZFile` ext=https://raw.githubusercontent. | js.json |
| 72 | 弹幕 | 影巢[jar] | Hdhive | `csp_Hdh` ext=https://raw.githubusercontent. | js.json |
| 73 | 弹幕 | 盘Ta[jar] | Panta | `csp_Panta` ext=https://raw.githubusercontent. | js.json |
| 74 | 弹幕 | 云盘[jar] | YunPan | `csp_YunPan` ext=https://raw.githubusercontent. | js.json |
| 75 | 弹幕 | 玩偶4k[jar] | Wo4k | `csp_Wo4k` ext=https://raw.githubusercontent. | js.json |
| 76 | 弹幕 | 小盘盘[jar] | Xpanpan | `csp_Xpanpan` ext=https://raw.githubusercontent. | js.json |
| 77 | 弹幕 | 高清网[jar] | Gaoqing | `csp_Gaoqing` ext=https://raw.githubusercontent. | js.json |
| 78 | 弹幕 | 音范丝[jar] | 音范丝 | `csp_Gaoqing` ext=https://raw.githubusercontent. | js.json |
| 79 | 网盘 | TG搜索[jar] | TGYunPan | `csp_TGYunPan` ext=https://raw.githubusercontent. | js.json |
| 80 | 磁力 | 影搜[jar] | YingSo | `csp_YingSo` ext=https://raw.githubusercontent. | js.json |
| 81 | 网盘 | 盘友圈 | 盘友圈 | `csp_Panyq` ext=https://raw.githubusercontent. | js.json |
| 82 | 网盘 | 88网盘[jar] | 88Pan | `csp_EightEight` ext=https://raw.githubusercontent. | js.json |
| 83 | 网盘 | 皮卡搜[jar] | PikaSo | `csp_PikaSo` ext=https://raw.githubusercontent. | js.json |
| 84 | 网盘 | 混合盘[jar] | csp_HunHePan | `csp_HunHePan` ext=https://raw.githubusercontent. | js.json |
| 85 | 网盘 | 秒搜[jar] | 秒搜 | `csp_MiaoSou` ext=https://raw.githubusercontent. | js.json |
| 86 | 网盘 | LIBVIO[jar] | Libvio | `csp_Libvio` ext=https://raw.githubusercontent. | js.json |
| 87 | 网盘 | 大盘搜[jar] | DaPanSo | `csp_DaPanSo` ext=https://raw.githubusercontent. | js.json |
| 88 | 网盘 | 千帆[jar] | Qianfan | `csp_Qianfan` ext=https://raw.githubusercontent. | js.json |
| 89 | 网盘 | 小云[jar] | YunSo | `csp_YunSo` ext=https://raw.githubusercontent. | js.json |
| 90 | 网盘 | 云盘[jar] | YunPanOne | `csp_YunPanOne` ext=https://raw.githubusercontent. | js.json |
| 91 | 阿里 | 纸条[jar] | Gitcafe | `csp_Gitcafe` ext=https://raw.githubusercontent. | js.json |
| 92 | 阿里 | 小紙條[jar] | 小紙條 | `csp_XiaoZhiTiao` ext=https://raw.githubusercontent. | js.json |
| 93 | 阿里 | 七夜[jar] | 七夜 | `csp_Dovx` ext=https://raw.githubusercontent. | js.json |
| 94 | 阿里 | 盤搜索[jar] | 盤搜索 | `csp_PanSearch` ext=https://raw.githubusercontent. | js.json |
| 95 | 阿里 | 找資源[jar] | 找資源 | `csp_Zhaozy` ext=https://raw.githubusercontent. | js.json |
| 96 | 阿里 | 易搜[jar] | 易搜 | `csp_YiSo` ext=https://raw.githubusercontent. | js.json |
| 97 | 阿里 | 盘搜[jar] | 盘搜 | `csp_PanSou` ext=https://raw.githubusercontent. | js.json |
| 98 | 阿里 | up云搜[jar] | UP云搜 | `csp_UpYun` ext=https://raw.githubusercontent. | js.json |
| 99 | 夸克  | 趣盘搜[jar] | Funletu | `csp_Funletu` ext=https://raw.githubusercontent. | js.json |
| 100 | 夸克 | 盘搜[jar] | QuarkPanso | `csp_QuarkPanso` ext=https://raw.githubusercontent. | js.json |
| 101 | 夸克 | 推送 | push_quark | `csp_Quark` ext=http://127.0.0.1:9978/file/TV/ | js.json |
| 102 | 應用商店 | 應用商店 | `csp_Market` ext=https://raw.githubusercontent. | js.json |
| 103 | 预告片 | csp_YGP | `csp_YGP` | 0825.json |
| 104 | 哔哩 | csp_Bili | `csp_Bili` ext=json-ext | 0825.json |
| 105 | 本地 | Local | `csp_Local` ext=null$$$null$$$1 | 0825.json |
| 106 | 企鹅 | csp_XPath_企鹅体育 | `csp_XPath` ext=https://raw.githubusercontent. | 0825.json |
| 107 | 爱看 | Ikanbot | `csp_Ikanbot` ext=https://v.ikanbot.com$$$proxy | 0825.json |
| 108 | 迅雷吧 | csp_Xunlei8 | `csp_Xunlei8` | 0825.json |
| 109 | 美剧天天 | MeijuTT | `csp_MeijuTT` | 0825.json |
| 110 | SeedHub | SeedHub | `csp_SeedHub` | 0825.json |
| 111 | 蚂蚁 | Mayiys | `csp_Mayiys` | 0825.json |
| 112 | 快看 | csp_Kuaikan | `csp_Kuaikan` | 0825.json |
| 113 | 泥巴 | 泥巴 | `csp_NiNi` | 0825.json |
| 114 | 星星 | 星星 | `csp_Star` | 0825.json |
| 115 | 影视 | 影视 | `csp_YingShi` | 0825.json |
| 116 | 櫻花 | 櫻花 | `csp_Ying` | 0825.json |
| 117 | 巴士 | 巴士 | `csp_Dm84` | 0825.json |
| 118 | 獨播 | 獨播 | `csp_XPathMacFilter` ext=https://raw.githubusercontent. | 0825.json |
| 119 | 小白 | 小白 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 120 | BT天堂 | BT天堂 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 121 | 布谷4K | 布谷tv | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 122 | 好恐怖 | haokongbu | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 123 | 圣城1080 | 圣城1080 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 124 | 秋霞 | 秋霞 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 125 | VidHub | VidHub | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 126 | rarbt | rarbt | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 127 | 文才 | XBPQ_文才 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 128 | 白嫖影视 | XBPQ_BaiPiao | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 129 | 黑狐影院 | XBPQ_黑狐影院 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 130 | 电影先生 | XBPQ_电影先生 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 131 | 麻衣影院 | XBPQ_麻衣影院 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 132 | 相声 | 相声 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 133 | 评书 | 评书 | `csp_XBPQ` ext=https://raw.githubusercontent. | 0825.json |
| 134 | 网盘 | AList | `csp_AList` ext=https://raw.githubusercontent. | 0825.json |
| 135 | 云盘 | WebDAV | `csp_WebDAV` ext=https://raw.githubusercontent. | 0825.json |
| 136 | 💻本地视频 | csp_LocalFile | `csp_LocalFile` | XYQ.json |
| 137 | 📀找资源(搜索) | csp_Zhaozy | `csp_Zhaozy` ext={"username":"fanty","password" | XYQ.json |
| 138 | 📀喵狸盘搜(搜索) | csp_AliPS | `csp_AliPS` ext=http://127.0.0.1:9978/file/XYQ | XYQ.json |
| 139 | 📀易搜(搜索) | csp_Yisou | `csp_Yisou` ext=http://127.0.0.1:9978/file/XYQ | XYQ.json |
| 140 | 📀UP云搜(搜索) | csp_Upyunso | `csp_Upyunso` ext=http://127.0.0.1:9978/file/XYQ | XYQ.json |
| 141 | 📀影搜(XYQH) | csp_XYQHiker_影搜 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 142 | 📀玩偶哥哥(优) | csp_XYQHikerAL_玩偶哥哥 | `csp_XYQHikerAL` ext=https://raw.githubusercontent. | XYQ.json |
| 143 | 📀云盘资源(搜索) | csp_XYQHikerAL_云盘资源 | `csp_XYQHikerAL` ext=https://raw.githubusercontent. | XYQ.json |
| 144 | 🧲Mp4电影 | csp_Mp4Mov | `csp_Mp4Mov` ext=https://www.padmp4.com | XYQ.json |
| 145 | 🧲新6V | csp_New6v | `csp_New6v` ext=https://www.xb6v.com | XYQ.json |
| 146 | 🧲SeedHub | csp_SeedHub | `csp_SeedHub` ext=https://www.seedhub.cc | XYQ.json |
| 147 | 🧲酷吧电影 | csp_KubaCL | `csp_KubaCL` ext=https://www.kubady2.com | XYQ.json |
| 148 | 🧲美剧天堂 | csp_MeijuTT | `csp_MeijuTT` ext=https://www.meijutt.net | XYQ.json |
| 149 | 🧲美剧迷 | csp_MeijuMi | `csp_MeijuMi` | XYQ.json |
| 150 | 🐞360影视 | 360_spider | `csp_SP360` | XYQ.json |
| 151 | 🐞厂长资源 | csp_Czsapp | `csp_Czsapp` ext=https://www.czys.top | XYQ.json |
| 152 | 🐞哔嘀影视 | csp_Bdys | `csp_Bdys` ext=https://www.yjys02.com | XYQ.json |
| 153 | 🧲哔嘀影视(XYQH) | csp_XYQHiker_哔嘀影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 154 | 🐞低端影视 | csp_Ddys | `csp_Ddys` | XYQ.json |
| 155 | 🔨荐片 | csp_JianPian | `csp_JianPian` ext=http://39.108.238.168:20000 | XYQ.json |
| 156 | 👾Ikanbot | csp_Ikanbot | `csp_Ikanbot` ext=https://v.aikanbot.com | XYQ.json |
| 157 | 兔小贝(XYQH) | csp_XYQHiker_兔小贝 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 158 | 兔小贝2(XYQH) | csp_XYQHiker_兔小贝2 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 159 | 播视童趣(XYQH) | csp_XYQHiker_播视童趣 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 160 | 🐞哔哩幼儿 | csp_Bili幼儿 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 161 | 🐞哔哩少儿 | csp_Bili少儿 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 162 | 🐞哔哩小学 | csp_Bili小学 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 163 | 🐞哔哩初中 | csp_Bili初中 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 164 | 🐞哔哩高中 | csp_Bili高中 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 165 | 🐞哔哩短剧 | csp_Bili短剧 | `csp_Bili` ext=https://raw.githubusercontent. | XYQ.json |
| 166 | 爱你短剧 | csp_XYQHiker_爱你短剧 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 167 | 河马短剧 | csp_XYQHiker_河马短剧 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 168 | 短剧屋 | csp_XYQHiker_短剧屋 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 169 | 🧑🏻‍农民影视 | csp_XYQHiker_农民影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 170 | 七新影视(XYQH) | csp_XYQHiker_七新影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 171 | 可可影视(XYQH) | csp_XYQHiker_可可影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 172 | 📡JustLive | csp_Yj1211 | `csp_Yj1211` | XYQ.json |
| 173 | 🐯虎牙直播(XYQH) | csp_XYQHiker_虎牙直播 | `csp_XYQHiker` ext=https://raw.githubusercontent. | XYQ.json |
| 174 | 新片｜预告 | 预告 | `csp_YGP` | xiaosa/api.json |
| 175 | 本地｜视频 | 本地 | `csp_LocalFile` | xiaosa/api.json |
| 176 | 哔哩｜视频 | 哔哩视频 | `csp_BiliYS` ext=json-ext | xiaosa/api.json |
| 177 | 三六零｜视频 | 三六零 | `csp_SP360` | xiaosa/api.json |
| 178 | 快映｜4K弹幕 | 小斑快映 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 179 | 玩偶｜4K弹幕 | 玩偶哥哥 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 180 | 木偶｜4K弹幕 | 木偶 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 181 | 蜡笔｜4K弹幕 | 蜡笔 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 182 | 至臻｜4K弹幕 | 至臻 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 183 | 多多｜4K弹幕 | 多多 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 184 | 欧哥｜4K弹幕 | 欧哥 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 185 | 二小｜4K弹幕 | 二小 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 186 | 虎斑｜4K弹幕 | 虎斑 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 187 | 闪电｜4K弹幕 | 闪电 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 188 | 百家｜4K弹幕 | 百家 | `csp_PanWebShare` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 189 | 团长｜4K弹幕 | 团长 | `csp_PanWebTz` | xiaosa/api.json |
| 190 | 雷鲸｜4K弹幕 | 雷鲸 | `csp_PanWebShareCloudLJ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 191 | 海绵｜4K弹幕 | 海绵 | `csp_PanWebShareCloudHM` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 192 | 夸父｜4K弹幕 | 夸父 | `csp_PanWebShareCloudKF` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 193 | 123｜4K弹幕 | 123 | `csp_PanWebShareCloud123` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 194 | 趣盘｜4K弹幕 | 趣盘 | `csp_PanWebQu` ext=json-ext | xiaosa/api.json |
| 195 | 盘库｜4K弹幕 | 盘库 | `csp_PanWebKuBa` ext=json-ext | xiaosa/api.json |
| 196 | 糖果｜搜索 | 糖果 | `csp_TGSou` | xiaosa/api.json |
| 197 | 音海｜搜索 | 音海 | `csp_YinHai` | xiaosa/api.json |
| 198 | 百酷｜搜索 | 百酷 | `csp_Baiku` | xiaosa/api.json |
| 199 | 短剧｜大全 | 短剧大全 | `csp_DuanjuDQ` | xiaosa/api.json |
| 200 | 短剧｜合集 | 短剧合集 | `csp_DuanjuHJ` | xiaosa/api.json |
| 201 | 星芽｜短剧 | 星芽短剧 | `csp_AppXY` | xiaosa/api.json |
| 202 | 短剧屋｜短剧 | 短剧屋 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 203 | 厂长｜影视 | 厂长影视 | `csp_Czsapp` ext=https://www.czzymovie.com/ | xiaosa/api.json |
| 204 | 绝对｜影视 | 绝对影视 | `csp_FourK` ext=https://www.4kvm.tv | xiaosa/api.json |
| 205 | 云播｜影视 | 云播影视 | `csp_Tvyb` ext=http://www.viptv01.com | xiaosa/api.json |
| 206 | 奇优｜影视 | 奇优影视 | `csp_Qiyou` | xiaosa/api.json |
| 207 | 苹果｜影视 | 苹果影视 | `csp_LiteApple` | xiaosa/api.json |
| 208 | 全看｜影视 | 全网影视 | `csp_Quanwk` ext=https://www.91qkw.com | xiaosa/api.json |
| 209 | 饺子｜影视 | 饺子影视 | `csp_Jiaozi` | xiaosa/api.json |
| 210 | 鸭梨｜影视 | 鸭梨影视 | `csp_KmeiJu` | xiaosa/api.json |
| 211 | 白白｜影视 | 白白影视 | `csp_SuBaiBai` ext=https://www.subaibai.com | xiaosa/api.json |
| 212 | 低端｜影视 | 低端影视 | `csp_Ddys` | xiaosa/api.json |
| 213 | 瓜子｜影视 | 瓜子影视 | `csp_Gz360` | xiaosa/api.json |
| 214 | 爱看｜影视 | 爱看机器人 | `csp_Ikanbot` | xiaosa/api.json |
| 215 | 明星｜影视 | 明星影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 216 | 麦田｜影视 | 麦田影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 217 | 免费｜影视 | 免费影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 218 | 面包｜影视 | 面包影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 219 | 永乐｜影视 | 永乐影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 220 | 雪糕｜影视 | 雪糕影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 221 | 骚火｜影视 | 骚火影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 222 | 七点｜影视 | 七点影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 223 | 三九｜影视 | 三九影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 224 | 三四｜影视 | 三四影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 225 | 农民｜影视 | 农民影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 226 | 盒子｜影视 | 盒子影视 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 227 | 影牛｜影视 | 电影牛 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 228 | 1905｜影视 | 1905 | `csp_Web1905` | xiaosa/api.json |
| 229 | 哆啦｜新番社 | 哆啦新番社 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 230 | 曼波｜动漫 | 曼波动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 231 | 稀饭｜动漫 | 稀饭动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 232 | 咕咕｜动漫 | 咕咕动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 233 | 多多｜动漫 | 多多动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 234 | 派对｜动漫 | 派对动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 235 | 黑猫｜动漫 | 黑猫动漫 | `csp_AppGet` ext=json-ext | xiaosa/api.json |
| 236 | 樱花｜动漫 | 樱花动漫 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 237 | 巴士｜动漫 | 巴士动漫 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 238 | 修罗｜磁力 | 修罗影视 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 239 | 七味｜磁力 | 七味 | `csp_QnMp4` ext=json-ext | xiaosa/api.json |
| 240 | 80S｜磁力 | 80S | `csp_BLSGod` | xiaosa/api.json |
| 241 | New6V｜磁力 | New6v | `csp_New6v` ext=https://www.xb6v.com | xiaosa/api.json |
| 242 | 迅雷吧｜磁力 | 迅雷吧 | `csp_Xunlei8` | xiaosa/api.json |
| 243 | 电影港｜磁力 | 电影港 | `csp_XYQHiker` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 244 | 狐狸君｜磁力 | 狐狸君 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 245 | Mp4电影｜磁力 | Mp4电影 | `csp_Mp4Mov` | xiaosa/api.json |
| 246 | 美剧天堂｜磁力 | 美剧天堂 | `csp_MeijuTT` | xiaosa/api.json |
| 247 | 电影天堂｜磁力 | 电影天堂 | `csp_DyGod` | xiaosa/api.json |
| 248 | 夸克｜云盘 | 夸克云盘 | `csp_PanQuark` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 249 | 百度｜云盘 | 百度云盘 | `csp_PanBaiDu` | xiaosa/api.json |
| 250 | 88｜看球 | 88看球 | `csp_Kanqiu` | xiaosa/api.json |
| 251 | 哔哩｜合集 | 哔哩合集 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 252 | 哔哩｜音乐 | 哔哩哔哩演唱会 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 253 | 哔哩｜听书 | 哔哩哔哩听书 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 254 | 哔哩｜相声 | 哔哩哔哩相声 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 255 | 哔哩｜小品 | 哔哩哔哩小品 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 256 | 哔哩｜戏曲 | 哔哩哔哩戏曲 | `csp_Bili` ext=json-ext | xiaosa/api.json |
| 257 | 急救｜教学 | 急救教学 | `csp_FirstAid` | xiaosa/api.json |
| 258 | 养生｜知识 | 养生堂 | `csp_HBPQ` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 259 | 版本｜信息 | 版本信息 | `csp_Market` ext=https://pizazz.s3.bitiful.net/ | xiaosa/api.json |
| 260 | ♨️网盘升级┃全速启动 | 豆豆 | `csp_DouDouGuard` | 367.json |
| 261 | 🎬️摸鱼┃电影┃预告 | update | `csp_YGPGuard` | 367.json |
| 262 | 🚀辅助┃配置┃中心 | config | `csp_Config` | 367.json |
| 263 | 🍁优汐木偶┃4K弹幕 | MoggV2 | `csp_PanWebShare` ext=http://我不是.摸鱼儿.top/json/mogg.j | 367.json |
| 264 | 🐂小二资源┃4K弹幕 | UcXmV2 | `csp_PanWebShare` ext=http://我不是.摸鱼儿.top/json/ex.jso | 367.json |
| 265 | 🖍︎蜡笔资源┃4K弹幕 | QuarkLaBiV2 | `csp_PanWebShare` ext=http://我不是.摸鱼儿.top/json/lb.jso | 367.json |
| 266 | 🏆️夸克至臻┃4K弹幕 | QuarkzzV2 | `csp_PanWebShare` ext=http://我不是.摸鱼儿.top/json/zz.jso | 367.json |
| 267 | 🍡多多网盘┃4K弹幕 | YYDSYS | `csp_PanWebShare` ext=http://我不是.摸鱼儿.top/json/yyds.j | 367.json |
| 268 | ✊️团长网盘┃4K弹幕 | QuarkTZ | `csp_PanWebTz` | 367.json |
| 269 | 🐋天翼雷鲸┃4K弹幕 | cloudLJ | `csp_PanWebShareCloudLJ` ext=http://我不是.摸鱼儿.top/json/lj.jso | 367.json |
| 270 | 🥶聚合搜索┃4K弹幕 | 聚搜 | `csp_PanWebSearch` ext=http://我不是.摸鱼儿.top/json/aliyun | 367.json |
| 271 | ☕️咖啡┃蓝光┃弹幕 | 咖啡 | `csp_AppGet2` ext=json-ext | 367.json |
| 272 | ❄️雪诺┃蓝光┃弹幕 | 诺雪 | `csp_AppGet2` ext=json-ext | 367.json |
| 273 | 🥷忍者┃蓝光┃弹幕 | 忍者 | `csp_AppGet` ext=json-ext | 367.json |
| 274 | 🍂菲菲┃蓝光┃弹幕 | 彼岸 | `csp_AppGet` ext=json-ext | 367.json |
| 275 | 🥪菲菲┃蓝光┃无广 | 菲儿影视 | `csp_AppFerr` ext=json-ext | 367.json |
| 276 | ⛱️雨滴┃蓝光┃无广 | 雨滴 | `csp_AppGet` ext=json-ext | 367.json |
| 277 | ☃️人人┃蓝光┃无广 | 人人 | `csp_AppGet` ext=json-ext | 367.json |
| 278 | 🌀沐风┃蓝光┃无广 | 664K | `csp_XBPQ` ext=http://我不是.摸鱼儿.top/api/moyu.ph | 367.json |
| 279 | 🕊️凝安┃蓝光┃无广 | 4Kdy | `csp_XBPQ` ext=http://我不是.摸鱼儿.top/api/moyu.ph | 367.json |
| 280 | 🫣耐看┃蓝光┃无广 | nk | `csp_WebNK` ext=w7TCmsONw6LDrsKYwoXCosKcwqDCrc | 367.json |
| 281 | 🐻即看┃多线┃纯净 | 即看 | `csp_AppSxGuard` ext=rfOX1voDIQhH8epBwtCFsub1+2malo | 367.json |
| 282 | 🏝奥特┃无广┃纯净 | Auete | `csp_AueteGuard` ext=https://auete.com/ | 367.json |
| 283 | 😎酷影┃蓝光┃纯净 | 酷影视 | `csp_AppYsV2Guard` ext=https://www.tvkuys.xyz/api.php | 367.json |
| 284 | 😭呜呜┃蓝光┃无广 | 五五影视 | `csp_XBPQ` ext=http://我不是.摸鱼儿.top/api/moyu.ph | 367.json |
| 285 | 🦋蝴蝶┃蓝光┃纯净 | 蝴蝶 | `csp_Hudie` ext=eaHR0cHM6Ly9kLmRjbW92aWUudG9w | 367.json |
| 286 | 🍧二三┃蓝光┃纯净 | 二三 | `csp_Muou` ext=caHR0cHM6Ly8xMjN5c3cuY29tfDIz5 | 367.json |
| 287 | 🍆洽洽┃蓝光┃纯净 | 洽洽影视 | `csp_AppGZ` | 367.json |
| 288 | 🫐三六┃蓝光┃纯净 | 三六零弹幕 | `csp_Sp360` | 367.json |
| 289 | 🇨🇳央视┃经典┃合集 | 央视经典 | `csp_BiliGuard` ext=http://我不是.摸鱼儿.top/api/moyu.ph | 367.json |
| 290 | 🇨🇳央视┃能量┃合集 | cctv | `csp_AuthCCTV` ext=http://我不是.摸鱼儿.top/json/cctv.j | 367.json |
| 291 | 😄DFun┃动漫┃蓝光 | mutefun | `csp_WebMuTeFun` ext=https://www.mutefun.tv | 367.json |
| 292 | 🌸樱花┃动漫┃樱花 | 樱花 | `csp_XBPQ` ext=http://我不是.摸鱼儿.top/api/moyu.ph | 367.json |
| 293 | 🍼曼波┃动漫┃蓝光 | mb | `csp_AppGetApp` ext=w7TCmsK8w5rDrcKYwoPCmcKkw57Cms | 367.json |
| 294 | 🍚稀饭┃动漫┃蓝光 | xifan | `csp_AppGetApp` ext=w7TCmsK8w5rDrcKYwoPCmcKkw57Cms | 367.json |
| 295 | 🌟星芽┃短剧┃热推 | duanju | `csp_AppXY` | 367.json |
| 296 | 💫异界┃动漫┃蓝光 | csp_Ysj | `csp_YsjGuard` | 367.json |
| 297 | 📖日漫┃动漫┃蓝光 | csp_Anime1 | `csp_Anime1Guard` | 367.json |
| 298 | 🍉西瓜┃娱乐┃蓝光 | xxg | `csp_AmuXiguaV2` ext=w7TCmsK/w7HDrsKIwqLCnFfCs8KTwq | 367.json |
| 299 | 📚少儿┃兔兔┃教育 | tu | `csp_WebRabbit` ext=https://tuxiaobei.com | 367.json |
| 300 | 🔍️聚合盘搜┃4K纯净 | searchEx | `csp_PanWebSearch` ext=http://我不是.摸鱼儿.top/json/aliyun | 367.json |
| 301 | 🌈优汐搜搜┃4K纯净 | UCsousou | `csp_UuSsGuard` ext=json-ext | 367.json |
| 302 | 🐌盘他盘他┃三盘搜索 | PanSso | `csp_PanSsoGuard` ext=json-ext | 367.json |
| 303 | 📁文件┃夸克┃资源 | QuarkYunPan | `csp_PanQuark` ext=http://我不是.摸鱼儿.top/json/quarkS | 367.json |
| 304 | 📁文件┃阿里┃资源 | AliYunPanV2 | `csp_PanAli` ext=http://我不是.摸鱼儿.top/json/aliSha | 367.json |
| 305 | 📁文件┃优视┃资源 | UcYunPan | `csp_PanUc` ext=http://我不是.摸鱼儿.top/json/ucShar | 367.json |
| 306 | 📁文件┃百度┃资源 | baidu | `csp_PanBaiDu` | 367.json |
| 307 | ⚽926┃看球┃直播 | 926看球 | `csp_kanqiu926Guard` | 367.json |
| 308 | 商店 | 商店 | `csp_Market` ext=https://raw.githubusercontent. | 0827.json |
| 309 | 賤賤 | 賤賤 | `csp_Jianpian` ext=https://raw.githubusercontent. | 0827.json |
| 310 | 異界 | 異界 | `csp_Ysj` | 0827.json |
| 311 | 哔哩 | 哔哩 | `csp_Bili` ext=json-ext | 0827.json |
| 312 | JustLive | JustLive | `csp_JustLive` | 0827.json |
| 313 | 雲搜 | 雲搜 | `csp_UpYun` | 0827.json |
| 314 | 盤搜 | 盤搜 | `csp_PanSou` | 0827.json |
| 315 | 紙條搜 | 紙條搜 | `csp_XiaoZhiTiao` | 0827.json |
| 316 | PikPak1 | PikPakShare1 | `csp_PikPakShare` ext=https://raw.githubusercontent. | 9918.json |
| 317 | PikPak18 | PikPakShare18 | `csp_PikPakShare` ext=https://raw.githubusercontent. | 9918.json |
| 318 | 磁力秀 | csp_BtSow | `csp_BtSow` ext=https://raw.githubusercontent. | 9918.json |
| 319 | 磁力搜 | csp_BtSearch | `csp_BtSearch` ext=https://raw.githubusercontent. | 9918.json |
| 320 | 磁力狗 | Clg | `csp_Clg` ext=./lib/tokenm.json$$$https://cl | 9918.json |
| 321 | Netflav | csp_Netflav | `csp_Netflav` | 9918.json |
| 322 | 桃花族 | Taohuazu | `csp_Taohuazu` ext=https://raw.githubusercontent. | 9918.json |
| 323 | 色花糖 | SeHuaTang | `csp_SeHuaTang` ext=https://raw.githubusercontent. | 9918.json |
| 324 | XOJav | Xojav | `csp_Xojav` ext=null$$$proxy | 9918.json |
| 325 | CableAV | Cable | `csp_Cable` | 9918.json |
| 326 | AvTime | AvTime | `csp_AvTime` | 9918.json |
| 327 | Top888 | Top888 | `csp_Top888` | 9918.json |
| 328 | JavBus | csp_JavBus | `csp_JavBus` ext=https://raw.githubusercontent. | 9918.json |
| 329 | JavDb | csp_JavDb | `csp_JavDb` ext=https://raw.githubusercontent. | 9918.json |
| 330 | 18AV | 18AV | `csp_Eighteen` | 9918.json |
| 331 | Jable | Jable | `csp_Jable` | 9918.json |
| 332 | MissAV | MissAV | `csp_Miss` | 9918.json |
| 333 | Hanime | Hanime | `csp_Hanime` | 9918.json |
| 334 | 黄色仓库 | 黄色仓库 | `csp_XBPQ` ext=./XBPQ/黄色仓库.json | 9918.json |
| 335 | 91黑料 | 91黑料 | `csp_XBPQ` ext=./XBPQ/91黑料.json | 9918.json |
| 336 | 色花堂 | 色花堂 | `csp_XBPQ` ext=./XBPQ/色花堂.json | 9918.json |
| 337 | 我爱AV | 我爱AV | `csp_XBPQ` ext=./XBPQ/我爱AV.json | 9918.json |

### collect（69）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 影视 | 量子[直连] | 量子 | `https://cj.lziapi.com/api.php/provide/vod/` | dianshi.json |
| 2 | 影视 | 非凡[直连] | 非凡 | `http://ffzy.tv/api.php/provide/vod/` | dianshi.json |
| 3 | 影视 | 索尼[直连] | 索尼 | `https://suoniapi.com/api.php/provide/vod/at/xml/` | dianshi.json |
| 4 | 影视 | 卧龙资源[直连] | 卧龙资源 | `https://wolongzyw.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 5 | 影视 | ikun资源[直连] | ikun资源 | `https://www.ikunzy.com/api.php/provide/vod/` | dianshi.json |
| 6 | 影视 | 尤酷[直连] | U酷资源風 | `https://api.ukuapi.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 7 | 影视 | 360资源[直连] | 360资源 | `https://360zy.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 8 | 影视 | 极速[直连] | 极速资源 | `https://jszyapi.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 9 | 影视 | 率率[直连] | 率率 | `https://suoniapi.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 10 | 百度 | 百度 | `https://api.apibdzy.com/api.php/provide/vod?ac=list` | dianshi.json |
| 11 | 影视 | 暴風[直连] | 暴風 | `https://bfzyapi.com/api.php/provide/vod/?ac=list` | dianshi.json |
| 12 | 海外看┃采集 | haiwaikan | `https://haiwaikan.com/api.php/provide/vod` | 0821.json |
| 13 | 快帆┃采集 | 快帆 | `https://api.kuaifan.tv/api.php/provide/vod` | 0821.json |
| 14 | 天堂┃采集 | 天堂 | `http://caiji.dyttzyapi.com/api.php/provide/vod/at/xml/` | 0821.json |
| 15 | 天涯┃采集 | 天涯 | `https://ty.tyyszy5.com/api.php/provide/vod/at/xml/?ac=list` | 0821.json |
| 16 | 玉兔┃采集 | 玉兔 | `https://apiyutu.com/api.php/providedao/vod/at/xml/` | 0821.json |
| 17 | 黄色┃采集 | 黄色 | `https://hsckzy888.com/api.php/provide/vod/at/xml/` | 0821.json |
| 18 | 黑木耳┃采集 | 黑木耳 | `https://json.heimuer.xyz/api.php/provide/vod` | 0821.json |
| 19 | 如意┃采集 | 如意 | `https://cj.rycjapi.com/api.php/provide/vod` | 0821.json |
| 20 | 360┃采集 | 360 | `https://360zyzz.com/api.php/provide/vod/at/xml/` | 0821.json |
| 21 | 茅台┃采集 | 茅台 | `https://caiji.maotaizy.cc/api.php/provide/vod/at/xml/` | 0821.json |
| 22 | 极速┃采集 | 极速 | `https://jszyapi.com/api.php/provide/vod/at/xml` | 0821.json |
| 23 | 豆瓣┃采集 | 豆瓣 | `https://caiji.dbzy5.com/api.php/provide/vod/at/xml/` | 0821.json |
| 24 | 魔爪┃采集 | 魔爪 | `https://mozhuazy.com/api.php/provide/vod/at/xml/` | 0821.json |
| 25 | CK┃采集 | CK | `https://ckzy.me/api.php/provide/vod/at/xml/` | 0821.json |
| 26 | JKun┃采集 | JKun | `https://jkunzyapi.com/api.php/provide/vod/at/xml/` | 0821.json |
| 27 | 百万┃采集 | 百万 | `https://api.bwzyz.com/api.php/provide/vod/at/xml/` | 0821.json |
| 28 | 搜AV┃采集 | 搜AV | `https://api.souavzy.vip/api.php/provide/vod/at/xml/` | 0821.json |
| 29 | 155┃采集 | 155 | `https://apilsbzy1.com/api.php/provide/vod/at/xml/` | 0821.json |
| 30 | lsb┃采集 | lsb | `https://subocj.com/api.php/provide/vod/at/xml/` | 0821.json |
| 31 | 乐视 | 乐视 | `https://leshiapi.com/api.php/provide/vod` | 0825.json |
| 32 | 七七｜影视 | 七七 | `http://6.qqqtv.top/api.php/provide/vod/` | xiaosa/api.json |
| 33 | ♥155(直连) | 155zy.com | `https://155api.com/api.php/provide/vod/?ac=list` | 9918.json |
| 34 | 💕探探(直连) | tantanzy.com | `https://apittzy.com/api.php/provide/vod/?ac=list` | 9918.json |
| 35 | 💕jkun(直连) | jkunzy.com | `https://jkunzyapi.com/api.php/provide/vod/?ac=list` | 9918.json |
| 36 | ♥搞起(直连) | gqzy.me | `https://caiji.gqjiexi.com/api.php/provide/vod/?ac=list` | 9918.json |
| 37 | 💕乐播(直连) | lebozy.com | `https://lbapi9.com/api.php/provide/vod/?ac=list` | 9918.json |
| 38 | 💕老鸭(直连) | laoyazy.com | `https://api.apilyzy.com/api.php/provide/vod/?ac=list` | 9918.json |
| 39 | ♥森林(直连) | senlinzy.com | `https://slapibf.com/api.php/provide/vod/?ac=list` | 9918.json |
| 40 | 💕甜蜜(直连) | timizy.cc | `https://tianmiapi.com/api.php/provide/vod/?ac=list` | 9918.json |
| 41 | ♥滴滴(直连) | didizy.com | `https://api.ddapi.cc/api.php/provide/vod/?ac=list` | 9918.json |
| 42 | 💕好色(直连) | haosezyw.com | `https://haosezyw.com/api.php/provide/vod/?ac=list` | 9918.json |
| 43 | 💕越南(直连) | vnzyz.com | `https://vnzyz.com/api.php/provide/vod/?ac=list` | 9918.json |
| 44 | 💕狼友会(直连) | lyhzyw.com | `https://api.lyhapi.com/api.php/provide/vod/?ac=list` | 9918.json |
| 45 | ★精品下(直连) | jingpinx.com | `https://www.jingpinx.com//api.php/provide/vod/?ac=list` | 9918.json |
| 46 | ★黄瓜(直连) | avre.club | `https://www.avre06.com/api.php/provide/vod/?ac=list` | 9918.json |
| 47 | 💕色虎(直连) | sesehuzy.com | `https://apisesehuzy.com/api.php/provide/vod/?ac=list` | 9918.json |
| 48 | ★奶香香(直连) | naixxzy.com | `https://naixxzy.com/api.php/provide/vod/?ac=list` | 9918.json |
| 49 | 💕老色逼(直连) | laosebizy.com | `https://apilsbzy1.com/api.php/provide/vod/?ac=list` | 9918.json |
| 50 | ★草榴(直连) | www.caoliuzyw.com | `https://www.caoliuzyw.com/api.php/provide/vod/?ac=list` | 9918.json |
| 51 | 美少女 | xjjzyapi.com | `https://xjjzyapi.com/cjapi/sda/vod/?ac=list` | 9918.json |
| 52 | 色猫(FQ) | semaozy.com | `https://caiji.semaozy.net/inc/apijson_vod.php` | 9918.json |
| 53 | 辣椒(FQ) | lajiaozy.com | `https://apilj.com/api.php/provide/vod/at/json/?ac=list` | 9918.json |
| 54 | 快av(FQ) | kuaiavzy.com | `https://kuaiavzy.com/api.php/provide/vod/?ac=list` | 9918.json |
| 55 | 玉兔资源 | 玉兔资源 | `https://apiyutu.com/api.php/provide/vod/at/xml/` | 9918.json |
| 56 | 奥斯卡 | 奥斯卡资源网 | `https://aosikazy.com/api.php/provide/vod/` | 9918.json |
| 57 | 草帽 | 草帽资源 | `https://cmzy.top/api.php/provide/vod/?ac=list` | 9918.json |
| 58 | 成人05 | wmcj8.com | `http://wmcj8.com/inc/sapi.php` | 9918.json |
| 59 | 成人06 | apilj.com | `http://apilj.com/api.php/provide/vod/at/json/` | 9918.json |
| 60 | 成人07 | 91md.me | `http://91md.me/api.php/provide/vod/` | 9918.json |
| 61 | 成人09 | dadiapi.com | `http://dadiapi.com/api.php` | 9918.json |
| 62 | 成人12 | bttcj.com | `http://bttcj.com/inc/sapi.php` | 9918.json |
| 63 | 成人15 | api.11bat.com | `http://api.11bat.com/api.php/provide/vod/at/xml` | 9918.json |
| 64 | 成人16 | api.maozyapi.com | `https://api.maozyapi.com/inc/apijson_vod.php` | 9918.json |
| 65 | 成人18 | apittzy.com | `https://apittzy.com/api.php/provide/vod/` | 9918.json |
| 66 | 成人23 | kkzy.me | `https://kkzy.me/api.php/provide/vod/` | 9918.json |
| 67 | 成人24 | shayuapi.com | `https://shayuapi.com/api.php/provide/vod/` | 9918.json |
| 68 | 成人26 | lbapi9.com | `https://lbapi9.com/api.php/provide/vod/` | 9918.json |
| 69 | 成人27 | api.apilyzy.com | `https://api.apilyzy.com/api.php/provide/vod/` | 9918.json |

### js-drpy（265）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 搜索 | 豆瓣[js] | drpy_js_豆瓣 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 2 | 影视 | TVB云播[js] | drpy_js_TVB云播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 3 | 闪雷┃MP4 | 闪雷 | `https://raw.githubusercontent.com/ches2010/tvbox/master/FTY/` ext=https://raw.githubusercontent. | dianshi.json |
| 4 | 🐯虎牙┃直播 | 虎牙直播js | `https://raw.githubusercontent.com/ches2010/tvbox/master/FTY/` ext=https://raw.githubusercontent. | dianshi.json |
| 5 | 🐟斗鱼┃直播 | 斗鱼js | `https://raw.githubusercontent.com/ches2010/tvbox/master/FTY/` ext=https://raw.githubusercontent. | dianshi.json |
| 6 | 🎧有声┃小说 | 有声小说js | `https://raw.githubusercontent.com/ches2010/tvbox/master/FTY/` ext=https://raw.githubusercontent. | dianshi.json |
| 7 | 📚儿童┃启蒙 | dr_兔小贝 | `https://raw.githubusercontent.com/ches2010/tvbox/master/FTY/` ext=https://raw.githubusercontent. | dianshi.json |
| 8 | 官源 | 360[js] | drpy_js_360影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 9 | 官源 | 芒果[js] | drpy_js_百忙无果 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 10 | 官源 | 腾讯[js] | drpy_js_腾云驾雾 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 11 | 官源 | 爱奇艺[js] | drpy_js_奇珍异兽 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 12 | 官源 | 搜狗[js] | drpy_js_菜狗 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 13 | 官源 | 优酷[js] | drpy_js_优酷 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 14 | 影视 | 厂长资源[js] | drpy_js_厂长资源 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 15 | 动漫 | 58动漫[js] | drpy_js_58动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 16 | 动漫 | AGE动漫[js] | drpy_js_AGE动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 17 | 动漫 | AnFuns[js] | drpy_js_AnFuns | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 18 | 动漫 | Anime1动畫[js] | drpy_js_Anime1动畫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 19 | 动漫 | NT动漫[js] | drpy_js_NT动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 20 | 动漫 | NyaFun[js] | drpy_js_NyaFun | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 21 | 动漫 | 爱弹幕[js] | drpy_js_爱弹幕 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 22 | 动漫 | 动漫岛[js] | drpy_js_动漫岛 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 23 | 动漫 | 路漫漫[js] | drpy_js_路漫漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 24 | 影视 | 喵次元[js] | drpy_js_喵次元 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 25 | 动漫 | 奇米动漫[js] | drpy_js_奇米动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 26 | 动漫 | 去看吧[js] | drpy_js_去看吧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 27 | 动漫 | 曲奇动漫[js] | drpy_js_曲奇动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 28 | 动漫 | 异世界动漫[js] | drpy_js_异世界动漫漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 29 | 动漫 | 怡萱动漫[js] | drpy_js_怡萱动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 30 | 音频 | A8音乐[js] | drpy_js_A8音乐 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 31 | 音频 | DJ音乐[js] | drpy_js_DJ音乐 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 32 | 音频 | 啦啦DJ[js] | drpy_js_啦啦DJ | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 33 | 音频 | 清风DJ[js] | drpy_js_清风DJ | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 34 | 广播 | 广播迷FM[js] | drpy_js_广播迷FM | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 35 | 广播 | 蜻蜓FM[js] | drpy_js_蜻蜓FM | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 36 | 听书 | i275听书[js] | drpy_js_i275听书 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 37 | 听书 | 爱上你听书[js] | drpy_js_爱上你听书网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 38 | 听书 | 博看听书[js] | drpy_js_博看听书 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 39 | 听书 | 海洋听书[js] | drpy_js_海洋听书 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 40 | 听书 | 六月听书[js] | drpy_js_六月听书 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 41 | 听书 | 有声小说[js] | drpy_js_有声小说 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 42 | 聚合 | 播客[js] | drpy_js_播客 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 43 | 聚合 | 短视频[js] | drpy_js_短视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 44 | 聚合 | 酷6网[js] | drpy_js_酷6网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 45 | 聚合 | 网易公版[js] | drpy_js_网易公版影像 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 46 | MV | 爱车MV[js] | drpy_js_爱车MV | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 47 | MV | 酷奇[js] | drpy_js_酷奇MV | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 48 | 配置接口完全免费 | bb | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 49 | 请勿相信视频中任何广告 | cc | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | dianshi.json |
| 50 | ⚽Jrs┃球迷 | Jrsjs | `https://gh-proxy.net/https://raw.githubusercontent.com/fanta` ext=https://gh-proxy.net/https://r | 0821.json |
| 51 | 🎤酷奇┃MV | 酷奇 | `https://gh-proxy.net/https://raw.githubusercontent.com/fanta` ext=https://gh-proxy.net/https://r | 0821.json |
| 52 | 弹幕┃我的哔哩[js] | drpy_js_我的哔哩 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 53 | 弹幕┃哔哩直播[js] | drpy_js_哔哩直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 54 | 直播┃JustLive[js] | drpy_js_JustLive | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 55 | 直播┃斗鱼[js] | drpy_js_斗鱼直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 56 | 直播┃虎牙[js] | drpy_js_虎牙直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 57 | 少儿┃童趣[js] | drpy_js_童趣 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 58 | 少儿┃兔小贝[js] | drpy_js_兔小贝 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 59 | 评书┃评书随身听[js] | drpy_js_评书随身听 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 60 | 相声┃相声随身听[js] | drpy_js_相声随身听 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 61 | 电视┃好趣网[js] | drpy_js_好趣网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0821.json |
| 62 | 搜索 | 磁力熊[js] | drpy_js_磁力熊搜索 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 63 | 搜索 | 路飞[js] | lf_js_search | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` | js.json |
| 64 | 影视 | 荐片[js] | drpy_js_荐片 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 65 | 路飞 | 磁力[js] | lf_js_p2p | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` | js.json |
| 66 | 电视 | 直播[js] | lf_js_lf_live | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 67 | 影视 | 酷云77[js] | drpy_js_酷云77 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 68 | 影视 | 快看[js] | drpy_js_快看 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 69 | 影视 | 爱看[js] | drpy_js_爱看 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 70 | 影视 | 低端[js] | drpy_js_低端 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 71 | 影视 | 南瓜影视[js] | drpy_js_南瓜影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 72 | 影视 | Auete[js] | drpy_js_Auete | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 73 | 影视 | cokemv[js] | drpy_js_cokemv | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 74 | 影视 | LIBVIO[js] | drpy_js_LIBVIO | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 75 | 影视 | voflix[js] | drpy_js_voflix | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 76 | 影视 | 两个BT[js] | drpy_js_两个BT | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 77 | 影视 | 量子影视[js] | drpy_js_量子影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 78 | 影视 | 农民影视[js] | drpy_js_农民 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 79 | 影视 | 在线之家[js] | drpy_js_在线之家 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 80 | 影视 | 量子资源[js] | drpy_js_量子资源 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 81 | 影视 | 369影视[js] | drpy_js_369影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 82 | 影视 | 热播库[js] | drpy_js_热播库 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 83 | 影视 | 蛋蛋剧[js] | drpy_js_蛋蛋剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 84 | 影视 | B站影视[js] | drpy_js_B站影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 85 | 影视 | 凌云影视[js] | drpy_js_凌云影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 86 | 影视 | 孜然影视[js] | drpy_js_孜然影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 87 | 影视 | 我爱跟剧[js] | drpy_js_我爱跟剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 88 | 影视 | 干饭影视[js] | drpy_js_干饭影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 89 | 影视 | 秋霞影视[js] | drpy_js_秋霞影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 90 | 影视 | 阿布影院[js] | drpy_js_阿布影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 91 | 影视 | 电影先生[js] | drpy_js_电影先生 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 92 | 影视 | 落攻阁影视[js] | drpy_js_落攻阁影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 93 | 影视 | 69美剧[js] | drpy_js_69美剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 94 | 影视 | 兄弟影视[js] | drpy_js_兄弟影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 95 | 影视 | 4k剧院[js] | drpy_js_4k剧院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 96 | 影视 | 爱迪影视[js] | drpy_js_爱迪影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 97 | 影视 | 539影视[js] | drpy_js_539影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 98 | 影视 | 333影视[js] | drpy_js_333影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 99 | 影视 | 996影视[js] | drpy_js_996影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 100 | 影视 | 188影视[js] | drpy_js_188影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 101 | 影视 | 哔嘀影视[js] | drpy_js_哔嘀影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 102 | 影视 | 7喜影院[js] | drpy_js_7喜影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 103 | 影视 | 18哈[js] | drpy_js_18哈 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 104 | 影视 | 567电影院[js] | drpy_js_567电影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 105 | 影视 | Fun4K[js] | drpy_js_Fun4K | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 106 | 影视 | KOK影院[js] | drpy_js_KOK影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 107 | 影视 | UrleBird[飞][js] | drpy_js_UrleBird[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 108 | 影视 | 爱看影院[js] | drpy_js_爱看影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 109 | 影视 | 北川影视[js] | drpy_js_北川影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 110 | 影视 | 尘落影视[js] | drpy_js_尘落影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 111 | 影视 | 达达龟[js] | drpy_js_达达龟 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 112 | 影视 | 蛋蛋赞[js] | drpy_js_蛋蛋赞 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 113 | 影视 | 多瑙影视[js] | drpy_js_多瑙影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 114 | 影视 | 剧迷su[js] | drpy_js_剧迷su | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 115 | 影视 | 看韩剧[js] | drpy_js_看韩剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 116 | 影视 | 看了么[js] | drpy_js_看了么 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 117 | 影视 | 蓝光影院[js] | drpy_js_蓝光影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 118 | 影视 | 首播影院[js] | drpy_js_首播影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 119 | 影视 | 小白菜电影[js] | drpy_js_小白菜电影 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 120 | 影视 | 影视工场[js] | drpy_js_影视工场 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 121 | 影视 | 影视之家[V2][js] | drpy_js_影视之家[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 122 | 影视 | 悠久影院[js] | drpy_js_悠久影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 123 | 影视 | 中国人线上看[js] | drpy_js_中国人线上看 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 124 | 影视 | 555电影[V2] | drpy_js_555电影[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 125 | 影视 | 69美剧[V2] | drpy_js_69美剧[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 126 | 影视 | cokemv[V2] | drpy_js_cokemv[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 127 | 影视 | UM电影[V2] | drpy_js_UM电影[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 128 | 影视 | 爱迪[V2][js] | drpy_js_爱迪[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 129 | 影视 | 宝片[V2] | drpy_js_宝片[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 130 | 影视 | 刺桐[V2][js] | drpy_js_刺桐[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 131 | 影视 | 咕噜[V2] | drpy_js_咕噜[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 132 | 影视 | 吼吼[V2] | drpy_js_吼吼[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 133 | 影视 | 曲奇影视[V2] | drpy_js_曲奇影视[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 134 | 影视 | 影探[V2] | drpy_js_影探[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 135 | 影视 | 北极狐[V2] | drpy_js_北极狐[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 136 | 影视 | 大米星球[V2] | drpy_js_大米星球[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 137 | 影视 | 奈飞狗[V2] | drpy_js_奈飞狗[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 138 | 影视 | 思古影视[V2] | drpy_js_思古影视[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 139 | 影视 | 映迷[V2] | drpy_js_映迷[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 140 | 影视 | 美益达[V2] | drpy_js_美益达[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 141 | 影视 | 落攻阁影视[V2] | drpy_js_落攻阁影视[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 142 | 影视 | 西门[V2] | drpy_js_西门[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=./js/西门[V2].js | js.json |
| 143 | 影视 | 路人甲[V2] | drpy_js_路人甲[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 144 | 影视 | 鑫鑫[V2] | drpy_js_鑫鑫[V2] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 145 | 磁力 | 4KHDR[js] | drpy_js_4KHDR | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 146 | 磁力 | 极影网[js] | drpy_js_极影网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 147 | 磁力 | MP4电影[js] | drpy_js_MP4US | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 148 | 磁力 | 电影港[js] | drpy_js_电影港 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 149 | 磁力 | 酷吧[js] | drpy_js_kuba | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 150 | 磁力 | 新版6v[js] | drpy_js_xb6v | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 151 | 磁力 | 美剧迷[js] | drpy_js_美剧迷 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 152 | 影视 | 低端影视[js] | drpy_js_ddys | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 153 | 影视 | libvio[js] | drpy_js_libvio | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 154 | 影视 | 团长影视[js] | drpy_js_tzfile | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 155 | 影视 | dydhhy[js] | drpy_js_dydhhy | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 156 | 影视 | 97tvs[js] | drpy_js_97tvs | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 157 | 影视 | rrdyw[js] | drpy_js_rrdyw | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 158 | 夸克 | 校长影视[js] | drpy_js_校长影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 159 | 夸克 | 人人影视搜[js] | drpy_js_yyets | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 160 | 夸克 | 奇妙搜[js] | drpy_js_奇妙搜[夸] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 161 | 夸克 | 爱盘搜[js] | drpy_js_愛盤搜 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 162 | 夸克 | 趣盘搜[js] | drpy_js_趣盤搜 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 163 | 影视 | meow搜[js] | drpy_js_meow | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 164 | 磁力 | 磁力熊[js] | drpy_js_磁力熊[磁] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 165 | 磁力 | 6V新版[js] | drpy_js_6V新版[磁] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 166 | 少儿 | 有声绘本[js] | drpy_js_有声绘本网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 167 | 弹幕 | 哔哩影视[js] | drpy_js_哔哩影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 168 | 体育 | 310直播[js] | drpy_js_310直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 169 | 体育 | 88看球[js] | drpy_js_88看球 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 170 | 体育 | JRKAN直播[js] | drpy_js_JRKAN直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 171 | 体育 | 抓饭体育[js] | drpy_js_抓饭体育 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 172 | 格斗 | 武享吧[js] | drpy_js_武享吧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 173 | 网盘 | Alist[js] | drpy_js_Alist | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 174 | 影视 | 爱看机器人[js] | drpy_js_爱看机器人 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 175 | 影视 | 爱看机器人2[js] | drpy_js_ikanbot2 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 176 | 影视 | 爱看机器人3[js] | drpy_js_ikanbot3 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 177 | 影视 | 百度测试[js] | drpy_js_jsapi2 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 178 | 影视 | 139影视[js] | drpy_js_139影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 179 | 影视 | 348电影网[js] | drpy_js_348电影网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 180 | 影视 | 4K电影网[js] | drpy_js_4K电影网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 181 | 影视 | 8号影院[js] | drpy_js_8号影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 182 | 影视 | JangoIndex[js] | drpy_js_JangoIndex | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 183 | 影视 | NY影院[js] | drpy_js_NY影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 184 | 影视 | 被窝电影[js] | drpy_js_被窝电影 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 185 | 影视 | 楚门影院[js] | drpy_js_楚门影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 186 | 影视 | 大米星球[js] | drpy_js_大米星球 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 187 | 影视 | 飞兔影视[js] | drpy_js_飞兔影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 188 | 影视 | 海兔影院[js] | drpy_js_海兔影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 189 | 影视 | 花猫TV[js] | drpy_js_花猫TV | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 190 | 影视 | 花屋影院[js] | drpy_js_花屋影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 191 | 影视 | 开眼视频[js] | drpy_js_开眼视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 192 | 影视 | 徕米TV[js] | drpy_js_徕米TV | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 193 | 影视 | 乐鱼影视[js] | drpy_js_乐鱼影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 194 | 影视 | 美剧网[js] | drpy_js_美剧网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 195 | 影视 | 美视网[js] | drpy_js_美视网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 196 | 影视 | 米爱影视[js] | drpy_js_米爱影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 197 | 影视 | 免费看[js] | drpy_js_免费看 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 198 | 影视 | 奈飞中文[js] | drpy_js_奈飞中文 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 199 | 影视 | 欧帝影院[js] | drpy_js_欧帝影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 200 | 影视 | 皮皮鸭[js] | drpy_js_皮皮鸭 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 201 | 影视 | 朴樱影视[js] | drpy_js_朴樱影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 202 | 影视 | 秋霞影院[js] | drpy_js_秋霞影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 203 | 影视 | 热播之家[js] | drpy_js_热播之家 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 204 | 影视 | 人人影视[js] | drpy_js_人人影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 205 | 影视 | 骚火电影[js] | drpy_js_骚火电影 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 206 | 影视 | 时光影视[js] | drpy_js_时光影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 207 | 影视 | 思古影视 | drpy_js_思古影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 208 | 影视 | 私人影院[js] | drpy_js_私人影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 209 | 影视 | 素白白[js] | drpy_js_素白白 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 210 | 影视 | 网飞.TV[js] | drpy_js_网飞.TV | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 211 | 影视 | 西瓜影院[js] | drpy_js_西瓜影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 212 | 影视 | 蜥蜴影视[js] | drpy_js_蜥蜴影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 213 | 影视 | 小狗影院[js] | drpy_js_小狗影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 214 | 影视 | 新视觉影视[js] | drpy_js_新视觉影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 215 | 影视 | 星辰影院[js] | drpy_js_星辰影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 216 | 影视 | 玄天[js] | drpy_js_玄天 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 217 | 影视 | 映迷影院[js] | drpy_js_映迷影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 218 | 影视 | 中国人[js] | drpy_js_中国人 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 219 | 影视 | 追剧迷[js] | drpy_js_追剧迷 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 220 | 影视 | 子子影视[js] | drpy_js_子子影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 221 | 起飞 | 大师兄影视[js] | drpy_js_大师兄影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 222 | 起飞 | 华乐影院[js] | drpy_js_华乐影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 223 | 起飞 | 天天视频[js] | drpy_js_天天视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 224 | 起飞 | 起飞影院[js] | drpy_js_起飞影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 225 | 起飞 | 兰花影院[js] | drpy_js_兰花影院 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 226 | 起飞 | 胖虎[js] | drpy_js_胖虎 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 227 | 起飞 | 555影视[js] | drpy_js_555影视[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 228 | 起飞 | if101[js] | drpy_js_if101[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 229 | 起飞 | KUBO影视[js] | drpy_js_KUBO影视[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 230 | 起飞 | 爱壹帆[js] | drpy_js_爱壹帆[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 231 | 起飞 | 独播库[js] | drpy_js_独播库[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 232 | 起飞 | 欧乐影院[js] | drpy_js_欧乐影院[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 233 | 起飞 | 小宝影院[js] | drpy_js_小宝影院[飞] | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | js.json |
| 234 | 88看球 | 88js | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0825.json |
| 235 | 美剧迷[js] | drpy_js_美剧迷p | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0825.json |
| 236 | 人人搜[js] | drpy_js_yyetsp | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 0825.json |
| 237 | 哔哩直播[js] | JS哔哩直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | XYQ.json |
| 238 | 虎牙直播(JS) | DRJS_虎牙 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | XYQ.json |
| 239 | 腾讯｜视频 | 腾讯视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 240 | 优酷｜视频 | 优酷视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 241 | 芒果｜视频 | 芒果视频 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 242 | 爱奇异｜视频 | 爱奇艺 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 243 | 双星｜4K弹幕 | 双星 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 244 | 校长｜4K弹幕 | 校长 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 245 | 人人｜4K弹幕 | 人人电影网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 246 | 全盘｜搜索 | 全盘 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 247 | 采集｜合集 | 采集之王 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 248 | 剧圈｜影视 | 剧圈圈 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 249 | 56｜动漫 | 56动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 250 | NT｜动漫 | NT动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 251 | 好看｜动漫 | 好看动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 252 | 奇米｜动漫 | 奇米动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 253 | 花子｜动漫 | 花子动漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 254 | 动画片｜动漫 | 动画片 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 255 | 路漫漫｜动漫 | 路漫漫 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 256 | 动漫岛｜动漫 | 动漫岛 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 257 | 去看吧｜动漫 | 去看吧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 258 | 爱弹幕｜动漫 | 爱弹幕 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 259 | 异世界｜动漫 | 异世界 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 260 | 短剧｜全网 | 短剧全网 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=https://raw.githubusercontent. | xiaosa/api.json |
| 261 | 哔哩貓 | bili_open | `https://raw.githubusercontent.com/ches2010/tvbox/master/cat/` ext=json-ext | 0827.json |
| 262 | ss直播 | drpy_js_ss直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 9918.json |
| 263 | Missav | drpy_js_Missav | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 9918.json |
| 264 | 玩偶姐姐 | drpy_js_玩偶姐姐 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 9918.json |
| 265 | 朱古力 | drpy_js_朱古力 | `https://raw.githubusercontent.com/ches2010/tvbox/master/lib/` ext=https://raw.githubusercontent. | 9918.json |

### py（12）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 甜圈｜短剧 | 甜圈短剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 2 | 偷乐｜短剧 | 偷乐短剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 3 | 爱看｜短剧 | 爱看短剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 4 | 锦鲤｜短剧 | 锦鲤短剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 5 | 剧王｜短剧 | 剧王短剧 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 6 | 金牌｜影视 | 金牌影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` ext=json-ext | xiaosa/api.json |
| 7 | 猎手｜影视 | 猎手影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 8 | 零度｜影视 | 零度影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 9 | 追星｜影视 | 追星影视 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 10 | 网络｜直播 | 网络直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 11 | 哔哩｜直播 | 哔哩直播 | `https://raw.githubusercontent.com/ches2010/tvbox/master/xiao` | xiaosa/api.json |
| 12 | 🦛河马┃短剧┃热推 | 河马短剧 | `http://我不是.摸鱼儿.top/py/河马短剧.py` | 367.json |

### other（7）

| # | 名称 | key | api / 规则 | 原配置 |
| --- | --- | --- | --- | --- |
| 1 | 成人11 | llzxcj.com | `http://llzxcj.com/inc/sck.php?ac=videolist` | 9918.json |
| 2 | 成人14 | f2dcj6.com | `http://f2dcj6.com/sapi?ac=videolist` | 9918.json |
| 3 | 成人19 | gdlsp.com | `https://www.gdlsp.com/api/xml.php` | 9918.json |
| 4 | 成人20 | msnii.com | `https://www.msnii.com/api/xml.php` | 9918.json |
| 5 | 成人21 | xrbsp.com | `https://www.xrbsp.com/api/xml.php` | 9918.json |
| 6 | 成人22 | kxgav.com | `https://www.kxgav.com/api/xml.php` | 9918.json |
| 7 | 成人25 | afasu.com | `https://www.afasu.com/api/xml.php` | 9918.json |

## 六、直播源

| 名称 | URL | EPG |
| --- | --- | --- |
| live | `./tv.txt` | https://epg.112114.xyz/?ch={name}&date={ |
| 初秋语•ipv4 | `https://raw.githubusercontent.com/ches2010/tvbox/master/list.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| YanG•综合 | `https://tv.iill.top/m3u/Gather` | http://epg.112114.xyz/?ch={name}&date={d |
| YanG•直播 | `https://tv.iill.top/m3u/Live` |  |
| YanG•体育 | `https://tv.iill.top/m3u/Sport` |  |
| 范明明•ipv6 | `https://live.fanmingming.com/tv/m3u/ipv6.m3u` | http://epg.112114.xyz/?ch={name}&date={d |
| YueChan•综合 | `https://github.moeyy.xyz/https://raw.githubusercontent.com/YueChan/Live/main/IPTV.m3u` | http://epg.112114.xyz/?ch={name}&date={d |
| Yoursmile•综合 | `https://agit.ai/Yoursmile7/TVBox/raw/branch/master/live.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| MemoryC•综合 | `https://ghp.ci/raw.githubusercontent.com/MemoryCollection/IPTV/refs/heads/main/itvlist.m3u` | http://epg.112114.xyz/?ch={name}&date={d |
| 肥猫•综合 | `http://我不是.肥猫.live/TV/tvzb.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| Ray•综合 | `https://github.moeyy.xyz/https://raw.githubusercontent.com/dxawi/0/main/tvlive.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| 俊于•综合 | `http://home.jundie.top:81/Cat/tv/live.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| 初秋语•电台 | `https://raw.githubusercontent.com/ches2010/tvbox/master/radio.txt` |  |
| 范明明•电台1 | `https://github.moeyy.xyz/https://raw.githubusercontent.com/fanmingming/live/main/radio/m3u` |  |
| 范明明•电台2 | `https://github.moeyy.xyz/https://raw.githubusercontent.com/fanmingming/live/main/radio/m3u` |  |
| 16万•MV | `https://github.moeyy.xyz/https://raw.githubusercontent.com/lystv/short/main/影视/tvb/MTV.txt` |  |
| YuanHsing•油管 | `https://github.moeyy.xyz/https://raw.githubusercontent.com/YuanHsing/YouTube_to_m3u/main/y` |  |
| Ray | `https://github.moeyy.xyz/https://raw.githubusercontent.com/dxawi/0/main/tvlive.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| 初秋语•综合 | `https://raw.githubusercontent.com/ches2010/tvbox/master/list.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| PG•油管 | `https://raw.githubusercontent.com/ches2010/tvbox/master/yo21.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| 本地live文件 | `http://127.0.0.1:9978/file/XYQTVBox/live.txt` | https://epg.112114.xyz/?ch={name}&date={ |
| FMM_IPV6 | `https://m3u.ibert.me/txt/fmml_ipv6.txt` | https://epg.112114.xyz/?ch={name}&date={ |
| 全是气氛组 | `` |  |
| 初秋语 | `https://raw.githubusercontent.com/ches2010/tvbox/master/listx.m3u` |  |
| 糖意 | `https://wp.weikete.cc/down.php/c3eecc14547f5e1e468718148c9f762f.txt` | http://epg.112114.xyz/?ch={name}&date={d |
| YanG | `https://tv.iill.top/m3u/Adult` |  |
| atsushi | `https://fongmi.cachefly.net/atsushi444/iptv-epg/main/Adult.m3u` |  |
| 小武哥 | `https://qu.ax/PEHy.txt` |  |
| AV | `https://raw.githubusercontent.com/ches2010/tvbox/master/livex.m3u` |  |
| 直播 | `https://fongmi.cachefly.net/YanG-1989/m3u/main/XX.m3u` |  |
| 牛播一（仅新版影视仓┃ok影视） | `http://127.0.0.1:9978/proxy?do=饭太硬&type=liveList` | http://epg.51zmt.top:8000/api/diyp/?ch={ |
| V4聚合（卡顿请按左┃右键换线） | `https://gh-proxy.net/https://raw.githubusercontent.com/wwb521/live/main/tv.m3u` | http://epg.51zmt.top:8000/api/diyp/?ch={ |
| V6范明明 | `https://gh-proxy.net/https://raw.githubusercontent.com/fanmingming/live/refs/heads/main/tv` |  |
| 平台直播 | `http://tv.iill.top/m3u/Live` |  |
| migu | `https://gitee.com/dream-deve/migu_video/raw/main/interface.txt` | https://epg.112114.xyz/?ch={name}&date={ |
| fmm | `https://live.fanmingming.com/tv/m3u/ipv6.m3u` | https://epg.112114.xyz/?ch={name}&date={ |
| ipv6 | `https://raw.githubusercontent.com/ches2010/tvbox/master/tvboxtv.txt` | https://epg.112114.xyz/?ch={name}&date={ |

## 七、解析线路（parses）

- **Json聚合**: `Demo` 
- **jx**: `https://jx.m3u8.tv/jx/jx.php?url=` 
- **羽路均沾**: `https://ylu.cc/index.php?url=` 
- **999**: `https://huayong.net/999/?v=` 
- **jx5**: `https://huayong.net/jx5/?url=` 
- **无尽**: `https://jx.wujinkk.com/dplayer/?url=` 
- **yemu**: `https://www.yemu.xyz/?url=` 
- **解析1**: `https://bd.jx.cn/?url=` 
- **解析2**: `https://jx.m3u8.tv/jiexi/?url=` 
- **解析3**: `https://player.mrgaocloud.com/player/?url=` 
- **vip4**: `https://huayong.net/vip4/?url=` 
- **免费分享**: `https://jx.xmflv.com/?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'mgtv', '芒果', 'imgo', 'letv', '
- **全民**: `http://api.wpsseo.cn/?v=` flag:['qiyi', 'imgo', '爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv',
- **m3u8**: `https://jx.m3u8.tv/jiexi/?url=` 
- **8090**: `https://www.8090.la/lander?url=` flag:['qiyi', 'imgo', '爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv',
- **看看**: `https://jx.m3u8.pw/?url=` 
- **巧技**: `http://pandown.pro/app/kkdy.php?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'sohu', '搜狐', 'letv', '乐视', 'mg
- **左岸**: `https://api.tyun77.cn/api.php/provide/parseDicturl?url=` flag:['爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv', '乐视', 'leshi', 
- **咸鱼**: `https://jx.xymp4.cc/?url=` 
- **无上**: `https://mfjx.iwsyy.xyz/?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'sohu', '搜狐', 'letv', '乐视', 'mg
- **巧计**: `https://zy.qiaoji8.com/xiafan.php?url=` flag:['QD4K', 'iyf', 'duanju', 'gzcj', 'GTV', 'GZYS', 'weggz', 'Ace']
- **虾米**: `https://jx.xmflv.com/?url=` 
- **淘片**: `https://jx.yparse.com/index.php?url=` 
- **冰豆**: `https://bd.jx.cn/?url=` 
- **七七**: `https://jx.77flv.cc/?url=` 
- **盘古**: `https://www.playm3u8.cn/jiexi.php?url=` 
- **夜幕**: `https://yemu.xyz/?url=` 
- **云解析**: `https://jx.yparse.com/index.php?url=` 
- **PM**: `https://www.playm3u8.cn/jiexi.php?url=` flag:['qiyi', 'imgo', '爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv',
- **8090**: `https://www.8090.la/8090/?url=` flag:['qiyi', 'imgo', '爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv',
- **咸鱼**: `https://jx.xyflv.cc/?url=` 
- **爱豆**: `https://jx.aidouer.net/?url=` 
- **巧技**: `http://pan.qiaoji8.com/tvbox/neibu.php?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'sohu', '搜狐', 'letv', '乐视', 'mg
- **巧技二**: `http://pan.qiaoji8.com/tvbox/gouzi.php?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'sohu', '搜狐', 'letv', '乐视', 'mg
- **飞云**: `https://daina.hk/api/?key=e1e84ecae847b6e4a1a27fd4611f45f5&url=` flag:['FYNB', 'mgtv', 'qiyi', 'imgo', '爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv',
- **米饭**: `https://api.json.icu/api/?key=b1f3b1a9e1c0a42ae969db10bd315a95&url=` flag:['爱奇艺', '奇艺', 'qq', '腾讯', 'youku', '优酷', 'pptv', 'PPTV', 'letv', '乐视', 'leshi', 
- **饭饭**: `http://api.888484.xyz/神秘哥哥/super.php?v=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'tucheng', 'sohu', '搜狐', 'letv'
- **世界**: `http://175.178.183.192:84/api/?key=f3913eb3f85a8298b3e6e427b8712b2e&url=` flag:['qq', 'mgtv', 'YuMi-vip']
- **Web聚合**: `Web` 
- **-咸鱼-**: `https://jx.xyflv.cc/?url=` 
- **-虾米-**: `https://jx.xmflv.com/?url=` 
- **-爱酷-**: `https://jx.zhanlangbu.com/?url=` flag:['qq', '腾讯', 'qiyi', '爱奇艺', '奇艺', 'youku', '优酷', 'mgtv', '芒果', 'letv', '乐视', 'pp
- **-云解析-**: `https://jx.yparse.com/index.php?url=` 
- **-777-**: `https://jx.jsonplayer.com/player/?url=` 
- **-剖云-**: `https://www.kkvip2022.com/vip/jiexi1/?url=` 
- **-全看-**: `https://jx.quankan.app/?url=` 
- **-爱豆-**: `https://jx.aidouer.net/?url=` 
- **聚合**: `Demo` 

## 八、已知问题与修复

**跳过的候选文件**（非标准 JSON、非配置或规则/扩展文件）：
- `0707.json`: sites 不足
- `config_open.json`: 解析失败(JSONDecodeError)
- `tv.json`: 解析失败(JSONDecodeError)

**相对路径修复**（原引用相对仓库根不存在；子配置内相对路径按其配置文件所在目录解析，其余改指向仓库内对应文件）：
- `FTY/…` → `FTY/…`（1 条）：兔小贝.js
- `wogg.json/…` → `json/…`（4 条）：wogg.json
- `lib/…` → `json/…`（3 条）：wogg.json
- `json/…` → `xiaosa/…`（24 条）：123.json、bj.json、ex.json、hb.json、hm.json、kf.json、lb.json、lj.json、mogg.json、og.json、quarkShare.json、sd.json、wogg.json、xbky.json、yyds.json、zz.json、哔哩合集.json、哔哩哔哩听书.json、哔哩哔哩小品.json、哔哩哔哩戏曲.json、哔哩哔哩演唱会.json、哔哩哔哩相声.json、哔哩视频.json、采集静态.json
- `js/…` → `xiaosa/…`（44 条）：56动漫.js、NT动漫.js、drpy2.min.js、人人电影网.js、优酷视频.js、全盘.js、剧圈圈.js、动漫岛.js、动画片大全.js、去看吧.js、双星.js、奇米动漫.js、好看动漫.js、异世界.js、校长.js、爱奇艺.js、爱弹幕.js、短剧全网.js、腾讯视频.js、芒果视频.js、花子动漫.js、路漫漫.js、采集之王.js
- `py/…` → `xiaosa/…`（11 条）：偷乐短剧.py、剧王短剧.py、哔哩直播.py、爱看短剧.py、猎手影视.py、甜圈短剧.py、网络直播.py、追星影视.py、金牌影视.py、锦鲤短剧.py、零度影视.py
- `XYQHiker/…` → `xiaosa/…`（6 条）：农民影视.json、巴士动漫.json、电影港.json、电影牛.json、盒子影视.json、短剧屋.json
- `XBPQ/…` → `xiaosa/…`（15 条）：七点影视.json、三九影视.json、三四影视.json、修罗影视.json、免费影视.json、养生堂.json、哆啦新番社.json、明星影视.json、樱花动漫.json、永乐影视.json、狐狸君.json、雪糕影视.json、面包影视.json、骚火影视.json、麦田影视.json

**未能解析的引用**（保持原样，可能失效，请自行处理）：
- `./XBPQ/91黑料.json`
- `./XBPQ/我爱AV.json`
- `./XBPQ/色花堂.json`
- `./XBPQ/黄色仓库.json`
- `./js/西门[V2].js`
- `./lib/tokenm.json`
- `./lib/wog.json`
- `./tv.txt`

**接口抽查**（type=1 直连采集接口）：
- 影视 | 量子[直连] (`量子`): OK 12051B — https://cj.lziapi.com/api.php/provide/vod/
- 影视 | 非凡[直连] (`非凡`): OK 5289B — http://ffzy.tv/api.php/provide/vod/
- 影视 | 卧龙资源[直连] (`卧龙资源`): EMPTY/非标准 6990B — https://wolongzyw.com/api.php/provide/vod/?ac=list
- 影视 | ikun资源[直连] (`ikun资源`): FAIL URLError 0B — https://www.ikunzy.com/api.php/provide/vod/
- 影视 | 尤酷[直连] (`U酷资源風`): FAIL URLError 0B — https://api.ukuapi.com/api.php/provide/vod/?ac=list
- 影视 | 360资源[直连] (`360资源`): OK 6061B — https://360zy.com/api.php/provide/vod/?ac=list
- 影视 | 极速[直连] (`极速资源`): OK 6073B — https://jszyapi.com/api.php/provide/vod/?ac=list
- 影视 | 率率[直连] (`率率`): OK 5783B — https://suoniapi.com/api.php/provide/vod/?ac=list

> 说明：整合仅合并配置与相对路径改写，不改动任何抓取规则本体；各源可用性取决于上游站点，建议导入后在应用内逐一测试。