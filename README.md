<div align="center">
<img src="./assets/icon.png" width="120px"/>

# WeType UI Enhanced

一个以 **微信输入法（WeType）界面美化与个性化** 为主要功能的 Xposed 模块，同时为作用域其它输入法解锁 MIUI 全面屏优化限制。


<p align="center">

![Android 12 or later](https://img.shields.io/badge/Android-12%2B-3DDC84?logo=android&amp;logoColor=white)
![LSPosed 102](https://img.shields.io/badge/LSPosed-Modern_API_102-5C6BC0)
![Jetpack Compose](https://img.shields.io/badge/UI-Jetpack%20Compose-blue)
![License](https://img.shields.io/badge/License-AGPLv3-orange)

</p>
</div>

---

## 此 Fork 与 1.28.2 测试版

此仓库属于原项目的 Fork 网络，沿用 AGPL-3.0 许可证。`main` 为本 Fork 的发布分支。1.28.1 基于 [NEORUAA/WeType_UI_Enhanced](https://github.com/NEORUAA/WeType_UI_Enhanced) 上游 `f863450` 的 1.28.0 源码；1.28.2（versionCode 37）将本次修复适配到上游 `b4f3060`，包含其新增 SVG 图标功能、性能优化及剪贴板清空确认页处理，并保留本 Fork 自定义字体。

1.28.2 为待实机确认的测试版，debug / release 构建与 89 个单元测试通过。适配保留上游的可选 getInputView 缓存和普通背景复用，并补充清空确认页的绘制遮挡；同时修正 SVG 导入提示通过 LocalContext 读取资源的 Compose lint 错误。此版本尚未进行实机回归，请验证翻译 / AI 展开、键盘选择页进出和剪贴板清空确认。上游 PR 当前关闭，待使用者确认后再提交。

1.28.1（versionCode 36）包含以下修复：

- 修复微信输入法 Tinker 热修复启用后，模块使用原 APK 类加载器而导致候选词背景圆角等 hook 未生效的问题。宿主 hook 在 `Application.attach` 完成后使用实际的 `Context.classLoader` 安装，热重载也沿用该加载器。
- 修复键盘重新布局时过早清空美化背景的问题，区分等待布局、实际隐藏和有效背景更新三种状态。
- 使用绘制合成替代旧的浮层显隐干预，处理透明设置页、表情和剪贴板等页面透出底层内容的问题。遮挡强度跟随宿主透明度变化，不修改宿主的显隐、位移或动画时长。
- 保留键盘选择页共用的返回控件与导航区域，避免整块候选栏被误遮挡。
- 为高级材质的 `RuntimeShader` 几何更新补充 Android 13 / API 33 检查。
- 保留本 Fork 的自定义字体资源。

本轮在微信输入法 **3.5.4（57201）** 上验证了候选背景圆角 0 / 60、键盘选择页返回控件，并收到设置与动画恢复正常的实机反馈；其他输入法版本和 ROM 的兼容性需要分别验证。源码包含 53 个通过的单元测试，覆盖背景状态、遮挡关系、导航区域和进入 / 退出透明度变化。1.28.1 安装包与已验证的修复代码一致，仅更新发布版本号及文档，未再次安装到测试手机。

## 功能

### 微信输入法美化

- 自定义浅色 / 深色模式下的窗口背景颜色与透明度
- 自定义浅色 / 深色按键颜色、透明度与圆角
- 自定义背景模糊强度、平滑圆角及边缘高光效果
- 自定义输入法全局品牌强调色
- 调节工具栏图标背景透明度
- 调节候选词背景透明度与圆角
- 调节首个候选词及候选栏拼音边距
- 支持阻止微信输入法热更新

### 高级材质与液态玻璃

在支持相应系统视效接口的 HyperOS 设备上，可在设置的「外观」分组中启用：

- **高级材质**：采用超级小爱输入法的系统磨砂背景效果，保留模块的自定义平滑圆角。
- **系统液态玻璃**：在高级材质下启用原生玻璃渲染，支持调节折射、厚度、模糊及高光等参数；使用「颜色」分组保存的浅色 / 深色背景色与透明度作为 tint。

两项功能默认关闭，需要系统视效处于开启状态；不支持的设备上，对应开关不可用。

### MIUI / HyperOS 附加功能

在支持小米全面屏键盘优化的 MIUI / HyperOS 系统上，提供三方输入法解锁全面屏键盘优化限制，解锁小米短语的包名校验，修复三方输入法无法获取系统剪贴板列表的问题。

该部分并非模块主要功能，在非小米系统上不会启用，也不影响 WeType 美化功能。

## 效果预览

默认效果为 iOS 27 Apple 官方设计稿内的配色、圆角等数值
<details open>
<summary>#FB7299 主题色截图</summary>
<table>
  <tr>
    <td><img src="./assets/prew/dark_1.jpg" width="200" alt="深色模式"></td>
    <td><img src="./assets/prew/dark_2.jpg" width="200" alt="深色模式"></td>
    <td><img src="./assets/prew/light_1.jpg" width="200" alt="浅色模式"></td>
    <td><img src="./assets/prew/light_2.jpg" width="200" alt="浅色模式"></td>
  </tr>
</table>
</details>

## 使用要求

- Android 12+
- LSPosed / 兼容的 Xposed 框架
- Xposed API Version ≥ 102
- 微信输入法

安装模块后，在 LSPosed 中启用模块并勾选 **微信输入法** 作用域，然后重启微信输入法。

模块生效后，可通过桌面入口进入设置；也可以点击微信输入法「关于」页面中的 Logo 打开寄生设置页。

部分设置修改后需要重启微信输入法进程才能完全生效。

## 测试环境

设备：Xiaomi 17 Pro

HyperOS 4.0.0.27 Beta

Android 17

LSPosed v2.1.1-it (7846)

微信输入法：3.5.3.56201

## 兼容性

模块主要针对微信输入法进行适配。微信输入法内部实现、资源名称或云端热修复发生变化时，部分功能可能暂时失效。

MIUI / HyperOS 相关附加功能仅针对小米系统，不适用于其他厂商的系统级输入法优化实现。

## 下载

请前往 [本 Fork 的 Releases](https://github.com/Sunshine-zxw/WeType_UI_Universal/releases) 下载。

- `WeType_UI_Enhanced-1.28.2_release.apk`：启用代码和资源压缩，供本次实机回归测试。
- `WeType_UI_Enhanced-1.28.2_debug.apk`：可调试构建，供开发和问题排查；不包含本次开发过程中的临时探针。
- `SHA256SUMS.txt`：两个安装包的 SHA256 校验值。

本 Fork 的 1.28.1 / 1.28.2 debug 与 release 安装包使用同一个本机测试证书签名，与本轮调试安装包的证书一致；它不是上游作者的正式发行证书。已核对上游 1.28.0 与本 Fork 的证书不同，普通设备不能从上游正式包直接覆盖安装本 Fork；已经安装本 Fork 同证书版本的用户可以覆盖更新。若需向上游用户提供直接更新，需要由上游维护者使用原签名证书重新构建签名。

### 从源码构建

需要 JDK 21、Android SDK Platform 37 和项目自带的 Gradle Wrapper：

```sh
./gradlew :app:assembleDebug :app:assembleRelease :app:testDebugUnitTest :app:lintRelease
```

产物位于 `app/build/outputs/apk/debug/` 与 `app/build/outputs/apk/release/`。debug 构建自动使用本机 debug keystore；release 构建需使用自己的密钥通过 `apksigner` 签名。仓库不包含签名私钥、设备日志、交接文档、录屏或诊断安装包。

上游下载渠道：

Xposed 模块仓库：https://github.com/Xposed-Modules-Repo/com.xposed.wetypehook

## 开源致谢

感谢项目 [MIUI_IME_Unlock(MIT)](https://github.com/RC1844/MIUI_IME_Unlock) 提供的解锁 MIUI 全面屏优化限制功能

感谢 [miuix](https://github.com/compose-miuix-ui/miuix) 提供的 Compose UI 库

## 开源许可

本项目自 2026.8.16 起换用 AGPL-3.0 许可协议，要求修改和分发的同时也公开源码，且使用相同的许可协议。
