# 水界漫游 · 观赏鱼图鉴

基于 **UniApp + Vue 3 + Vite** 的跨平台观赏鱼图鉴活动页应用。

## 功能特性

- 🏔️ **山水墨韵主题** — 中国原生观赏鱼图鉴（12种鱼类，搜索筛选，详情弹窗）
- 🐟 **原生鱼图鉴** — 精选8种中国原生观赏鱼卡片展示 + 科普小课堂
- 🕊️ **超级海鸥主题** — 夏日海水鱼图鉴（12种鱼类，搜索筛选，详情弹窗）

## 技术栈

- **UniApp** — 跨平台框架（H5 / 微信小程序 / App）
- **Vue 3** — Composition API + `<script setup>`
- **Vite** — 构建工具
- **SCSS** — 样式预处理

## 项目结构

```
src/
├── App.vue                          # 根组件
├── main.js                          # 入口文件
├── manifest.json                    # UniApp 应用配置
├── pages.json                       # 页面路由配置
├── uni.scss                         # 全局 SCSS 变量
├── components/                      # 公共组件
│   ├── FishCard.vue                 # 鱼类卡片组件
│   ├── FishModal.vue                # 鱼类详情弹窗组件
│   └── SearchFilter.vue             # 搜索筛选组件
├── data/                            # 数据文件
│   ├── nativeFishData.js            # 原生鱼数据
│   └── seaFishData.js               # 海水鱼数据
├── pages/                           # 页面
│   ├── index/index.vue              # 首页导航
│   ├── activity-native-fish/index.vue   # 山水墨韵主题页
│   ├── activity-native-fish-cn/index.vue # 原生鱼图鉴页
│   └── activity-sea/index.vue       # 超级海鸥主题页
└── static/                          # 静态资源
```

## 快速开始

```bash
# 安装依赖
npm install

# H5 开发模式
npm run dev:h5

# H5 构建
npm run build:h5

# 微信小程序开发模式
npm run dev:mp-weixin

# 微信小程序构建
npm run build:mp-weixin
```

## 许可证

ISC
