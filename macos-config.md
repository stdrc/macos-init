# macOS 初始化配置

## 0) 目标与原则
- 本文档用于在新 Mac 上执行一次完整初始化，不用于记录历史进展。
- 所有安装行为默认从官网入口发起：先访问官网，再获取下载链接、安装脚本或安装命令。
- 默认使用 `agent-browser` 执行官网访问与下载动作（例如点击下载按钮、抓取下载链接）。
- GUI 应用不使用 Homebrew Cask 管理，优先官网安装包直装。
- Apple Silicon 机器上，安装包架构必须满足：优先 `arm64`，可接受 `universal`，避免仅 `x86_64`。
- 每项配置执行后应做最小验证，确保可复现。

## 1) 执行顺序（必须按顺序）

### 1.1 基础工具链
- 安装 `Homebrew`（官方脚本；仅用于少量基础 CLI，不用于 GUI 应用管理）。
- 安装 `uv`（官网 install 脚本，优先通过官网页面获取脚本入口）。
- 安装 `Volta`（官网 install 脚本，优先通过官网页面获取脚本入口）。
- 通过 `Volta` 安装 `Node.js`（自动包含 `npm`）。

### 1.2 自动化工具
- 通过 `npm` 或 `npx` 使用 `agent-browser`。
- 运行 `agent-browser install` 安装浏览器运行时（Playwright Chromium）。

### 1.3 官网应用安装
- 安装 `Bitwarden`（官网 DMG）。
- 安装 `Typeless`（官网安装包）。
- 安装 `Visual Studio Code`（官网安装包）。
- 安装 `AltTab`（官网 GitHub Release 安装包）。
- 安装 `ChatGPT`（官网 DMG）。
- 安装 `Claude`（官网 DMG）。
- 安装 `Microsoft Edge`（官网 DMG 或 PKG，优先确保最终 app 架构含 `arm64`）。
- 安装 `WeChat`（官网 DMG）。
- 安装 `OrbStack`（官网 Apple Silicon 安装包）。
- 安装微信输入法（官网下载安装器；页面动态加载时使用 `agent-browser` 点击“下载 macOS 版”）。

## 2) 必配系统设置

### 2.1 桌面与窗口
- 设置壁纸为 `Ventura Graphic.madesktop`（非缩略图路径）。
- 设置“根据最近使用自动重新排列 Spaces”为关闭。
- 设置“点击壁纸显示桌面”为关闭。
- 设置 `Stage Manager` 为关闭。

### 2.2 Dock
- 设置 Dock 自动隐藏为开启。
- 设置 Dock 图标大小为 `36`。
- 设置“在 Dock 中显示最近使用的应用”为关闭。

### 2.3 指针与手势
- 设置鼠标指针速度为 `2.0`。
- 设置触控板指针速度为 `2.0`。
- 设置三指拖移为开启。
- 设置三指左右/上下多任务手势为关闭。
- 设置四指左右/上下多任务手势为开启。

### 2.4 Finder（侧边栏与默认路径）
- 设置 Finder 新窗口默认打开路径为 `~`（home 目录）。
- 侧边栏 Favorites 保留并排序为：`~` 在 `Applications` 前。
- 侧边栏关闭以下项目：`Recents`、`Documents`、`AirDrop`、`Shared`。
- iCloud Drive 开启 `Desktop & Documents` 同步（桌面与文稿）。
- 如通过 UI 自动化执行 Finder 设置，使用 `⌘,` 打开 Finder Settings，并使用 `⌘3` 进入 `Sidebar` 标签后勾选/取消项目。
- Finder 侧边栏目标顺序（Favorites 段）：`~` -> `Applications` -> `Desktop` -> `Downloads`。

#### 2.4.1 Finder 配置执行流程
1. 打开 Finder 设置：在 Finder 前台发送 `⌘,`。
2. 配置 General：发送 `⌘1` 进入 `General` 标签。
3. 在 `General` 中勾选：`Store your Desktop & Documents folders in iCloud Drive...`（即 Desktop & Documents 同步）。
4. 配置 Sidebar：发送 `⌘3` 进入 `Sidebar` 标签。
5. 在 `Sidebar` 中确认/调整：
   - `~`（home）勾选。
   - `Recents`、`Documents`、`AirDrop`、`Shared` 取消勾选。
   - `iCloud Drive` 勾选。
6. 关闭设置窗口后，打开一个新的 Finder 窗口并验证侧边栏顺序；必要时通过拖拽将 `richard` 放到 `Applications` 前。
7. 通过 Finder 偏好写入确保新窗口默认路径：
   - `defaults write com.apple.finder NewWindowTarget -string PfHm`
   - `defaults write com.apple.finder NewWindowTargetPath -string "file://${HOME}/"`
   - `killall Finder`

### 2.5 显示
- 关闭 `True Tone`（自动色调/原彩显示）。

#### 2.5.1 True Tone 配置执行流程（UI 自动化）
1. 打开显示设置页：`open "x-apple.systempreferences:com.apple.Displays-Settings.extension"`。
2. 在 `System Settings > Displays` 主内容区定位 `True Tone` 开关（`AXCheckBox name=True Tone`）。
3. 若当前值为开启（`value=1`），点击该开关切换为关闭（`value=0`）。
4. 再次读取控件值确认 `True Tone` 为关闭状态。

## 3) Dock 排列规范（按分类）
- 分类顺序：生活 -> 娱乐 -> 生产力。
- 生活：`App Store`、`Calendar`、`Notes`、`Photos`、`Voice Memos`、`WeChat`。
- 娱乐：`Music`、`Safari`、`Microsoft Edge`。
- 生产力：`Terminal`、`Visual Studio Code`、`ChatGPT`、`Claude`。
- 分类之间插入 `Spacer` 分隔。

## 4) 验收标准（必须检查）
- `node -v`、`npm -v`、`uv --version`、`volta --version` 可执行。
- 应用存在于 `/Applications` 且可正常启动。
- 关键应用架构检查通过（含 `arm64`）：
  - `lipo -archs "/Applications/Microsoft Edge.app/Contents/MacOS/Microsoft Edge"`
  - `lipo -archs "/Applications/Visual Studio Code.app/Contents/MacOS/Electron"`
  - `lipo -archs "/Applications/AltTab.app/Contents/MacOS/AltTab"`
  - `lipo -archs "/Applications/WeChat.app/Contents/MacOS/WeChat"`
  - `lipo -archs "/Applications/OrbStack.app/Contents/MacOS/OrbStack"`
  - 其他应用按 `CFBundleExecutable` 对应二进制检查。
- Dock 顺序与分类符合第 3 节。
- 手势、Dock、桌面相关设置与第 2 节一致。
- Finder 侧边栏与默认路径符合第 2.4 节。
- 显示设置符合第 2.5 节（`True Tone` 关闭）。
- iCloud 同步验收：`defaults read MobileMeAccounts` 中存在 `ServiceID = com.apple.Dataclass.CloudDesktop` 且 `status = active`。

## 5) 备注（执行策略）
- 遇到 GUI 安装器需要管理员权限时，允许人工确认安装步骤。
- 官网优先级高于其他来源；除非官网不可用，不使用第三方下载站。
- 若某配置项不存在稳定、可复用的 `defaults`/CLI 修改路径，则统一使用 UI 自动化完成配置。
- UI 自动化执行系统设置时，避免依赖设置页搜索框输入关键词（输入法会影响搜索词），优先使用直达 deep link + 控件定位。
- 若自动化工具无法直接控制某页面，退回“打开官网 + 人工点击安装”的兜底流程。
