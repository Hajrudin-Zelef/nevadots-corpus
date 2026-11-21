---
id: collect-261121-veille-ia/modeles-open/kling-image-4-0-review-8k-photos-long-text-and-image-sets-tested
title: "Kling Image 4.0 Review: 8K Photos, Long Text and Image Sets, Tested"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["image", "kling", "prompt", "text", "long", "light", "photos", "sets"]
source: https://medium.com/@foxlab/kling-image-4-0-review-8k-photos-long-text-and-image-sets-tested-cc6ddb5e363c
source_anchor: ""
source_lines: [1, 127]
section: "Modeles_Open"
---
Kling AI released Kling Image 4.0 in November 2026. Kling leads with three upgrades: photos that look more real, with native 8K output; much longer text inside an image; and sets of up to 10 matching images from one prompt. We ran seven prompts through it on Kling’s own site to see how those claims hold up on everyday jobs: a portrait, a close-up, a poster, a menu, an ad set, a long product page, and one prompt we had already run on MiniMax H3 Image.

Below are the prompts word for word, what came back, and what we’d write differently next time. For a summary of the model itself, see the Kling Image 4.0 model page. Kling Image 4.0 is coming soon to the CVY.AI generator.

## What Kling says is new

From Kling’s launch post:

- **Native 8K.** The first Kling model to draw images at 8K instead of enlarging a smaller one.
- **More realistic photos.** Kling describes a single model that both reads the prompt and draws the image, and says this is why skin, materials and the edge between light and shadow look natural.
- **Long text.** Up to 2,000 characters in one image, prompts of up to 15,000 tokens, more than 70 preset fonts, and seven languages: English, Simplified Chinese, Traditional Chinese, Japanese, Korean, Spanish and Portuguese.
- **Image sets and long images.** Up to 10 matching images from one prompt, or one tall image such as a full product detail page.
- **Color.** More than 110 preset color grades, or the palette of a reference image.

We tested each of these with one prompt.

## Test 1: a candid portrait

Realism is easiest to judge on a face in natural light, so we started with a street portrait.

*Prompt (3:4, 4K):* *Candid street portrait of a woman in her sixties laughing at a fruit stall in a covered market, morning light falling through a dusty skylight, freckles and fine wrinkles on her skin, a knitted grey cardigan, oranges and figs in wooden crates behind her, shot on a 50mm lens at f/2, natural colors, slight film grain. No text, no watermark.*


The image came back at 3,520 × 4,672 pixels, about 16 megapixels, for 6 credits on Kling’s own site at 4K. Everything in the prompt is there: the grey cable-knit cardigan, oranges and figs in wooden crates, morning light through a dusty glass roof with specks floating in it, and the soft background of a fast 50mm lens. At full size the freckles, the fine lines around her eyes and the loose, backlit strands of hair read like a photograph, and her hand resting on the crate looks natural.

## Test 2: 8K detail

8K only matters if the detail survives when you zoom in. We picked a subject full of small textures and looked at it at 100%.

*Prompt (3:2, 8K, 10 credits on Kling’s site):* *Close-up of an old watchmaker’s hands repairing a mechanical pocket watch on a wooden workbench, open brass gears, tiny screws and steel tweezers, a loupe lying on the table, warm desk-lamp light from the left, visible skin texture and creases, fine scratches on the brass, dust floating in the light beam. Ultra-detailed. No text, no watermark.*


The file is 9,984 × 6,656 pixels, about 66 megapixels and 88 MB as a PNG, for 10 credits on Kling’s site. Everything we listed is on the bench: steel tweezers, scattered screws and spare gears, a loupe, scratched brass and dust in the beam of a warm lamp from the left. At 100% the crop still shows the fine radial brushing on the steel wheel, the slots in the screw heads, the red jewels in their settings and the worn patina on the brass.

## Test 3: a poster with a full schedule

Most image models handle a headline. Kling says this one handles paragraphs, so we asked for a poster with a whole evening’s program.

*Prompt (2:3, 4K):* *A printed poster for a community night market in a warm retro risograph style, cream paper, orange and deep blue inks. At the top, a large title: “RIVERSIDE NIGHT MARKET”. Below it: “Every Friday · 6 PM — 11 PM · Pier 9”. Then a schedule list, one line each: “6:00 PM Doors open & street food”, “7:00 PM Live jazz by The Low Tide Trio”, “8:30 PM Handmade crafts auction”, “9:30 PM Lantern walk along the river”, “10:30 PM Last call”. At the bottom in small text: “Free entry · Cash and cards accepted · Rain or shine”. Illustrations of paper lanterns and food stalls around the text. Every word spelled exactly as written, no other text.*


The poster came back at 3,328 × 4,992 pixels. Every line we asked for is spelled exactly right, including the middle dots and the dash in “6 PM — 11 PM”, and the five schedule rows line up in two neat columns, with lanterns, a bridge over the river and two market stalls drawn around them.

## Test 4: a menu in Chinese

Chinese text is where many models still trip, so we tried a tea-shop menu with prices.

*Prompt (3:4, 2K, 4 credits on Kling’s site):* *一张手绘风格的奶茶店价目表海报，米色牛皮纸背景，墨绿色和砖红色字体，四周画着茶叶、桂花和水果的小插画。顶部大标题「山野茶事」，下面一行小字「每日鲜萃 · 现点现做」。价目表每行一项：「茉莉奶绿 16元」「桂花乌龙 18元」「黑糖鲜奶 19元」「白桃气泡茶 17元」「芋泥波波奶茶 21元」。底部小字：「少糖 · 去冰 · 加料 +3元」。所有文字准确无误，不要其他文字。*


At 2K it came back at 1,760 × 2,336 pixels in about 20 seconds, for 4 credits. Every character is right: the brush-style title 山野茶事, the subtitle, all five drinks with their prices, and the small line at the bottom. Around the text it drew only what we asked for, tea leaves, osmanthus and fruit, and picked a white peach to go with the 白桃 drink on the menu.

## Test 5: a six-image ad set

Image sets are new for Kling. We asked for six ads for one made-up product and checked whether the can looked the same in every frame.

*Prompt (3:4, 2K, set of 6):* *A set of 6 product photos for a fictional cold brew coffee brand “NORTHBEAM COLD BREW”: a matte navy can with a small white mountain logo and the brand name in white. 1) Hero shot of the can on wet dark stone with ice cubes, studio light. 2) The can in a hiker’s hand at a mountain summit at sunrise. 3) Close-up of cold brew pouring into a glass full of ice. 4) Flat lay of the can with coffee beans and an open notebook. 5) The can on a wooden desk next to a laptop in soft morning office light. 6) Three cans in a cooler at a beach picnic. Same can design, same label and colors in every image.*


All six came back at 1,760 × 2,336 pixels, for 24 credits on Kling’s site. The can is the same in every frame: matte navy, the small white mountain, NORTHBEAM in capitals with COLD BREW underneath, even when it is lying on its side in the flat lay or half buried in ice in the cooler. Each scene follows its numbered line, from the hero shot on wet stone with ice cubes to the hiker raising the can against a sunrise over the clouds. In set mode the images arrive one after another, so a set of six takes noticeably longer than a single picture.

## Test 6: a long product page

Kling also says it can make one tall image, such as a full product detail page. We asked for a five-section page for a made-up ceramic coffee set.

*Prompt (one long image):* *Create an e-commerce product detail page, as one long vertical image, for a handmade ceramic pour-over coffee set called “HEARTH & CLAY”: a speckled oatmeal-glazed dripper, a matching server and two cups. Sections from top to bottom: 1) Hero photo of the set on a wooden counter in morning light with the headline “Slow Coffee, Made by Hand”. 2) Three feature panels with icons and short captions: “Hand-thrown stoneware”, “Even 3-hole extraction”, “Dishwasher safe”. 3) A close-up of the glaze texture. 4) A size chart: “Dripper 11 × 9 cm · Server 600 ml · Cup 180 ml”. 5) A closing banner: “Brew something slower.” Clean modern layout, all text spelled exactly as written.*


The image came back at 1,326 × 3,136 pixels. The top half works: the headline “Slow Coffee, Made by Hand” and all three feature captions are spelled exactly right, and the glaze close-up shows the oatmeal speckles we asked for. The lower half falls apart. The size chart lost most of its numbers and reads “Dripper 600 ml · Cup”, with garbled text on the next line; an extra section of nonsense words appears; “Brew something slower.” is spelled right but sits over a mirrored, unreadable copy of itself; and the page ends in a blurry ceramic shape. In our tests, the long page was the one job Kling Image 4.0 did not finish well.

## Test 7: the same prompt on Kling Image 4.0 and MiniMax H3 Image

On November 17 we ran a perfume shot on MiniMax H3 Image for its model page. We gave Kling Image 4.0 the exact same prompt at the same size.

*Prompt (3:4, 2K):* *A cinematic commercial photograph of a hand-blown glass perfume bottle standing on a wet black basalt rock at the edge of a calm sea at blue hour. The bottle is filled with amber liquid and topped with a brushed brass cap; a small engraved label on the glass reads “NOCTURNE · EAU DE PARFUM”. Thin mist drifts across the water, a single warm rim light from behind makes the liquid glow, and soft reflections of the bottle ripple in the tide pool below. Shallow depth of field, 85mm lens, ultra-detailed water droplets on the glass, deep teal and amber color palette. No other text, no watermark, no signature.*


Both models spelled “NOCTURNE · EAU DE PARFUM” correctly, middle dot included, and both put the bottle on wet black rock with mist over the sea. Kling Image 4.0 handled the details better: the amber perfume sits inside a thick clear glass base and walls, the way a real bottle looks, the glass is covered in fine droplets, and the reflection in the tide pool below is clean. MiniMax H3 Image got the teal and amber mood and a rounder, hand-blown shape, but the bottle doesn’t add up: the bottom third turns clear and blue, while a strip of amber still runs down the right side next to it. Kling’s 2K image took a little over 10 seconds.

MiniMax H3 Image is already in the CVY.AI generator at 8 credits for a 2K image, so you can run the same prompt yourself and compare.

## Prompts that work for Kling Image 4.0

- **Plan the shot.** Subject, place, light, lens and colors, in that order. “50mm lens, overcast light” tells the model more than “beautiful photo”.
- **Quote every word that must appear.** Put the text in quotes and say where it goes: “the title at the top reads …”. For a schedule or a menu, give one line per item, the way it should be printed.
- **Name the language and the font feel.** “Brush-script title”, “bold condensed sans-serif”, “handwritten price tags”.
- **Number the images in a set.** One line per image, and repeat what must stay the same: “same can design, same color grade in every image”.
- **Give colors as names or codes.** “Navy and white” works; so does a hex code such as #1F3A5F when a brand color has to match.
- **Say “no other text” for signs and banners too.** Kling likes to decorate a poster with labels of its own, such as stall signs and a slogan ribbon. If only your words may appear, end the prompt with “No other text anywhere, including signs and banners.”

## When it comes to CVY.AI

Kling Image 4.0 is coming soon to CVY.AI. Keep an eye on the model menu in the CVY.AI generator.

Until then, you can run the same kinds of jobs today: GPT Image 2.6 Flare for posters and graphics with text, Nano Banana Pro for editing your own photos, and MiniMax H3 Image for realistic product shots at up to 4K.

## FAQ

**What is Kling Image 4.0?**

Kling AI’s image model, released in November 2026. It makes realistic photos at up to native 8K, posters and graphics with up to 2,000 characters of text, and sets of up to 10 matching images from one prompt.

**Does Kling Image 4.0 really make 8K images?**

Yes. Our 8K test came back at 9,984 × 6,656 pixels, about 66 megapixels, and a 100% crop still showed the brushing on the watch gears and the slots in the screw heads.

**How long can the text in a Kling Image 4.0 image be?**

Up to 2,000 characters, according to Kling, with more than 70 preset fonts and seven languages. In our poster test every line of the schedule came out spelled exactly as written.

**Can Kling Image 4.0 write Chinese?**

Yes. Simplified and Traditional Chinese are among its seven languages. In our test every character of a five-item tea menu, prices included, came out right at 2K.

**How many images can Kling Image 4.0 make from one prompt?**

Up to 10 matching images. Our six-image ad set kept the same can, logo and colors in all six pictures.

**Kling Image 4.0 or MiniMax H3 Image?**

On the same perfume prompt both spelled the label right, and Kling Image 4.0 handled the bottle better: the liquid and glass look like a real bottle, while MiniMax H3 Image’s bottle turned clear at the bottom with a strip of amber still down one side. MiniMax H3 Image is in the CVY.AI generator now, and Kling Image 4.0 is coming soon.

**When can I use Kling Image 4.0 on CVY.AI?**

Coming soon. Keep an eye on the model menu in the CVY.AI generator.

Ready to try a prompt now? Open the CVY.AI generator, pick a model, and paste any of the prompts above.
