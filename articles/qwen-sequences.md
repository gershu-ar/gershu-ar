# 🎨 Qwen / Krea 2 on ComfyUI
### *The Totally Unrequested Prompting Guide*
---
**Working with NSFW & SFW models**  
*For the Purist Prompter*  

##### This guide was 97% human-written by a non-English language native (sorry for destroying your language), with 3% assistance from *Google Gemini 3.6-flash*—mostly for Markdown formatting assistance.

🔞 `RATED R` — *Contains strong language*

[![](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__01007_.png)](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__01007_.png)

*Rendered using Krea 2 NSFW model [FinePorn](#foot-notes--qa--recommended-models) on [ComfyUI](https://github.com/comfy-org/ComfyUI "ComfyUI") portable v0.36.0, revealing model's capacity to generate comic-like illustrations*<br>*Original PNG with workflow embeded*

------------
## THE BIG OLD INDEX
1. [The 101](#the-101)<br>
2. [Bootcamp](#bootcamp)<br>
2.1. [QWEN/Krea 2 vs SDXL](#qwenkrea-2-vs-sdxl)<br>
2.2. [Prompting on Qwen](#prompting-on-qwen)<br>
2.2.1. [Structure, structure, structure](#structure-structure-structure)<br>
2.2.2. [One -or a few- word to rule them all](#one--or-a-few--word-to-rule-them-all)<br>
2.3. [Remember](#remember)<br>
2.4. [Every great lie has a lot of detail](#every-great-lie-has-a-lot-of-detail)<br>
2.5. [Mark Twain yourself thru Qwen](#mark-twain-yourself-thru-qwen)<br>
3. [NSWF vs SFW](#nswf-vs-sfw)<br>
4. [Melting faces, water droplets and artifacts](#melting-faces-water-droplets-and-artifacts)<br>
5. [Don'T be so Negative: try to see the Light](#dont-be-so-negative-try-to-see-the-light)<br>
6. [Cut!](#cut)<br>
7. [*Nutshelling* it](#nutshelling-it)<br>
8. [Foot notes / Q&A / Recommended models](#foot-notes--qa--recommended-models)

------------
Disclaimer: There are probably a ton of errors in this article.  Suggestions are always welcome, drop me a line!
- **Want to improve this article?** Fork this repo, edit the file, and submit a **Pull Request**.
- **Want to leave a comment or feedback?** Start a conversation in [Discussions](https://github.com/gershu-ar/gershu-ar/discussions "Discussions").
------------

<br>

## The 101
**Prompt** → **Qwen** (encode) → **Krea** 2 (denoise) → VAE (decode)<br>
Congratulations, you're all set for Bootcamp.

👆 [Back to index](#the-big-old-index)
<br><br>
## Bootcamp

Welcome!

My **tool of choice is** **[ComfyUI portable](https://github.com/Comfy-Org/ComfyUI/)** (v0.36.0 as of SEP/2026).

[Utilized models](#foot-notes--qa--recommended-models "Used models") vary but the Qwen3-VL-4B FP8 Scaled *Text Encoder* and the Qwen *Image VAE* remained in place for all the examples generated in this guide.

Knowledge gathered and examples renderered on a local setup:  Windows 11 25H2, iCore Ultra 7 265K, 64 GB RAM (DDR5) with an NVIDIA RTX 3090 (24 GB VRAM, Studio Driver 616.92) as the powerhouse being able to handle `bf16` quantization but using  `int8` models because it simply **fliiiies** on Ampere architecture, leaving a massive overhead of free VRAM for the Text Encoder and whatever else I require.  Output quality is perfect for my needs and artifacts are neglegible to none.

> `bf16`, `int8`, `nf4`... at the [very end](#foot-notes--qa--recommended-models "very end") of this guide the appropiate quantization model for your GPU is suggested.

Also, for your convenience, [you can find here](https://github.com/gershu-ar/gershu-ar/blob/main/articles/workflows/Purist%20Krea%202.json "you can get here") a **simple**, **purist Qwen / Krea 2 ComfyUI workflow** in case you feel like testing prompts along.  Just hit on the three dots and select _Download_, then open in ComfyUI.  

The workflow is based on the [official Comfy](https://comfy.org/workflows/11657ed32877-11657ed32877/ "official Comfy") `text 2 image` (t2i) workflow, using *Nodes 2.0* style format.  We're going [Amish](https://amishamerica.com/do-amish-use-technology/) on this one:

It has been *de-automatizated* (stripped the LLM prompt enhancement and automatic negative creation) and *un-LoRAed* : there're just two text boxes (**positive** and **negative** prompts), seed, model and image size selector (in *megapixels*), and Ksampler settings.  Back to the basics! *Utility nodes* are on the subgraph and out of the way.

The workflow is arranged for live image previews and final saved/generated image view on a widescreen format.

You can, of course, arrange the workflow as you please.  **That's the beauty of ComfyUI** ❤️

<img width="1857" height="902" alt="image" src="https://github.com/user-attachments/assets/dcd54423-89ce-4a93-b421-a03db95fcf04" />

*A Qwen / Krea 2 [purist ComfyUI workflow](https://github.com/gershu-ar/gershu-ar/blob/main/articles/workflows/Purist%20Krea%202.json) with generation preview, the auxiliary nodes are comfortable set in a subgraph.*
<br><br>

***NSFW .... wait...? Am I here to learn how to create porn?***

| No  | Yes |
| ------------ | ------------ |
| Don't be confused: NSFW models render perfectly dressed scenarios and I consider them excellent at rendering dressed people precisely because clothing is not enforced by the model and I get to choose what and how clothing is being worn.  On SFW models there's the possibility of ruining certaing clothing or even a pose on the account of avoiding showing nudity or the posibility of nudity.  | If you want to, you can.  |

**This guide is about Qwen / Krea 2 prompting engineering**, *not for determining the use you give to these tools*.  That, amigo, is entirely up to **you**.

> Only one strong request: **Kindly keep children out and away its use**.  Do not use these tools to generate content of people under 18 years old in sexual or suggestive situations.  Neither allow people under 18 years old to access these tools.

Enough introduction.  Let's start.

#### **QWEN/KREA 2 vs SDXL**
Qwen and Krea 2 do not fill in the empty spaces.  In Qwen / Krea 2 no prompt, no tokenization, no embed.   If you have not described it, it will not be there.  *Mostly*.  Gaps will be completed to round up a context and provide a presentable product, but remove from your head the idea of the model doing what you don't want to.  Qwen and Krea 2 require rich prompts to properly generate specific rich renders and **oh boy if they deliver**!

**You can throw simple prompts and Qwen/Krea 2 will provide**, just don't confuse it with **specific shots**, where light, angle, character description and such play a role, specially if you're under the idea of continuity in between frames (story telling, video generation).

As for SDXL's CLIP **ViT-L** it was created to be **light**, **tolerant**, and to **guess**, not to follow strict orders.  SDXL does an amazing job at being casual, but you will pay the price in Qwen/Krea 2 if your mindset is along that pipeline.

Extensive detailed prompts in SDXL require a lot of external nodes to position region, render faces properly, have body proportions as desired on that dreamed setting that randomly appeared without a true control.

Also, SDXL experiences something denominated *CLIP chunking* (aka *context window,* aka *token chunking*, aka *prompt truncation*, aka *token boundary effects*), with about 75 useful tokens per chunk:

    Prompt Chunk 1
    [0-75]
    
    Prompt Chunk 2
    [76-150]
    
    Prompt Chunk 3
    [151-225]

This means the prompt gets cut into pieces and then fed to the model.  If the chunks do not make sense separately the model is unforgiving about the results -> the longer the prompt, the less sense the output made.

Yes, mods/nodes exist to tackle the issue to different degrees, but they require installation, certain level of technical knowledge and of course dedicating time to it: just for a prompt to make sense.   The good news: **this does not happen on Qwen / Krea 2**.  As you will read time and time again in this guide, Qwen and Krea 2 overcome plenty of technical limitations SDXL experiences and all it cares is about clean instructions.  If you make sense, Qwen and Krea 2 will make sense.

To sum up: If you're coming from SDXL you will notice that **Qwen/Krea 2 is a whole new ball game in prompting**. Takes a few days to catch up on the style but the results are exponentially better, **Krea 2 has a lot of potential**.  You will not go back to SDXL.  Guaranteed.

**Keep in mind these tips are not noticeable on small prompts but the richer the prompt gets, the more characters are part of it, that's when word precision becomes a must.**

👆 [Back to index](#the-big-old-index)
<br><br>
#### **PROMPTING ON QWEN**
##### Structure, structure, structure

Qwen interprets and generates embeddings in natural language.  The limitations of SDXL were obvious  is not present here: every word counts, every sentence has a meaning and *poetry* is not corny but what makes a fantastic output.

A suggested prompting technique involves **separating the prompt in categories**, specially for complex scenes with multiple characters.

Here's a crude explanation of a sample prompt:

    Subject details: Uniquely name and describe each character on the scene.
    
    Attire context: Using the names you defined above, describe how are they dressing.
    
    Action: Describe the actions your characters are doing, calling them by their given name.
    
    Camera: As name suggests, this is where your camera stands.  Hard to master.
    
    Environment: Where are the characters in? Describe it as detailed as possible.  Time of day, objects around, furniture, background, setting.
    
    Lighting: Same as camera, the lighting prompting is hard to master (models force light by default and prompt needs to tackle it wisely).
    
    Textures: Skins, fabrics, liquids, etc.

Make your prompts **modular**, so they're easy to modify and even easier to add and remove exceptional/eventual elements and do not worry about the order: Qwen creates embeds from text creating a context, **Krea 2 does not care the order of those embeddings**, as long as they all make sense.  In SDXL the prompt order was extremely important since it layered from the embeddings, in Qwen / Krea 2 what is important is the entire context, not the order you present it.



**EXAMPLE**<br>
Let's prompt for a simple scene:

    Subject details: A man, 45-years-old middle-aged, unfit, ugly. A dog, brown.
    
    Attire context:  Man is wearing sports pants, a t-shirt and sneakers. Dog is wearing an orange leash.
    
    Action: Man is walking Dog on the pathway. Dog is sniffing on another person.
    
    Camera: Wide full body shot taken from across the room at eye level, 35mm lens, Man is framed completely within the surrounding environment with ample distance between camera and subject.
    
    Environment: At a city park on a pathway.
    
    Lighting: Natural afternoon light, golden hour.
    
    Textures: Realistic skin.

Ouput:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/878310eda7fcc06262d6c2c7bd3a1530.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/878310eda7fcc06262d6c2c7bd3a1530.png)
*Rendered using NSFW [FinePorn](#foot-notes--qa--recommended-models) model - Original PNG with workflow embeded*

**EXAMPLE**<br>
Slightly modifying the same scene by adding "Dog is setting paws on Man" (only changing the dog action), rest remains the same:

    Action: Man is walking Dog on the pathway. Dog is setting paws on Man.
    
Output:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)
*Rendered using NSFW [FinePorn](#foot-notes--qa--recommended-models) model - Original PNG with workflow embeded*

**Qwen/Krea 2 did what I ask of them.**

After contextualizing the scene by creating the embeds, it filled the gaps and presented the intention with accuracy, even repeating the same background and scenery. **It prioritized the big picture over small detail**, and this is where we need to come in.

As you see on the examples both renders look **almost the same**. Liberties are visible. Since I did not specify about sneakers' colors, they're different. Grey for the t-shirt?, yes, but different shades of grays. The leash just "orange" is not detailed enough, needs refinement. Pants are not the same.

Also pay attention how it ignores *ugly* and *fit*. That dude does not stand out: his human face is what is expected to be. See, *Ugly* by itself has no meaning, it needs to be described: *long nose, deformed cheeks*, whatever. Same for the *unfit*. What is *unfit*? Unless you describe it, *unfit* is a shallow concept free of meaning. *Unfit* can refer to several things not just to a body type description.

*Ugly*, *unfit*, ... like using *relaxed*. There's no universal explanation for what *relaxed* is supposed to be in any given situation unless there's a context to it.  If you want somebody with a natural pose and "relaxed", do so by exactly prompting it as such: "The person has a naturally distended body pose, being at ease, resting shoulders and arms, and a face expression of tranquility and calmness".

Oh, Qwen, you cheeky, adorable bastard.

👆 [Back to index](#the-big-old-index)
<br><br>
##### One -or a few- word to rule them all
So, to recap: "**Woman standing with a bath robe**" will produce a woman with an open bath robe showing the underneath nude body. **Why is she nude**? Because the **default statistical prior** of a robe is not to be closed/buttoned. 

"**Long pants are seen over the floor**" will render a pair of perfectly positioned pants. **Why**? Because that's the **default statistical prior of the pants**: straight, perfectly in order, shape and position for the camera, as if presented for sale or in a drawer. *The perfect image of the pants* vs the image of realistic the pants.

**Be detailed**. Qwen is expecting the *closed* for the bath robe and *messy* scenario for the pants for a realistic output.  For Qwen that distinction is everything and so is for you: the pants, or robe or whatever you're prompting, will maintain coherence in between renders.

**Beware: *a single misplaced word can ruin your entire scene*.**

"Wearing summer clothes" might give you somebody on a bathing suit, but chances are you will get a naked person. **Why**? Because "summer clothes" is not an item, is a **group of items**, a generic term Qwen does not properly understand.  Maybe if there's a "beach" or a "summer" word on the rest of the prompt you might get somebody wearing a bathing short because there is context but avoid it: describe the bathing suit: "Bikini top and bikini bottom" are not the same as just "Bikini". "Bikini" alone refers to the bottom, not to a bikini set (bottom and top).

And be descriptive about the color of the bikini too, Krea 2 will try to mantain the same bikini style too for coherence, even if you don't specify it.

👆 [Back to index](#the-big-old-index)
<br><br>
#### REMEMBER

You're not in Kansas anymore, Dorothy: **This is NOT SDXL.**

Using `(pants:1.5)` won't get you anywhere, there's no embed weighing on Qwen. Every word is as important as the rest, the weighing is done by Krea 2 once it receives the embeddings.  The more an embedding is repeated and playing a role on the context, the more it will comply to what is ask from it... as long as you don't over confuse it.

Thus, **a single word has Qwen-sequences and it can ruin everything.**

Same goes for LoRAs or whatever embeddings you used or want to use:

`<LORA:XXX bla bla>`

Don't.  There is not even..

`// Hey, this is a comment`

... on Qwen.

SDXL used to ignore `// COMMENT` but Qwen will give it some interpretation based on context and create an embedding **confusing the model**.  



👆 [Back to index](#the-big-old-index)
<br><br>
#### EVERY GREAT LIE HAS A LOT OF DETAIL

You prompt for "**Pants**".

**Qwen/Krea 2 have mercy**: No item color specified? Pants colors will randomly change... or not: if the prompt has been repeated long enough along the +1 seed trend, modifying the scene (and as long as you don't mess with the pants), will maintain the random color it adopted by its own the first time.

This is playing with an uncertainty and a false security: the pants color will change randomly on every X render only to return to the series predominant color. But avoid that, use: "color pants" (color=black, white, green, etc). That will guarantee the exact same pants on each frame.

Extend it even more for detail and coherence: "Black sports pants with laces with a white tip". You will be getting the right color, type (pants), subtype (sports) and details (laces with a white tip).

**Details allow the repeating results effortlessly and provide the base for any realistic render:** **detail**. Life has detail, clothing fabric is not perfect, light is always terrible and shit happens. Models are trained on the basis of perfection, so Qwen is waiting to encode the terrible reality of life and Krea 2 to show it: don't be afraid to add extra details about a character's background or their intention within the context of the scene.

If your character is about to jump off a plane, add to the context: "The character is jumping off a plane for the first time in his life and is visibly mildly scared". Besides jumping off a plane, a sense of wonder, fear, curiosity will be added to the character's posture and even face expression if you haven't done so by prompting.  Qwen/Krea 2 won't be filling a blank but adapting to the situation.

Keep in mind the software you're using and the cache's circuit/purges. Instant results might not be visible from one render to the other: it might take 5/6 renders to start seeing results and a settled prompt into the cache (at least on ComfyUI). The more renders you run, the more you will see how the produced render takes the exact shape as you prompted it.

👆 [Back to index](#the-big-old-index)
<br><br>
#### MARK TWAIN YOURSELF THRU QWEN

**TEXT**
One thing Qwen and Krea 2 are **EXCELLENT** at is text. **Wildly good at**.  On short text, that is.  Long text? That's a whole different chapter.

Still, in between getting proper rendered text while working with SDXL and winning the lottery, winning the lottery is a sure thing.

Let's start with a simple setup by implementing two simple prompt modifications to the *Action*  block:

    Action: Man is holding a professionally designed sign that reads "I love dogs and I'm a drunk".  Dog is standing next to Man staring at Man.  Dog has a small sign hanging from the beck that reads: "This cat is crazy".

**Produces exactly that**. The more you describe the medium (paper, sign, wall advertising, graph, etc.) the more realistic it will be:

[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)
*Rendered using NSFW [FinePorn](#foot-notes--qa--recommended-models) model - Original PNG with workflow embeded*

The shown example is crude and hasty at best, but illustrates simply how **easily you can add text** to any level of customization and simplicity or complexity you require.

**HAND WRITTING**

As for** handwritten text** -if used in extensive or specific amount- Dante's Hell is a kid's playground in comparison, short specific phrases work like instant noodles, on the other hand, long text requires patience and proper prompting.

As the guide insists upon itself time and time again: prompt and models are not all the same.  In the case of handwritting text testing found out interesting results are worth sharing and, surprisingly enough, creator's **NSWF Krea 2 models show to excel over the official Krea RAW and Turbo variants** in the tests I run (read below).

Visual human-like results require heavy prompting: the *prior* to handwritten text are perfect handwritten characters, aligned, with clean strokes, the *perfect idea of handwritting*.  Every day human writting is anything but perfect: people usually don't write aligned, they do it with characters varying in size, with different pressured applied on the paper, etc.  The good news it you can get realistically looking handwritting text if you prompt it correctly, just a heads up that is not an easy task. 

> Adding human-like handwritting text prompts is pending.

Again that is also model dependant: some models understand better than others what you want to achieve.

For a simple handwritten note, you can throw the idea in clearly, paying special detail when need on complex or long words.  You will need several renders to get 100% perfect matching text and the used model plays heavily on the result:



    [PAPER SCENE MODULE]
    A single sheet of white paper lying on a wooden desk, photographed from above, realistic natural lighting, focus on the paper, clean composition, realistic paper texture, documentary-style photography.
    
    [HANDWRITING STYLE MODULE]
    Casual handwritten text written with a black ballpoint pen, authentic human handwriting, slightly uneven character shapes, subtle stroke variations, minor baseline drift, natural spacing inconsistencies, readable but imperfect, not calligraphic, not typographic, not computer-generated looking.
    
    [TEXT CONTENT MODULE]
    The paper starts as a note:
    "Dear GitHub reader:"
    Word "GitHub"  must be in blue and spelled exactly: G i t H u b
    
    The paper follows:
    "Long texts can be an issue on diffusion models: don't blame on Qwen or CLIP ViT-L. It's just a matter of how the whole system is designed."
    
    Word "diffusion" must be in blue and spelled exactly: d i f f u s s i o n
    Word "models" must be in green and spelled exactly: m o d e l s
    Word "ViT-L" must be spelled exactly: V i T - L
    
    On another line:
    "Handwriting you say? Well, here is official Krea 2's handwritting" with words "Krea 2's" in orange.
    
    Signing the document in red:
    "George Washington" with a double underslash.
    
    [READABILITY MODULE]
    Clearly visible text, legible handwriting, high text fidelity, easy to read, sharp focus on the writing.

<br>

*Produces a note dubiously signed by a Mr. Washington:*
<br><br>

[![](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00056_.png)](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00056_.png)
*Rendered using NSFW [Gonzalomo v4.0](#foot-notes--qa--recommended-models) model - Original PNG with workflow embeded*

Notice in the prompting the amount of stress some words required to be renderer appropriately.  

Output size also matters: If your render requires extensive, precise text, keep the image output or on above `2 PM - 1256x1672 pixels` , that gives Ksampler more room to maneuver, more pixels and space to work with. 

Despite the render was generated with NSWF [Gonzalomo v4.0](#foot-notes--qa--recommended-models) model, due credit goes to official Krea 2 in the understanding the base text training looks like undeniably related:

[![](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/comparison.jpg)](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/comparison.jpg)
*Left: Rendered using NSFW [Gonzalomo v4.0](#foot-notes--qa--recommended-models) model - Right: Hastily rendered using Official Krea 2 Turbo model*

The tests run with all the models mentioned in the guide were equally satisfactory showing similar outputs with the same rendering mistakes on the same words, evidencing a relation to the same base training model/s.

Even so, is worth to mention NSFW [Gonzalomo v4.0](#foot-notes--qa--recommended-models) model notably surpassed the official Krea 2 model on the same handwriting testing tasks, with NSFW [FinePorn v4.0](#foot-notes--qa--recommended-models) being particularly more detailed under certain scenarios.

This adds to the positive understanding that retrained, repurposed models **not necessarily** neglect the other *elements in the universe*.  **Refinements on the models from the base versions are notorious and evident**; even more, trained NSFW models demonstrate remarkable flexibility and enormous generation capacities, superseding the base model of Krea 2 in every conceived SFW/NSFW scenario.


**OVERLAY GRAPHICS**

If you want more complex text or even **overlay graphics**, you can absolutely can by even creating them on the fly: Qwen / Krea 2 excel at it too, not as mathematicaly precise as Ideogram 3.0/4.0 which has been designed to work with text and coordinates in mind, but it certainly creates a commendable job with ease.

> Remember: **there're no universal difussion models**.  Each model was trained for and is capable of something **different**.  Some excel at doing *this*, other's excel at doing *that*; read the model's card and the creators notes to understand what model you have and what it can and cannot do.  *Try them all!*, I say.  Then decide.

One great feature about Qwen / Krea 2 is you can dedicate an entire prompt to the overlays/graphics or simply take advantage of the modular system and create an overlay graphic line without compromising the rest of your work.

Appending a new module to the sample prompt:

`GRAPHICS: there's a professional TV news lower-third overlay that reads "Cheddar cheese is the best cheese ever", with a graph title that reads "Dawgs, Virginia". The news company is "CNN".  The overlay graphic has a news station format and is positioned at the very bottom of the frame as a full-width banner spanning across the screen.`

The ouput:

[![](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00896_.png)](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00896_.png)

*Rendered using NSFW [Gonzalomo](#foot-notes--qa--recommended-models) model - Original PNG with workflow embeded*

As you can see, Qwen / Krea 2 perfectly understood what I wanted and created the requested overlay graphics by word, **even using CNN's real life logo**.

It *filled the gap* (overlay's extra graphics/text) with **gibberish** but that's **partially my fault**: I did not fully specify all details the model was expecting to render the idea properly: model was trained with specific overlay graphics and I did not met the requisites.  **Usually news overlays exhibit a lot of extra information and I was short on my prompting**.  That can be solved by enhancing the prompt and/or creating prompts for very specific overlay graphics with very detailed instructions.

**FULL SIZED GRAPHICS**

You can also generate full sized graphics easily:

[![](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00902_.png)](https://github.com/gershu-ar/gershu-ar/blob/main/articles/img/K2__00902_.png)
*Rendered using NSFW [Sick Ollie](#foot-notes--qa--recommended-models) model. - Original PNG file with workflow embeded.*

If you're a movie fan or a +40 years old you'll recognize the poster style almost instantly.  Zero challenge in rendering it... except for the word *enchantment*, **all models struggled** with that word, and they all rendered it "Enachantenent".

Here's the full prompt for the "Under the Sea Enchantment" dance poster:



    SUBJECT:
    a hand-painted high school dance poster from the 1950s, displayed as the main subject of the image
    
    COMPOSITION:
    the poster fills almost the entire frame, photographed straight-on, perfectly centered, no people, no surrounding room visible
    
    CAMERA:
    front view, poster photography, medium format camera, sharp focus, highly legible typography
    
    LIGHTING:
    even indoor lighting, soft illumination, no glare, no reflections, vivid colors
    
    POSTER DESIGN:
    large vertical poster with a rich deep blue background, decorative hand-painted lettering, retro American school event aesthetic, playful typography, pastel pink lettering, mint green lettering, hand-drawn hearts, seaweed decorations, waves, underwater motifs, charming handmade appearance, authentic 1950s school art project
    
    MAIN TITLE:
    "SATURDAY NIGHT DANCE"
    
    EVENT NAME:
    Word "ENCHANTMENT" must be spelled exactly: E N C H A N T M E N T
    
    TAGLINE:
    "BE THERE OR BE SQUARE!"
    
    TEXT STYLE:
    all text is large, readable, centered, beautifully hand-painted, vintage sign-painter lettering, highly accurate typography
    
    COLOR PALETTE:
    deep ocean blue, turquoise, mint green, coral pink, cream highlights
    
    STYLE:
    1950s Americana, nostalgic, handmade poster art, school dance promotion, iconic retro design, graphic design masterpiece
    
    QUALITY:
    perfect typography, flawless text rendering, poster design focus, graphic design showcase


A word of caution: you can run into issues rendering long or complex words in Qwen / Krea 2.  Yet, always keep in mind there's a way to fix it: no LoRAs, no pills, no diets but the *prompting finesse* way.

In the shown example to make Qwen / Krea 2 render **enchantment** properly all I had to do is to be **more specific**.

The original prompt was:

    EVENT NAME:
    Word "ENCHANTMENT"

The fix was:

    EVENT NAME:
    Word "ENCHANTMENT" must be spelled exactly: E N C H A N T M E N T

And voilá.  A proper beautiful full sized graphic in seconds.  **The absolute terror of every graphic designer**.

> As a side note, if I were to add "The" before "word"  ("*The word*..."), Qwen / Krea 2 would understand that I want "The" to be *announcing*/*presenting* the word **ENCHANTMENT**: that will render "The" above "**ENCHANTMENT**".  Go ahead and try both options, with and without "The" before "word" and you will notice it.

👆 [Back to index](#the-big-old-index)
<br><br>
### NSWF VS SFW

You need to render a character wearing a shirt. But even as you prompt the word "shirt" like a lunatic, the character appears time and time again with the shirt up, exposing chest. Why? Models learn the human body as a base structure, and clothing is learned as an additional layer. **When context is ambiguous, the model may fall back to the underlying anatomical prior**. **The base model, the layer is essentially a nude body**.

This is not about Qwen so much as the NSFW model: a SFW model should avoid that from the base.

"But my NSFW renders dressed people to me--", yeah, I get it. **Characters are not drawn nude by default, yet they do when the scene context plays in.** Some prompts tend to sex and nudity to a point that getting people naked is the obvious so in fact dressing them is the contrary. Creating context to fight the context in a way.

If I'm placing a man and a woman on a bed, and the action involves *caressing*, *kissing*, with an i*ntimate environment *and* low-key dim warm lighting* with a *fireplace* on the back chances are Qwen/Krea 2 will interpret that as a *clothes off scenario*. There's a sexualized context going on and will try to get one or both characters as dispossessed of clothing as possible. If clothes are not specified or the clothing is easily/highly removable (boxers, bras, bikini bottom,...) Qwen/Krea 2 go for it: I know I would ;)

Since prompting is everything, go for something like ***fully** wearing a XXX*. Stressing the action will create a stronger embed.

- **fully** wearing the t-shirt and the pants
- **fully** wearing a buttoned work shirt
- **fully** wearing bra and bottom underwear
- etc.

Same goes for the negative prompting (below). Go for the detail about the clothing, the room, the face, the body, the light, camera, everything.

**Detail** -if coherent- **is what Qwen and Krea 2 feed about**.

👆 [Back to index](#the-big-old-index)
<br><br>
### MELTING FACES, WATER DROPLETS AND ARTIFACTS
Are you getting renders with faces melting like wax? Or excesive, unsolicited water droplets like the characters are sweating as a fake witness would do?

**This isn’t caused by INT8, FP8, or the VAE.** The water droplets/melted skin artifacts come from the checkpoint’s own wet‑skin prior, which is common in **models trained on glamour or NSFW datasets**.

Qwen isn’t misinterpreting embeddings — it simply triggers that prior when the prompt includes ambiguous wording or lighting conditions the model statistically associates with moisture.

**Check your prompt for terms that activate specular highlights, or reinforce the opposite:** *matte skin*, *dry skin*, *no water droplets*, *studio lighting*. "Wet <female sexual organ>", "semen" or even "saliva" can surely trigger it, even if they're perfectly prompted and situated into the scene context.

*Let the prompt undo what the prompt has done* I always say 😁

A very personal note on the matter:<br>
I have experienced this while working on prompts near/on water, even if there's a "pool in the background".  For a reason, characters appear with a wet skin (not so much melting faces).  I have produced renders with visible clear wet skin that seem to get triggered by prompting the word "kissing", in the contextual understanding there's saliva involved in a kiss.  Not all models react the same, but I have seen this as a generality in all tested Krea 2 models.  Perhaps is the way the models are trained (they mostly all use the same base training models after all), so there's a chance this is an embedding being taken out of proportion or misunderstood that is just getting copied from model to model.  Maybe is Qwen creating the wrong embeddings under certain scenarios?


👆 [Back to index](#the-big-old-index)
<br><br>
### DON'T BE SO NEGATIVE: TRY TO SEE THE LIGHT


The negative prompt, never understood, never appreciated and never used correctly has a purpose in our scenario since fighting difussion model's vices can be a tricky work.

Do not overuse the negative prompt but **use it**.

    avoid brunette hair

Yes, it will tell the model not to create characters with brunette hair.  Useful negative if you're randomizing hair color in your prompt.

If you're looking to create scenarios in low ISO/darkness, perhaps this would reinforce the positive lighting prompt:

    avoid flash photography.  avoid cold white lighting.  avoid studio lighting.
    

It won't completely eliminate artificial or extra unrequested lightning -very model dependant-, but it will surely tell the model to behave.

And **yes**, **use `avoid` inside the negative clip**, it is not a double negation but a **reinforced semantic distance**. **A double negative creates a stronger semantic push away from the concept**, the same way `fully wearing` emphasizes the *be* wearing something.

As for the **lighting** issue itself, it's quite the **nightmare** if you want to work with nuances on **dark/low light/moody** scenarios. **Daylight scenarios are fairly easy, low-ISO's are a true challenge.**

> Guide will be expanded to add advanced lighting setups.

Simple prompting for high-ISO (daylight) scenarios can be:

| Style / Concept | Lighting Configuration (Prompt) |
| :--- | :--- |
| **Bright Window Diffusion** | `LIGHTING: Bright daytime interior flooded with soft natural daylight streaming through large window sheer curtains. Clean high-key ambient illumination with gentle highlight falloff, open shadow details, balanced high-ISO film grain, and crisp true-to-life skin tones without harsh sunlight.` |
| **Direct Sunlight & Hard Contrast** | `LIGHTING: Intense direct midday sunlight pouring into the room, creating strong geometric light patches and high-contrast dark shadows. Vibrant exposure with natural lens flare, crisp highlights, visible ambient ISO grain, and dramatic warm light patterns across the subject.` |
| **Overcast Soft Box Effect** | `LIGHTING: Bright daylight indoor scene lit by soft diffuse skylight from an overcast afternoon. Evenly distributed natural ambient light eliminating harsh shadows, smooth skin texture highlights, neutral color temperature, and clean photo-journalistic grain.` |
| **Golden Hour Golden Glow** | `LIGHTING: Late afternoon golden hour natural light coming through side windows at a low angle. Warm amber directional illumination filling the room with a rich golden glow, soft elongated shadows, high-ISO ambient texture, and vivid natural contrast.` |

If it's low-ISO shots we're talking about, then game changes.

Positive `Lighting` embeds alone cannot always succeed.  Not all models are equally compliant with the prompt and those who do require very specific words.

To understand how sensitive `Lighting` prompting gets, imagine that if you turn the camera (modifying `Camera` prompt module) from the front to the side of a character, you also need to adjust the `Lighting` prompt if you're to maintain the same light effect.  It can get tricky for specific lighting scenarios, but going back to the basics always gets the job done.

Some examples for **low-ISO & ambient night lighting** prompting:

| Estilo / Concepto | Configuración de Iluminación (Prompt) |
| :--- | :--- |
| **Corner Warmth & Soft Shadows**| `LIGHTING: Nighttime interior with no camera flash. The room is illuminated entirely by a single floor lamp with a fabric shade in the corner, casting a soft orange warm glow across the walls. Deep natural shadows in the unlit areas, with soft warm light gently falling on one side of her face and sweater. Authentic high-ISO low-light photography atmosphere with clean natural shadow falloff.` |
| **Cozy Tungsten Ambience**| `LIGHTING: Dim night lighting with zero artificial flash. Ambient evening warm light originating from two muted table lamps and a floor lamp scattered in the background. The center of the room relies purely on this indirect tungsten illumination, creating soft highlights, rich dark corners, visible fine noise in the shadows, and a cozy warm-toned nighttime aesthetic.` |
| **Off-Frame Amber Directional**| `LIGHTING: Available-light night indoor photo, shot strictly without flash. Soft directional amber lighting from a warm floor lamp situated just outside the camera frame. Strong contrast between gentle warm highlights on the subject and deep atmospheric shadows filling the rest of the dark living room.` |
| **Moody Backlit Silhouette** | `LIGHTING: A dark room lit solely from behind by a warm ceiling lamp, creating a strong backlight rim effect. The faces are in deep shadow and partially underexposed, with no fill light or camera flash. The background behind has a soft warm glow while her silhouette and profile remain mostly dark and moody, ambient high-ISO grain.` |

Different models will produce different outputs: there's no single, universal way to interpret lighting and Krea 2 models do not hide it.

**Be aware all Krea 2 models handle lighting differently**.  Sometimes a specific lighting shot can only be achieved with a specific model.

All models **look the same, but they are not.**

👆 [Back to index](#the-big-old-index)
<br><br>
### CUT!
Prompting for camera angles proves challening on any difussion model, also for the reason on how they have been trained, but **with precise prompting you can achieve practically any shot you can think of**.

> This section will be expanded eventually; for the time being some examples are provided for simple camera prompting.

Start with simple camera prompting, as natural as you can:

`CAMERA: Wide shot, full-body`,<br>
`CAMERA: Low angle shot`,<br>
`CAMERA: Cell phone amateur tilted shot, blurred background`<br>
etc.

You also can (and should) mention the subject's into the camera's prompt:

A `tight focus on facial features` prompt is not the same as `tight focus on the hand of the female` if your're rendering a macro close-up.  If there're two people on the scene, the first prompt will focus on both (since facial features **apply to both**), not producing a true macro close-up, while the second will focus on the hand of the female: **specific character, specific body part**.

Prompting for the render to stylize with specific lenses or filters does work and make wonders.  Examples will be added latter on, this guide is on-going.

For the time being, explore with these six camera examples:

**Full Body & Environmental**

| Angle / Plane | Prompt |
| :--- | :--- |
| **High Angle Wide Shot** | `CAMERA: Shot from a high angle looking down at a steep downward tilt, wide environmental 24mm lens perspective, full-length shot capturing the subject's entire body from head to toe, wide framing with generous headroom and visible floor space.` |
| **Low Angle Wide Shot** | `CAMERA: Extreme low-angle shot positioned inches above floor level looking upward, wide-angle 28mm camera focal length, environmental full-body framing showing the complete figure against the room's height.` |
| **Distant Eye-Level Shot** | `CAMERA: Wide full-body shot taken from across the room at eye level, 35mm lens, subject is framed completely within the surrounding environment with ample distance between camera and subject.` |

**Close-ups & Portraits**

| Angle / Plane | Prompt |
| :--- | :--- |
| **Candid Smartphone Close-Up** | `CAMERA: Tight close-up portrait taken with a mobile phone main camera, shot slightly above eye level, intimate close framing focusing tightly on the face and shoulders, shallow depth of field with natural background separation.` |
| **Over-the-Shoulder / Dutch Angle** | `CAMERA: Medium close-up shot over the shoulder, camera positioned at a subtle Dutch angle (tilted frame), 50mm portrait lens perspective, tight framing from the chest up.` |
| **Extreme Detail Close-Up** | `CAMERA: Macro close-up shot, 85mm lens perspective, tight focus on facial features and eyes with the rest of the scene falling into a heavy blurred bokeh.` |

It's suggested to use IA (Gemini, Copilot, ChatGPT, etc) to assist in creating prompts to render **very specific camera angles** views.<br><br>
👆 [Back to index](#the-big-old-index)
<br><br>
### *NUTSHELLING* IT

Doing brain surgery with a jackhammer: SDXL felt like that.
Qwen/Krea 2 are closer to Star Wars: ahead of its time and to succeed just make sure the two proton torpedoes go into the two meters wide thermal exhaust port located in the station’s meridian trench. Remember: **detail**.
<br><br>
👆 [Back to index](#the-big-old-index)

### FOOT NOTES / Q&A / RECOMMENDED MODELS

**What Krea 2 models deliver what they promise?**<br>
Haven't tested that many, but these ones I found to be **superb realistic models** and amazing at creating full graphics:

| Model | Official Source | Mirrors | Recommendation |
| :--- | :--- | :--- | :--- |
| **GonzaLomo v4.0** | [🌐 Go (Civitai)](https://civitai.com/models/286469) | [🌐 Go (HF Mirror)](https://huggingface.co/Quiho/GonzaLomo_Krea_2_v4.0_checkpoint) | High character consistency & anatomical control; ideal for precise positioning. |
| **CyberRealistic Krea 2 v2.0** | [🌐 Go (Cyberdelia)](https://cyberdelia.nl) • [🌐 Go (HF Repo)](https://huggingface.co/cyberdelia/CyberRealistic) • [🌐 Go (Civitai)](https://civitai.red/models/2831028/cyberrealistic-krea-2) | N/A | Best for photorealism, skin textures, and natural lighting without heavy stylized artifacts. |
| **Moody Krea 2 Mix v7.0** | [🌐 Go (Civitai)](https://civitai.red/models/2731187/moody-krea-2-mix-uncensored-weekend-3days-blue-buzz-purchase-3buzz?modelVersionId=3209007) | [🌐 Go (HF FP8)](https://huggingface.co/EllaPriest45/Krea2_Checkpoints/blob/main/Moody%20Krea%202%20Mix%20(uncensored)%20FP8%20-%20Krea2.safetensors) | Great for cinematic lighting, atmospheric/darker aesthetics, and unconstrained composition. |
| **FinePorn v4.0** | [🌐 Go (Civitai)](https://civitai.red/models/2762538/fineporn-v4-int8-or-nvfp4-or-bf16-or-fp8) | [🌐 Go (HF INT8)](https://huggingface.co/EllaPriest45/Krea2_Checkpoints/blob/main/FinePorn%20v4.0%20Turbo%20INT8%20-%20Krea2%20-%20this%20is%20an%20amateur%20photo%20taken%20from%20smartphone%2Cbad%20quality%20photo%2CThe%20lighting%20is%20clear%2Csoft%20diffused%2Cthe%20details%20are%20sharp%2Cand%20the%20feeling%20of%20the%20photo%20is%20casual%20and%20spontaneous.safetensors) | Specialized in explicit anatomy accuracy and raw amateur/smartphone aesthetic prompts. |
| **Sick Ollie v1.0** | [🌐 Go (Civitai)](https://civitai.red/models/2676616/sick-ollie) | [🌐 Go (HF Repo)](https://huggingface.co/EllaPriest45/Krea2_Checkpoints) | Strong prompt adherence for unconventional poses and stylization with strict compliance. |


> A word of warning: NSFW models **are** able to produce **illegal content**, thus communities like CivitAI are overrun by the kind of people who look for that kind of tools to produce that kind of content.  Avoid engaging, commenting and, above all, providing information on what you do, social networks, etc.  **A lot of Jeffrey Epsteins**, stalkers and twisted deviants are anxiously waiting for your data.  Dangers of IA are very real when in the wrong hands.  Exercise **caution** and don't engage.

**"*What version should I download?*"**<br>
Here's a simple Krea 2 quantization guide by popular GPU model:

| Quantization | Approx. Size (Checkpoint) | Min. Recommended VRAM | Suggested GPUs (NVIDIA / AMD) | Quality vs. Speed Impact |
| :--- | :--- | :--- | :--- | :--- |
| **BF16** (Original) | ~24.5 GB | **24 GB - 48 GB+** | RTX 3090, RTX 4090, RTX 5090, A100, RTX 6000 Ada | **Maximum (100%)**. Lossless quality. May require VRAM offloading on 24GB GPUs due to the Text Encoder. |
| **INT8** (ConvRot) | ~12.6 GB | **12 GB - 16 GB** | RTX 3060 (12GB), RTX 4070 (12/16GB), RTX 4080 (16GB), RX 6800 / 7800 XT | **Excellent**. Virtually imperceptible quality loss and optimal generation speeds on Ampere/Ada architectures. |
| **MXFP8 / FP8** | ~12.2 GB - 12.6 GB | **12 GB - 16 GB** | RTX 3080 (12GB), RTX 4070 Ti, RTX 4080, RX 7900 GRE | **Very Good**. The standard balanced format, though INT8 often performs slightly faster in render times. |
| **NF4 / NVFP4** | ~7.1 GB | **6 GB - 8 GB** | RTX 2060 (6GB), RTX 3060 Ti (8GB), RTX 4060 (8GB), RX 6600 / 7600 | **Acceptable**. Minor loss in fine details and text rendering, but enables execution on entry-level GPUs. |

> **Note:** Running Krea 2 also requires loading an additional Text Encoder (such as *Qwen3-VL-4B*) and the VAE. If you are using an **8 GB VRAM** card, it is recommended to generate at base resolutions like 1024x1024 or 1536x1536 before upscaling.


**Comply with the law**<br>
If you're working on **commercial productions**, keep in mind that while software like ComfyUI is free, the models are not automatically free for commercial use. Yes, they may be free to download and run, but that doesn't mean you can sell the outputs. **Each model has its own license and copyright terms**, so technically speaking, **you can't commercialize your renders without checking first**. Always read the creator notes and model cards before downloading to understand the license terms, crediting requirements, or royalty obligations for commercial work.

**"*But my workflow is smart*"**<br>
Prompt enhancer, LLMs and the wonders of filtering the prompt by a light IA.
My opinion: If I'm to direct the orquestra remove middle management from it. I want to hear and control the instruments. These tips are meant for the **purist prompter**.

**LoRAs**<br>
Yeah, I don't use them. 

**Updates**<br>
On it. Will be adding tips and try to improve format.  I find markdown very unfriendly but hey, life is imperfect.

**Thanks to**<br>
GitHub for being weirdly awesome, and not banning me after so many consecutive edits.<br>
To all the people behind ComfyUI, model creators and coders that make the true miracle.
<br>
-- Gershu / gershu.ar<br><br>
👆 [Back to index](#the-big-old-index)
<br>
End of guide.<br><br>
[![CC BY 4.0](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)  
This document is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
