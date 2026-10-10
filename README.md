# mySkills

个人 AI Agent Skills 聚合仓库。

本仓库本身**不存放** skill 源码，而是通过 **Git Submodule** 把上游若干优秀的 skills 仓库聚合到一起，方便统一克隆、统一更新，再通过**目录链接（Junction / Symbolic Link）**把各个 skill 挂载到用户自己的 SKILL 空间里使用。

- 主仓库：<https://github.com/3417993939/mySkills.git>
- 许可证：MIT（见 [LICENSE](./LICENSE)）
- 暂不引入的聚合仓库见 [一些不适合拉取项目的github项目.md](./一些不适合拉取项目的github项目.md)

## 子模块清单

| 子模块目录 | 上游仓库 | 分支 | 说明 |
| --- | --- | --- | --- |
| `anthropics_skills` | <https://github.com/anthropics/skills.git> | main | Anthropic 官方 skills（docx / pptx / pdf / xlsx / skill-creator 等） |
| `mattpocock_skills` | <https://github.com/mattpocock/skills.git> | main | Matt Pocock 的工程与写作类 skills |
| `vercel_labs_agent_skills` | <https://github.com/vercel-labs/agent-skills.git> | main | Vercel Labs 前端 / 部署类 skills |
| `agents365_ai_drawio_skill` | <https://github.com/Agents365-ai/drawio-skill.git> | main | Draw.io 图表绘制 skill |
| `cathrynlavery_diagram_design` | <https://github.com/cathrynlavery/diagram-design.git> | main | 架构图 / 流程图等专业图形设计 skill |
| `gpui-kit` | <https://github.com/longbridge/gpui-kit.git> | main | GPUI Kit 桌面应用开发及设计规范 skill |

> 各子模块的 skill 统一放在 `<子模块>/skills/` 目录下，部分上游会有二级分类目录（如 `mattpocock_skills/skills/engineering/<skill>`），**每个包含 `SKILL.md` 的目录就是一个 skill**。

## 目录结构

```text
mySkills/
├── .gitmodules                       # 子模块清单（路径 / 地址 / 分支）
├── README.md
├── LICENSE
├── 一些不适合拉取项目的github项目.md
├── anthropics_skills/                # 子模块
│   └── skills/
│       ├── docx/SKILL.md
│       └── ...
├── mattpocock_skills/                # 子模块
│   └── skills/
│       ├── engineering/<skill>/SKILL.md
│       ├── misc/<skill>/SKILL.md
│       └── productivity/<skill>/SKILL.md
├── vercel_labs_agent_skills/         # 子模块
│   └── skills/<skill>/SKILL.md
├── agents365_ai_drawio_skill/        # 子模块
│   └── skills/drawio-skill/SKILL.md
├── cathrynlavery_diagram_design/     # 子模块
│   └── skills/diagram-design/SKILL.md
└── gpui-kit/                         # 子模块
    └── skills/
        ├── gpui-kit/SKILL.md
        └── gpui-kit-design-guides/SKILL.md
```

---

## 约定

- 子模块**只读同步**：本仓库只负责克隆、拉取与更新子模块，**不在本地修改任何子模块内容**；需要改动请直接向上游仓库提交 PR。
- 上游 skill 内容可信度由上游自身 / 外部流程保证，本文档不重复做内容审查。
- 基于以上约定，子模块工作区始终保持干净，更新时可直接追上游最新版并使用 `--merge`（见 3.2），不会因本地改动产生冲突。

---

## 一、环境准备

- 安装 **Git**（`git --version` 可正常输出）。
- **Windows 额外注意**：创建目录链接需要权限，二选一即可
  - 开启「设置 → 系统 → 开发者模式」，之后可在普通权限下创建链接；
  - 或以**管理员身份**打开终端。
- 建议把本仓库克隆到一个**固定的绝对路径**，因为链接目标会写死这个路径，中途移动目录会导致链接失效。本文档统一以
  `D:\projects\skillpro\mySkills` 为例。

## 二、克隆项目

### 2.1 推荐：克隆时连同子模块一起拉取

```bash
git clone --recurse-submodules https://github.com/3417993939/mySkills.git
cd mySkills
```

### 2.2 已克隆但子模块是空的

如果之前用了普通 `git clone`（子模块目录为空），在仓库根目录执行：

```bash
# 初始化并拉取全部子模块（含嵌套子模块）
git submodule update --init --recursive
```

只想初始化某一个子模块：

```bash
git submodule update --init anthropics_skills
```

## 三、更新项目与子模块

### 3.1 更新主仓库

```bash
cd D:\projects\skillpro\mySkills
git pull
```

`git pull` 之后，如果 `.gitmodules` 或子模块记录的提交有变化，需要再同步子模块（见 3.2）。

### 3.2 更新所有子模块到各自远程最新版

```bash
# 按 .gitmodules 中配置的 branch（main）拉取上游最新提交
git submodule update --remote --merge --recursive
```

- `--remote`：改用**上游远程最新提交**，而不是主仓库里记录的那个固定提交；
- `--merge`：把上游更新合并进本地子模块分支，避免游离 HEAD；
- `--recursive`：递归处理嵌套子模块。

### 3.3 只更新某个子模块

```bash
git submodule update --remote --merge mattpocock_skills
# 或者
git submodule update --remote --merge vercel_labs_agent_skills
```

### 3.4 更新后提交子模块指针

子模块更新后，主仓库里记录的是「子模块当前指向的提交」，需要提交这个指针变更：

```bash
git status                                  # 会看到 modified: <子模块目录>
git add .gitmodules                          # 暂存子模块配置
git add anthropics_skills mattpocock_skills vercel_labs_agent_skills agents365_ai_drawio_skill cathrynlavery_diagram_design gpui-kit
git commit -m "chore: 更新子模块"
git push
```

> 只 `add .gitmodules` 和实际变化的子模块目录即可；不要用 `git add ./*` 之类把根目录其它临时文件一并暂存——本主仓库是公开仓库，避免误提交无关内容。

### 3.5 新增一个子模块

```bash
git submodule add -b main https://github.com/<owner>/<repo>.git <本地目录名>
git commit -m "chore: 新增子模块 <本地目录名>"
```

### 3.6 常用排查命令

```bash
git submodule status            # 查看各子模块当前提交；前缀 - 表示未初始化，+ 表示与记录不一致
git submodule foreach --recursive git status   # 逐个查看子模块工作区状态
cat .gitmodules                 # 查看子模块配置
```

## 四、把 SKILL 链接到用户 SKILL 空间

原理：**不复制文件**，而是给每个 skill 目录在用户 SKILL 空间里建一个链接（Windows 用 Junction，macOS / Linux 用软链接）。这样上游更新后，用户空间的 skill 自动跟着更新。

常见 SKILL 空间位置：

| 工具 | 目录（Windows） | 目录（macOS / Linux） |
| --- | --- | --- |
| CodeBuddy / CodeBuddy CN | `%USERPROFILE%\.codebuddy\skills` | `~/.codebuddy/skills` |
| Claude Code | `%USERPROFILE%\.claude\skills` | `~/.claude/skills` |
| 通用 Agent 目录 | `%USERPROFILE%\.agents\skills` | `~/.agents/skills` |

### 4.1 手动创建单个链接

#### Windows（PowerShell，Junction）

```powershell
New-Item -ItemType Junction `
  -Path "$env:USERPROFILE\.codebuddy\skills\docx" `
  -Target "D:\projects\skillpro\mySkills\anthropics_skills\skills\docx"
```

#### Windows（CMD，Junction）

```cmd
mklink /J "%USERPROFILE%\.codebuddy\skills\docx" "D:\projects\skillpro\mySkills\anthropics_skills\skills\docx"
```

#### macOS / Linux

```bash
ln -s /path/to/mySkills/anthropics_skills/skills/docx ~/.codebuddy/skills/docx
```

### 4.2 批量创建（推荐）

自动扫描所有子模块 `skills/` 目录下含 `SKILL.md` 的目录，并逐个建成链接。

#### Windows PowerShell

```powershell
$repo   = "D:\projects\skillpro\mySkills"
$target = Join-Path $env:USERPROFILE ".codebuddy\skills"

$submodules = @(
  "agents365_ai_drawio_skill", "anthropics_skills", "cathrynlavery_diagram_design",
  "gpui-kit", "mattpocock_skills", "vercel_labs_agent_skills"
)

New-Item -ItemType Directory -Force -Path $target | Out-Null

$skillRoots = $submodules | ForEach-Object { Join-Path $repo "$_\skills" }
$skills = Get-ChildItem -Path $skillRoots -Directory -Recurse -Depth 2 |
  Where-Object { Test-Path (Join-Path $_.FullName "SKILL.md") }

foreach ($s in $skills) {
  $link = Join-Path $target $s.Name
  if (Test-Path $link) { Write-Host "跳过（已存在）: $($s.Name)"; continue }
  New-Item -ItemType Junction -Path $link -Target $s.FullName | Out-Null
  Write-Host "已链接: $($s.Name)  ->  $($s.FullName)"
}
```

#### macOS / Linux Bash

```bash
REPO="/path/to/mySkills"
TARGET="$HOME/.codebuddy/skills"
mkdir -p "$TARGET"

for root in "$REPO"/*/skills; do
  [ -d "$root" ] || continue
  while IFS= read -r skill; do
    name="$(basename "$skill")"
    if [ -e "$TARGET/$name" ]; then
      echo "跳过（已存在）: $name"
    else
      ln -s "$skill" "$TARGET/$name"
      echo "已链接: $name -> $skill"
    fi
  done < <(find "$root" -maxdepth 2 -type d -exec sh -c 'test -f "$1/SKILL.md"' _ {} \; -print)
done
```

### 4.3 清理 / 重建链接

> **删除链接的安全规则（务必遵守）**：只能使用下方命令删除链接本身，它们都不带递归。**任何时候都不要**使用 `Remove-Item -Recurse`、`rd /s`、`rm -rf`，也**不要**直接在文件资源管理器里删除链接目录——Windows 会把 Junction / 符号链接当作真实目录处理，递归删除会**穿透删除源目录里的真实文件**，造成不可逆数据损失。

```powershell
# Windows：仅删除链接本身，不影响源目录（不要加 -Recurse）
Get-ChildItem "$env:USERPROFILE\.codebuddy\skills" |
  Where-Object { $_.LinkType -eq "Junction" -and $_.Target -like "D:\projects\skillpro\mySkills\*" } |
  Remove-Item
```

```bash
# macOS / Linux：只删软链接，不删源目录
find "$HOME/.codebuddy/skills" -maxdepth 1 -type l -lname "/path/to/mySkills/*" -delete
```

---

## 五、发送给 AI 的提示语（可直接复制）

把下面这段整体发给 AI，让它自动为本项目的所有 skill 在用户 SKILL 空间建立链接目录：

```text
请帮我完成一个「批量建立 SKILL 链接目录」的任务。

【背景】
项目路径：mySkills项目的根目录
这是一个聚合仓库，用 git submodule 引入了以下 6 个子模块：
agents365_ai_drawio_skill、anthropics_skills、cathrynlavery_diagram_design、
gpui-kit、mattpocock_skills、vercel_labs_agent_skills
每个子模块的 skill 都放在其 skills/ 目录下，部分上游存在二级分类目录
（例如 mattpocock_skills/skills/engineering/<skill>）。
判断标准：任何包含 SKILL.md 文件的目录，就是一个独立的 skill，目录名即 skill 名。

【目标】
把上述每个 skill 目录，以「目录链接」的方式挂载到用户 SKILL 空间(以codebuddy举例)：
- 目标目录：%USERPROFILE%\.codebuddy\skills
  （即 Windows 下的 C:\Users\<当前用户>\.codebuddy\skills）
- 采用链接而非复制，保证上游子模块更新后 skill 自动同步。

【要求】
1. 先用命令确认当前操作系统（win32 / darwin / linux）。
2. 扫描 6 个子模块的 skills/ 目录（递归，最多 2 层），找出所有含 SKILL.md 的目录，
   先输出一份清单（skill 名 -> 源路径），让我确认数量。
3. 创建链接：
   - Windows：使用目录联接（Junction）：
     New-Item -ItemType Junction -Path "<目标>\<skill名>" -Target "<源路径>"
     若报权限错误，提示我开启开发者模式或以管理员身份重跑。
   - macOS / Linux：使用软链接：ln -s <源路径> "<目标>/<skill名>"
4. 幂等处理：目标目录里已存在的同名项一律跳过并输出「跳过（已存在）」，
   不要覆盖、不要删除任何已存在的目录或链接。
5. 若目标目录不存在则先创建。
6. 全部完成后，输出一张汇总表：skill 名 / 来源子模块 / 链接状态（新建 / 已存在跳过）。
7. 全程只做链接，不修改本项目内任何文件，也不要执行 git 提交。

请先执行第 1、2 步并把清单发我确认，确认后再执行第 3 步。
```
