# Nail Mold AI Studio（AI 美甲模具灵感库）MVP 需求文档

## 1. 项目概述

**项目名**：Nail Mold AI Studio  
**中文名**：AI 美甲模具灵感库

这是一个面向硅胶美甲模具用户的**移动端 H5 / Web App**。用户通过产品包装二维码扫码进入指定模具页面，快速获得：

- 模具画法灵感
- 配色建议
- 节日主题推荐
- AI 生成方案
- 用户作品上传与浏览

> MVP 核心目标：先验证用户是否愿意使用、分享、并带来复购，而不是一次性做完整社区。

---

## 2. 背景与问题

目标用户（美甲爱好者、美甲师、DIY 用户）在购买模具后常见痛点：

1. 不知道该模具能做哪些风格。
2. 不知道如何上色。
3. 不知道节日场景怎么搭配。
4. 不知道一个模具能延展出多少方案。
5. 做完作品后缺少展示与交流入口。

业务侧痛点：

- 无法沉淀用户作品与偏好数据。
- 无法建立“模具使用 -> 内容消费 -> 分享 -> 复购”的闭环。

---

## 3. MVP 范围（第一版）

### 3.1 必做功能（6 项）

1. 二维码扫码进入对应模具页面。
2. 模具风格画法库展示。
3. AI 生成配色/画法/搭配建议。
4. 节日主题推荐。
5. 用户上传作品与经验。
6. 后台管理（模具、风格、节日、作品审核、基础数据）。

### 3.2 暂不做

- 复杂社交（私信、群聊）
- 直播
- 付费会员
- 完整商城
- 复杂 AI 图像生成
- 多商家入驻

---

## 4. 用户角色与权限

### 4.1 游客（未登录）

可用：

- 扫码访问模具页
- 浏览画法与节日推荐
- 使用有限次数 AI 生成
- 浏览公开作品

不可用：

- 上传作品
- 点赞/收藏
- 后台管理

### 4.2 登录用户

可用：

- 上传作品
- 点赞/收藏
- 保存 AI 方案
- 管理自己的作品

### 4.3 管理员

可用：

- 模具/风格/节日内容管理
- 用户作品审核（通过/拒绝/下架）
- 推荐内容配置
- AI Prompt 管理
- 数据统计查看

---

## 5. 核心流程

### 5.1 扫码进入

二维码建议短链接：

- `https://yourdomain.com/m/HG-001`

落地重定向至：

- `/mold/HG-001`

扫码后页面需在首屏快速展示：

- 模具名与编号
- 模具主图
- 热门风格
- AI 生成入口
- 用户作品预览

二维码旁 CTA 文案建议：

- **Scan for AI Nail Art Ideas**
- **Scan to Unlock 30+ Nail Art Looks**

### 5.2 模具详情页流程

页面结构：

1. 顶部（主图、模具名、编号）
2. 模具介绍
3. 热门风格方案
4. AI 生成入口
5. 节日推荐
6. 用户作品墙
7. 相关模具推荐

### 5.3 AI 生成流程

用户输入：

- 风格（Cute / Gothic / Coquette / Y2K / Mermaid / Christmas...）
- 节日（可选）
- 颜色偏好（可选）
- 难度（Easy / Medium / Advanced）
- 甲型（Short / Medium / Long）
- 补充需求（文本）

系统输出（结构化 JSON）：

```json
{
  "title": "",
  "style": "",
  "color_palette": [],
  "base_nail": "",
  "mold_coloring_method": "",
  "steps": [],
  "matching_tips": [],
  "suitable_occasion": "",
  "recommended_related_molds": []
}
```

---

## 6. 页面与路由（MVP）

- `/` 首页
- `/mold/[moldId]` 模具详情页
- `/mold/[moldId]/ideas` 风格画法库
- `/mold/[moldId]/generate` AI 生成页
- `/holidays` 节日列表
- `/holiday/[holidaySlug]` 节日详情
- `/gallery` 用户作品墙
- `/upload` 上传作品
- `/profile` 用户中心
- `/admin` 后台首页
- `/admin/molds` 模具管理
- `/admin/ideas` 风格管理
- `/admin/user-works` 作品审核
- `/admin/holidays` 节日管理
- `/admin/analytics` 数据统计

---

## 7. 数据模型（建议）

### 7.1 `molds`

- `id`
- `mold_id`
- `name`
- `slug`
- `theme`
- `description`
- `difficulty`
- `main_image_url`
- `gallery_image_urls`
- `suitable_styles`
- `created_at`
- `updated_at`

### 7.2 `style_ideas`

- `id`
- `mold_id`
- `title`
- `style_tags`
- `holiday_tags`
- `color_palette`
- `base_nail`
- `steps`
- `matching_tips`
- `difficulty`
- `image_url`
- `created_at`
- `updated_at`

### 7.3 `holidays`

- `id`
- `name`
- `slug`
- `description`
- `season_month`
- `cover_image_url`
- `created_at`
- `updated_at`

### 7.4 `user_works`

- `id`
- `user_id`
- `mold_id`
- `title`
- `image_url`
- `style_tags`
- `color_description`
- `process_description`
- `status`（`pending/approved/rejected`）
- `likes_count`
- `saves_count`
- `created_at`
- `updated_at`

### 7.5 `users`

- `id`
- `email`
- `nickname`
- `avatar_url`
- `role`
- `created_at`
- `updated_at`

### 7.6 `ai_generations`

- `id`
- `user_id`
- `mold_id`
- `input_style`
- `input_holiday`
- `input_color`
- `input_difficulty`
- `input_nail_shape`
- `input_text`
- `output_text`
- `created_at`

### 7.7 建议补充表

- `likes`
- `saves`
- `scan_events`

---

## 8. 技术栈（MVP 推荐）

- **前端**：Next.js + React + Tailwind CSS + shadcn/ui
- **后端**：Supabase（PostgreSQL、Auth、Storage）
- **AI**：OpenAI API（文本生成）
- **部署**：Vercel

策略：

1. 先使用 Mock Data 打通全流程。
2. 再逐步接入 Supabase。
3. 最后接入 AI 与数据统计。

---

## 9. AI 系统提示词（可直接使用）

```text
You are an AI nail art design assistant for 3D silicone nail molds.

Your job is to help users create practical, beautiful, and manufacturable nail art ideas based on a specific mold.

Rules:
1. The ideas must be suitable for resin casting silicone molds.
2. Do not suggest impossible ultra-thin floating structures.
3. Do not suggest real gemstones or metal parts unless the user asks.
4. Focus on coloring, matching, style, holiday theme, and press-on nail composition.
5. Output clear steps for beginners.
6. Always include color palette, base nail suggestion, mold piece coloring method, matching tips, and suitable occasion.
7. Keep the tone friendly and practical.
8. Output in the user’s selected language.
```

---

## 10. 数据统计（第一版必须有）

按模具维度统计：

1. 二维码扫码次数
2. 页面访问次数
3. AI 生成次数
4. 用户上传作品数量

全局统计：

- 热门风格
- 热门节日
- 收藏最多作品
- 相关推荐点击次数

---

## 11. 二维码实施规范

1. 不建议把二维码做在模具细节里。
2. 推荐位置：包装卡片/模具背面标签。
3. 模具本体仅保留编号与品牌。
4. 二维码要求：黑码白底、高对比、尺寸足够、避免弯折反光区域。
5. 使用动态短链接便于后续改版与活动切换。

---

## 12. 第一批上线主题建议（6 个）

1. Christmas 圣诞
2. Bow 蝴蝶结
3. Ocean 海洋
4. Flower 花朵
5. Gothic 哥特
6. Angel / Cupid 天使爱神

---

## 13. MVP 验收标准（DoD）

1. 不同二维码可进入不同模具页。
2. 模具页可展示图片、介绍、风格方案。
3. AI 可生成结构化美甲建议。
4. 用户可上传作品，状态默认待审核。
5. 管理员可审核作品。
6. 管理员可新增/编辑模具。
7. 审核通过作品可在作品墙展示。
8. 页面适配手机端。
9. 页面加载性能可接受。
10. 后台可查看扫码与访问统计。

---

## 14. 一句话总结

开发一个**移动端优先**的 AI 美甲模具灵感程序：用户扫码直达模具页，获取画法与配色灵感、AI 方案并上传作品；后台可管理内容与审核，并追踪扫码与行为数据，快速验证内容驱动复购模型。
