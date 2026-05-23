# skills

团队共用的 **Cursor Agent Skills** 仓库。每个 Skill 为独立目录，入口文件为 `SKILL.md`。

## 目录结构

```
skills/
├── README.md
├── .gitmodules
└── skills/
    ├── frontend-code-standards/     # Git 子模块
    └── vibe-coding-new-project/     # Git 子模块
```

## Skill 清单

| Skill | 路径 | 说明 | 何时使用 |
|-------|------|------|----------|
| **frontend-code-standards** | [`skills/frontend-code-standards`](skills/frontend-code-standards) | Vue 3 / ES6+ / CSS（Tailwind·UnoCSS 原子类或 BEM）前端代码生成规范 | 编写或生成前端代码、Vue 组件、样式；要求遵循团队前端规范时 |
| **vibe-coding-new-project** | [`skills/vibe-coding-new-project`](skills/vibe-coding-new-project) | Vibe Coding 新项目：默认落盘 RESEARCH / PRD / TECH_DESIGN / AGENTS / README / TODO / `.cursorignore` / **DESIGN.md**（UI 设计规范，范本见 market-nest，标准见 [getdesign.md](https://getdesign.md/what-is-design-md)） | 从 0 到 1、新项目初始化、写 PRD / AGENTS / TODO、按五步工作流推进时（用户明确不要文档时可跳过） |

子模块仓库：

- [beyondOurself/frontend-code-standards](https://github.com/beyondOurself/frontend-code-standards)
- [beyondOurself/vibe-coding-new-project](https://github.com/beyondOurself/vibe-coding-new-project)

## 安装到 Cursor

**方式一：用户级（全局）**

```powershell
git clone --recurse-submodules git@github.com:beyondOurself/skills.git
# 将 skills/skills/* 下的各目录复制或链接到：
#   %USERPROFILE%\.cursor\skills\
```

**方式二：项目级**

将需要的 Skill 目录放到项目 `.cursor/skills/` 下。

克隆后若子目录为空，见下方 [首次拉取子模块](#首次拉取子模块)。

---

## Git 子模块操作

主仓库（本仓库）只记录**子模块指向的 commit**，子模块源码在各自独立仓库维护。  
下文路径均相对于主仓库根目录 `skills/`（即 clone 下来的目录）。

| 子模块 | 路径 | 远程仓库 |
|--------|------|----------|
| frontend-code-standards | `skills/frontend-code-standards` | `git@github.com:beyondOurself/frontend-code-standards.git` |
| vibe-coding-new-project | `skills/vibe-coding-new-project` | `https://github.com/beyondOurself/vibe-coding-new-project.git` |

### 概念速览

- **子仓库**：`skills/frontend-code-standards` 等目录，各自 `git push` 到对应 GitHub 仓库。
- **主仓库**：`beyondOurself/skills`，通过一条「子模块指针」记录当前应检出哪个 commit。
- **你在子仓库改完并 push 后**，主仓库还要再提交一次「指针更新」，别人 `clone` 主仓库时才会拉到新 commit。

---

### 首次拉取子模块

已 clone 主仓库但子目录为空时：

```powershell
cd H:\beyondOurself\skills   # 换成你的主仓库路径

git submodule update --init --recursive
```

全新克隆（推荐）：

```powershell
git clone --recurse-submodules git@github.com:beyondOurself/skills.git
cd skills
```

---

### 场景 A：在子仓库里改了代码，同步到主仓库（最常用）

**适用**：你修改了 `SKILL.md` 等，希望团队通过主仓库 `skills` 拉到最新子模块版本。

#### 第一步：在子仓库提交并推送

以 `frontend-code-standards` 为例（`vibe-coding-new-project` 把路径换成对应目录即可）：

```powershell
cd H:\beyondOurself\skills\skills\frontend-code-standards

# 1. 确认当前在分支上（子模块默认可能是 detached HEAD，先切分支）
git status
git checkout master          # 或 main，以子仓库默认分支为准
# 若提示没有 master：git branch -a 查看远程分支名

# 2. 修改文件后提交
git add .
git commit -m "docs(skill): 更新前端规范说明"

# 3. 推送到子仓库远端
git push origin master       # 分支名与上一步一致
```

`vibe-coding-new-project` 子仓库默认分支多为 `master`，命令相同，仅路径不同：

```powershell
cd H:\beyondOurself\skills\skills\vibe-coding-new-project
git checkout master
git add .
git commit -m "docs(skill): 更新 Vibe Coding 模版"
git push origin master
```

#### 第二步：回到主仓库，更新子模块指针并推送

子仓库 push 成功后，**主仓库还不知道**新 commit，需要显式记录：

```powershell
cd H:\beyondOurself\skills

# 1. 进入子模块目录，拉到刚 push 的 commit（若已在子目录 push 过可跳过 pull）
cd skills\frontend-code-standards
git pull origin master
cd ..\..

# 2. 在主仓库中「登记」子模块新指针
git add skills/frontend-code-standards

# 若同时更新了多个子模块，可一起 add：
# git add skills/frontend-code-standards skills/vibe-coding-new-project

# 3. 查看：应看到子模块路径显示为新 commit，而不是 SKILL.md 内容 diff
git status
git diff --cached

# 4. 提交主仓库
git commit -m "chore(submodule): 同步 frontend-code-standards 至最新"

# 5. 推送主仓库
git push origin main
```

**验收**：

- 子仓库 GitHub 上能看到新 commit。
- 主仓库 GitHub 最新 commit 里，`skills/frontend-code-standards` 显示为新的短 hash（如 `1b92cfe → xxxxxxx`）。

---

### 场景 B：别人已更新主仓库，我本地要拉最新子模块

主仓库有人完成了「场景 A」第二步后，你本地需要：

```powershell
cd H:\beyondOurself\skills

# 1. 拉主仓库
git pull origin main

# 2. 按主仓库记录的指针，检出各子模块对应 commit
git submodule update --init --recursive
```

若 `git pull` 后子模块仍不对，可强制对齐主仓库指针：

```powershell
git submodule update --init --recursive --force
```

---

### 场景 C：不进入子目录，在主仓库一键把子模块拉到各自远端最新

**注意**：会把子模块移到各自远程分支最新 commit，可能与主仓库当前指针不一致；更新后仍需按 **场景 A 第二步** 提交主仓库。

```powershell
cd H:\beyondOurself\skills

git submodule update --init --recursive
git submodule update --remote skills/frontend-code-standards
git submodule update --remote skills/vibe-coding-new-project

git status
git add skills/frontend-code-standards skills/vibe-coding-new-project
git commit -m "chore(submodule): 同步子模块至远端最新"
git push origin main
```

---

### 场景 D：新增一个子模块（维护者）

```powershell
cd H:\beyondOurself\skills

# 1. 若原目录已被主仓库跟踪，先移除（保留文件需先备份）
git rm -r skills/<skill-name>

# 2. 添加子模块
git submodule add git@github.com:beyondOurself/<repo>.git skills/<skill-name>

# 3. 提交 .gitmodules 与子模块指针
git add .gitmodules skills/<skill-name>
git commit -m "chore(submodule): 添加 <skill-name> 子模块"
git push origin main
```

并在本 `README.md` 的「Skill 清单」与上表补充一行。

---

### 常见问题

| 现象 | 处理 |
|------|------|
| 子目录为空 | `git submodule update --init --recursive` |
| 子模块处于 `detached HEAD` | `cd` 进子目录 → `git checkout master`（或 `main`）再改代码 |
| 主仓库 `git status` 显示 `modified: skills/xxx (new commits)` | 子仓库已更新但未登记指针 → 按 **场景 A 第二步** `git add` 并提交主仓库 |
| `git pull` 后主仓库冲突在子模块指针 | 解决冲突后执行 `git submodule update --init --recursive` |
| 想放弃子模块本地改动，对齐主仓库指针 | `git submodule update --init --recursive --force` |

### 推荐提交信息

| 操作 | 示例 |
|------|------|
| 子仓库内改 skill 内容 | `docs(skill): 更新 xxx 规范` |
| 主仓库只改子模块指针 | `chore(submodule): 同步 frontend-code-standards 至最新` |

---

## 新增 Skill

1. 在独立仓库创建 Skill 并 push（或先在主仓库目录开发再拆仓）。
2. 按上文 [场景 D：新增一个子模块](#场景-d新增一个子模块维护者) 接入主仓库。
3. 在本 `README.md` 的「Skill 清单」与子模块表中补充一行。

## 相关链接

- 前端规范细则：[beyondOurself/rules — web.md](https://github.com/beyondOurself/rules/blob/main/web.md)
- UI 设计规范格式：[getdesign.md — What is DESIGN.md?](https://getdesign.md/what-is-design-md)
- DESIGN.md 范本参考：`ui.vintage.loongzero.com/themes/market-nest/DESIGN.md`
