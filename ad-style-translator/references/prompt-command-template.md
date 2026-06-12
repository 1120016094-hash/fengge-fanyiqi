# Prompt Command Template

Use this template to convert a selected style route into a downstream image-generation command. The skill using this template should not generate images; it should only output the command for another image-generation system.

## Route Recommendation Template

```markdown
我建议给你这几个方向：

1. [Route label]
适合原因：[Connect product/objective/audience to route.]
画面会像：[Observable visual result.]
风险：[Where it may fail or feel mismatched.]

2. [Route label]
...

我的建议优先级：[Best route] > [Second] > [Third]，因为 [brief reason].

你可以选一个方向，也可以选择两个方向混合。我下一步会把选择转成给下游图像生成 AI 使用的广告风格创作命令。
```

## Final Creative Command Template

```markdown
**推荐方向**
[Route label, with a short rationale. Mention the designer/method here if useful.]

**给下游图像生成 AI 的创作命令**
广告任务：
- 为 [brand/product/category] 创作 [format/size/channel]。
- 核心传播目标是 [objective]，面向 [audience]。
- 必须包含：[mandatory elements].

视觉方向：
- 构图：[specific layout, scale, focal point, spacing, hierarchy].
- 色彩：[palette, saturation, contrast, background].
- 字体与文字：[type behavior, density, size, placement, copy tone; avoid asking the image model to render long exact text if it is weak at typography].
- 材质与质感：[paper, print, vector, physical material, photography, packaging, etc.].
- 图像主体：[product/person/object/symbol and how it appears].
- 人物状态：[if applicable; otherwise say no people].
- 创意逻辑：[how the visual idea communicates the product benefit or brand belief].

画面文案：
- 主标题：[headline or placeholder].
- 副标题/利益点：[optional].
- 文案语气：[direct, quiet, manifesto, witty, etc.].

负向要求：
- 不要使用任何特定设计师姓名作为风格提示。
- 不要复制现有作品、品牌 logo、已知海报版式。
- 避免：[stock photo look, generic gradient, overdecorated layout, unreadable text, wrong mood, etc.].

生成参数建议：
- 画幅：[vertical poster / square / 16:9 / packaging front view].
- 精细度：[high-resolution print-ready / clean vector / photographed installation].
- 文字处理：[keep text minimal / reserve blank area for later typesetting / generate only abstract placeholder text].

使用说明：
- 将以上命令复制给下游图像生成 AI。
- 当前 skill 只负责风格翻译，不负责生成图片。
```

## English Prompt Variant

Provide an English version only when useful for the user's downstream image model.

```markdown
**English image prompt**
Create a [format] advertisement for [product/category]. Use [composition], [color], [typography behavior], [material], [subject], and [copy tone]. The image should communicate [objective] to [audience]. Avoid naming or imitating any specific designer, avoid copying existing posters, avoid generic stock-ad styling, avoid [route-specific negatives].
```

## Variant Template

```markdown
**可选变体**
1. [Variant A: keeps the same route but changes composition or emphasis.]
2. [Variant B: keeps the strategy but changes subject or material.]
3. [Variant C: safer/more commercial or bolder/more experimental.]
```
