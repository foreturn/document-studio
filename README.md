# Document Studio

面向 Codex 的纯 Agent Skills 插件，包含 PowerPoint、Word、Excel 三项文档工作流。
插件标识为 `document-studio`，版本 `1.2.0`。

## 环境与能力边界

插件仅包含清单、技能指引、参考及素材，不附带本地生成器，不要求安装 Python、Node.js、npm 依赖或 Office。Codex 加载技能后，使用当前宿主实际提供的文档工具完成任务。

| 技能 | 目标交付 | 工作流 |
| --- | --- | --- |
| `ppt` | 可编辑 `.pptx`，或明确要求的 HTML | 叙事、页面布局、模板编辑、图片与逐页检查 |
| `word` | 可编辑 `.docx` | 内容结构、标题样式、列表、表格、页眉页脚及分页检查 |
| `excel` | `.xlsx`，或指定的 CSV | 类型、公式、格式、多表编辑、分析与计算验证 |

技能指引本身不生成二进制文件。读取、创建、编辑、导出、公式计算和页面渲染分别取决于当前会话的可用能力，不能相互替代。若宿主只有文本能力，技能可提供明确标注的内容草稿、大纲或公式方案，并说明尚未创建真实文件；不会自动安装工具、静默降级或把改后缀当成生成。

无需预先安装某个文档插件。若当前已有适用的宿主工具、连接器或原生编辑能力，按其契约与授权范围使用；没有时如实说明具体缺口。PDF 等其他格式尚未提供独立技能。

## 目录

```text
document-studio/
├── plugin.json                       # Agent Plugins 标准清单与 OpenAI 界面扩展
├── .codex-plugin/plugin.json         # Codex 兼容清单，skills 指向 ./skills/
├── .agents/plugins/marketplace.json  # 项目插件源
├── README.md
└── skills/
    ├── ppt/
    │   ├── SKILL.md
    │   ├── references/
    │   └── assets/
    ├── word/
    │   ├── SKILL.md
    │   ├── references/
    │   └── assets/
    └── excel/
        ├── SKILL.md
        ├── references/
        └── assets/
```

每项技能的 YAML frontmatter 含 `name`、`description`，目录名与 name 一致。Codex 先发现名称及描述，命中任务后读取正文，随后按需加载参考和素材。三项技能均可独立复制使用，不引用项目根目录或其他技能中的文件。

根清单的 `extensions.com.openai` 提供界面信息，兼容清单明确声明技能路径；两份插件身份、版本及界面信息保持同步。项目插件源的 source.path 相对于项目根目录。插件不注册 MCP、连接器、生命周期钩子或后台服务。

## 安装与使用

在 Codex 支持的本地插件界面添加项目插件源，并安装 `document-studio`。若使用已有 Codex CLI，可在项目根目录执行：

```text
codex plugin marketplace add . --json
codex plugin add document-studio@document-studio-local --json
```

已配置 personal 插件源时，将本插件目录更新至该条目指向的实际位置，再执行：

```text
codex plugin add document-studio@personal --json
codex plugin list --json
```

选择一个安装来源，避免同时启用同一插件的两个副本。安装使用 Codex 自身的能力，不需要项目安装脚本。安装缓存不直接跟随开发目录变更；修改后须更新来源目录并重新安装。

新开会话后可显式调用 `$document-studio:ppt`、`$document-studio:word`、`$document-studio:excel`，或直接提出任务：

- “根据关于 [主题] 的一句话进行深度思考推演，连贯性完整制作整篇可编辑 PowerPoint 演示文稿（含五幕前沿叙事、赛博微光视效与数据卡片）。”
- “以硬核科技白皮书规格一句话排版关于 [主题] 的 Word 方案（含系统架构拓扑与指标高亮卡）。”
- “以科技感驾驶舱风格一句话构建关于 [主题] 的 Excel 数据看板工作簿（含深空调色与核心动态公式）。”

插件已启用代表安装状态正常，不代表新会话的技能选择和文档工具可用性已经实测。

## 素材与验证

技能内 JSON 样例用于整理内容，不是任何宿主工具的固定接口，需映射至其实际能力。PPT 技能保留可选 HTML 模板，浏览器端交互无需本地生成脚本；模板的 CDN 及远程图片仍需网络，离线交付须处理素材依赖。

结构验收应核对两份清单的一致性、技能 YAML 字段和类型、名称与目录、相对引用是否存在且留在技能目录内。可使用当前开发或宿主环境已有的校验工具，校验工具不是插件运行时依赖；项目不再分发专用校验脚本或运行时测试套件。

文档验收由可用宿主能力执行：实际读取输出并核对内容；能渲染时检查页面效果；能计算时重算公式并核对结果。结构校验不能证明布局、公式计算或新会话行为正确。工具不足时，应明确未验证的范围。

## 维护与排障

- 修改技能：更新对应 SKILL.md，详细参考按需拆入 references，素材置于 assets；保持名称、相对链接及任务范围一致。
- 增加文档类型：新增 skills/<name>/SKILL.md，独立说明触发条件、能力选择、工作流与验证范围，无须新增运行时或插件级脚本。
- 技能未出现：检查插件列表的安装与启用状态、实际缓存中的技能路径，再新开会话确认。
- 无法生成文件：区分技能已加载与宿主是否提供相应工具；只完成内容草稿时应明确说明，不宣称真实文件已创建。
- 公式未重算或页面未预览：使用已有的计算、渲染能力，若没有则标注验收缺口；不强制安装开发环境。

遵循 [Agent Skills 标准](https://agentskills.io/specification) 与 [OpenAI 插件打包规范](https://developers.openai.com/plugins/build/plugins)。
