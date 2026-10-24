# 手机备份聊天导出（本人数据）

> 篝火出品 · 收录于篝火 App 应用区 · 基于开源项目 [BlueMatthew/WechatExporter](https://github.com/BlueMatthew/WechatExporter)（GPL-2.0）

把**自己 iPhone 备份里的微信聊天记录**导出成 HTML / PDF / 文本，在电脑本地保存和查看。全程在本机处理。

> [!IMPORTANT]
> **只导出你本人的数据。** 请只用于导出自己手机、自己账号的聊天记录；不要导出、翻看或传播他人的备份和隐私。使用者自行承担合规责任。

## 下载

到 [Releases](https://github.com/campfire-code/wechat-backup-exporter/releases/latest) 下载：

| 电脑 | 文件 |
|---|---|
| Windows 7 及以上（64 位） | `wechat-backup-exporter_版本号_v1.9.5.13-x64-win.zip` |
| Mac（上游只提供 x64 版） | `wechat-backup-exporter_版本号_v1.9.5.13-x64-macos.zip` |

- 压缩包来自上游官方发布，篝火没有修改，附 `SHA256SUMS.txt` 可核对。解压后直接运行，无需安装。
- 上游另有命令行版（`-cmd.zip`），本仓库不转发布。
- Windows 如果提示缺少 dll，请先安装 [Visual C++ 2017 redist](https://aka.ms/vs/16/release/vc_redist.x64.exe)。
- **本机（macOS）未实测**：篝火整理时没有 iPhone 备份可用，Windows 版也未实测，以上均依据上游文档。

## 怎么用

1. 用 iTunes（Mac 用访达）把 iPhone 备份到电脑，**备份时不要设置加密口令**。Android 手机需要先把聊天记录迁移到一台 iPhone / iPad 再备份（见上游说明）。
2. 解压下载的压缩包，运行 `WechatExport.exe`（Windows）或 `WechatExporter`（Mac）。
3. 按界面提示选择备份和导出目录，选择要导出的聊天与格式（HTML / PDF / 文本）。
4. 在菜单「选项」里可以设置：页面加载方式、过滤功能、**增量导出**（只导出上次之后的新消息）。

**说明**
- 界面语言：Windows 版界面是中文。Mac 版带有简体中文语言包（源码 `WechatExporter/zh-Hans.lproj`），**跟随系统语言**，没有软件内的语言开关；如果打开后是英文，到「系统设置 → 通用 → 语言与地区」，把「简体中文」拖到首选语言第一位，重启软件即可。窗口标题栏的软件名 WechatExporter 是上游写死的原文，无法修改。
- 本说明卡上的界面图是依上游中文界面文案画的**示意图**，不是真实截图：上游自带的截图含有个人信息，没有使用。
- 导出 PDF 需要电脑装有 Chrome 或 Edge，耗时较长，请不要关闭自动弹出的命令行窗口。
- 软件启动时会向上游作者的服务器（`src.wakin.org`）请求一次更新信息，请求里带当前版本号；除此之外聊天内容不会上传。
- 日志检查：源码中没有把密钥、口令明文打印到日志的写法（按关键词检索，非逐行审计）。

## 验收用例

| 操作 | 应得到 |
|---|---|
| 准备一份未加密的 iPhone iTunes 备份，选择一个聊天导出为 HTML | 导出目录里生成 HTML 页面，用浏览器能看到文字、图片和语音 |
| 勾选增量导出，同一聊天再导一次 | 第二次只包含上次之后的新消息 |

## 和上游的区别

- 仅新增中文说明与篝火发布流程（`tool.json`、`.github/workflows/release.yml`），软件代码与上游一致。
- 上游 1.8.0.8 之前的版本有异步加载的已知 bug，本仓库转发布的是 1.9.5.13，不受影响。

## 归属与许可证

本仓库由**篝火**整理发布，是篝火 App 应用区收录的开源工具之一。软件全部功能与版权归 [BlueMatthew/WechatExporter](https://github.com/BlueMatthew/WechatExporter) 原作者所有，按 GPL-2.0 许可证再分发，源码即本仓库。全部工具目录见 [toolkit-hub](https://github.com/campfire-code/toolkit-hub)。

---

以下为上游原始说明：

# WechatExporter
  
 
> **BUG!!! 1.8.0.7以前的版本异步加载方式存在一个比较严重的小白bug：当设置为滚动到页面底部异步加载时，越靠后面的页码，加载的消息数量越少；设置为页面打开全部消息异步加载时，消息只能加载到一半。如果iTunes备份还存在，请使用版本1.8.0.8重新导出一遍。如果过往的备份已经清除了，可以下载补丁程序[Win64版本](https://github.com/BlueMatthew/WechatExporter/releases/download/v1.8.0.8/patch_x64_win.zip)/[MacOS 64版本](https://github.com/BlueMatthew/WechatExporter/releases/download/v1.8.0.8/patch_x64_macos.zip)并解压，把wxexpatch.exe/wxexppatch拷贝到导出目录，并执行，来修复已经导出的页面（补丁修复的文件清单可查看日志文件 patch.log）。**    

本程序参考 https://github.com/stomakun/WechatExport-iOS 修改成C++来实现，便于在各个平台以更少依赖运行。同时增加了聊天群名称的解析支持和更多消息类型的导出支持。导出支持Text、HTML、PDF三种格式。  
  
- 导出的聊天记录页面可以设置为打开时一次性加载完成（默认方式）、打开时异步加载、页面滑动到底部时加载更多三种方式，可以在菜单“选项”中修改加载方式。  

- 可以在导出的页面增加过滤功能，功能也需要在菜单“选项”中设置。
   
- PDF格式，实质是导出打开时一次性加载完成的HTML页面，然后通过Google Chrome或者Microsoft Edge浏览器的功能转成PDF文件，转PDF文件耗时较长，请不要关闭自动弹出的命令行窗口。  
  
- 增量导出：菜单“选项”中，如果设置了增量导出，则会仅仅导出上一次导出的最后一条消息之后的部分，通过此功能，再一次备份之后，微信中的聊天记录可以删除，下一次导出，可以把同一个聊天群的消息合并在一起。  
  
## 操作步骤：
1. 通过iTunes将手机备份到电脑上(备份时不要选择设置口令)，Windows操作系统一般位于目录：C:\用户[用户名]\AppData\Roaming\Apple Computer\MobileSync\Backup\。Android手机可以找一个iPad/iPhone设备，把聊天记录迁移到iPad/iPhone设备上，然后通过iTunes备份到电脑上。
![iTunesBackup-960](https://user-images.githubusercontent.com/37573096/125906418-090d4ac8-a2ba-4a26-9db2-c6dbed4b0a3c.png)
  
2. 下载本代码的执行文件：[Windows x64版本](https://github.com/BlueMatthew/WechatExporter/releases/download/v1.8.0.10/v1.8.0.10_x64_win.zip) 或者 [MacOS x64版本](https://github.com/BlueMatthew/WechatExporter/releases/download/v1.8.0.10/v1.8.0.10_x64_macos.zip)，然后解压压缩文件

3. 执行解压出来的WechatExport.exe/WechatExporter (Windows下如果运行报缺少必须的dll文件，请安装[Visual C++ 2017 redist](https://aka.ms/vs/16/release/vc_redist.x64.exe)后再尝试运行)

4. 按界面提示进行操作。  
![Windows界面截屏](https://src.wakin.org/github/wxexp/screenshots/win.png) ![MacOS界面截屏](https://src.wakin.org/github/wxexp/screenshots/mac.png###)

5. 导出后的页面示例：  
![导出后的页面示例截屏](https://src.wakin.org/github/wxexp/demo/demo.png)
  
[点击链接可打开网页：https://src.wakin.org/github/wxexp/demo/](https://src.wakin.org/github/wxexp/demo/)

## 模版修改
解压目录下的res\templates(MacOS版本位于Contents\Resources\res)子目录里存放了输出聊天记录的html页面模版，其中通过两个%包含起来的字符串，譬如，%%NAME%%，不要修改之外，其它页面内容和格式都可以自行调整。  
  
特别感谢Chao.M帮忙优化当前的模版。  
  
## 系统依赖：
Windows版本：Windows 7+(XP不支持), [Visual C++ 2017 redist](https://aka.ms/vs/16/release/vc_redist.x64.exe) at [The latest supported Visual C++ downloads](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)  
MacOS版本：MacOS 10.10(Yosemite)+


## 程序编译
程序依赖如下第三方库：
- libxml2: http://www.xmlsoft.org/  
- libcurl: https://curl.se/libcurl/  
- libsqlite3: https://www.sqlite.org/index.html   
- libprotobuf: https://github.com/protocolbuffers/protobuf  
- libjsoncpp: https://github.com/open-source-parsers/jsoncpp  
- lame: http://lame.sourceforge.net/ 
- silk: https://github.com/collects/silk (也参考了： https://github.com/kn007/silk-v3-decoder)  
- libplist: https://github.com/libimobiledevice/libplist  https://github.com/libimobiledevice-win32/libplist  
- libiconv(windows only): https://www.gnu.org/software/libiconv/  
- openssl(windows only)：https://github.com/openssl/openssl   
- WTL (windows only)：https://sourceforge.net/projects/wtl/  

MacOS下，libxml2,libcurl,libsqlite3直接使用了Xcode自带的库，其它第三方库需自行编译。  
libmp3lame需手动删除文件include/libmp3lame.sym中的行：lame_init_old  

Windows环境下，silk自带Visual Studio工程文件，可以直接利用Visual Studio编译，其余除了libplist之外，都通过vcpkg可以编译。libplist在vcpkg中也存在，但是在编译x64-windows-static target的时候报了错，于是直接通过Visual Studio建了工程进行编译。

https://github.com/BlueMatthew/WechatExporter/releases/download/v1.0/x64-windows-static.zip
https://github.com/BlueMatthew/WechatExporter/releases/download/v1.0/x86-windows-static.zip
https://github.com/BlueMatthew/WechatExporter/releases/download/v1.0/x64-windows-static-dbg.zip
https://github.com/BlueMatthew/WechatExporter/releases/download/v1.0/x86-windows-static-dbg.zip
https://github.com/BlueMatthew/WechatExporter/releases/download/v1.0/x64-macos-static.zip  
  
已测试iTunes和微信版本  
iTunes 12.3.3.17 + 微信6.5.9  
iTunes 12.5.1.21 + 微信6.3.30  
iTunes 12.10.10.2 + 微信7.0.2  
iTunes 12.10.9.3 + 微信 7.0.15  
iTunes 12.9.5.5 + 微信 7.0.2  
Windows 10 + iTunes 12.11.0.26(Microsoft Store) + 微信 7.0.2  
Windows 10 + iTunes 12.11.0.26(Microsoft Store) + 微信 8.0.1  
Mac Catalina (Embedded iTunes) + 微信 8.0.1/8.0.2  
Windows 7 + iTunes 12.10.9.3 + 微信版本 8.0.2  
Windows 10 + iTunes 12.11.3.17 + 微信 8.0.7  
Windows 7 + iTunes 12.10.9.3/Mac Catalina (Embedded iTunes) + 微信 7.0.2 + iOS 9.3.5  
Windows + iTunes 12.10.3.1+ 微信 7.0.10 + iOS 13.3 (@lazybug163)  
MacOS 11.6（Embedded iTunes）+ iOS Version: 15.0 + 微信 8.0.9  

