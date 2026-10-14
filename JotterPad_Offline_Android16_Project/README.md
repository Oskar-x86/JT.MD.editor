# JotterPad Offline (Android 16 / API Level 36 适配版工程)

本工程严格按照《JotterPad (Android) 深度功能分析与离线/连线功能架构文档》第 3 章【离线功能】完整复刻，并针对 **Android 16 (API Level 36 "Baklava")** 进行了原生规范适配。

## 一、Android 16 (`targetSdk = 36`) 专项适配清单

1. **强制全面屏边到边布局 (Edge-to-Edge Enforcement)**：
   - Android 16 已正式废弃并禁用 `R.attr.windowOptOutEdgeToEdgeEnforcement`。本工程通过 `WindowCompat.setDecorFitsSystemWindows(window, false)` 与 `WindowInsetsCompat.Type.systemBars() | Type.ime()` 原生处理状态栏、手势导航栏及软键盘避让。
2. **预测式返回手势 (Predictive Back Navigation)**：
   - Android 16 默认启用预测式返回动画且不再触发旧版 `onBackPressed()`。本工程在 `AndroidManifest.xml` 中声明 `android:enableOnBackInvokedCallback="true"`，并在 `MainActivity` 中注册 `OnBackInvokedCallback`。
3. **大屏与折叠屏自适应布局 (Adaptive Layouts on `sw600dp+`)**：
   - Android 16 在宽度 `>= 600dp` 的大屏/折叠屏/平板及桌面窗口模式下强制忽略 `screenOrientation` 与 `resizeableActivity="false"`。本工程采用响应式流式网格与自适应双栏预览，无方向锁定。
4. **Safer Intents 与 16 KB 内存页兼容**：
   - 已在 `AndroidManifest.xml` 中启用 `android:intentMatchingFlags="enforceIntentFilter"`，并在 Gradle 中启用 16 KB 页面对齐配置。

## 二、如何一键生成 `.apk` 安装包（两种方式）

### 方式 A：免配环境，使用 GitHub Actions 云端自动打包（推荐，2 分钟完成）
1. 将本压缩包解压并上传至你的任意 GitHub 仓库（公开或私有均可）。
2. 进入仓库顶部的 **Actions** 标签页，点击 **Build Android 16 (API 36) APK** -> **Run workflow**。
3. 等待约 2 分钟编译完成后，在页面底部的 **Artifacts** 区域即可直接下载编译好的 `JotterPad-Offline-Android16-APK.zip`（内含可直接安装到 Android 16 手机的 `.apk` 文件）。

### 方式 B：使用 Android Studio 本地打包
1. 打开 Android Studio (Ladybug / Meerkat 或更新版本)，选择 **Open** 打开本工程目录。
2. 等待 Gradle 同步完成（已配置 `compileSdk = 36`、`targetSdk = 36`）。
3. 点击顶部菜单 **Build -> Build Bundle(s) / APK(s) -> Build APK(s)**，即可生成 `.apk` 安装包。
