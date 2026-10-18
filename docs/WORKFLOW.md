# 开发工作流：Git Worktree + Merge Request

## 目录布局

```
~/Documents/dev/
├── wubi-practice/                 # 主 worktree（main 分支，只做主干/发布）
└── wubi-worktrees/
    ├── feat-xxx/                  # 每个功能一个 worktree
    ├── fix-xxx/                   # 每个修复一个 worktree
    └── ...
```

主仓库目录**永远保持在 `main` 分支且干净**，所有开发在并行的 worktree 中进行。

## 新开一个任务

```bash
cd ~/Documents/dev/wubi-practice

# 1. 同步主干
git checkout main && git pull

# 2. 新建分支 + worktree（一条命令完成）
git worktree add ../wubi-worktrees/feat-<名称> -b feat/<名称>

# 3. 进入 worktree 开发
cd ../wubi-worktrees/feat-<名称>
```

命名约定：
- 功能：`feat/<简短英文名>`
- 修复：`fix/<简短英文名>`
- 文档：`docs/<简短英文名>`
- 重构：`refactor/<简短英文名>`

## 提交

```bash
git add -A
git commit -m "feat: 简短中文说明"
```

提交信息前缀：`feat:` / `fix:` / `docs:` / `refactor:` / `test:` / `chore:`

## 创建 MR（Merge Request）

```bash
git push -u origin feat/<名称>
gh pr create --base main --head feat/<名称> \
  --title "feat: 标题" \
  --body "## 变更\n- ...\n\n## 验证\n- ..."
```

## 合并后清理

```bash
cd ~/Documents/dev/wubi-practice

# 删除本地分支与 worktree
git worktree remove ../wubi-worktrees/feat-<名称>
git branch -d feat/<名称>
git pull
```

## 查看所有 worktree

```bash
git worktree list
```

## 约定

1. **不在 main 上直接提交**（除极小的文档修正）
2. 一个 worktree = 一个分支 = 一个 MR
3. 每个 MR 必须说明「变更」与「验证」
4. 合并后立即清理 worktree，避免目录堆积
5. 只有一个 `index.html`，改动前先自测：在浏览器打开确认无控制台报错
