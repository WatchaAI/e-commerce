# Workflow Checklist

## 1. Collect inputs step by step

When the user starts a new detail-page task, collect inputs in this order. Do not ask for every asset in one long list unless the user explicitly wants to batch-submit everything.

### Step 1 — 产品图

Ask for product images first. Always remind the user that real products should preferably use real-shot photos; front, back, side, close-up, packaging, certificate, ingredient list, and parameter-label images help prove the product structure and avoid hallucinated details. Accept hero image, detail close-ups, multi-view image, packaging, accessories, certificates, ingredient list, parameter label, or any current product material. If the user provides front/side/back views or a combined three-view, treat it as the strongest product identity source.

After receiving images:

- identify each image role
- summarize visible product traits
- note usable close-up details
- state which claims are visible-only and which still need user confirmation

Then run Step 1.5 before asking for product information.

### Step 1.5 — 产品图瑕疵分析、白底精修图生成与多角度一致性检查

This is mandatory after product upload unless skipped by the user. Output:

```markdown
【产品图识别】
- 图片1：角色 / 可用位置 / 对应精修图文件名
- 图片2：角色 / 可用位置 / 对应精修图文件名
- 继续列出每一张上传产品图；上传几张就生成并列出几张白底精修图

【产品可见特征】
- 必须保留：颜色、轮廓、结构、材质、logo/文字位置、比例

【图片瑕疵诊断】
- 画质问题：xxx
- 光影/背景问题：xxx
- 结构证明不足：缺少背面 / 侧面 / 细节图（如适用）

【定制精修提示词】
xxx

【精修图生成要求】
- 每张上传产品图都要生成一张对应的白底精修图；不要只处理第一张
- 产品图输入环节的最终输出项必须是产品白底三视图，用于后续详情页生图
- 保持产品身份不变
- 只优化摄影质感、光线、清洁度、透视和商业精致度；背景必须为干净纯白电商白底
- 不新增未经提供的配件、功能、认证、材质、文字或功效

【白底三视图二次瑕疵分析与精修提示词】
- 在生成最终产品白底三视图前，必须再分析一次多图一致性风险，不能只写“已检查”或“基本一致”
- 重点检查：材质、颜色、色相、饱和度、明度、色温、光源方向、阴影强度、比例、透视、结构、logo/标签位置、纹理、包装文字和独特细节
- 输出一条白底三视图专用精修提示词，目标是避免最终多图里的材质、颜色、光影、比例和产品身份不一致
- 如果发现颜色、材质或产品身份特征存在明显不一致，先输出返修提示词并重新修正失败图片，再进入产品白底三视图

【产品白底三视图】
- 这是产品图输入环节的最后输出项
- 使用上一步【白底三视图二次瑕疵分析与精修提示词】输出的三视图专用精修提示词，生成一张白底产品三视图，用于后续详情页生图锁定产品身份
- 必须保持同一产品、同一颜色、同一结构、同一比例、同一logo/标签位置和同一关键细节
- 如果用户没有提供足够角度，只输出可确认角度组成的白底产品视图表，并把缺失角度标记为待补充；不要臆造背面、侧面或隐藏结构
```

Analyze defects: blur, noise, exposure, color cast, reflection, deformation, dirty background, cropping, shadows, wrinkles/scratches, logo legibility, perspective, material texture, and missing proof angles. Create a customized product-retouching prompt for each uploaded product image or each image role, then generate one white-background refined product image per uploaded product image. Do not stop after only the first image. After generation, run a second consistency-risk analysis specifically for the final white-background three-view: compare material, color, hue, saturation, brightness, color temperature, lighting direction, shadow strength, scale, perspective, proportions, structure, logo/label placement, texture, packaging text, and distinctive details. Output a dedicated three-view retouching prompt. Then use that prompt to output a clean white-background product three-view image when front/side/back views exist, or the closest available white-background product view sheet with missing angles marked pending. Treat the product white-background three-view as the main product identity reference in later detail-page screens, while keeping original photos and individual refined images as proof references.

Only move to Step 2 after all uploaded product images have corresponding refined images and the multi-angle consistency report shows PASS, or after the user explicitly skips retouching/checking.

### Step 2 — 产品信息

Ask for category, brand/product name, selling points, target audience, usage scenarios, specs, price tier, platform, and mandatory copy claims.

After receiving product information:

- separate confirmed facts from pending facts
- avoid inventing specs, certifications, ingredients, materials, or performance data
- organize the information into a selling-point base if enough material is present

Then ask for the detail-page reference style image.

### Step 3 — 详情页参考风格图

Ask for reference style images, reference screens, or a complete reference long page.

After receiving references:

- identify visual style, color mood, typography density, layout rhythm, and content pacing
- determine screen count from the visible reference modules when appropriate
- slice long-page references before image generation when the page contains multiple modules

Proceed without a missing group only when the user explicitly asks to skip it. Mark skipped or missing data as pending in downstream outputs.

## 2. Identify the current production stage

Check what the user already provided:

- product photos
- refined hero image / refined product image generated from the product-photo retouching gate, with multi-angle consistency checked when multiple angles exist
- multi-view image
- product notes / specs / ingredient list / package contents
- existing planning markdown
- existing visual-design markdown
- reference detail-page image
- complete reference page image that should be sliced into screen/module references
- packaging or accessories images

If a later-stage asset exists, continue from that stage unless the user asks for a restart.

## 2.5 Run the connected reference-count workflow

After 产品图 + 产品信息 + 详情页参考图 are all available or explicitly skipped, read `connected_reference_count_workflow.md` and use it as the default downstream workflow. It replaces the older separate planning, visual-design, extraction, and generation stages.

Default sequence:

1. Connect product information, product white-background three-view, and reference style image.
2. Identify the reference image's visible screen/module count as `N`, then generate the complete N-page detail-page visual plan.
3. Extract each page prompt, the unified style prompt, and the unified negative prompt.
4. Generate each page at `9:16` from the extracted page prompt + unified style prompt + unified negative prompt + product multi-view only. Do not pass reference detail-page images or slices into the final image-generation call.

Do not generate images directly from the full plan.

## 2.6 Legacy screen-count fallback

Use this only when the user explicitly asks for a custom page count or the older flexible planning format.

Use this priority order:

1. User-specified screen count or range.
2. Number of visible screens/modules in the uploaded reference page or reference screen set.
3. Practical module count after semantic slicing when boundaries are unclear.
4. Default full-page recommendation: 8–12 screens, using 10 only as a stated default.

If the reference page has 6 visible modules, plan 6 screens. If it has 13 visible modules, plan 13 screens. If the user asks for 10 despite a different reference count, use the user's count and note the compression/expansion.

## 3. Build the connected reference-count plan

Use `connected_reference_count_workflow.md` to output:

- 详情页整体定位
- 第1页 through 第N页
- page purpose, visual content, layout structure, copy, AI image-generation prompt, copy layout notes, and image notes for every page
- unified style prompt
- unified negative prompt

Do not output only titles or only prompts. The plan must include copy and layout logic.

## 4. Extract prompts page by page

After the N-page detail-page visual plan above is complete, extract prompts from that visual plan iteratively:

- `提取详情页第1页的提示词，提取统一风格提示词，提取负面提示词`
- repeat for 第2页 through 第N页

Each extracted prompt must include:

- 页面名称
- 页面专属AI图片生成提示词
- 统一风格提示词
- 统一负面提示词
- 最终生图输入组合

## 5. Generate 9:16 page images

Generate one page per image unless the user asks for a combined long page. For each page, use:

- extracted page-specific prompt
- unified style prompt
- unified negative prompt
- product multi-view
- output ratio `9:16`

Keep product identity locked to the product white-background three-view.

## 6. Legacy planning fallback

Use this communication order only when the user explicitly asks for the older flexible planning format:

Use this communication order as a flexible sequence, not a fixed 10-screen requirement:

1. 核心吸引
2. 价值放大
3. 场景带入
4. 功能拆解
5. 差异优势
6. 细节透明
7. 材质 / 工艺 / 成分 / 触感
8. 信任建立
9. 结果表达
10. 参数收口 / 购买转化

Keep each screen specific and executable. Compress or expand the sequence to match the target screen count. Keep a hero opening and a parameter/package/CTA closeout when the page has enough screens.

## 7. Legacy visual design fallback

Define before screen design:

- 整体风格
- 主色调
- 光影风格
- 材质语言
- 构图基调

Then distribute layout rhythm on purpose according to the target count:

- 大图型: hero, atmosphere, result, or lifestyle screens
- 图文型: value explanation, scene, function, material, trust screens
- 多框型: comparison, proof, detail, parameter, package screens

For each screen in the visual design plan, first analyze the matched reference screen/module, then write the target screen design and an execution-ready image-generation prompt. The reference analysis must cover layout structure, product/subject placement, typography hierarchy, information density, color/lighting, background treatment, and which visual traits should be inherited. The generation prompt must explicitly combine the locked product identity, matched reference-screen rhythm, screen objective, composition, lighting, copy placement, and unsupported-claim limits.

Adjust if the category needs it, but avoid a single layout repeated for all screens.

For the default connected workflow, use `connected_reference_count_workflow.md` instead of this fallback section.

## 8. Legacy extraction tasks

For requests like “提取视觉风格定义与第1到X屏”:

- preserve headings
- preserve wording as much as possible
- extract only requested blocks
- deliver a new markdown file
- determine `X` from the user-provided detail-page reference screen count, visible sliced reference screens, or semantic module count
- keep `视觉体系定义` together with every screen from `第1屏` through `第X屏`
- do not split extraction into fixed partial grouped deliverables; the range must match the actual reference-page count

## 9. Legacy image generation tasks

Generate only the requested screens.

For the default connected reference-count workflow, generate each page from the extracted page prompt, not directly from the full plan. Each final image-generation input must combine: extracted page-specific prompt + unified style prompt + unified negative prompt + product multi-view. Do not include the reference detail-page image or sliced reference modules in final image generation. Use vertical `9:16`.

### Complete reference-page slicing

When the reference image is one complete long page or one image with multiple stacked screens, prepare screen-specific references for analysis and prompt writing:

1. Inspect the full reference and identify visible screen/module boundaries.
2. Slice by semantic blocks whenever possible: hero, value explanation, scene, feature module, detail/proof, closeout.
3. If boundaries are unclear, split by practical vertical content chunks and label them as modules.
4. Match each target generated screen to the closest sliced reference.
5. Keep the full reference available for overall style analysis, but use the matched slice only for prompt writing about screen layout, composition, typography placement, and information density.

Suggested names:

- `参考图_第1屏.png`
- `参考图_第2屏.png`
- `参考图_模块03.png`

Do not pass the unsliced full reference or sliced modules into final per-screen generation for the default connected workflow; the final image input is the extracted prompt plus product multi-view.

### Reference usage pattern

- reference page = style and pacing
- sliced reference screen = screen-specific layout and rhythm
- product image = identity lock
- packaging image = package or trust screens
- multi-view image = detail or evidence screens

### Screen notes

- **Screen 1**: strongest drama, hero center, minimal copy
- **Middle screens**: follow the reference-page module count and rhythm; use value explanation, scene immersion, feature breakdown, comparison, detail, material/texture/formula/craft, proof, and result screens as relevant
- **Final screen**: parameter grid, package contents, CTA, or purchase summary

## 10. Quality control

Before delivery, verify:

- refined product image exists or retouching was explicitly skipped
- multi-angle consistency has been checked when front/back/side/detail images exist
- same product identity across all screens
- screen count matches the user request or visible reference-page module count
- complete reference pages were sliced and matched to generated screens when applicable
- titles align with planning and visual-design markdown
- no unsupported technical claims
- info-heavy screens are cleaner than hero screens
- final closeout screen looks like a conversion page, not another hero banner
