# DeepSeek Harness 教程

> 本教程由 **[Dataphany](https://dataphany.com)** 编写整理。
> Tutorial by **[Dataphany](https://dataphany.com)**.

> 🌐 **在线阅读 / Online Reading:** <https://dataphany.github.io/deepseek-harness-tutorial/>
>
> 本页为完整教程（上：中文版，下：English Version）。网页版支持中英切换、复制按钮与截图，体验更佳，建议访问上方网址。
> This page contains the full tutorial — Chinese above, English below. The interactive web version supports language switching, copy buttons, and screenshots; we recommend opening the URL above for the best experience.

📖 **[中文版](#中文版)** ｜ **[English](#english-version)**

---

## 中文版

### 一、DeepSeek Harness 是什么

**DeepSeek Harness**（简称 DSH）是 DeepSeek 开源的智能体开发框架，自带一个浏览器端的 **Web 图形界面**，默认运行在 `http://127.0.0.1:3080`。它相当于一个“带手的 AI 助手”：

- 💬 交互式对话：像聊天一样向 AI 下达任务；
- 🛠️ 工具调用：读写工作区文件、执行命令、搜索网络、委派子代理、维护计划；
- 🔐 权限审批：涉及敏感操作时会先询问你，操作透明可控；
- 📋 后台任务：长时间运行的任务可在后台执行并随时查看状态；
- 🧩 模型路由：支持 DeepSeek 官方 API，也支持 OpenAI 兼容端点与自定义 Provider。

本教程覆盖两部分内容：**① 在 Windows / macOS 上从零安装；② 界面使用教程（配截图与操作说明）**。

---

### 二、安装 DeepSeek Harness（Windows / macOS）

> 📌 **标识说明：** 带 **PowerShell** / **终端** 标识的代码块在对应平台的命令行中执行；带 **浏览器** 标识的操作在浏览器中完成；带 **文件资源管理器 / 访达** 标识的操作在系统的文件管理器中完成。

#### Windows

**准备工作**

**1. 安装 PowerShell 7**（如已安装可跳过）

在浏览器打开 <https://github.com/PowerShell/PowerShell/releases>，选择 `PowerShell-7.x.x-win-x64.msi` 下载，双击安装。

```powershell
pwsh --version   # 验证安装
```

**2. 安装 Git**

在浏览器打开 <https://git-scm.com/download/win>，下载后双击安装，一路 Next。

```powershell
git --version   # 验证安装
```

**3. 安装 Node.js LTS**

在浏览器打开 <https://nodejs.org/>，下载 LTS 版本 `.msi`，双击安装，一路 Next。

```powershell
node -v
npm -v
```

**4. 配置 DeepSeek API Key（永久写入系统变量）**

在浏览器打开 <https://platform.deepseek.com/> 登录，进入 API Keys 页面创建密钥。将下方命令中的 `sk-你的真实密钥` 替换为你的实际密钥：

```powershell
setx DEEPSEEK_API_KEY "sk-你的真实密钥"

# 重启 PowerShell 后验证
echo $env:DEEPSEEK_API_KEY
```

> 💡 也可以跳过环境变量方式，安装完成后直接在 Web 界面「设置 → 模型」里填入密钥。

**5. 安装 pnpm**

```powershell
npm install -g pnpm
```

**安装 DeepSeek Harness**

以下所有命令均在 **PowerShell** 中顺序执行：

**6. 克隆仓库**

```powershell
git clone https://github.com/deepseek-ai/deepseek-harness.git
```

**7. 进入项目目录**

```powershell
cd deepseek-harness
```

**8. 增加 Node.js 内存限制**（避免构建失败）

```powershell
$env:NODE_OPTIONS="--max-old-space-size=4096"
```

**9. 安装项目依赖**（此步骤可能耗时较长）

```powershell
pnpm install
```

**10. 构建项目**（此步骤可能耗时较长）

```powershell
pnpm run build
```

**启动使用**

**11. 启动 Web 界面**

```powershell
pnpm dsh web
```

**12. 在浏览器中打开以下地址**

```
http://127.0.0.1:3080
```

> 💡 **提示：** 设置面板中显示 **“Provided by the launch environment (read-only)”** 表示 API Key 已正确识别，可以直接使用。

**13. 获取 GitHub 最新更新（命令行）**

DeepSeek Harness 官方仓库会持续更新，随时可通过 Git 命令行拉取最新代码：

```powershell
# 1. 进入项目目录
cd deepseek-harness
# 2. 从 GitHub 拉取最新更新（官方仓库 master 分支）
git pull origin master
# 3. 同步依赖（锁文件有变化时执行）
pnpm install
# 4. 重新构建（前端界面有变化时执行）
pnpm run build
# 5. 重新启动 Web 界面
pnpm dsh web
```

> 💡 **如果第 2 步 `git pull` 失败**（提示本地有未提交改动）：按下面完整流程处理。每一步之后都用 `git status` 查看输出，决定是否进入下一步——有没有冲突也以它的输出为准：

```powershell
# 1. 查看本地改动（有未提交改动时 git pull 才会失败）
git status
# 2. 提交本地改动（把 wip 换成你自己的说明文字）
git add .
git commit -m "wip"
# 3. 重新拉取最新更新
git pull origin master
# 4. 查看是否冲突（若显示标着 both modified 的冲突文件，继续第 5 步；否则到此完成）
git status
# 5.（仅当有冲突时）用编辑器打开冲突文件：保留想要的内容，删除 <<<<<<< / ======= / >>>>>>> 标记行，保存；然后标记为已解决并完成合并提交
git add .
git commit -m "merge"
```

**14.（可选·推荐）把 DSH 数据从 C 盘迁到其他盘**

DSH 默认把 **Home**（所有会话、设置、凭据、技能等数据）放在 `~/.dsh`，在 Windows 上即 `C:\Users\你的用户名\.dsh`，并随使用不断增大。留在 C 盘会挤占系统盘、拖慢电脑，建议迁到其他盘。迁移目的地可自选，但要满足两条规则：① 不在 C 盘；② 在 `deepseek-harness` 项目目录之外（避免以后更新/重建项目时被牵连）。以下以 `F:\AgentHarness\.dsh` 为例，可换成你自己的目录。

> ⚠️ 先把正在运行的 `pnpm dsh web` 停掉：回到运行它的窗口按 `Ctrl+C`。否则有文件正被占用，迁移会失败。

```powershell
# 把 C 盘的 .dsh 复制到目标位置（只复制，源文件原样保留；路径换成你自己的）
robocopy "$env:USERPROFILE\.dsh" "F:\AgentHarness\.dsh" /E /R:1 /W:1
```

> robocopy 的退出码 0 或 1 都表示成功（8 及以上才是失败），看到提示别当报错；若目标目录不存在会自动创建。本命令只复制、不删除：C 盘原来的 .dsh 会原样保留，先不要清理，等确认新位置稳定后再自行删除。

```powershell
# 把新位置永久写入用户环境变量 DSH_HOME（DSH 启动时会优先读取它；路径换成你自己的）
setx DSH_HOME "F:\AgentHarness\.dsh"
```

> `setx` 只对之后新开的程序生效——执行完请关闭并重新打开终端。

```powershell
# 验证（在重新打开的终端里执行，应显示新路径 F:\AgentHarness\.dsh）
echo $env:DSH_HOME
```

> 显示新路径后，进入项目目录重新执行 `pnpm dsh web` 启动；浏览器里原来的会话、设置与凭据都还在（新位置读取的是同一份数据的副本）。在确认新位置稳定之前，C 盘原来的 `.dsh` 请先保留不要删除。

> 🗑️ **确认新位置稳定后，再删除 C 盘上的旧文件**：“稳定”指重启后能正常启动、原会话与设置都还在（也可以先正常使用几天再决定）。删除前先确认 `echo $env:DSH_HOME` 显示的是新路径，并先关掉 `pnpm dsh web`。删除操作不可恢复。

```powershell
# （确认稳定后）删除 C 盘上旧的 .dsh —— 不可恢复，请谨慎执行
Remove-Item "$env:USERPROFILE\.dsh" -Recurse -Force
```

> 📋 **C 盘上属于 DSH 安装、可识别并按需删除的内容**（每一项都等它在新位置的对应物验证正常后再删）：
> 1. `C:\Users\你的用户名\.dsh` — 旧 Home，即上面的删除命令目标；
> 2. 当初克隆在 C 盘的 `deepseek-harness` 项目目录（含 `node_modules`，通常体积最大）— 新盘副本验证正常、能按步骤 13 更新后再删。

> 💡 **项目与依赖缓存同样占 C 盘（通常比 Home 更大）**：① `deepseek-harness` 项目目录（含 `node_modules`）若当初克隆在 C 盘，请把整个文件夹搬到其他盘，或在其他盘重新克隆一份（之后照常按第 13 步 `git pull` 更新）；② pnpm 依赖缓存与 npm 缓存默认也在 C 盘，可重定向到其他盘，先执行 `pnpm store path` 查看当前位置，再执行下面命令（路径可换成你自己的）：

```powershell
# 把 pnpm 依赖缓存重定向到其他盘（路径换成你自己的）
pnpm config set store-dir "F:\AgentHarness\.pnpm-store"
# 把 npm 缓存重定向到其他盘
npm config set cache "F:\AgentHarness\npm-cache"
```

> 重定向后，下一次 `pnpm install` / `npm install`（包括第 13 步的更新构建）会写到新位置；旧缓存先别急着删，等在新位置成功执行过一次安装确认正常后再清理。

#### macOS

**准备工作**

**1. 安装 Homebrew**（macOS 的包管理器，后续安装均依赖它）

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**2. 安装 Git**

```bash
brew install git
git --version   # 验证安装
```

**3. 安装 Node.js LTS**（推荐使用 nvm）

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.5/install.sh | bash   # 安装 nvm
source ~/.zshrc
nvm install --lts   # 安装最新的 LTS 版本
nvm use --lts       # 切换到该版本
node -v
npm -v
```

**4. 安装 PowerShell 7（可选，但推荐）**

```bash
brew install powershell/tap/powershell
pwsh --version   # 验证安装
```

**5. 配置 DeepSeek API Key（永久写入 shell 配置文件）**

在浏览器打开 <https://platform.deepseek.com/> 登录，进入 API Keys 页面创建密钥。将下方命令中的 `sk-你的真实密钥` 替换为你的实际密钥，写入 `~/.zshrc`：

```bash
echo 'export DEEPSEEK_API_KEY="sk-你的真实密钥"' >> ~/.zshrc
source ~/.zshrc
echo $DEEPSEEK_API_KEY   # 验证
```

**6. 安装 pnpm**（推荐使用 Homebrew）

```bash
brew install pnpm
pnpm --version   # 验证安装
```

**安装 DeepSeek Harness**

以下所有命令均在 **终端** 中顺序执行：

**7. 克隆仓库**

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
```

**8. 进入项目目录**

```bash
cd deepseek-harness
```

**9. 增加 Node.js 内存限制**（避免构建失败）

```bash
export NODE_OPTIONS="--max-old-space-size=4096"
```

**10. 安装项目依赖**（此步骤可能耗时较长）

```bash
pnpm install
```

**11. 构建项目**（此步骤可能耗时较长）

```bash
pnpm run build
```

**启动使用**

**12. 启动 Web 界面**

```bash
pnpm dsh web
```

**13. 在浏览器中打开以下地址**

```
http://127.0.0.1:3080
```

> 💡 **提示：** 设置面板中显示 **“Provided by the launch environment (read-only)”** 表示 API Key 已正确识别，可以直接使用。

**14. 获取 GitHub 最新更新（命令行）**

DeepSeek Harness 官方仓库会持续更新，随时可通过 Git 命令行拉取最新代码：

```bash
# 1. 进入项目目录
cd deepseek-harness
# 2. 从 GitHub 拉取最新更新（官方仓库 master 分支）
git pull origin master
# 3. 同步依赖（锁文件有变化时执行）
pnpm install
# 4. 重新构建（前端界面有变化时执行）
pnpm run build
# 5. 重新启动 Web 界面
pnpm dsh web
```

> 💡 **如果第 2 步 `git pull` 失败**（提示本地有未提交改动）：按下面完整流程处理。每一步之后都用 `git status` 查看输出，决定是否进入下一步——有没有冲突也以它的输出为准：

```bash
# 1. 查看本地改动（有未提交改动时 git pull 才会失败）
git status
# 2. 提交本地改动（把 wip 换成你自己的说明文字）
git add .
git commit -m "wip"
# 3. 重新拉取最新更新
git pull origin master
# 4. 查看是否冲突（若显示标着 both modified 的冲突文件，继续第 5 步；否则到此完成）
git status
# 5.（仅当有冲突时）用编辑器打开冲突文件：保留想要的内容，删除 <<<<<<< / ======= / >>>>>>> 标记行，保存；然后标记为已解决并完成合并提交
git add .
git commit -m "merge"
```

**15.（可选·推荐）把 DSH 数据从系统盘迁到其他位置**

DSH 默认把 **Home**（所有会话、设置、凭据、技能等数据）放在 `~/.dsh`（启动盘上），并随使用不断增大。如果 Mac 只有一块磁盘，本步可跳过；如果还有第二块磁盘/分区，建议迁过去，避免挤占系统盘。目的地可自选，但要满足两条规则：① 不在系统盘；② 在 `deepseek-harness` 项目目录之外。以下以 `/Volumes/Data/.dsh` 为例（Data 是第二块磁盘的名字，可换成你自己的；不建议放移动硬盘，拔掉后数据将无法访问）。

> ⚠️ 先把正在运行的 `pnpm dsh web` 停掉：回到运行它的窗口按 `Ctrl+C`。否则有文件正被占用，迁移会失败。

```bash
# 1. 确认目标磁盘存在（把 Data 换成你自己的磁盘名）
mkdir -p "/Volumes/Data"
# 2. 把 ~/.dsh 复制到目标位置（只复制，源保留；路径换成你自己的）
cp -R ~/.dsh "/Volumes/Data/.dsh"
```

```bash
# 1. 把新位置写入 ~/.zshrc（永久生效；路径换成你自己的）
echo 'export DSH_HOME="/Volumes/Data/.dsh"' >> ~/.zshrc
# 2. 加载配置（或重启终端）
source ~/.zshrc
# 3. 验证
echo $DSH_HOME
```

> 在终端中执行；最后一行应显示你的新路径。然后进入项目目录重新执行 `pnpm dsh web`，浏览器里原来的会话、设置与凭据都还在（新位置读取的是同一份数据的副本）。在确认新位置稳定之前，源位置的 `~/.dsh` 请先保留不要删除。

> 🗑️ **确认新位置稳定后，再删除源位置的旧文件**：“稳定”指重启后能正常启动、原会话与设置都还在（也可以先正常使用几天再决定）。删除前先确认 `echo $DSH_HOME` 显示的是新路径，并先关掉 `pnpm dsh web`。删除操作不可恢复。

```bash
# （确认稳定后）删除源位置的 ~/.dsh —— 不可恢复，请谨慎执行
rm -rf ~/.dsh
```

> 📋 **系统盘上属于 DSH 安装、可识别并按需删除的内容**（每一项都等它在新位置的对应物验证正常后再删）：
> 1. `~/.dsh` — 原 Home 残留，即上面的删除命令目标；
> 2. 当初克隆在系统盘的 `deepseek-harness` 项目目录（含 `node_modules`，通常体积最大）— 新磁盘副本验证正常、能按步骤 14 更新后再删。

> 💡 **项目与依赖缓存同样占系统盘空间（通常比 Home 更大）**：① `deepseek-harness` 项目目录（含 `node_modules`）若当初克隆在系统盘，请把整个文件夹搬到其他磁盘，或在其他磁盘重新克隆一份（之后照常按第 14 步 `git pull` 更新）；② pnpm 依赖缓存与 npm 缓存也可重定向到其他磁盘，先执行 `pnpm store path` 查看当前位置，再执行下面命令（路径可换成你自己的）：

```bash
# 把 pnpm 依赖缓存重定向到其他磁盘（路径换成你自己的）
pnpm config set store-dir "/Volumes/Data/.pnpm-store"
# 把 npm 缓存重定向到其他磁盘
npm config set cache "/Volumes/Data/npm-cache"
```

> 重定向后，下一次 `pnpm install` / `npm install`（包括第 14 步的更新构建）会写到新位置；旧缓存先别急着删，等在新位置成功执行过一次安装确认正常后再清理。

---

### 三、界面使用教程（配截图）

安装完成后，在浏览器打开 `http://127.0.0.1:3080` 即可看到 Web 界面，下面按使用顺序介绍界面的各个部分。

#### 3.1 启动与基础配置

1. **启动服务**：在终端执行 `pnpm dsh web`，看到输出 `dsh web: http://127.0.0.1:3080` 即启动成功，在浏览器中打开该地址。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/01-terminal-start.png" alt="终端启动" width="90%"></td></tr></table>

2. **初始界面**：浏览器打开 `http://127.0.0.1:3080` 后的初始界面。首次使用需先配置模型密钥，再选择工作区，之后才能开始对话。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/02-browser-home.png" alt="初始界面" width="90%"></td></tr></table>

3. **配置模型**：打开「设置 → 模型」，在 DeepSeek 卡片中填入 API 密钥并保存，模型路由立即生效，无需重启服务。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/03-settings-models.png" alt="设置模型" width="90%"></td></tr></table>

4. **环境变量密钥**：若密钥由环境变量（`DEEPSEEK_API_KEY`）提供，设置面板显示 “Provided by the launch environment (read-only)”，表示已正确识别，可直接使用。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/04-api-key-recognized.png" alt="环境变量密钥" width="90%"></td></tr></table>

#### 3.2 工作区与主界面

1. **选择工作区**：点击「选择工作区」，添加并选中项目目录。选中后输入框才可用，agent 的文件读写都发生在这个工作区内。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/05-select-workspace.png" alt="选择工作区" width="90%"></td></tr></table>

2. **主界面**：主界面包含：会话与导航区、对话消息区（agent 的回复与工具调用在此展示）、底部输入框。输入任务后回车即可开始。

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/06-main-layout.png" alt="主界面" width="90%"></td></tr></table>

---

### 常见问题（FAQ）

- `git 不是内部或外部命令` / `git: command not found` → 未安装 Git，请回到安装章节的准备工作对应步骤。
- `node 不是内部或外部命令` / `node: command not found` → 未安装 Node.js 或未加载 nvm，请回到准备工作对应步骤。
- `pnpm: command not found` → 重新安装 pnpm：Windows 执行 `npm install -g pnpm`；macOS 执行 `brew install pnpm` 或 `npm install -g pnpm`。
- 构建时内存不足 → 确保已执行 `$env:NODE_OPTIONS="--max-old-space-size=4096"`（Windows）/ `export NODE_OPTIONS="--max-old-space-size=4096"`（macOS）。
- API Key 未被识别 → Windows 检查 `echo $env:DEEPSEEK_API_KEY`；macOS 检查 `echo $DEEPSEEK_API_KEY`；如未显示，重启终端后重试，或改用界面「设置 → 模型」直接填入密钥。
- 浏览器打开 `127.0.0.1:3080` 无响应 → 确认运行 `pnpm dsh web` 的窗口没有被关闭；查看启动输出是否出现 `dsh web: http://127.0.0.1:3080`。
- 迁移 Home 后找不到原来的会话/设置 → 关闭并重新打开终端，运行 `echo $env:DSH_HOME`（macOS 运行 `echo $DSH_HOME`）确认是新路径；确认已用 `setx DSH_HOME` / `export DSH_HOME` 写入永久环境变量，然后重新执行 `pnpm dsh web`。

---

### 📎 相关链接

- [DeepSeek Harness 官方仓库](https://github.com/deepseek-ai/deepseek-harness)
- 许可：[GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)

---

---

## English Version

### 1. What is DeepSeek Harness?

**DeepSeek Harness** (DSH) is an open-source agent framework by DeepSeek. It ships with a browser-based **Web UI**, served by default at `http://127.0.0.1:3080` — an AI assistant with hands:

- 💬 Interactive chat to hand tasks to the AI;
- 🛠️ Tool use: read/write workspace files, run commands, search the web, delegate to subagents, and maintain plans;
- 🔐 Approval: sensitive operations ask you first — transparent and safe;
- 📋 Background jobs: run long tasks in the background and check status anytime;
- 🧩 Model routing: DeepSeek API, plus OpenAI-compatible endpoints and custom providers.

This tutorial covers: **① installing from scratch on Windows / macOS; ② a Web UI guide** with screenshots and instructions.

---

### 2. Install DeepSeek Harness (Windows / macOS)

> 📌 **Legend:** Blocks marked **PowerShell** / **Terminal** run in the command line of that platform; **Browser** actions happen in a web browser; **File Explorer / Finder** actions happen in the file manager.

#### Windows

**Preparation**

**1. Install PowerShell 7** (skip if already installed)

Open <https://github.com/PowerShell/PowerShell/releases> in your browser, download `PowerShell-7.x.x-win-x64.msi` and double-click to install.

```powershell
pwsh --version   # verify
```

**2. Install Git**

Open <https://git-scm.com/download/win>, download the installer and click Next all the way through.

```powershell
git --version   # verify
```

**3. Install Node.js LTS**

Open <https://nodejs.org/>, download the LTS `.msi` and install.

```powershell
node -v
npm -v
```

**4. Set up your DeepSeek API Key (persistently, via environment variable)**

Open <https://platform.deepseek.com/>, sign in, and create a key on the API Keys page. Replace `sk-your-real-key` below with your actual key:

```powershell
setx DEEPSEEK_API_KEY "sk-your-real-key"

# Restart PowerShell to verify
echo $env:DEEPSEEK_API_KEY
```

> 💡 You can also skip the environment variable and enter the key later in the Web UI under Settings → Models.

**5. Install pnpm**

```powershell
npm install -g pnpm
```

**Install DeepSeek Harness**

Run the following commands in order in **PowerShell**:

**6. Clone the repository**

```powershell
git clone https://github.com/deepseek-ai/deepseek-harness.git
```

**7. Enter the project directory**

```powershell
cd deepseek-harness
```

**8. Increase the Node.js memory limit** (to avoid build failures)

```powershell
$env:NODE_OPTIONS="--max-old-space-size=4096"
```

**9. Install project dependencies** (this may take a while)

```powershell
pnpm install
```

**10. Build the project** (this may take a while)

```powershell
pnpm run build
```

**Launch**

**11. Start the Web UI**

```powershell
pnpm dsh web
```

**12. Open the following address in your browser**

```
http://127.0.0.1:3080
```

> 💡 **Tip:** If the settings panel shows **“Provided by the launch environment (read-only)”**, your API Key was detected and is ready to use.

**13. Pull the latest update from GitHub (command line)**

The DeepSeek Harness repository is updated continuously — pull the latest code any time from the command line:

```powershell
# 1. Enter the project directory
cd deepseek-harness
# 2. Pull the latest update from GitHub (the official repo's master branch)
git pull origin master
# 3. Sync dependencies (when the lockfile changed)
pnpm install
# 4. Rebuild (when the web frontend changed)
pnpm run build
# 5. Restart the Web UI
pnpm dsh web
```

> 💡 **If step 2 (`git pull`) fails** (it says you have uncommitted local changes): follow the full flow below. After each step, run `git status` and read its output to decide whether to continue — it is also what tells you whether there are conflicts:

```powershell
# 1. Inspect local changes (uncommitted changes are why git pull fails)
git status
# 2. Commit the local changes (replace wip with your own message)
git add .
git commit -m "wip"
# 3. Pull the latest update again
git pull origin master
# 4. Check for conflicts (if files marked "both modified" appear, continue to step 5; otherwise you are done)
git status
# 5. (only if there are conflicts) open the files in an editor, keep what you want, delete the <<<<<<< / ======= / >>>>>>> marker lines, and save; then mark them as resolved and finish the merge commit
git add .
git commit -m "merge"
```

**14. (Optional but recommended) Move DSH data off the C drive**

By default DSH keeps its **Home** — all sessions, settings, credentials, skills and other data — under `~/.dsh`, which on Windows is `C:\Users\your-name\.dsh`, and it keeps growing with use. Leaving it on the C drive eats into the system disk and slows the machine down, so moving it to another drive is recommended. Pick any destination, but follow two rules: ① not on the C drive; ② outside the `deepseek-harness` project folder (so future updates/rebuilds never touch it). The example below uses `F:\AgentHarness\.dsh` — replace it with your own folder.

> ⚠️ First stop the running `pnpm dsh web`: go back to its window and press `Ctrl+C`. Otherwise some files are locked and the move fails.

```powershell
# Copy the C-drive .dsh to the destination (copy only — the source is kept as-is; use your own paths)
robocopy "$env:USERPROFILE\.dsh" "F:\AgentHarness\.dsh" /E /R:1 /W:1
```

> robocopy exit codes 0 and 1 both mean success (8 or higher means failure); the destination folder is created automatically if missing. This command only copies — the original .dsh on the C drive is left untouched; do not clean it up until the new location has proven stable.

```powershell
# Persist the new location in the user environment variable DSH_HOME (DSH reads it on startup; use your own path)
setx DSH_HOME "F:\AgentHarness\.dsh"
```

> `setx` only affects programs started afterwards — close and reopen your terminal after running it.

```powershell
# Verify (run in the reopened terminal — it should print the new path F:\AgentHarness\.dsh)
echo $env:DSH_HOME
```

> Once it shows the new path, go to the project folder and re-run `pnpm dsh web`; your previous sessions, settings and credentials are still there (the new location reads the same data — it was copied). Keep the original `.dsh` on the C drive until you have confirmed the new location is stable.

> 🗑️ **Delete the old C-drive files only after the new location is stable**. “Stable” means it starts normally after a restart with your previous sessions and settings intact (you may also use it for a few days before deciding). Before deleting, confirm `echo $env:DSH_HOME` shows the new path and stop `pnpm dsh web` first. Deletion cannot be undone.

```powershell
# (once it is stable) delete the old C-drive .dsh — irreversible, run with care
Remove-Item "$env:USERPROFILE\.dsh" -Recurse -Force
```

> 📋 **C-drive items belonging to the DSH setup that you can identify and delete as needed** (delete each one only after its counterpart in the new location has been verified):
> 1. `C:\Users\your-name\.dsh` — the old Home, i.e. the target of the delete command above;
> 2. if you cloned `deepseek-harness` onto the C drive: that project folder (including `node_modules`, usually the largest) — delete it only after the new copy starts and updates (step 13) normally.

> 💡 **The project and dependency caches also take C-drive space (usually more than Home)**: ① if you cloned the `deepseek-harness` project folder (including `node_modules`) onto the C drive, move the whole folder to another drive, or clone it afresh there (then update as usual with `git pull`, step 13); ② the pnpm and npm caches default to the C drive too — run `pnpm store path` to see the current one, then redirect them (use your own paths):

```powershell
# Redirect the pnpm store to another drive (use your own path)
pnpm config set store-dir "F:\AgentHarness\.pnpm-store"
# Redirect the npm cache to another drive
npm config set cache "F:\AgentHarness\npm-cache"
```

> After redirecting, the next `pnpm install` / `npm install` (including step 13's update/rebuild) writes to the new location; do not delete the old cache until one install has succeeded in the new location.

#### macOS

**Preparation**

**1. Install Homebrew** (the macOS package manager; later steps depend on it)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**2. Install Git**

```bash
brew install git
git --version   # verify
```

**3. Install Node.js LTS** (nvm recommended)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.5/install.sh | bash   # install nvm
source ~/.zshrc
nvm install --lts   # install latest LTS
nvm use --lts       # switch to it
node -v
npm -v
```

**4. Install PowerShell 7 (optional but recommended)**

```bash
brew install powershell/tap/powershell
pwsh --version   # verify
```

**5. Set up your DeepSeek API Key (persistently, in your shell config)**

Open <https://platform.deepseek.com/>, sign in, and create a key. Replace `sk-your-real-key` below with your actual key, then write it to `~/.zshrc`:

```bash
echo 'export DEEPSEEK_API_KEY="sk-your-real-key"' >> ~/.zshrc
source ~/.zshrc
echo $DEEPSEEK_API_KEY   # verify
```

**6. Install pnpm** (recommended: via Homebrew)

```bash
brew install pnpm
pnpm --version   # verify
```

**Install DeepSeek Harness**

Run the following commands in order in **Terminal**:

**7. Clone the repository**

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
```

**8. Enter the project directory**

```bash
cd deepseek-harness
```

**9. Increase the Node.js memory limit** (to avoid build failures)

```bash
export NODE_OPTIONS="--max-old-space-size=4096"
```

**10. Install project dependencies** (this may take a while)

```bash
pnpm install
```

**11. Build the project** (this may take a while)

```bash
pnpm run build
```

**Launch**

**12. Start the Web UI**

```bash
pnpm dsh web
```

**13. Open the following address in your browser**

```
http://127.0.0.1:3080
```

> 💡 **Tip:** If the settings panel shows **“Provided by the launch environment (read-only)”**, your API Key was detected and is ready to use.

**14. Pull the latest update from GitHub (command line)**

The DeepSeek Harness repository is updated continuously — pull the latest code any time from the command line:

```bash
# 1. Enter the project directory
cd deepseek-harness
# 2. Pull the latest update from GitHub (the official repo's master branch)
git pull origin master
# 3. Sync dependencies (when the lockfile changed)
pnpm install
# 4. Rebuild (when the web frontend changed)
pnpm run build
# 5. Restart the Web UI
pnpm dsh web
```

> 💡 **If step 2 (`git pull`) fails** (it says you have uncommitted local changes): follow the full flow below. After each step, run `git status` and read its output to decide whether to continue — it is also what tells you whether there are conflicts:

```bash
# 1. Inspect local changes (uncommitted changes are why git pull fails)
git status
# 2. Commit the local changes (replace wip with your own message)
git add .
git commit -m "wip"
# 3. Pull the latest update again
git pull origin master
# 4. Check for conflicts (if files marked "both modified" appear, continue to step 5; otherwise you are done)
git status
# 5. (only if there are conflicts) open the files in an editor, keep what you want, delete the <<<<<<< / ======= / >>>>>>> marker lines, and save; then mark them as resolved and finish the merge commit
git add .
git commit -m "merge"
```

**15. (Optional but recommended) Move DSH data off the startup disk**

By default DSH keeps its **Home** — all sessions, settings, credentials, skills and other data — under `~/.dsh` on the startup disk, and it keeps growing with use. If your Mac has only one disk you can skip this step; if you have a second disk/partition, moving it there is recommended. Pick any destination, but follow two rules: ① not on the startup disk; ② outside the `deepseek-harness` project folder. The example below uses `/Volumes/Data/.dsh` (Data is the name of your second disk — replace it; avoid external drives, since the data becomes unreachable once unplugged).

> ⚠️ First stop the running `pnpm dsh web`: go back to its window and press `Ctrl+C`. Otherwise some files are locked and the move fails.

```bash
# 1. Make sure the destination disk exists (replace Data with your own disk name)
mkdir -p "/Volumes/Data"
# 2. Copy ~/.dsh to the destination (copy only — the source stays; use your own path)
cp -R ~/.dsh "/Volumes/Data/.dsh"
```

```bash
# 1. Write the new location into ~/.zshrc (persistent; use your own path)
echo 'export DSH_HOME="/Volumes/Data/.dsh"' >> ~/.zshrc
# 2. Reload the config (or restart Terminal)
source ~/.zshrc
# 3. Verify
echo $DSH_HOME
```

> Run in Terminal; the last line should print your new path. Then re-run `pnpm dsh web` in the project folder — your previous sessions, settings and credentials are still there (the new location reads the same data — it was copied). Keep the original `~/.dsh` until you have confirmed the new location is stable.

> 🗑️ **Delete the old files at the source location only after the new location is stable**. “Stable” means it starts normally after a restart with your previous sessions and settings intact (you may also use it for a few days before deciding). Before deleting, confirm `echo $DSH_HOME` shows the new path and stop `pnpm dsh web` first. Deletion cannot be undone.

```bash
# (once it is stable) delete the old ~/.dsh — irreversible, run with care
rm -rf ~/.dsh
```

> 📋 **Startup-disk items belonging to the DSH setup that you can identify and delete as needed** (delete each one only after its counterpart in the new location has been verified):
> 1. `~/.dsh` — the old Home, i.e. the target of the delete command above;
> 2. if you cloned `deepseek-harness` onto the startup disk: that project folder (including `node_modules`, usually the largest) — delete it only after the new copy starts and updates (step 14) normally.

> 💡 **The project and dependency caches also take startup-disk space (usually more than Home)**: ① if you cloned the `deepseek-harness` project folder (including `node_modules`) onto the startup disk, move the whole folder to another disk, or clone it afresh there (then update as usual with `git pull`, step 14); ② the pnpm and npm caches can be redirected to another disk too — run `pnpm store path` to see the current one, then redirect them (use your own paths):

```bash
# Redirect the pnpm store to another disk (use your own path)
pnpm config set store-dir "/Volumes/Data/.pnpm-store"
# Redirect the npm cache to another disk
npm config set cache "/Volumes/Data/npm-cache"
```

> After redirecting, the next `pnpm install` / `npm install` (including step 14's update/rebuild) writes to the new location; do not delete the old cache until one install has succeeded in the new location.

---

### 3. Web UI Guide

After installation, open `http://127.0.0.1:3080` in your browser to see the Web UI. Here is a walkthrough of its parts in order of use.

#### 3.1 Startup & Basic Setup

1. **Start the service**: Run `pnpm dsh web` in the terminal. When you see `dsh web: http://127.0.0.1:3080` in the output, it started successfully — open that address in your browser.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/01-terminal-start.png" alt="Start the service" width="90%"></td></tr></table>

2. **Initial screen**: After opening `http://127.0.0.1:3080`, on first use configure your model API key, then select a workspace before starting a conversation.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/02-browser-home.png" alt="Initial screen" width="90%"></td></tr></table>

3. **Configure models**: Open Settings → Models, enter your API key in the DeepSeek card and save. The model route is effective immediately — no server restart needed.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/03-settings-models.png" alt="Configure models" width="90%"></td></tr></table>

4. **Environment-provided key**: If your key comes from an environment variable (`DEEPSEEK_API_KEY`), the settings panel shows “Provided by the launch environment (read-only)” — recognized and ready to use.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/04-api-key-recognized.png" alt="Environment-provided key" width="90%"></td></tr></table>

#### 3.2 Workspace & Main Layout

1. **Select workspace**: Click Select Workspace to add and select a project directory. The input box is enabled only after selecting one; the agent operates on files within this workspace.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/05-select-workspace.png" alt="Select workspace" width="90%"></td></tr></table>

2. **Main layout**: session/navigation area, message area (agent replies and tool calls), and an input box at the bottom. Type a task and press Enter to begin.

<table align="center"><tr><td align="center" style="border: 3px solid #e6e9ee; border-radius: 6px; padding: 6px;"><img src="images/06-main-layout.png" alt="Main layout" width="90%"></td></tr></table>

---

### FAQ

- `git is not recognized` / `git: command not found` → Git is not installed; go back to the corresponding Preparation step.
- `node is not recognized` / `node: command not found` → Node.js is not installed or nvm is not loaded.
- `pnpm: command not found` → Reinstall pnpm: `npm install -g pnpm` on Windows; `brew install pnpm` or `npm install -g pnpm` on macOS.
- Out of memory during build → make sure you ran `$env:NODE_OPTIONS="--max-old-space-size=4096"` (Windows) or `export NODE_OPTIONS="--max-old-space-size=4096"` (macOS).
- API Key not recognized → check `echo $env:DEEPSEEK_API_KEY` on Windows or `echo $DEEPSEEK_API_KEY` on macOS; if empty, restart the terminal and retry, or enter the key directly in Settings → Models.
- The page at `127.0.0.1:3080` doesn't respond → make sure the window running `pnpm dsh web` is still open, and check that the startup output shows `dsh web: http://127.0.0.1:3080`.
- Old sessions/settings are missing after moving Home → close and reopen the terminal and run `echo $env:DSH_HOME` (macOS: `echo $DSH_HOME`) to confirm the new path; make sure `DSH_HOME` was persisted via `setx` / `export` in `~/.zshrc`, then re-run `pnpm dsh web`.

---

### 📎 Links

- [DeepSeek Harness official repository](https://github.com/deepseek-ai/deepseek-harness)
- License: [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)
