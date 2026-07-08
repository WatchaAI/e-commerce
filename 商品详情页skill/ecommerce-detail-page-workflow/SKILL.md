---
name: ecommerce-detail-page-workflow
description: Step-by-step e-commerce detail-page workflow that first asks for 产品图 with a real-photo reminder, analyzes product image defects, creates a customized product retouching prompt, generates refined product images, then collects 产品信息 and 详情页参考风格图 before creating 完整详情页策划、视觉体系、分屏提取和分屏生图. Use when users ask for 商品详情页、详情页策划、卖点梳理、视觉方案、首屏KV、分屏海报、长图拆屏、按参考图屏数生成、参考详情页复刻风格、产品四视图展示、参数页、包装页、或把产品图做成整套电商详情页。 Works across watches, beauty, apparel, shoes, bags, jewelry, home goods, food, 3C electronics, appliances, and other consumer products.
---

# Ecommerce Detail Page Workflow

Build reusable product detail pages from product assets, reference pages, and structured selling points.

Use this skill to convert raw product material into a staged e-commerce workflow. By default, guide the user through the material collection steps in order:

0. Step-by-step material intake
   - Step 1: ask for product images first, while reminding the user that real products should preferably use real photos, including front/back/side proof images when available
   - Step 1.5: after product images are available, analyze product-image defects, create a customized retouching prompt, generate white-background refined product images, then check multi-angle image consistency such as color consistency while keeping all other product traits unchanged
   - Step 2: after refined product images or an explicit skip are available, ask for product information
   - Step 3: after product information is available, ask for detail-page reference style images

After intake is complete, use the connected workflow for downstream production:

1. Connect 产品图 + 产品信息 + 详情页参考图
2. Identify the reference image's visible screen/module count and generate a complete N-page detail-page visual plan
3. Extract each page's image-generation prompt plus unified style and negative prompts
4. Generate each 9:16 page from the extracted prompt + product multi-view only
5. Skill化复用与打包

For execution checklists, read `references/workflow.md`.
For strict output skeletons, read `references/output_templates.md`.
For category-specific emphasis, read `references/category_notes.md`.
After 产品图 + 产品信息 + 详情页参考图 are all available, read `references/connected_reference_count_workflow.md`; this replaces the older downstream planning/design/extraction/generation logic.

## Map the request first

## Step-by-step intake rule

When the user says they want to make a detail page but has not provided all required inputs, do not ask for every asset at once. Guide them in this order:

1. **产品图**: ask the user to upload the product image first. Always remind them: if this is a real product, provide real-shot photos when possible; front, back, side, close-up, packaging, certificate, ingredient list, and parameter-label images help preserve authenticity and avoid invented details. Product images can include hero image, detail image, multi-view image, packaging, accessories, certificates, ingredient list, or parameter label. If they upload front/side/back views or a product three-view image, treat it as the highest-priority product identity source. If they upload several images, identify each image role before moving on.
1.5. **产品图诊断与精修**: after product images are present, analyze image defects before asking for product information. Check blur, noise, exposure, color cast, reflection, deformation, dirty background, cropping, shadows, wrinkles/scratches, logo legibility, perspective, material texture, and whether front/back/side proof is missing. Then output a customized retouching prompt, use it to generate white-background refined product image(s), and check multi-angle image consistency before moving on. If the user explicitly says not to retouch or already provides a refined product image, skip generation and still check consistency when multiple angles exist.
2. **产品信息**: after refined product images are generated or explicitly skipped, ask for category, brand/product name, selling points, target audience, usage scenarios, specs, price tier, platform, and any claims that must appear. If some facts are missing, mark them as pending instead of inventing them.
3. **详情页参考风格图**: after product information is present, ask for detail-page reference style images or a complete reference long page. Use this reference for visual language, rhythm, typography density, color mood, and screen/module count when appropriate.

Only proceed to selling-point整理, screen planning, visual design, extraction, or detail-page screen image generation after these three input groups are either provided or explicitly skipped by the user, and after product-image retouching plus a visible multi-angle consistency report are completed or explicitly skipped. If the user says to continue without one group, proceed and clearly mark missing information as pending.

Use short prompts between steps, for example:

- “先发产品图。我先判断产品外观、图片角色和可用细节；如果是真实产品，建议发实拍图，最好包含正面、背面、侧面和细节证明图，这样后面不会把产品结构画偏。”
- “产品图已收到。我会先分析瑕疵并给出定制精修提示词，生成白底精修商品图后，再检查多角度图片一致性；确认后再发产品信息：品类、品牌/型号、卖点、规格、目标人群和平台。”
- “产品信息已收到。现在发详情页参考风格图或参考长图，我会按它的视觉节奏和模块数量来规划。”

Identify what the user already has:

- Product image or multiple product images
- Refined hero product image
- Multi-view or four-view image
- Product notes / feature list / product specs
- Existing planning markdown
- Existing visual-design markdown
- Reference long-page image
- Complete reference image that contains a full detail page or multiple stacked screens
- Packaging / accessories / certificates / ingredient list / parameter list

Then route to one mode:

- **Connected reference-count mode** → when 产品图 + 产品信息 + 详情页参考图 are all available, read `references/connected_reference_count_workflow.md`, identify the reference screen/module count, output the complete N-page visual plan, extract each page prompt, then generate from extracted prompts
- **Extraction mode** → extract requested prompts or screen blocks from existing markdown
- **Image-generation mode** → create one or more screen images from extracted prompts
- **Full-pipeline mode** → complete all stages in order

If the user already has a later-stage file, continue from that stage unless the user explicitly asks to redo earlier stages.


## Product-photo retouching gate

Before asking for product information, turn every uploaded product image into a matching cleaner white-background detail-page source image unless the user explicitly skips this step. If the user uploads 2 product images, generate 2 refined images; if they upload N product images, generate N refined images. If the product inputs include front, side, and back views or a combined product three-view, the final product-input deliverable must also include a clean white-background product three-view image for downstream page generation.

Required output after product upload:

1. 图片角色识别: list each image and its role.
2. 产品可见特征: summarize only visible traits that must be preserved.
3. 瑕疵诊断: note image-quality problems and missing proof angles.
4. 定制精修提示词: write a product-specific prompt for generating a refined product image.
5. 精修图生成: generate one white-background refined product image for each uploaded product image, keeping product identity unchanged. Do not stop after only the first image when multiple product images are provided.
6. 白底三视图二次瑕疵分析与精修提示词: after refined images are generated, re-analyze multi-image consistency risks before creating the final three-view. Check material, color, hue, saturation, brightness, color temperature, lighting direction, shadow strength, scale, perspective, proportions, structure, logo/label placement, texture, and distinctive details. Then output a dedicated white-background three-view retouching prompt designed specifically to prevent material/color/identity inconsistency in the final multi-view product sheet.
7. 产品白底三视图: as the final product-image input deliverable, use the dedicated three-view retouching prompt produced in step 6, then output a white-background three-view product image that preserves the same product identity, color, structure, proportions, logo/label placement, and visible details across all angles. If the user did not provide enough angles, create the closest available white-background product view sheet and mark missing angles as pending instead of inventing hidden structure.

Retouching prompt must preserve: product color, silhouette, proportions, logo placement, structure, material texture, package text, labels, and any visible defects that are part of the actual product design. Improve only photography quality, light, cleanliness, perspective, and commercial polish. The refined product image must use a clean pure-white e-commerce background, with no props, scene background, gradients, shadows that obscure edges, or decorative elements. After generating all refined images, visibly check consistency across multiple angles and output the report: color temperature, hue, saturation, material texture, logo/label placement, silhouette, proportions, structure, packaging text, and distinctive details must stay consistent; only lighting cleanup and white-background presentation may change. If color or any identity feature is inconsistent, do not proceed to product information; output a repair prompt and regenerate the failed white-background image(s) first. Do not add unprovided accessories, functions, certifications, materials, text, or claims.

## Keep the workflow conversion-driven

After the three required input groups are available, use `references/connected_reference_count_workflow.md` as the source of truth for page count, page structure, output format, prompt extraction, and final image generation. The older flexible conversion sequence below is only fallback guidance if the user explicitly asks for a different custom structure:

1. Core attraction / hero claim
2. Core value expansion
3. Scenario or audience fit
4. Function breakdown
5. Difference or comparison
6. Detail transparency
7. Material / craftsmanship / formula / texture
8. Trust / evidence / proof
9. Result after use or wearing
10. Parameters / package / CTA closeout

One screen = one primary communication task.
Do not overload one screen with many unrelated claims.

## Screen-count rule

Do not force every detail page to 10 screens.

Determine the target screen count in this priority order:

1. If the user explicitly specifies a screen count or range, use that count.
2. If the user uploads a reference long page or multiple reference screens, use the number of visible screens/modules in the reference as the target count.
3. If the reference page has ambiguous boundaries, segment it into practical content modules and use that module count.
4. If there is no reference screen count and no user-specified count, suggest 8–12 screens for a full detail page, with 10 as a common default only after stating that it is a default.

When the reference count differs from the default conversion sequence, compress, expand, or merge communication jobs while keeping one primary task per screen. Always preserve a hero opening and a parameter/package/CTA closeout when the page has enough screens.

## Non-negotiable rules

- Lock the same product identity across all generated screens.
- Preserve visible product traits: color, form, layout, silhouette, materials, logo placement, and proportion.
- Inherit the rhythm and visual language of reference pages, not the exact product design or brand story of the reference.
- Do not invent unsupported claims.
- If data is not verified from user material, write it as pending, omit it, or present it as a placeholder field.
- Separate visible-value claims from technical-spec claims.

## Handle product information carefully

Visible and usually safe to describe when clearly shown:

- color combinations
- silhouette / shape
- visible texture
- visible structure
- wearing or usage style
- packaging contents that are clearly shown

Require verification before writing as fact:

- exact dimensions / weight
- ingredients / formula percentages
- movement model / chip model / battery capacity
- waterproof level / durability grades / certification
- material purity / precious-metal claims
- warranty / service / origin / patent / medical or efficacy claims

## Stage 1 — connected reference-count visual plan

When 产品图 + 产品信息 + 详情页参考图 are available, read `references/connected_reference_count_workflow.md`, identify the reference image's visible screen/module count as `N`, and generate the complete N-page detail-page visual plan from the connected master prompt logic.

The plan must include:

- 详情页整体定位
- page 1 through page N
- each page's page purpose, visual content, layout, titles/copy, AI image prompt, copy layout notes, and image-reference notes
- unified style prompt
- unified negative prompt

Keep this stage grounded in user-provided assets.

## Stage 2 — prompt extraction

After the N-page detail-page visual plan from Stage 1 is complete, do not generate images directly. Extract each page prompt from that Stage 1 visual plan using this instruction pattern:

- `提取详情页第1页的提示词，提取统一风格提示词，提取负面提示词`
- continue through 第N页

Each extracted prompt must include the page-specific image prompt, unified style prompt, unified negative prompt, and final input combination.

Use the extraction structure in `references/connected_reference_count_workflow.md`.

## Stage 3 — 9:16 page image generation

Generate each page only from its extracted prompt.

Each final generation input must connect:

- page-specific extracted prompt
- unified style prompt
- unified negative prompt
- product multi-view

Do not pass the reference detail-page image, reference style image, or sliced reference modules into the final image-generation call. The reference image is used earlier for page count, style analysis, and prompt writing only.

Default output ratio is vertical `9:16`.

## Legacy visual system fallback

Use this fallback only when the user explicitly asks for the older flexible planning/design format instead of the connected reference-count workflow.

Always define page-wide design first:

- 整体风格
- 主色调
- 光影风格
- 材质语言
- 构图基调

Then design each screen with execution-ready detail.

Each screen must include:

- 主标题
- 副标题
- 版式类型
- 参考图本屏分析: analyze the matched reference screen/module for layout structure, product placement, typography hierarchy, color/lighting, background, decorative elements, and information density
- 画面设计
- 字体与排版
- 本屏生图提示词: write an execution-ready image-generation prompt for this screen that combines the product identity lock, matched reference-screen style/rhythm, screen objective, composition, lighting, copy placement, and claim boundaries

Control rhythm intentionally and adapt proportions to the target count:

- Hero / atmosphere screens use **大图型**
- Benefit / scene / function screens use **图文型**
- Comparison / proof / parameter screens use **多框型**

Screen 1 must behave like a hero KV:

- strongest visual center
- clear depth
- strong light hierarchy
- minimal copy

## Stage 4 — extraction mode fallback

When the user asks to “提取” parts of the visual-design markdown:

- keep original wording unless normalization is necessary
- keep numbering and structure intact
- extract only the requested block
- save as a new markdown deliverable
- when extracting a grouped visual deliverable from a reference detail page, determine `X` from the user-provided reference-page screen count or visible sliced module count, then extract `视觉体系定义 + 第1到X屏` as one continuous block
- do not create fixed partial grouped deliverables; the grouped range must follow the actual reference-page screen count

Typical extraction tasks:

- 视觉体系定义 + 第1到X屏（X = 用户输入的详情页参考图屏数 / 可见模块数）
- 仅提取某一屏
- 仅提取视觉体系定义

## Stage 5 — screen image generation fallback

Generate one screen per image unless the user asks for a combined long page.

Default output ratio is vertical `9:16`.

### Full-reference slicing rule

If a reference image is a complete long detail page or a single image containing multiple screens, slice or segment it before final generation.

Use the sliced references this way:

- whole reference image = overall style, color, typography, pacing
- sliced screen / module reference = direct layout and rhythm reference for the matching generated screen
- adjacent sliced screens = transition and spacing reference when the matching boundary is ambiguous

Prefer semantic slicing by visible screen/module boundaries. If the reference has no clear boundaries, split it into practical vertical chunks based on content blocks and use each chunk only for the closest target screen.

Name sliced references clearly, such as `参考图_第1屏.png`, `参考图_第2屏.png`, or `参考图_模块03.png`.

Use references with explicit roles:

- reference detail-page image = style / pacing / layout rhythm
- sliced reference screen = screen-specific composition / information density / typography placement
- product image or multi-view image = product identity lock
- packaging image = package / accessories / evidence screens

When writing generation prompts, state clearly:

1. image intent and style
2. what each reference image is used for
3. what product identity must stay unchanged
4. screen layout and scene
5. on-image text if needed
6. exclusions and negative constraints

For prompts based on a complete reference page, explicitly mention both the whole-page reference and the relevant sliced reference for that screen.

For the default connected reference-count workflow, do not generate directly from the full plan. First extract `统一风格提示词`, `统一负面提示词`, and each page's `AI 图片生成提示词` using `references/connected_reference_count_workflow.md`. Then generate each 9:16 page from: extracted page prompt + unified style prompt + unified negative prompt + product multi-view. Do not include the reference detail-page image or sliced reference modules in the final image-generation input.

## Screen-type guidance

Adjust visuals by screen job:

- **Hero screen**: strongest atmosphere, biggest product, light drama
- **Info-heavy screens**: cleaner background, more typography space
- **Comparison screens**: modular blocks, do not fake competitor specs
- **Proof screens**: use multi-view, packaging, certificates, ingredient lists, or before/after evidence only when provided
- **Closeout screen**: parameter grid, package contents, CTA, purchase summary

## Category adaptation

Do not force the same language on every product.

Examples:

- **Watch / jewelry / bag** → stress material, silhouette, wearing scenes, refined details
- **Beauty / skincare / makeup** → stress texture, formula points, use sequence, before-after logic, ingredient proof if provided
- **Apparel / shoes** → stress fit, fabric, cut, wearing scenarios, outfit compatibility
- **Food / supplements** → stress flavor, ingredients, source, serving scenes, package size; do not invent health claims
- **3C / appliance** → stress structure, function, interface, use scenes, parameter modules, package contents
- **Home goods** → stress material, touch, scale, styling scenes, practical details

Read `references/category_notes.md` when the product category matters to the page logic.

## Deliverables by stage

Typical file outputs:

- `产品电商卖点数据.md`
- `详情页屏数规划.md` or `N屏详情页策划.md`
- `产品N屏详情页视觉设计方案.md`
- `详情页视觉风格定义与第1到X屏.md`
- `参考图_第X屏.png` or `参考图_模块XX.png` when slicing a complete reference page
- `第X屏_*.png`

When the user asks to package the workflow as a skill, validate and package this skill directory into a `.skill` file.
