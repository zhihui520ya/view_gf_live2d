# View GF Live2D

> （官方补药再跑路了，我还以为真见不到心心念念的枪娘了）

> **⚠️ 免责声明：本仓库仅供学习研究使用，请于下载后 24 小时内删除所有提取的游戏资源文件。**
> **游戏资源版权归原版权方（Sunborn / MICA Team）所有，请支持正版。**
>
> *If you are a representative of the copyright holder and believe this repository infringes upon your rights, please contact us and we will remove the content immediately.*

---

# View GF Live2D

Girls' Frontline (少女前线) Live2D 角色模型浏览器。

从游戏资源包中提取所有 Live2D 模型，在浏览器中浏览全部 440 个角色。

## 文件结构

```
view_gf_live2d/
├── live2d_viewer.html    # 主页面
├── live2d_models.json    # 模型列表（440 个角色名）
├── package.json          # npm 依赖
├── .gitignore
└── README.md             # 本文件
```

## 前置准备

### 1. 安装依赖

```bash
npm install
```

这会安装:
- `pixi.js@6.5.10` — 2D 渲染引擎
- `pixi-live2d-display@0.4.0` — Live2D 的 PixiJS 插件

### 2. 放置提取的模型资源

游戏提取出的资源需要放在 HTTP 服务器的根目录下，结构与提取工具的输出保持一致：

```
服务器根目录/
├── extracted/
│   ├── live2d/                    # 贴图文件（560 张 texture_00_*.png）
│   └── text/live2d/              # 模型文件
│       ├── 4type_5305_normal/      # 每个角色一个目录
│       │   ├── model.model3.json
│       │   ├── model.moc3
│       │   ├── model.cdi3.json
│       │   └── *.motion3.json
│       ├── 4type_5305_destroy/
│       └── ...
├── node_modules/                  # npm 依赖
├── live2d_viewer.html             # 查看器
└── live2d_models.json             # 模型列表
```

### 3. 启动 HTTP 服务器

```bash
cd 服务器根目录
python3 -m http.server 8080
```

### 4. 打开浏览器

访问 http://localhost:8080/live2d_viewer.html

## 使用说明

- **下拉选择**角色，点击**加载角色**按钮
- **自动轮播**：循环展示所有角色（每 3 秒切换）
- **拖拽**：按住角色拖动移动位置
- **滚轮**：缩放角色（滚轮在画布区域操作）
- 模型位置会被约束在可视区域内，不会跑出屏幕

## 依赖说明

| 依赖 | 版本 | 用途 |
|------|------|------|
| pixi.js | ^6.5.10 | 2D WebGL 渲染引擎 |
| pixi-live2d-display | ^0.4.0 | Live2D Cubism 4 的 PixiJS 适配层 |
| live2dcubismcore | 5-r.5 (CDN) | Live2D Cubism 4 Core SDK（从官方 CDN 加载） |

### Cubism Core

Cubism 4 Core SDK（`live2dcubismcore.min.js`）从 Live2D 官方 CDN 加载：
`https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js`

该文件包含 WebAssembly 运行时，用于解析和渲染 .moc3 格式的模型。

## 模型数据处理

从游戏 (Unity IL2CPP) Asset Bundle 中提取的 Live2D 资源经过了以下处理：

1. **剥离 Unity 包装头** — .moc3 文件前 48 字节的 Unity 序列化包装已移除
2. **修正 model3.json 路径** — 所有文件引用改为相对于 model3.json 的相对路径
3. **贴图映射** — 根据 Asset Bundle 处理顺序将 560 张贴图正确映射到 440 个模型
4. **贴图路径** — `../../../live2d/texture_00_NNN.png`（从模型目录到贴图目录的相对路径）

## 模型来源

Live2D 模型提取自游戏 `GIRLS' FRONTLINE` 的 Unity Asset Bundle (`.ab`) 文件。

## 许可证

- 本仓库仅包含查看器代码，**不包含**游戏提取的模型资源文件
- Live2D Cubism SDK 需遵守 [Live2D 专有软件许可协议](https://www.live2d.com/eula/live2d-proprietary-software-license-agreement_en.html)
- 游戏资源版权归原版权方所有
