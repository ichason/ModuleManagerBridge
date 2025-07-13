# GitHub Actions 自动构建说明

我已经为你的项目配置了GitHub Actions自动构建。以下是使用说明：

## 文件说明

### 1. `.github/workflows/build.yml` - 常规构建
- **触发条件**: 当你推送代码到 `main`, `master`, 或 `art` 分支时
- **功能**: 
  - 自动编译项目
  - 构建Release APK
  - 生成API JAR文件
  - 上传构建产物到GitHub Actions artifacts

### 2. `.github/workflows/release.yml` - 发布构建
- **触发条件**: 当你创建以 `v` 开头的标签时（如：`v1.0.0`）
- **功能**:
  - 自动编译项目
  - 构建Release APK
  - 生成API JAR文件
  - 自动创建GitHub Release
  - 将构建产物上传到Release页面

## 技术说明

### 🔧 Android SDK兼容性解决方案

由于你的项目使用较老的Android SDK版本（API 23, Build Tools 23.0.3）和Gradle 3.2，我们采用了以下兼容性解决方案：

**问题**: 
- 最新的Android SDK工具需要Java 17+
- 但你的项目使用Gradle 3.2，只支持Java 8

**解决方案**:
- 使用Java 8作为构建环境
- 手动安装兼容Java 8的Android SDK工具（版本9.0）
- 只安装项目实际需要的SDK组件：
  - `platforms;android-23` - Android 6.0 SDK平台
  - `build-tools;23.0.3` - 构建工具

这样既保证了构建的兼容性，又避免了不必要的版本冲突。

## 使用方法

### 常规开发构建
1. 将代码推送到你的GitHub仓库
2. GitHub Actions会自动开始构建
3. 构建完成后，你可以在Actions页面下载构建产物

### 发布新版本
1. 创建并推送一个版本标签：
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
2. GitHub Actions会自动构建并创建Release
3. 用户可以从Release页面下载APK和API文件

## 构建产物

每次构建会生成以下文件：
- `app-release.apk` - 应用的APK文件
- `api.jar` - API JAR文件
- `api-sources.jar` - API源码JAR文件

## 查看构建状态

1. 访问你的GitHub仓库
2. 点击 "Actions" 标签
3. 你可以看到所有的构建历史和状态

## 故障排除

如果构建失败，请检查：
1. 代码是否有编译错误
2. 依赖项是否正确
3. Android SDK版本是否匹配

## 环境配置

当前配置使用：
- Java 8 （与Gradle 3.2兼容）
- Android SDK 23
- Android Build Tools 23.0.3
- 手动安装的Android SDK工具（版本9.0，兼容Java 8）

如果需要修改版本，请编辑对应的workflow文件。 