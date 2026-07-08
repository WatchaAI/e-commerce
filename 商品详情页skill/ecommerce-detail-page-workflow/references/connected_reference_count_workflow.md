# Connected Reference-Count Detail Page Workflow

Use this as the main workflow after 产品图 + 产品信息 + 详情页参考图 are all available or explicitly skipped. It replaces the older downstream logic for planning, visual design, prompt extraction, and final image generation. Keep the earlier intake steps unchanged.

## Inputs And Priority

Inputs:

- 基本产品信息: name, category, selling points, scenarios, target user/stage, specs, platform, language, required claims.
- 产品三视图: front, side, back, or a combined three-view. This is the highest-priority product identity source.
- 产品白底三视图: generated at the product retouching gate; use it as the final identity lock for all page generation.
- 详情页参考图: controls style only.
- 语言要求: Chinese, English, or bilingual; all copy must follow it.

Product identity rules:

- Preserve product appearance, structure, proportion, color, material, texture, logo position, pattern position, label position, accessories, parts, edge shape, and detail relationships.
- Do not redesign the product, change color/structure, add nonexistent parts/buttons/holes/accessories/patterns/decor, misclassify it, or let pages show different variants.

Reference style rules:

- First reverse-engineer the reference image: main color, auxiliary color, lighting direction, background material, composition, title position, selling-point module style, whitespace ratio, typography temperament, commercial atmosphere, and category/lifestyle atmosphere.
- The full detail page must clearly look extended from the reference style. Do not copy the reference product or replace the user product.

## Commercial Claim Rules

Prioritize trust, product recognition, real usage scenarios, clear benefit logic, detail transparency, usage convenience, category relevance, and purchase-conversion clarity.

Use only claims supported by product information, visible product evidence, packaging/certificates supplied by the user, or clearly marked placeholders.

Avoid unsupported or sensitive claims such as medical effects, quantified lab results, certifications, exact materials, origin, patents, performance grades, ingredient percentages, waterproof grades, battery capacity, or guarantee terms unless the user explicitly provides proof. If a fact is missing, use a placeholder, mark it pending, or omit it.

## Product Type Mapping

Automatically identify the product type from product information and the three-view, then adapt the page structure to the category and reference screen/module count:

- Beauty / skincare / makeup: texture, formula points, use sequence, ingredient proof if provided, result atmosphere without fake efficacy.
- Apparel / shoes / accessories: fit, fabric, cut, wearing scenarios, styling compatibility, detail close-ups.
- Bags / jewelry / watches: silhouette, material, craftsmanship, wearing scenes, premium details, packaging/trust.
- Food / supplements: flavor, ingredients, origin, serving scenes, packaging size; do not invent health claims.
- 3C / appliances: structure, function, interface, use scenarios, parameter modules, package contents.
- Home goods / furniture: material, touch, scale, home styling, practical details, usage scenarios.
- Baby / family products: safety feeling, trust, stage fit, real scenes, convenience, cleaning, family-life immersion.
- Other consumer goods: choose the conversion logic that best matches visible product function, user-provided selling points, and platform needs.

## Connected Master Prompt Logic

Use this role and task when generating the plan:

```text
你是一位世界顶尖的电商详情页视觉设计大师、品牌策划师、电商转化设计师、商业美术指导、版式设计师和 AI 图片提示词专家。

根据用户上传的【基本产品信息】【产品白底三视图】【参考风格图】以及【语言要求】，先识别参考风格图包含多少屏/模块，再自动判断产品属于哪一类电商产品，并生成一套完整的、适合电商转化的【N页商品详情页视觉方案】。N 必须优先等于参考图可见屏数或可见模块数。

这不是普通海报，也不是单纯场景图，而是一套具有产品识别度、信任感、真实使用场景、核心卖点表达、使用便捷说明和购买转化逻辑的电商详情页方案。

优先使用用户填写的信息；信息不完整时可根据产品白底三视图合理补全可见事实，但不能虚构医学功效、认证、检测结果、实验数据、绝对安全结论或具体年龄数字。

产品白底三视图是唯一产品依据。参考风格图只控制色调、光影、背景材质、构图、留白、版式、字体气质和商业氛围。整套详情页必须明显继承参考图视觉语言，像从参考图延展出来的一套同品牌页面。
```

## Page Count Rule

Determine `N` in this priority order:

1. If the user explicitly specifies a page/screen count, use it.
2. If the reference image is a complete long page or contains multiple stacked screens/modules, identify the visible screen/module count and set `N` to that count.
3. If the reference boundaries are ambiguous, segment it into practical semantic modules and set `N` to the module count.
4. If no count can be determined from user input or reference images, suggest 8-12 pages and use 8 only as the fallback default.

Always state the page-count basis in the output, such as `参考图可见 6 个模块，因此输出 6 页`.

## Adaptive Page Structure

Use this as the default communication sequence, then compress, expand, merge, or split it to fit `N` while keeping one primary task per page:

1. 首屏主视觉图: first-look attraction, brand feel, product recognition.
2. 核心卖点图片结合页: real image-supported 3-4 selling points; do not make an icon page.
3. 材质 / 结构 / 细节信任页: material, structure, craftsmanship, texture, formula, or technical detail depending on category.
4. 真实使用场景页: realistic scenario that matches category and target audience.
5. 尺寸 / 参数 / 适配说明页: do not invent dimensions/specs; reserve annotation areas when missing.
6. 使用步骤 / 安装 / 清洁 / 搭配说明页: 3-step columns or 01/02/03 cards according to product type.
7. 适用人群 / 痛点解决页: match target users, buying concerns, and scenario pain points.
8. 品牌收尾 / 购买转化页: full product display, brand closing line, CTA, logo/slogan area.

If `N` is fewer than 8, preserve a hero opening and conversion closeout, then merge related middle sections. If `N` is greater than 8, split complex middle sections into more detailed pages based on the reference rhythm.

## Strict Output Format

For the overall section output:

```markdown
详情页整体定位

产品类型：
产品调性：
参考风格总结：
适合平台：
页数依据：
建议页数：N页
建议输出比例：9:16
语言：
统一色调：
统一版式风格：
核心表达重点：
```

For pages 1 through N output each page as:

```markdown
第X页：页面名称
页面作用：
画面内容：
版式结构：
主标题：
副标题：
卖点标签：
补充说明文案：
AI 图片生成提示词：
文案排版说明：
配图说明：
```

For the final page output:

```markdown
第N页：品牌收尾 / 购买转化页
页面作用：
画面内容：
版式结构：
主标题：
副标题：
CTA文案：
品牌收尾语：
AI 图片生成提示词：
文案排版说明：
配图说明：
```

End with:

```markdown
统一风格提示词：
统一负面提示词：
```

## Unified Style Prompt Rule

Every page prompt must inherit this baseline and adapt it to the reference style:

```text
高级电商详情页视觉，真实商业摄影质感，严格保持上传产品白底三视图中的外观、结构、比例、颜色、材质、图案、logo和关键细节一致。必须深度参考用户上传的参考风格图，延续其主色调、辅助色、背景材质、光影方向、明暗关系、构图方式、留白比例、版式结构、字体气质、图文关系和商业氛围。整套详情页必须像从参考风格图延展出来的一组同品牌页面。画面必须专业、干净、有品牌感、信任感和电商转化感，包含明确标题区、副标题区、卖点模块区、留白区和图文排版结构，不能只是普通海报。可以生成简洁短标题和标签，但不要生成复杂长段文字。每一页图片提示词中必须明确写出：参考风格图的色调、光影、构图、背景材质、留白方式、字体气质和版式关系。
```

## Unified Negative Prompt Rule

```text
不要改变产品外观，不要改变产品结构，不要改变产品颜色，不要改变产品比例，不要重新设计产品，不要增加不存在的零件，不要增加不存在的配件，不要改变logo位置，不要改变图案位置，不要把产品识别成错误品类，不要虚构认证，不要虚构检测数据，不要使用医疗化表达，不要使用绝对化表达，不要写未经证实的功效文案，不要画面过暗，不要高饱和刺激色，不要背景杂乱，不要与参考风格严重偏离，不要过度促销感，不要产品变形，不要透视错误，不要边缘粗糙，不要让人物或道具遮挡产品主体，不要缺少标题区，不要缺少副标题区，不要缺少卖点区，不要缺少图文排版结构，第2页不要icon式卖点，不要抽象符号代替真实卖点图。
```

## Prompt Extraction Loop

After the N-page visual plan is complete, do not generate images directly. Extract prompts from that visual plan iteratively:

1. Use: `提取详情页第1页的提示词，提取统一风格提示词，提取负面提示词`.
2. Output/save as `第1页生图提示词`.
3. Repeat for 第2页 through 第N页.
4. Each extracted result must contain the page-specific `AI 图片生成提示词`, `统一风格提示词`, and `统一负面提示词`.

Extraction structure:

```markdown
# 第X页生图提示词

页面名称：
页面专属AI图片生成提示词：
统一风格提示词：
统一负面提示词：
最终生图输入组合：
第X页专属提示词 + 统一风格提示词 + 统一负面提示词 + 产品图多视图
```

## Final Image Generation Rule

Generate each page only from its extracted prompt. For every page:

- use output ratio `9:16`
- connect the extracted page prompt with the product multi-view
- do not include the reference detail-page image, reference style image, or matched reference slice as an image input at final generation time
- keep product identity locked to the white-background three-view
- preserve the unified style and negative constraints
- generate one page per image unless the user explicitly asks for a combined long page
