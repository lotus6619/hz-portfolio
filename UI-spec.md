# UI-spec — 赫兹个人介绍网站 设计规范

> 读者：AI 编程工具（Claude Code / Cursor / Trae）
> 版本：v2.0
> 风格方向：暗黑玻璃态 + 不对称 Bento + 翡翠绿 accent

---

## 1. 配色系统

### 暗色主题（默认）

| Token | 值 | 用途 |
|---|---|---|
| `--bg` | `#08080a` | 页面底色，接近纯黑 |
| `--bg-card` | `#12121a` | 卡片/容器底色 |
| `--bg-card-hover` | `#1a1a24` | 卡片 hover 态 |
| `--bg-nav` | `rgba(8,8,10,0.78)` | 导航栏玻璃底色 |
| `--text` | `#e2e8f0` | 正文 |
| `--text-dim` | `#94a3b8` | 次要文字/辅助信息 |
| `--heading` | `#f1f5f9` | 标题 |
| `--accent` | `#10b981` | 翡翠绿，全站唯一强调色 |
| `--accent-glow` | `rgba(16,185,129,0.12)` | 翡翠微光（hover/光斑用） |
| `--border` | `rgba(255,255,255,0.06)` | 默认边框，极淡 |
| `--border-hover` | `rgba(16,185,129,0.28)` | hover 时边框带翡翠色 |
| `--radius` | `2rem` | 全局圆角（squircle 风格） |
| `--radius-sm` | `calc(2rem - 0.375rem)` | 内层圆角（Doppelrand 内芯用） |

### 亮色主题

| Token | 值 |
|---|---|
| `--bg` | `#f8f9fb` |
| `--bg-card` | `#ffffff` |
| `--bg-card-hover` | `#f1f5f9` |
| `--bg-nav` | `rgba(248,249,251,0.78)` |
| `--text` | `#1e293b` |
| `--text-dim` | `#94a3b8` |
| `--heading` | `#0f172a` |
| `--accent` | `#059669` |
| `--accent-glow` | `rgba(5,150,105,0.08)` |
| `--border` | `rgba(0,0,0,0.06)` |
| `--border-hover` | `rgba(5,150,105,0.22)` |

---

## 2. 字体

- **标题字体**：`Cabinet Grotesk`，weight 800
- **正文字体**：`Cabinet Grotesk`，weight 400/500
- **等宽字体**：`JetBrains Mono`（备用，仅代码类内容）
- **加载方式**：Fontshare CDN
  ```html
  <link href="https://api.fontshare.com/v2/css?f[]=cabinet-grotesk@800,700,500,400&display=swap" rel="stylesheet">
  ```

### 字号层级

| 元素 | 字号 | letter-spacing |
|---|---|---|
| Hero 标题 | `clamp(3rem, 7vw, 5.5rem)` | `-0.03em` |
| 板块标题 | `clamp(1.8rem, 4vw, 2.8rem)` | `-0.02em` |
| 卡片标题 | `1.25rem` | `-0.01em` |
| 正文 | `1rem` | `0` |
| 辅助文字 | `0.85rem` | `0` |
| 等宽/标签 | `0.75rem` | `0.02em` |

---

## 3. 材质与组件规则

### 3.1 Doppelrand 双边框（核心材质语言）

全站所有卡片/容器使用双层嵌套结构：

```html
<!-- 外壳 -->
<div class="doppelrand-outer">
  <!-- 内芯 -->
  <div class="doppelrand-inner">
    <!-- 实际内容 -->
  </div>
</div>
```

CSS：
- **外壳**：`border: 1px solid var(--border)` + `border-radius: var(--radius)` + `padding: 1.5px`（或 `p-[1.5px]`）
- **内芯**：`background: var(--bg-card)` + `border-radius: var(--radius-sm)` + `box-shadow: inset 0 1px 1px rgba(255,255,255,0.05)` + `padding: 2rem`

hover 时外壳边框变为 `var(--border-hover)`，内芯 inset shadow 变亮。

### 3.2 Button-in-Button（CTA 按钮结构）

```html
<button class="cta-btn">
  <span>按钮文字</span>
  <span class="cta-icon-circle">
    <!-- 图标 -->
  </span>
</button>
```

- 主按钮：`rounded-full px-6 py-3.5`
- 图标圈：独立的 `w-9 h-9 rounded-full bg-white/8`，位于按钮内部右侧
- hover 时：图标圈 `translate-x-0.5 -translate-y-0.5 scale-105`
- active 时：整体 `scale-[0.98]`

### 3.3 导航栏 Fluid Island

- 不贴顶，浮动在页面上方 16px 处
- 药丸形：`rounded-full`
- 玻璃态：`backdrop-blur-xl` + `bg: var(--bg-nav)`
- 最大宽度 640px，水平居中
- 高度 ≤ 56px
- 滚动时保持固定
- 汉堡展开：全屏 overlay，导航链接 staggered 淡入（delay 100ms/150ms/200ms）

---

## 4. 布局规则

### 4.1 区块间距
- 板块之间：`py-24` 到 `py-32`
- 板块内标题到底部内容：`mb-16`
- 普通间距：`gap-6` 到 `gap-8`

### 4.2 内容宽度
- 正文行宽：`max-w-[65ch]`
- 板块容器：`max-w-[1200px] mx-auto px-6`

### 4.3 板块结构（6个）

| # | 板块 | 布局类型 |
|---|------|----------|
| 1 | Hero | 不对称：左侧大标题 + 右侧氛围光斑 |
| 2 | 关于我 | Doppelrand 大卡片包裹叙事文字 + 标签云 |
| 3 | 项目 | Bento 网格（不等大格子） |
| 4 | 专辑画廊 | 3D CSS rotateY 圆环（保留原版结构） |
| 5 | 联系 | Doppelrand 小卡片 + 社交图标行 |
| 6 | 留言板 | Doppelrand 表单 + 消息列表 |

### 4.4 禁止项
- ❌ 3列等大卡片
- ❌ 进度条/技能条
- ❌ section 编号 eyebrow（01/02/03）
- ❌ em-dash（—）
- ❌ 滚动提示（Scroll ↓）
- ❌ 纯黑 `#000` / 纯白 `#fff`
- ❌ `h-screen`（用 `min-h-[100dvh]`）
- ❌ `window.addEventListener('scroll', ...)`

---

## 5. 动效规范

### 5.1 过渡曲线
全局使用 `cubic-bezier(0.32, 0.72, 0, 1)`，禁止 `linear` / `ease-in-out`。

### 5.2 滚动进入
- IntersectionObserver 触发
- 元素从 `opacity: 0; transform: translateY(24px); filter: blur(4px)` → `opacity: 1; transform: translateY(0); filter: blur(0)`
- 持续 700ms
- 板块间 stagger 80ms

### 5.3 卡片 Hover
- 外壳边框色变为 `var(--border-hover)`
- 内芯 inset shadow 亮度加倍
- transform: `translateY(-2px)`
- 持续 400ms

### 5.4 导航链接 Hover
- 颜色变 accent
- 下划线 2px 从中间向外展开（`scaleX` 动画）

### 5.5 reduced-motion
所有动画被 `@media (prefers-reduced-motion: reduce)` 覆盖为 `duration: 0` 或直接 instant。

---

## 6. 图标

- 库：Tabler Icons webfont
- CDN：`https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.x/tabler-icons.min.css`
- 用法：`<i class="ti ti-icon-name"></i>`
- stroke-width：默认 1.5

---

## 7. 3D 专辑画廊（保留 + 微调）

### 保留
- CSS 3D `rotateY` 圆环结构
- 拖拽旋转交互
- 封面翻转（flip）动画
- 自动旋转
- 圆点导航

### 微调
- 卡片尺寸从 90px → 100px
- 封面边框带翡翠微光 hover
- 控件按钮改为 Button-in-Button 风格
- 圆点样式更新为 accent 色

### 专辑数据
- 用户稍后提供新图片，当前保留原数据占位

---

## 8. 图片

- Hero 氛围光斑：纯 CSS 实现（radial-gradient），无需外部图片
- 专辑封面：用户提供
- 证书图片：`assets/064c1a6861a9201965ea738461f7f29d.jpg`（保留）

---

## 9. 技术选型

- 纯 HTML + CSS + 原生 JS（单文件）
- 无框架依赖
- 留言板数据：localStorage
- 主题切换：CSS 变量 + `data-theme` 属性 + localStorage
