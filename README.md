# 🚀 GitHub Pages 部署指南

---

## 📋 前置准备

### 1. 配置 Git 用户信息

```bash
# 替换为你的 GitHub 用户名和邮箱
git config --global user.name "你的 GitHub 用户名"
git config --global user.email "你的邮箱@users.noreply.github.com"
```

### 2. 生成 SSH Key（如未配置）

```bash
# 生成 SSH Key
ssh-keygen -t ed25519 -C "你的邮箱@users.noreply.github.com"

# 查看公钥
cat ~/.ssh/id_ed25519.pub

# 将输出的内容添加到 GitHub:
# Settings → SSH and GPG keys → New SSH Key
```

---

## 🎮 部署步骤

### Step 1: 创建 GitHub 仓库

1. 访问 https://github.com/new
2. 仓库名：`openclaw-projects`
3. 描述：`小红的 OpenClaw 学习项目展示`
4. 设为 **Public**（公开）
5. **不要** 勾选 "Add a README file"
6. 点击 "Create repository"

### Step 2: 初始化并推送代码

```bash
cd /root/.openclaw/workspace/projects

# 初始化 Git
git init

# 添加所有文件
git add -A

# 提交
git commit -m "Initial commit: 项目展示页 + 放置挂机游戏"

# 添加远程仓库（替换为你的用户名）
git remote add origin git@github.com:你的用户名/openclaw-projects.git

# 推送
git branch -M main
git push -u origin main
```

### Step 3: 启用 GitHub Pages

1. 进入仓库页面
2. 点击 **Settings** → **Pages**
3. Source 选择：**Deploy from a branch**
4. Branch 选择：**main** / **/(root)**
5. 点击 **Save**

### Step 4: 等待部署完成

GitHub 会在 1-2 分钟内完成部署，访问：
```
https://你的用户名.github.io/openclaw-projects/
```

---

## 🎮 访问你的游戏

部署完成后，你可以通过以下链接访问：

| 项目 | 链接 |
|------|------|
| **项目展示页** | `https://你的用户名.github.io/openclaw-projects/` |
| **放置挂机游戏** | `https://你的用户名.github.io/openclaw-projects/idle-game/` |

---

## 🔄 更新项目

每次修改后，推送更新：

```bash
cd /root/.openclaw/workspace/projects

git add -A
git commit -m "更新内容描述"
git push
```

GitHub Pages 会自动重新部署，通常 1-2 分钟后生效。

---

## 📊 项目结构

```
projects/
├── index.html              # 项目展示页（GitHub Pages 首页）
├── idle-game/
│   └── index.html          # 放置挂机游戏
├── stock-monitor/          # (未来) 股票盯盘助手
└── cli-tools/              # (未来) CLI 工具集
```

---

## 🎨 自定义域名（可选）

如果你想用自定义域名：

1. 在 GitHub Pages 设置中添加自定义域名
2. 在你的域名 DNS 服务商添加 CNAME 记录：
   ```
   CNAME 你的域名 → 你的用户名.github.io
   ```

---

## 📝 注意事项

1. **GitHub Pages 是静态托管**，只能部署 HTML/CSS/JS
2. **存储空间限制**：1GB
3. **带宽限制**：每月 100GB（对个人项目足够）
4. **构建限制**：每小时 10 次构建

---

## 🐳 服务器项目部署（待完成）

对于需要后端的项目（股票盯盘、CLI 工具），使用 Docker 部署到云服务器：

```bash
# 股票盯盘助手
cd /root/.openclaw/workspace/projects/stock-monitor
docker build -t stock-monitor .
docker run -d -p 8080:8080 stock-monitor

# 访问：http://49.232.215.84:8080
```

详细部署流程见 `DEPLOYMENT.md`

---

## ✅ 部署检查清单

- [ ] Git 用户信息已配置
- [ ] SSH Key 已生成并添加到 GitHub
- [ ] GitHub 仓库已创建
- [ ] 代码已推送
- [ ] GitHub Pages 已启用
- [ ] 可以正常访问项目展示页
- [ ] 可以正常访问放置游戏

---

*祝你部署顺利！🎉*
