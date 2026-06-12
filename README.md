# 完美复刻蒸馏写作技能 (Perfect Replicate Distillation Writing Skill)

> **Clone any author's writing style** by deeply analyzing their content across 7+1 dimensions.

基于 JinDaGe（金大哥）在 jindage.com 上发布的写作风格克隆技能规格重建的开源版本。

## 概述

这个技能接收 3 篇以上的作者文章，通过 4 个阶段的深度分析，输出两样东西：

- **Output A：便携写作风格 Prompt** —— 可用于任何 LLM（Claude、ChatGPT 等）的自包含风格指令
- **Output B：Persona 子技能** —— 可直接安装到 Claude Code 的人物写作技能

## 分析框架：7+1 维度

| 维度 | 测量内容 | 分析方式 |
|------|----------|----------|
| 1. 词汇 (Lexical) | 高频词、指纹词、搭配、词汇丰富度 | 并行 |
| 2. 句法 (Syntactic) | 平均句长、从句类型比、标点指纹、段落节奏 | 并行 |
| 3. 语气 (Tonal) | 口语化指数、句长波动、形式度、确定性 | 并行 |
| 4. 篇章 (Discourse) | 开头/结尾模式、段落节奏、逻辑流 | 串行 |
| 5. 修辞 (Rhetoric) | 比喻密度、排比、设问、具体/抽象比 | 并行 |
| 6. 情感 (Emotion) | 极性分布、情感弧线、立场标记 | 并行 |
| 7. 视角 (Perspective) | 主题簇、人称分布、意象系统、价值词 | 并行 |
| 8. 格式适配 | 结构惯性、体裁匹配度、公式推荐 | 最后 |

## 4 阶段工作流

### Phase 0：语料摄取
- 接受 ≥3 篇文章（≥2,000 字/词）
- 理想 ≥10 篇 / ≥10,000 字
- 自动去重、清洗、统计

### Phase 1：7+1 维度分析
- 对 7 个维度进行系统化测量（并行 + 串行混合）
- 最后进行格式适配评估

### Phase 2：风格画像合成
- 每个维度提炼 3-5 条核心结论
- 跨维度验证提取 3-5 项"写作基因"
- 生成 5-10 条禁用清单

### Phase 3：双输出生成
- **Output A**：便携风格 Prompt
- **Output B**：Persona 子技能

### Phase 4：盲测
- 生成 2 篇 200-300 字测试文本
- 与原作进行 7 维度逐项评分
- 整体匹配度 >70% 为合格

## 项目结构

```
perfectly-replicate-writing-skills/
├── SKILL.md                              # 主技能定义（Claude Code SKILL.md）
├── README.md                             # 本文件
├── LICENSE                               # MIT License
├── references/
│   ├── 7-dimensions-framework.md         # 7+1 维度分析框架详情
│   ├── writing-formulas.md              # 14 种写作公式参考
│   └── persona-template.md              # Persona 子技能生成模板
└── examples/
    └── 抓马教育-style/                    # 示例输出：抓马教育品牌风格分析
        ├── SKILL.md                      # 生成的 Persona 子技能
        └── style-card.md                 # 7+1 维度完整风格卡片
```

## 使用方式

### 作为 Claude Code 技能安装

```bash
# 克隆项目
git clone https://github.com/zrh091110225/perfectly-replicate-writing-skills.git
cd perfectly-replicate-writing-skills

# 安装技能
cp -r SKILL.md ~/.claude/skills/perfectly-replicate-writing-skills/
cp -r references ~/.claude/skills/perfectly-replicate-writing-skills/
```

然后在 Claude Code 中触发：

> "帮我分析这些文章的作者风格"  
> "克隆这个作家的写作风格"  
> "用李笑来的风格写一篇关于效率的文章"

### 作为独立工具使用

直接按 4 阶段工作流手动执行分析：
1. 收集 ≥3 篇目标作者的文章
2. 按 7+1 维度逐一分析
3. 合成风格画像
4. 生成风格 Prompt 和 Persona 子技能

## 品味原则

1. **反复出现 ≥3 次的才是真特征** —— 一次是即兴，三次才是风格
2. **保留矛盾** —— 作者不一致的写法不要强行调和
3. **要具体** —— "平均句长 12 字，30% 的句子少于 8 字" 有用；"喜欢用短句" 没用
4. **禁用清单比正向特征更重要** —— 从"从不做什么"入手经常更能定义风格边界
5. **不猜测** —— 不虚构作者背景，不把通用写作建议当作独有风格
6. **不磨平瑕疵** —— 不自然的表达、怪癖和棱角，恰恰是风格标记

## 许可

MIT License

## 致谢

- 框架设计灵感来自 [JinDaGe](https://jindage.com) 在 skill 市场上发布的 [Perfectly Replicate Writing Skills](https://jindage.com/en/skills/perfectly-replicate-writing-skills)
- 本版本根据公开的规格描述重建，版权归原作者所有
