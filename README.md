[English](./README_EN.md) | **中文**

# 海中信 (Letter in the Sea)

一个温暖的数字漂流瓶应用，让情感随波逐流。

## 🌊 项目介绍

「海中信」是一个基于 Svelte + TypeScript + Vite 构建的 Web 应用。用户可以将自己的心事、思绪或想要放下的过往写成信件投入数字海洋，也可以随机拾取其他人留下的信件，感受陌生人的故事与情感。

### 核心功能

- **投信入海** - 写下你的心事，让它漂流在数字海洋中
- **拾取回音** - 随机读取其他人的信件，感受不同的人生故事
- **本地存储** - 所有信件都保存在浏览器本地，保护隐私
- **时间记录** - 自动记录每封信的投递时间
- **优雅动画** - 流畅的页面切换和投递动画效果

## 🚀 快速开始

### 安装依赖

```bash
# 使用 pnpm（推荐）
pnpm install

# 或使用 npm
npm install
```

### 开发环境

```bash
# 启动开发服务器
pnpm dev

# 或
npm run dev
```

### 构建项目

```bash
# 构建生产版本
pnpm build

# 或
npm run build
```

### 预览构建结果

```bash
# 预览生产构建
pnpm preview

# 或
npm run preview
```

## 📁 项目结构

```
letter-in-the-sea/
├── src/
│   ├── lib/
│   │   ├── WriteLetter.svelte    # 写信组件
│   │   ├── PickupBottle.svelte   # 拾信组件
│   │   └── Counter.svelte        # 计数器组件（示例）
│   ├── types/
│   │   └── letter.ts             # TypeScript 类型定义
│   ├── assets/                   # 静态资源
│   ├── App.svelte               # 主应用组件
│   ├── main.ts                  # 应用入口
│   └── app.css                  # 全局样式
├── public/                      # 公共资源
├── index.html                   # HTML 入口
└── package.json                 # 项目配置
```

## 🎨 技术特性

- **Svelte 5** - 使用最新版本的 Svelte 框架
- **TypeScript** - 完整的类型支持
- **Vite** - 快速的开发构建工具
- **响应式设计** - 适配移动端和桌面端
- **动画效果** - 使用 Svelte 内置过渡动画
- **本地存储** - 使用 localStorage 保存数据

## 💡 使用场景

- 记录无法言说的心事
- 分享深夜的思绪
- 放下想要忘记的过往
- 感受陌生人的温暖
- 寻找情感的共鸣

## 🛠️ 开发说明

### 推荐的 IDE 配置

- [VS Code](https://code.visualstudio.com/) + [Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)

### 类型检查

```bash
# 运行 Svelte 和 TypeScript 类型检查
pnpm check

# 或
npm run check
```

## 📝 许可证

本项目仅供学习和个人使用。

---

> 写了信又丢进海里，让情感随波逐流。
