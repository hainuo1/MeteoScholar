# 气象科学研究数据平台 — v6.1
> 基于 Vue 3 + FastAPI + PostgreSQL + PostGIS 的前后端分离气象科学研究平台
> 
---

## 项目概述

气象科学研究数据平台是一个面向大气科学研究的综合性 Web 平台，采用前后端分离架构——前端使用 Vue 3 + Vite 构建 SPA 单页应用，后端使用 FastAPI 提供 REST API 纯数据服务，前后端通过 HTTP + JSON 通信，JWT 无状态认证。

平台包含三大灾害观测模块和学术文献管理模块：

- **热带气旋观测分析**：基于 IBTrACS 国际最佳路径数据集，提供台风历史路径可视化、强度时序分析、风场结构展示、PostGIS 空间密度统计及 DeepSeek AI 科学解读
- **地震数据分析**：基于 USGS 地震目录，提供东亚地震事件查询、震中分布、区域地震活动趋势、震级-深度属性对比、PostGIS 邻近空间统计及 AI 灾害评估
- **龙卷风数据分析**：基于 NOAA SPC 龙卷风历史数据库（1950–2024），提供龙卷风事件查询、EF 等级频次分布、灾害影响评估、PostGIS 邻近密度分析及 AI 风险评估
- **学术文献检索与管理**：支持多维度文献检索（含 PostgreSQL tsvector 全文检索 + 中文兜底）、PDF 上传与全文提取、引文网络分析、文献收藏管理、检索浏览历史追踪及数据统计分析

---

## 核心功能

| 模块 | 功能描述 |
|:---|:---|
| 🌪️ **台风观测** | 路径可视化、强度时序分析、风圈展示、PostGIS 空间密度分析、AI 科学解读、出行建议 |
| 🌍 **地震分析** | 全球地震事件查询、震中分布、区域活动趋势、事件属性对比、PostGIS 邻近空间统计、AI 灾害评估 |
| 🌬️ **龙卷风分析** | 龙卷风事件查询（排序：EF等级↓ 死亡↓）、EF 等级频次分布、灾害记录解析、PostGIS 邻近密度分析、AI 风险评估 |
| 🔍 **文献检索** | 标题、作者、分类、关键词、全文五种维度检索 + PostgreSQL tsvector 全文检索 + ILIKE 中文兜底 + ts_rank 相关性排序 |
| 📄 **文献管理** | PDF 上传（自动提取全文）、编辑、删除，个人文献库 |
| ⭐ **文献收藏** | 一键收藏/取消，收藏列表分页 |
| 🔗 **引文分析** | 文献引用关系管理，引用网络查看 |
| 📊 **统计总览** | 高频关键词、活跃用户、高被引文献、浏览/下载/收藏排行 TOP10 |
| 📝 **使用记录** | 检索历史 + 浏览历史追踪，窗口化分页 |
| 🔐 **用户认证** | 注册、登录、密码找回（验证码 + 安全问答），JWT 无状态认证，登录失败锁定，找回密码安全信息不泄露 |
| 🤖 **AI 分析** | 集成 DeepSeek 大语言模型，按分析类型（趋势/风险/影响）prompt 工程，首次结果缓存 |

---

## 技术架构

```
浏览器 (localhost:5173)
    ↓
Vite 开发服务器 → Vue 3 SPA
    ↓ axios + JWT Bearer Token
FastAPI ASGI Server (localhost:5000)
    ↓ SQLAlchemy 2.0 + psycopg2
PostgreSQL + PostGIS (localhost:5432)
```

| 层级 | 技术 | 说明 |
|:---|:---|:---|
| **前端框架** | Vue 3.5（Composition API） | SPA 单页应用 |
| **前端构建** | Vite 6.1 | 极速 HMR 开发服务器 |
| **前端路由** | Vue Router 4.5 | 23 条路由 + 导航守卫 |
| **状态管理** | Pinia 2.3 | 轻量级响应式 store |
| **HTTP 客户端** | Axios 1.7 | JWT 拦截器 + 401 自动跳转 |
| **CSS 框架** | Tailwind CSS（CDN） | Utility-first 原子化 CSS |
| **图标库** | Font Awesome 4.7（CDN） | 矢量图标 |
| **地图可视化** | Leaflet 1.9 | OSM + 高德瓦片双底图 |
| **图表绘制** | Canvas 2D | 自绘柱状图、趋势图、频次图 |
| **后端框架** | FastAPI 0.115 | ASGI 异步框架 + 自动 OpenAPI 文档 |
| **ASGI 服务器** | Uvicorn 0.30 | 高性能 Python ASGI |
| **数据库** | PostgreSQL + PostGIS 3.x | 关系型 + 空间数据 |
| **ORM** | SQLAlchemy 2.0 + raw SQL | 混合模式：ORM 建模 + text() 查询 |
| **全文检索** | tsvector + GIN 索引 | PostgreSQL 原生全文检索（simple 分词 + 中文 ILIKE 兜底） |
| **认证** | PyJWT 2.10 + cryptography | HS256 JWT 无状态认证 |
| **配置管理** | pydantic-settings 2.5 | .env 环境变量自动加载，密钥不入库 |
| **AI 接口** | DeepSeek API（deepseek-chat） | Prompt 工程 + 结果缓存 |
| **PDF 解析** | pdfplumber 0.11 | 按页提取全文文本 |
| **验证码** | Pillow 10.4 | 服务端生成 CAPTCHA 图片 |
| **密码哈希** | Werkzeug ≥3.0.4 | scrypt 加密 |
| **跨域** | FastAPI CORSMiddleware | allow_credentials=True + 指定 origins |
| **空间扩展** | PostGIS + geoalchemy2 | geometry(Point/LineString/Polygon, 4326) + GIST 索引 |
| **一键启动** | AutoHotkey v2 | Windows 批处理启动脚本 |

---

## 数据来源：

| 模块 | 数据文件 | 约记录数 | 来源 |
|:---|:---|:---|:---|
| 台风 | `数据集/热带气旋/ibtracs.ALL.list.v04r01.csv` | ~800,000 路径点 | IBTrACS v04r01 |
| 地震 | `数据集/地震/*.csv`（7 个年代文件） | ~80,000 条 | USGS Earthquake Catalog |
| 龙卷风 | `数据集/龙卷风/1950-2024_actual_tornadoes.csv` | ~70,000 条 | NOAA SPC |

---

© 2026 南京农业大学 · 信息与计算科学专业

丁俊杰 · hainuo@stu.njau.edu.cn
