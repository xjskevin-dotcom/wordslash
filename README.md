# 千词万斩 · WordSlash

背单词闯关 App。整份应用是一个自包含 HTML（含 3600 词、585 道真题、全部玩法），用 Capacitor 套壳成安卓 App，用 GitHub Actions 自动出 APK——**你不需要安装 Android Studio**。

## 目录结构
```
wordslash-app/
├── www/                    App 本体
│   ├── index.html          游戏（1.4MB，全部逻辑与数据）
│   ├── manifest.webmanifest
│   ├── sw.js               离线缓存
│   └── icon-192/512.png    图标
├── package.json            Capacitor 依赖
├── capacitor.config.json   包名 com.wordslash.app
└── .github/workflows/build-apk.yml   自动构建
```

## 出 APK（推荐：GitHub 云构建，全程网页操作）

1. 在 GitHub 新建一个仓库（可设为私有）
2. 把 `wordslash-app` 里的**全部文件**上传上去（包含 `.github` 文件夹）
3. 进仓库 **Actions** 标签页 → 左侧「Build Android APK」→ 右上 **Run workflow**
4. 等 3~5 分钟，构建完成后在该次运行页面底部 **Artifacts** 下载 `千词万斩-APK`
5. 解压得到 `千词万斩-v1.0.apk`，传到手机安装（首次需允许「未知来源」）

> 打 tag 会自动发布到 Releases：`git tag v1.0 && git push --tags`，
> 之后 APK 直接挂在仓库 Releases 页，把那个下载链接放到你官网即可。

## 本地出 APK（如果你有 Android Studio）
```bash
npm install
npx cap add android
npx cap sync android
cd android && ./gradlew assembleDebug
# 产物: android/app/build/outputs/apk/debug/app-debug.apk
```

## 关于签名
- 默认构建 **debug 签名 APK**，可直接安装、可挂官网分发。
- 若要上架应用商店（Play/华为/小米），需 release 签名：生成 keystore，
  在仓库 Settings → Secrets 配置 `KEYSTORE_BASE64 / KEY_ALIAS / KEY_PASSWORD`，
  并把 workflow 的 `assembleDebug` 换成 `assembleRelease` + 签名步骤。

## 网页版
`www/` 直接托管到任意静态空间（GitHub Pages / Netlify / Vercel）即可作为免安装网页版，
手机 Chrome 打开可「添加到主屏幕」离线使用。

## 隐私
纯本地运行，不联网、不收集任何用户数据。发音等外部请求可选、失败自动降级。
