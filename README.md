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

**更新子模块**

```powershell
git submodule update --init --recursive
git submodule update --remote skills/frontend-code-standards skills/vibe-coding-new-project
```

## 新增 Skill

1. 在 `skills/<skill-name>/` 新建目录，添加 `SKILL.md`（含 YAML frontmatter：`name`、`description`）。
2. 在本 `README.md` 的「Skill 清单」中补充一行。
3. 若需独立仓库维护，可改为 Git 子模块并更新 `.gitmodules`。

## 相关链接

- 前端规范细则：[beyondOurself/rules — web.md](https://github.com/beyondOurself/rules/blob/main/web.md)
- UI 设计规范格式：[getdesign.md — What is DESIGN.md?](https://getdesign.md/what-is-design-md)
- DESIGN.md 范本参考：`ui.vintage.loongzero.com/themes/market-nest/DESIGN.md`
