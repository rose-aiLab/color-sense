[README.md](https://github.com/user-attachments/files/29694661/README.md)
# 调色眼 · 色感训练

一个通过"猜颜色混合结果"来训练色感的小游戏。系统给出几种颜色和各自的比例，你先在心里想象混合后的结果，再从调色板中选出最接近的一格。每局 10 题，成绩自动保存在本机，可以看到自己的进步曲线。

## 在线部署（GitHub Pages，约 2 分钟）

1. 在 GitHub 上新建一个仓库（比如叫 `color-sense`），选择 Public。
2. 把本项目的 `index.html` 上传到仓库根目录（网页上点 "Add file → Upload files" 即可）。
3. 进入仓库的 **Settings → Pages**，在 "Build and deployment" 下：
   - Source 选择 **Deploy from a branch**
   - Branch 选择 **main**，目录选 **/(root)**，点 Save。
4. 等待约 1 分钟，页面顶部会出现你的网址，形如：
   `https://你的用户名.github.io/color-sense/`

打开这个网址就能玩了，手机和电脑都支持。

## 本地试玩

无需安装任何东西，直接双击 `index.html` 用浏览器打开即可。

## 功能说明

- **双色 / 三色混合**：从两种颜色按比例混合起步，可切换到三色模式提高难度。
- **颜料混合（默认）**：模拟真实画材的减色混合（简化 Kubelka-Munk 模型），蓝 + 黄 = 绿，适合练习绘画调色直觉。
- **光色混合**：线性 RGB 加色混合，模拟灯光/屏幕叠色，适合数字设计方向。
- **科学打分**：用国际标准 CIEDE2000 感知色差公式（ΔE00）计算你选的颜色与真实结果的差距，ΔE ≤ 1.8（人眼几乎不可分辨）得满分 100。
- **进步曲线**：每局平均分保存在浏览器本地（localStorage），结算页显示最近 20 局的成绩折线。

## 技术结构（方便二次开发）

全部代码在单个 `index.html` 中，无任何依赖。核心模块：

| 模块 | 位置 | 说明 |
|---|---|---|
| 颜色空间转换 | `rgbToLab` / `labToRgb` | sRGB ↔ CIELAB（D65） |
| 色差计算 | `deltaE00` | CIEDE2000，已通过 Sharma 标准测试集 9/9 |
| 颜料混合 | `mixPaint` | 逐通道简化 Kubelka-Munk 减色模型 |
| 光色混合 | `mixLight` | 线性空间加权平均 |
| 题目生成 | `genQuestion` / `genPalette` | 随机基色 + 24 格调色板（含不同难度干扰色） |
| 打分 | `scoreFromDE` | ΔE00 → 0–100 分 |

### 扩展方向

- 四色及以上混合：`genQuestion` 中的 `count` 已支持任意数量，只需在界面加选项。
- 限定调色盘模式：固定 12 色颜料盘，练习"用手头的颜料调出目标色"。
- 名画配色还原、每日挑战、排行榜（需要后端）等。
