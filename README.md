# 🎮 Steam Controller 3D Demo

一个基于 **Three.js + 原生前端技术** 的高质量 3D 产品展示 Demo，用于展示 Steam Controller 的外观、交互细节与沉浸式购买流程。

该项目并非简单的 3D 渲染示例，而是一个**接近真实产品官网交互水准的 Web 3D Demo**，重点体现：

* Three.js 工程化使用方式
* 前端 UI / 动效 / 3D 渲染的协同设计
* 性能优化与交互体验意识

---

## ✨ 核心特性

* 🧊 **高质量 3D 模型渲染**（GLTF + PBR 材质）
* 🌀 **自动旋转 + 用户打断的 OrbitControls 交互策略**
* 🎯 **3D 热点（Hotspot）系统**：世界坐标 → 屏幕坐标映射
* 🎨 **产品换色系统**（动态修改材质颜色）
* 🪟 **模态框（Modal）产品信息展示**（非跳页）
* 🧠 **UI 过渡期间暂停 Three.js 渲染**，避免性能竞争
* ⚡ **性能优化**：

  * 限制 DPR（devicePixelRatio）
  * UI 动画期间暂停渲染循环
  * 避免 display 切换引发的 Layout Thrash

---

## 🧱 技术栈

* **Three.js**（WebGL 3D 渲染）
* **GLTFLoader**（加载 glb 模型）
* **OrbitControls**（相机控制）
* **原生 HTML / CSS / JavaScript**
* **CSS 动画 + transform 过渡**（避免重排）

> ❗ 未使用任何前端框架，专注演示 Three.js 在真实前端项目中的使用方式。

---

## 📂 项目结构

```bash
3d-demo/
├─ index.html              # 主入口（完整 Demo）
├─ models/
│  └─ source/
│     └─ Steam Controller10.glb
├─ README.md
```

---

## 🚀 本地运行方式

> ⚠️ 由于使用 ES Module + importmap，**必须通过本地服务器运行**。

### 方法一：VS Code Live Server（推荐）

1. 安装插件 **Live Server**
2. 右键 `index.html` → `Open with Live Server`

### 方法二：Node.js 简单服务器

```bash
npx serve .
```

然后在浏览器访问提示的本地地址即可。

---

## 🧠 设计思路说明（面试友好）

### 为什么 UI 操作时要暂停 Three.js 渲染？

Three.js 默认通过 `requestAnimationFrame` 持续渲染，如果在 UI 动画（overlay / modal / blur）期间仍然渲染 3D，会造成：

* 主线程竞争
* GPU 合成层压力过大
* 按钮点击、动画出现明显卡顿

**解决方案：**

* 在 UI 过渡期间设置 `isPaused = true`
* 临时跳过 render loop
* UI 结束后恢复渲染

这是商业级 Web 3D 项目的常见优化策略。

---

### Hotspot 是如何实现的？

1. 在 Three.js 中定义热点的 **世界坐标（Vector3）**
2. 每帧通过 `vector.project(camera)` 投影到 NDC
3. 转换为屏幕坐标，驱动 DOM 元素定位

这种方式可以保证：

* 相机旋转时热点位置始终正确
* 不依赖写死的屏幕坐标

---

## 📌 可扩展方向

该 Demo 已具备良好的扩展基础，可进一步升级为：

* Vue / React 组件化 Three.js 展示模块
* 数据驱动的产品配置（颜色 / 型号 / 配件）
* 使用 GSAP 管理复杂动画时间线
* Blender 中使用 Empty / Locator 作为热点锚点
* 接入真实商品接口（规格 / 评论 / 价格）

---

## 👤 适用场景

* 前端工程师作品集
* Three.js / WebGL 学习参考
* 产品级 3D 展示 Demo
* 面试中讲解 3D + 前端整合方案

---

## 📄 License

本项目仅用于学习与展示目的，3D 模型版权归原作者所有。

---

> 如果你对 **Web 3D / Three.js / 前端可视化** 感兴趣，这个 Demo 是一个非常适合深入打磨和扩展的起点。
