# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

这是一个五线谱记忆小工具，用于帮助用户学习识别五线谱上的音符。部署在 GitHub Pages，域名: piano.orenoid.com

## 术语说明

与用户沟通时使用以下术语：

- **大键盘** - 指页面下方的 88 键钢琴键盘（full-piano），显示 9 个音组，用于第一步选择音组
- **小键盘** - 指页面上方的 7 白键小键盘（paino），用于第二步选择具体音符（C/D/E/F/G/A/B）
- **音组** - 钢琴上按八度划分的组，从 A0-B0（第0组）到 C8（第8组），共9个可点击区域
- **中央C组** - 指 Group 4 (C4-B4)，高音谱表和低音谱表在此重叠
- **高音谱表** - 页面上的高音谱号区域，音符范围 G3-D6
- **低音谱表** - 页面上的低音谱号区域，音符范围 B1-F4
- **灰色遮罩** - 选择正确音组后显示的反馈（mask-gray）
- **红色遮罩** - 选择错误音组时，在正确音组位置显示的闪烁提示（mask-red）
- **黄色遮罩** - 未选音组直接点小键盘时显示的警告（mask-yellow）

## Architecture

这是一个纯前端静态应用，无需构建步骤。

### 文件结构

- `index.html` - 主应用文件（包含 HTML、CSS、JavaScript）
- `piano-88-keys.svg` - 88键钢琴键盘 SVG 图片
- `stave.gif`, `note.gif`, `right.gif`, `wrong.gif` - 静态图片资源
- `development/` - 开发/测试目录，包含与根目录相同的文件用于测试

### 核心功能

1. **双音谱表练习** - 支持高音谱表、低音谱表、双音谱表三种模式
2. **88键钢琴键盘** - 大键盘显示9个音组（A0-B0, C1-B1, ..., C8），可点击选择音组
3. **小键盘答题** - 7个白键用于选择具体音符
4. **状态机交互**:
   - IDLE: 等待选择音组
   - GROUP_SELECTED: 已选组，等待选择音符
   - ANSWERING: 答题中（防止重复触发）

### 音组映射逻辑

音符类名到钢琴音组的映射（基于实际五线谱位置）:

**高音谱表 (notesUp)** - 从 G3 到 D6:
- Group 6: Up3D, Up3C (D6, C6)
- Group 5: Up3B-Up2C (B5-C5)
- Group 4: Up2B-Up1C (B4-C4, 中央C组)
- Group 3: Up1B-Up0G (B3-G3)

**低音谱表 (notesDown)** - 从 B1 到 F4:
- Group 4: Down3F-Down3C (F4-C4, 中央C组)
- Group 3: Down3B-Down2C (B3-C3)
- Group 2: Down2B-Down1C (B2-C2)
- Group 1: Down1B (B1)

### 交互流程

1. 用户看到五线谱上的音符
2. 在大键盘上点击对应的音组
   - 正确: 显示灰色遮罩，进入 GROUP_SELECTED 状态
   - 错误: 在正确音组上显示红色闪烁遮罩
3. 在小键盘上点击具体音符
   - 正确: 显示绿色提示，250ms 后进入下一题
   - 错误: 显示红色提示，保持 GROUP_SELECTED 状态允许重试
4. 如果未选音组直接点小键盘: 显示黄色遮罩和"请先选择音组"提示

## Development

### 部署

- 主分支: `gh-pages`
- 部署方式: GitHub Pages
- 开发测试: `development/` 目录

### 常用操作

```bash
# 复制 development 到根目录（发布前）
cp development/index.html index.html
cp development/piano-88-keys.svg piano-88-keys.svg

# 提交并推送
git add -A
git commit -m "描述"
git push origin gh-pages
```

### 注意事项

- 所有图片路径使用相对路径（如 `stave.gif` 而非完整 URL）
- 切换谱表模式（高音/低音/双音）时会调用 `initNewQuestion()` 重置游戏状态
- 红色遮罩有闪烁动画（0.5s 闪两次），黄色遮罩无闪烁
- 错误提示显示时间: 2.75 秒
- 答对后重置延迟: 250ms

### 开发工作流

- `development/` 目录用于开发测试，改动后直接提交推送
- GitHub Pages 会自动部署 `gh-pages` 分支的最新代码
- 测试环境预览: `https://piano.orenoid.com/development/`
- 生产环境: `https://piano.orenoid.com/`
