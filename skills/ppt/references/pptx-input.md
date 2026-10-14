# 演示内容结构参考

[sample-presentation.json](../assets/sample-presentation.json) 用于组织可编辑 PPTX 的 title 与 slides。每页可包含 chapter、tag、title、content、bullets、metrics、image_prompt、image_path 与 notes。字段是内容编排草案，使用时按当前宿主工具的实际形状和母版映射。

## 科技感元素在 PPTX 形状中的映射规范

- **顶栏徽标（tag / chapter）**：映射为页面顶部小号文本框或微型圆角矩形，字号 10~12pt，全大写字母配微冷灰或荧光青文本（如 `[01 · MACRO HORIZON]`）。
- **动作型标题（title）**：映射为页面主标题占位符，字号 22~28pt 加粗，亮白（`#FFFFFF`）或冰蓝（`#E0F2FE`），直接亮明技术观点。
- **导引论述（content）**：映射为副标题或观点段落，字号 13~15pt，微冷灰（`#94A3B8`），陈述技术机理或背景。
- **要点与特性（bullets）**：映射为 2~3 栏并列卡片容器（深色填充圆角矩形），每栏内包含短句与高亮关键词，避免连贯长文本堆砌。
- **量化指标卡（metrics）**：映射为独立的大字突出卡片（Hero Metric Card），数值采用 32~40pt 加粗等宽字体，配小号单位与指标说明。
- **全息配图（image_prompt / image_path）**：右侧或全屏底图布局，遵循 [配图提示词工程规范](image-prompt-engineering.md) 提示宿主生成并插入图片。
- **演讲备注（notes）**：写入 PPTX 备注页（Slide Notes），记录演讲台本、技术细节与防幻觉事实出处。
