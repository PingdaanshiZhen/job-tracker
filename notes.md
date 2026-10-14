# 开发笔记


### 环境准备踩的坑

**1. PowerShell 禁止运行 npm 脚本**
- 现象：在 PowerShell 里运行 `npm` 报错，提示"在此系统上禁止运行脚本"
- 原因：Windows 默认的执行策略不允许运行 `.ps1` 脚本，而 npm 在 PowerShell 里是通过 `npm.ps1` 启动的
- 解决：把当前用户的执行策略改为 `RemoteSigned`（本地脚本可以运行，网上下载的脚本需要签名）
  ```
  Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
  ```

**2. 装好 Claude Code 后找不到 `claude` 命令**
- 安装方式：用官方安装脚本安装（不是 npm 全局安装）
- 原因：`claude.exe` 装在用户目录下的 `.local\bin` 里（`xxx\.local\bin`），这个目录不在 PATH 中，终端就找不到 `claude` 命令
- 解决：
  1. 先用完整路径运行 `claude.exe`，确认程序本身是好的，问题只出在 PATH
  2. 手动把 `xxx\.local\bin` 加入**用户** PATH
  3. **重新打开终端**才生效：已经打开的终端还在用旧的 PATH
- 体会：遇到"找不到命令"，先用完整路径试一下，就能分清是程序没装好还是 PATH 没配好

### 方案评审：我对 AI 初版方案做的调整

AI 先给出了目录结构、数据模型和 5 个里程碑。我评审后做了这些调整：

- **拆小 M1**：先做 M0，只让 Vite + React + Tailwind 跑起来，页面显示一行字，并部署到 Vercel；路由、导航、假数据表格和 `vercel.json` 放到 M1。
  - 理由：每一步越小，出问题时越容易定位
- **拆小 M2**：拆成 M2a（新增 + 显示 + 刷新不丢）和 M2b（编辑 + 删除）
- **砍掉暂时不需要的功能**，放到 M5 之后作为可选加分项：
  - `statusHistory`（状态变更历史）
  - JSON 导出/导入
  - date-fns（日期逻辑先手写）
- 其余问题按 AI 的推荐：JavaScript、不做拖拽、状态可回退、React Router、基础手机适配

体会：AI 的方案很完整，但不一定适合我现在的水平。**先把范围砍小、步子拆细**，比一次做很多更容易真正学会。


### 其他记录

- 初始化后本地分支是 `master`，推送前改名为 `main`，和 GitHub 默认分支一致
- 推送前先用 `git ls-remote origin` 确认远程仓库是空的，避免和 GitHub 上自动生成的 README 冲突
- `LF will be replaced by CRLF` 警告：Windows 和 Linux 的换行符不同，git 会自动转换，可以忽略
- npm 每次都提示 `Unknown user config`：用户目录下的 `.npmrc` 里有两行格式不对，暂时不影响使用，之后再修
- Vite 新版模板用的代码检查工具是 `oxlint`，不是 ESLint，运行 `npm run lint`

### 结果

- M0 完成：本地能运行，`npm run build` 通过，已推送到 GitHub，并部署到 Vercel
- 下一步：M1（路由 + 导航 + 假数据表格 + `vercel.json`）
