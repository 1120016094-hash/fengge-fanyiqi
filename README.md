# 风格翻译器

一个 Codex skill：根据用户提供的广告类型、品牌或模糊风格描述，推荐多个设计师/方法论路线供用户选择，并把选中的方向转化为下游图像生成 AI 可执行的广告风格创作命令。

## Skill 边界

- 推荐多个适合的设计师/广告风格路线。
- 解释每个路线为什么适合、画面会像什么、风险在哪里。
- 在用户选择路线后，输出结构化的下游图像生成命令。
- 不调用图像生成工具。
- 不生成或编辑图片。

## 目录结构

```text
.
├── README.md
├── ad-style-translator.skill
└── ad-style-translator/
    ├── SKILL.md
    ├── evals/
    │   └── evals.json
    └── references/
        ├── designer-style-matrix.md
        └── prompt-command-template.md
```

## 核心流程

1. 解析用户广告需求：品牌、品类、广告类型、目标受众、传播目标、风格关键词。
2. 推荐 3-7 个设计师/方法论路线。
3. 用户选择一个路线，或选择两个路线混合。
4. 将模糊风格转译为可执行的视觉参数：构图、色彩、字体、材质、主体、人物状态、文案语气、创意逻辑、负向要求。
5. 输出给下游图像生成 AI 使用的创作命令。

## 参考设计师路线

当前内置十个路线：

- Paula Scher
- Michael Bierut
- Kenya Hara / 原研哉
- Kashiwa Sato / 佐藤可士和
- Taku Satoh / 佐藤卓
- David Carson
- Stefan Sagmeister / Sagmeister & Walsh
- Malika Favre
- Noma Bar
- Droga5 / David Droga

skill 会把设计师名称作为推荐和讨论标签，但最终提示词会转译为可观察的视觉特征，不把“模仿某位设计师风格”作为下游图像生成命令。

## 示例

用户：

```text
我要做一个丰田汽车广告，但还没定风格。请根据汽车广告这个类型，多推荐几个设计师路线给我选。
```

skill 会推荐适合汽车广告的多个路线，例如：

- Michael Bierut：系统化信任，适合主品牌可靠感。
- 佐藤卓：日常产品信任，适合家用、省油、安全。
- 原研哉：安静留白，适合混动、新能源、低碳。
- David Carson：反网格破坏，适合 GR、运动、年轻化。
- Noma Bar：负空间双关，适合安全、环保、智能驾驶。

用户选择后，skill 再输出给下游图像生成 AI 的创作命令。
