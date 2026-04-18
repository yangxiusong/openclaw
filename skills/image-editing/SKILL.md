---
name: image-editing
description: 图片编辑和处理。使用场景：(1) 图片裁剪和尺寸调整，(2) 滤镜和调色，(3) 文字添加和水印，(4) 图片压缩和优化，(5) 格式转换，(6) 批量处理，(7) 封面图制作，(8) 社交媒体配图生成。
---

# 图片编辑与处理

## 概述

本技能提供完整的图片编辑和处理能力，从基础裁剪到高级合成，帮助用户高效处理各类图片需求，特别适用于公众号、小红书等社交媒体内容创作。

## 核心能力

### 1. 基础编辑

- 裁剪和旋转
- 尺寸调整
- 格式转换
- 压缩优化
- 质量调整

### 2. 图像处理

- 滤镜应用
- 色彩调整（亮度、对比度、饱和度）
- 锐化和模糊
- 去噪和增强
- 背景移除

### 3. 图文合成

- 添加文字
- 添加水印
- 添加贴纸
- 多图拼接
- 模板套用

### 4. 社交媒体配图

- 公众号封面（900x383px）
- 小红书封面（3:4 或 1:1）
- 微博配图（9 宫格）
- 朋友圈配图
- 视频封面

### 5. 批量处理

- 批量调整尺寸
- 批量添加水印
- 批量格式转换
- 批量压缩
- 批量重命名

## 快速开始

### 常用图片规格

**公众号**

- 封面大图：900 x 383 px（2.35:1）
- 封面小图：200 x 200 px（1:1）
- 文中图片：宽度 1080 px 以内
- 二维码：最小 258 x 258 px

**小红书**

- 封面推荐：3:4（960 x 1280 px）
- 方形封面：1:1（1080 x 1080 px）
- 横向封面：16:9（1920 x 1080 px）
- 笔记配图：3:4 或 1:1

**微信视频号**

- 封面：1080 x 1920 px（9:16）
- 缩略图：1080 x 1080 px（1:1）

## 工作流

### 工作流 1: 公众号封面制作

**步骤 1: 准备素材**

```
- 主图（高清、相关）
- 标题文字（简洁、吸引人）
- 品牌 Logo（可选）
```

**步骤 2: 调整尺寸**

```bash
# 使用 ImageMagick
convert input.jpg -resize 900x383^ -gravity center -extent 900x383 output.jpg
```

**步骤 3: 添加文字**

```bash
convert output.jpg \
  -font "SimHei" -pointsize 48 -fill white \
  -gravity center -annotate 0 "标题文字" \
  final.jpg
```

**步骤 4: 优化压缩**

```bash
# 控制文件大小在 500KB 以内
convert final.jpg -quality 85 final_optimized.jpg
```

### 工作流 2: 小红书封面制作

**步骤 1: 选择比例**

```
竖版 3:4 - 信息流更醒目
方版 1:1 - 通用性强
横版 16:9 - 适合场景图
```

**步骤 2: 设计布局**

```
推荐布局：
- 顶部：吸引眼球的标题
- 中部：主体图片
- 底部：补充说明/品牌标识
```

**步骤 3: 添加装饰**

```
- emoji 点缀
- 边框装饰
- 标签贴纸
- 滤镜美化
```

**步骤 4: 导出优化**

```
- 尺寸：960 x 1280 px（3:4）
- 格式：JPG 或 PNG
- 质量：85-90%
- 文件大小：< 5MB
```

### 工作流 3: 批量图片处理

**场景：批量添加水印**

```bash
#!/bin/bash
for img in *.jpg; do
  convert "$img" \
    -gravity southeast \
    -fill white \
    -pointsize 24 \
    -annotate +10+10 "© 我的品牌" \
    "watermarked_$img"
done
```

**场景：批量调整尺寸**

```bash
#!/bin/bash
for img in *.jpg; do
  convert "$img" -resize 1080x1080^ -gravity center -extent 1080x1080 "resized_$img"
done
```

## 图片处理命令

### ImageMagick 基础命令

**调整尺寸**

```bash
# 等比缩放
convert input.jpg -resize 50% output.jpg

# 指定宽度（高度自动）
convert input.jpg -resize 1080x output.jpg

# 指定尺寸（可能变形）
convert input.jpg -resize 800x600! output.jpg

# 裁剪填充
convert input.jpg -resize 800x600^ -gravity center -extent 800x600 output.jpg
```

**格式转换**

```bash
# JPG 转 PNG
convert input.jpg output.png

# PNG 转 JPG（白色背景）
convert input.png -background white -alpha remove output.jpg

# 转 WebP（更小体积）
convert input.jpg -quality 85 output.webp
```

**压缩优化**

```bash
# 降低质量
convert input.jpg -quality 80 output.jpg

# 优化元数据
convert input.jpg -strip output.jpg

# 限制文件大小
convert input.jpg -define jpeg:extent=500KB output.jpg
```

**添加文字**

```bash
# 简单文字
convert input.jpg -font "Arial" -pointsize 36 -fill white -gravity center -annotate 0 "Hello" output.jpg

# 带描边文字
convert input.jpg \( -size 800x600 xc:none -font "Arial" -pointsize 48 -fill black -gravity center -annotate 0 "文字" \) -gravity center -composite output.jpg

# 文字换行
convert input.jpg -font "Arial" -pointsize 32 -fill white -gravity center -annotate 0 "第一行\n第二行\n第三行" output.jpg
```

**添加水印**

```bash
# 文字水印
convert input.jpg -gravity southeast -fill white -pointsize 24 -annotate +10+10 "© 水印" output.jpg

# 图片水印
convert input.jpg logo.png -gravity southeast -composite output.jpg

# 平铺水印
convert input.jpg \( logo.png -write mpr:logo +delete \) -fill mpr:logo -draw "rectangle 0,0 800,600" output.jpg
```

**多图拼接**

```bash
# 横向拼接
convert img1.jpg img2.jpg +append output.jpg

# 纵向拼接
convert img1.jpg img2.jpg -append output.jpg

# 网格拼接（2x2）
convert img1.jpg img2.jpg img3.jpg img4.jpg \( -clone 0..1 -append \) \( -clone 2..3 -append \) +delete -append output.jpg
```

**滤镜效果**

```bash
# 黑白
convert input.jpg -colorspace Gray output.jpg

# 复古
convert input.jpg -sepia-tone 80% output.jpg

# 模糊
convert input.jpg -blur 0x3 output.jpg

# 锐化
convert input.jpg -sharpen 0x2 output.jpg

# 亮度调整
convert input.jpg -brightness-contrast 10x20 output.jpg

# 饱和度调整
convert input.jpg -modulate 100x150 output.jpg
```

### Python PIL/Pillow 示例

**基础处理**

```python
from PIL import Image, ImageDraw, ImageFont

# 打开图片
img = Image.open('input.jpg')

# 调整尺寸
img_resized = img.resize((800, 600), Image.LANCZOS)

# 裁剪
img_cropped = img.crop((100, 100, 500, 500))

# 旋转
img_rotated = img.rotate(90, expand=True)

# 保存
img_resized.save('output.jpg', quality=85, optimize=True)
```

**添加文字**

```python
# 创建绘图对象
draw = ImageDraw.Draw(img)

# 加载字体
font = ImageFont.truetype("simhei.ttf", 36)

# 添加文字
draw.text((100, 100), "标题文字", fill=(255, 255, 255), font=font)

# 保存
img.save('output.jpg')
```

**添加水印**

```python
# 打开水印
watermark = Image.open('logo.png')

# 计算位置（右下角）
position = (img.width - watermark.width - 10, img.height - watermark.height - 10)

# 粘贴水印
img.paste(watermark, position, watermark)

# 保存
img.save('output.jpg')
```

## 设计原则

### 封面设计原则

**公众号封面**

- 标题清晰可见（避免被裁剪）
- 图片质量高（不模糊）
- 色彩对比强（吸引点击）
- 品牌一致（风格统一）
- 文字精简（10 字以内）

**小红书封面**

- 视觉冲击力强
- 标题醒目（大字体、高对比）
- 信息层次清晰
- 使用 emoji 点缀
- 符合平台调性（真实、生活化）

### 配色原则

**安全配色**

- 主色 1 种 + 辅色 1-2 种
- 文字与背景高对比
- 避免过多颜色（< 5 种）

**常用配色方案**

```
商务风：深蓝 + 白 + 灰
清新风：浅蓝 + 白 + 绿
活力风：橙 + 黄 + 白
高端风：黑 + 金 + 白
```

### 字体选择

**中文字体**

- 黑体（通用、清晰）
- 宋体（正式、传统）
- 楷体（文艺、手写）
- 微软雅黑（屏幕显示好）

**英文字体**

- Arial（通用）
- Helvetica（现代）
- Georgia（正式）
- Impact（标题、醒目）

## 脚本工具

### scripts/image-resize.sh

批量调整图片尺寸（待创建）

### scripts/add-watermark.sh

批量添加水印（待创建）

### scripts/cover-generator.py

自动生成社交媒体封面（待创建）

### scripts/image-optimizer.py

图片压缩优化（待创建）

## 参考资料

### references/platform-specs.md

各平台图片规格要求（待创建）

### references/design-templates.md

设计模板和案例（待创建）

### references/color-palettes.md

配色方案和工具（待创建）

## 素材模板

### assets/templates/wechat-cover.psd

公众号封面模板（待创建）

### assets/templates/xiaohongshu-cover.psd

小红书封面模板（待创建）

### assets/fonts/

常用中文字体（待创建）

## 在线工具推荐

**设计工具**

- Canva - 在线设计平台
- 稿定设计 - 中文模板丰富
- 创客贴 - 社交媒体模板
- Figma - 专业设计工具

**压缩工具**

- TinyPNG - PNG/JPG压缩
- Squoosh - Google 图片压缩
- 图压 - 本地压缩工具

**背景移除**

- remove.bg - AI 抠图
- 稿定抠图 - 中文工具
- Photoshop - 专业工具

## 注意事项

1. **版权合规** - 使用正版素材，注意版权
2. **肖像权** - 使用人物照片需授权
3. **品牌规范** - 遵循品牌 VI 规范
4. **平台规则** - 遵守各平台图片规范
5. **文件备份** - 保留原始高清文件
6. **格式选择** - 照片用 JPG，图形用 PNG
7. **色彩模式** - 屏幕显示用 RGB，印刷用 CMYK

## 相关技能

- `content-creation` - 内容创作（配图需求）
- `wechat-mp` - 公众号运营（封面制作）
- `xiaohongshu-ops` - 小红书运营（封面设计）

---

**最后更新：** 2026-03-14  
**维护者：** 小灵 🧭
