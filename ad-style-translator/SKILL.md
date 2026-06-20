---
name: ad-style-translator
description: Recommend multiple suitable graphic-design or advertising style routes from a user's ad type, category, brand, or vague style request, then translate the user's chosen route into an executable advertising style command for downstream image-generation AI. Use this skill whenever the user mentions advertising style, poster style, designer reference, visual direction, KV, print ad look, "像某种设计师/广告风格", or asks to turn fuzzy style words into image prompt directions. This skill does not generate images; it only recommends designers/routes and writes production-ready style commands.
---

# Ad Style Translator

## License Notice

All rights reserved. 未经许可不得修改、分发、商用。

Use this skill to help a user move from an advertising type or fuzzy style request to a concrete, image-generation-ready creative command.

The core job is not to imitate a designer directly. The job is to identify which design logic would serve the user's brief, explain the options in plain language, and translate the chosen direction into visual parameters an image model can execute.

## Scope Boundary

This skill stops at the creative-command layer.

- Do recommend several designer/style routes for the user to choose from.
- Do translate the selected route into a detailed AI-executable advertising style command.
- Do include Chinese and, when useful, English prompt variants for downstream image-generation AI.
- Do not call image-generation tools.
- Do not create or edit images.
- Do not continue into production unless the user explicitly switches to a separate image-generation task outside this skill.

## Reference Loading

When recommending routes or writing prompts, read:

- `references/designer-style-matrix.md` for the ten style routes and their suitable/unsuitable use cases.
- `references/prompt-command-template.md` when you need the final command format.

## Operating Principles

- Treat designer names as internal reference labels and user-facing discussion aids, not as final prompt tokens.
- Do not put "in the style of [living designer]" or "by [designer]" into the final image-generation prompt.
- Translate style into observable choices: composition, color, typography, material, subject, figure behavior, copy tone, and creative logic.
- Preserve the user's brand/product objective. Style is a tool for persuasion, not decoration.
- If a route is weak for image generation, say so. For example, Droga5 is a strategy/methodology route, not a stable visual style.
- Avoid asking many questions. If the product, format, or objective is missing and cannot be inferred, ask up to three short questions. Otherwise make reasonable assumptions and mark them as assumptions.
- Use a two-step flow by default: first recommend routes, then write the command after the user chooses. If the user asks you to choose, pick the best route and write the command immediately.

## Workflow

### 1. Parse The Brief

Extract or infer:

- Product/brand/category
- Advertising type, such as car ad, tea drink launch poster, anti-fraud public-interest KV, beauty social poster, retail promotion, etc.
- Deliverable format: poster, KV, packaging, banner, social post, print ad, etc.
- Communication objective: launch, awareness, promotion, brand upgrade, social issue, product benefit, etc.
- Target audience and usage context
- Mandatory copy, logo, product shot, legal text, or platform size
- User's fuzzy style words, such as "高级", "克制", "街头", "反叛", "日式", "聪明", "强冲击", "艺术化", "包装感", "女性感"
- Any negative constraints, such as "不要太花", "不要像促销", "不要赛博", "不要太日系"

### 2. Map Fuzzy Style To Candidate Routes

Use the designer matrix to recommend 3-7 routes. Prefer routes that match both the advertising category and the communication job. For broad categories like "car ad" or "tea drink ad", give a wider route pool first because the user may be exploring.

A useful mapping:

- Loud, typographic, cultural, street-energy -> Paula Scher route
- Corporate, systematic, logo/identity, trust-building -> Michael Bierut route
- Quiet, white space, natural, philosophical, lifestyle -> Kenya Hara route
- Retail, iconic, red/white block, brand-symbol repetition -> Kashiwa Sato route
- Everyday product trust, packaging, long-term shelf presence -> Taku Satoh route
- Rebellious, youth, music, extreme sports, messy authenticity -> David Carson route
- Handmade, conceptual, physical process, personal truth -> Stefan Sagmeister route
- Fashion, beauty, sensual elegance, flat vector silhouette -> Malika Favre route
- Clever negative-space, editorial, social issue, visual pun -> Noma Bar route
- Social stance, real action, manifesto copy -> Droga5 route, paired with a visual route if image output is needed

For category-first requests, use category logic before style adjectives:

- Automotive main-brand trust -> Michael Bierut, Taku Satoh, Kashiwa Sato
- Automotive hybrid/EV/low-carbon -> Kenya Hara, Noma Bar, Sagmeister, Droga5 paired with Bierut or Hara
- Automotive youth/sport/GR/off-road -> David Carson, Paula Scher, Droga5
- Automotive fashion/urban lifestyle -> Malika Favre, Paula Scher, Kashiwa Sato
- FMCG/packaged food/drink trust -> Taku Satoh, Kenya Hara, Kashiwa Sato
- Cultural/event posters -> Paula Scher, Sagmeister, David Carson
- Public-interest/social issue -> Noma Bar, Droga5, Bierut
- Beauty/fashion -> Malika Favre, Kenya Hara, Paula Scher

### 3. Recommend Routes For User Selection

Unless the user already selected a route or explicitly asked you to choose, present route options before writing the final command.

For each route, include:

- Route name: designer/method label
- Why it fits the brief
- What the visual would look like
- Risk or mismatch
- Best-use scenario

Keep the recommendation concise and choice-oriented. Do not overwhelm the user with the full matrix. End by asking the user to select one route, combine two routes, or ask you to choose the best route.

### 4. Translate The Selected Route

After the user selects a route, or when you must choose for them, create an image-generation command with these sections:

1. Creative direction name
2. Advertising task
3. Visual command
4. Typography/copy command
5. Composition and layout command
6. Color/material command
7. Subject/scene command
8. Negative prompt / avoid list
9. Optional variants, usually 2-3

Write the command so a downstream image model can act without extra interpretation. Replace vague words like "高级" with visible properties such as "large quiet negative space, small refined typography, low-saturation off-white paper texture, restrained product placement."

The output should be usable even if the downstream image AI is a separate system. Do not assume access to any generation tool.

### 5. Final Prompt Safety Pass

Before finalizing, check:

- The final prompt does not directly ask for a living designer's style.
- It does not request copying a known work, logo, campaign, or exact layout.
- It includes the ad's product/category and deliverable.
- It specifies typography behavior, because ad posters often fail when text is vague.
- It includes a negative prompt to prevent generic stock-ad output.
- If the selected route is Droga5, it includes a real-action or manifesto idea and pairs it with a separate visual language.
- It is clearly labeled as a prompt/command for downstream image AI, not as an instruction for the current agent to generate images.

## Output Pattern

If the user has not chosen a route yet:

```markdown
我建议先给你 3-7 个方向：

1. [Route Name]
适合原因：
画面会像：
风险：

2. [Route Name]
...

你可以选一个，也可以让我把两个方向组合。我下一步只会把它转成可直接给下游图像 AI 使用的广告风格创作命令。
```

If the route is chosen or obvious:

```markdown
**推荐方向**
[One-sentence rationale]

**给下游图像生成 AI 的创作命令**
[Structured command from the template]

**可选变体**
1. ...
2. ...
3. ...

**使用说明**
把上面的命令交给你的下游图像生成 AI；本 skill 不负责生图。
```

## Quality Bar

A good result should feel like an art director's translation layer:

- It helps the user choose a direction, not merely names famous designers.
- It converts style into operational image instructions.
- It keeps strategy, visuals, copy, and format aligned.
- It makes clear what to avoid, because negative constraints often determine whether the generated ad feels professional.
