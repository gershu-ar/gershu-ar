# 🎨 Qwen / Krea 2 on ComfyUI
### *The Totally Unrequested Prompting Guide*
---
**Working with NSFW & SFW models**  
*For the Purist Prompter*  

This guide was 97% human-written by a non-English language native, with 3% assistance from *Google Gemini 3.6-flash*—mostly for Markdown formatting.

🔞 `RATED R` — *Contains strong language*

[![](https://github.com/gershu-ar/gershu-ar/blob/1a76545fc4263706b4555df94f6163861354c7b3/articles/img/cover_qwen_krea2.jpg)](https://github.com/gershu-ar/gershu-ar/blob/1a76545fc4263706b4555df94f6163861354c7b3/articles/img/cover_qwen_krea2.jpg)

Hastily generated using Krea 2 NSFW model [Sick Ollie](https://civitai.red/models/2676616/sick-ollie "Sick Ollie") on [ComfyUI](https://github.com/comfy-org/ComfyUI "ComfyUI") portable v0.36.0

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
Congratulations, you're all set for BOOTCAMP.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
## Bootcamp

Welcome! 

My **tool of choice** was, is and I think it will always be [ComfyUI portable](https://github.com/Comfy-Org/ComfyUI/) (v0.36.0 as of SEP/2026) using a Qwen3-VL-4B FP8 Scaled *Text Encoder* and the Qwen *Image VAE*.

On rendered examples I have used several models (Gonzalomo mostly, at the end I share links), running on local setup with a RTX 3090 (24 GB VRAM) so I could go for `bf16` quantization but  `int8` simply **flies** on Ampere, leaving a massive overhead of free VRAM for the Text Encoder -namely, Qwen- and whatever else I require.  Output quality is perfect for the task I require.

> `bf16`, `int8`, `nf4`... at the very end this guide the quantization appropiate model for your GPU is presented.

For your convenience [you can find here](https://github.com/gershu-ar/gershu-ar/blob/main/articles/workflows/Purist%20Krea%202.json "you can get here") a **simple**, **purist Qwen / Krea 2 ComfyUI workflow** in case you feel like testing prompts along (hit on the three dots and select _Download_, then open in ComfyUI).  The workflow has been *un-Qwened* (removed the LLM prompt enhancement and automatic negative creation) and *un-LoRAed* (no nodes for them), just two text boxes (positive and negative) and a few settings for model, CFG, steps and the-like.

***NSFW .... wait...? Am I here to learn how to create porn?***

| No  | Yes |
| ------------ | ------------ |
| Don't be confused: NSFW models render perfectly dressed scenarios and I consider them excellent at rendering dressed people precisely because clothing is not enforced by the model and I get to choose what and how clothing is being worn.  On SFW models there's the possibility of ruining certaing clothing or even a pose on the account of avoiding showing nudity or the posibility of nudity.  | If you want to, you can.  |

**This guide is about Qwen / Krea 2 prompting engineering**, *not for determining the use you give to these tools*.  That, amigo, is entirely up to **you**.

> Only one strong request: **Kindly keep children out and away its use**.  Do not use these tools to generate content of people under 18 years old in sexual or suggestive situations.  Neither allow people under 18 years old to access these tools.

Enough introduction.  Let's start.

#### **QWEN/KREA 2 vs SDXL**
Qwen and Krea 2 do not fill in the empty spaces.  In Qwen no prompt, no tokenization, no embed.   If you have not described it, it will not be there.  Mostly.  Gaps will be completed to round up a context and provide a presentable product, but remove from your head the idea of the model doing what you don't want to.  Qwen and Krea 2 require rich prompts to properly generate specific rich renders and oh boy if they deliver!

You can throw simple prompts and Qwen/Krea 2 will provide, just don't confuse it with specific shots, where light, angle, character description and such play a role, specially if you're under the idea of continuity in between frames (story telling, video generation).

As for SDXL's CLIP **ViT-L** it was created to be **light**, **tolerant**, and to **guess**, not to follow strict orders.  SDXL does an amazing job at being casual, but you will pay the price in Qwen/Krea 2 if your mindset is along that pipeline.

Extensive detailed prompts in SDXL require a lot of external nodes to position region, render faces properly, have body proportions as desired on that dreamed setting that randomly appeared without a true control.

If you're coming from SDXL you will notice that **Qwen/Krea 2 is a whole new ball game in prompting**. Takes a few days but the results are exponentially better, **Krea 2 has a lot of potential**.

Once you try Qwen and Krea 2 you will not go back to SDXL.  Guaranteed.

**Keep in mind these tips are not noticeable on small prompts but the richer the prompt gets, the more characters are part of it, that's when word precision becomes a must.**

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### **PROMPTING ON QWEN**
##### Structure, structure, structure

Qwen interprets and generates embeddings in natural language.  The token limitation and fractionalization of SDXL is not present here: every word counts, every sentence has a meaning and *poetry* is not corny but what makes a fantastic output.

A suggested prompting technique involves** separating the prompt in categories**, most specially for complex scenes:

    Subject details: Uniquely name and describe each character on the scene.
    
    Attire context: Using the names you defined above, describe how are they dressing.
    
    Action: Describe the actions your characters are doing, calling them by their given name.
    
    Camera: As name suggests, this is where your camera stands.  Hard to master.
    
    Environment: Where are the characters in? Describe it as detailed as possible.  Time of day, objects around, furniture, background, setting.
    
    Lighting: Same as camera, the lighting prompting is hard to master (models force light by default and prompt needs to tackle it wisely).
    
    Textures: Skins, fabrics, liquids, etc.

**EXAMPLE**<br>
Setting up a simple a scene:

    Subject details: A man, 45-years-old middle-aged, unfit, ugly. A dog, brown.
    
    Attire context:  Man is wearing sports pants, a t-shirt and sneakers. Dog is wearing an orange leash.
    
    Action: Man is walking Dog on the pathway. Dog is sniffing on another person.
    
    Camera: Wide full body shot taken from across the room at eye level, 35mm lens, Man is framed completely within the surrounding environment with ample distance between camera and subject.
    
    Environment: At a city park on a pathway.
    
    Lighting: Natural afternoon light, golden hour.
    
    Textures: Realistic skin.

Ouput:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/878310eda7fcc06262d6c2c7bd3a1530.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/878310eda7fcc06262d6c2c7bd3a1530.png)


**EXAMPLE**<br>
Slightly modifying the same scene by adding "Dog is setting paws on Man" (only changing the dog action), rest remains the same:

    Action: Man is walking Dog on the pathway. Dog is setting paws on Man.
    
Output:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)

**Qwen/Krea 2 did what I ask of them.**

After contextualizing the scene by creating the embeds, it filled the gaps and presented the intention with accuracy, even repeating the same background and scenery. **It prioritized the big picture over small detail**, and this is where we need to come in.

As you see on the examples both renders look **almost the same**. Liberties are visible. Since I did not specify about sneakers' colors, they're different. Grey for the t-shirt?, yes, but different shades of grays. The leash just "orange" is not detailed enough, needs refinement. Pants are not the same.

Also pay attention how it ignores *ugly* and *fit*. That dude does not stand out: his human face is what is expected to be. See, *Ugly* by itself has no meaning, it needs to be described:* long nose, deformed cheeks*, whatever. Same for the *unfit*. What is *unfit*? Unless you describe it, *unfit* is a shallow concept free of meaning. *Unfit* can refer to several things not just to a body type description.

*Ugly*, *unfit*, ... like using *relaxed*. There's no universal explanation for what *relaxed* is supposed to be in any given situation unless there's a context to it.  If you want somebody with a natural pose and "relaxed", do so by exactly prompting it as such: "The person has a naturally distended body pose, being at ease, resting shoulders and arms, and a face expression of tranquility and calmness".

Oh, Qwen, you cheeky, adorable bastard.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### One -or a few- word to rule them all
So, to recap: "**Woman standing with a bath robe**" will produce a woman with an open bath robe showing the underneath nude body. **Why is she nude**? Because the **default statistical prior** of a robe is not to be closed/buttoned. 

"**Long pants are seen over the floor**" will render a pair of perfectly positioned pants. **Why**? Because that's the** default statistical prior of the pants**: straight, perfectly in order, shape and position for the camera, as if presented for sale or in a drawer. *The perfect image of the pants* vs the image of realistic the pants.

**Be detailed**. Qwen is expecting the *closed* for the bath robe and *messy* scenario for the pants for a realistic output.  For Qwen that distinction is everything and so is for you: the pants, or robe or whatever you're prompting, will maintain coherence in between renders.

**Beware: *a single misplaced word can ruin your entire scene*.**

"Wearing summer clothes" might give you somebody on a bathing suit, but chances are you will get a naked person. **Why**? Because "summer clothes" is not an item, is a **group of items**, a generic term Qwen does not properly understand.  Maybe if there's a "beach" or a "summer" word on the rest of the prompt you might get somebody wearing a bathing short because there is context but avoid it: describe the bathing suit: "Bikini top and bikini bottom" are not the same as just "Bikini". "Bikini" alone refers to the bottom, not to a bikini set (bottom and top).

And be descriptive about the color of the bikini too, Krea 2 will try to mantain the same bikini style too for coherence, even if you don't specify it.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### REMEMBER

You're not in Kansas anymore: **This is not SDXL.**

Using `(pants:1.5)` won't get you anywhere, there's no embed weighing on Qwen. Every word is as important as the rest, the weighing is done by Krea 2 once it receives the embeddings.  The more an embedding is repeated and playing a role on the context, the more it will comply to what is ask from it... as long as you don't over confuse it.

Thus, **a single word has Qwen-sequences and it can ruin everything.**

Same goes for LoRAs or whatever embeddings you used or want to use:

`<LORA:XXX bla bla>`

Don't.  There is not even..

`// Hey, this is a comment`

... on Qwen.

SDXL used to ignore `// COMMENT` but Qwen will give it some interpretation based on context and create an embedding **confusing the model**.  



👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### EVERY GREAT LIE HAS A LOT OF DETAIL

You prompt for "**Pants**".

**Qwen/Krea 2 have mercy**: No item color specified? Pants colors will randomly change... or not: if the prompt has been repeated long enough along the +1 seed trend, modifying the scene (and as long as you don't mess with the pants), will maintain the random color it adopted by its own the first time.

This is playing with an uncertainty and a false security: the pants color will change randomly on every X render only to return to the series predominant color. But avoid that, use: "color pants" (color=black, white, green, etc). That will guarantee the exact same pants on each frame.

Extend it even more for detail and coherence: "Black sports pants with laces with a white tip". You will be getting the right color, type (pants), subtype (sports) and details (laces with a white tip).

**Details allow the repeating results effortlessly and provide the base for any realistic render:** **detail**. Life has detail, clothing fabric is not perfect, light is always terrible and shit happens. Models are trained on the basis of perfection, so Qwen is waiting to encode the terrible reality of life and Krea 2 to show it: don't be afraid to add extra details about a character's background or their intention within the context of the scene.

If your character is about to jump off a plane, add to the context: "The character is jumping off a plane for the first time in his life and is visibly mildly scared". Besides jumping off a plane, a sense of wonder, fear, curiosity will be added to the character's posture and even face expression if you haven't done so by prompting.  Qwen/Krea 2 won't be filling a blank but adapting to the situation.

Keep in mind the software you're using and the cache's circuit/purges. Instant results might not be visible from one render to the other: it might take 5/6 renders to start seeing results and a settled prompt into the cache (at least on ComfyUI). The more renders you run, the more you will see how the produced render takes the exact shape as you prompted it.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### MARK TWAIN YOURSELF THRU QWEN
One thing Qwen and Krea 2 are **EXCELLENT** at is text. **Wildly good at**.

> In between getting proper rendered text while working with SDXL and winning the lottery, winning the lottery is a sure thing.

So, if we implement two simple modifications to *action* from the example:

    Action: Man is holding a professionally designed sign that reads "I love dogs and I'm a drunk".  Dog is standing next to Man staring at Man.  Dog has a small sign hanging from the beck that reads: "This cat is crazy".

**Produces exactly that**. The more you describe the medium (paper, sign, wall advertising, graph, etc.) the more realistic it will be:

[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)

Again: *being drunk* is super general, not everybody looks or behaves the same when drunk. In this case, Qwen/Krea 2 did their best to represent the scenario. If I were to add "Red face, glassy eyes, sloppy grin, vacant stare" the result would surely differ. Dog, on the other hand, strangely looks like its smiling, doesn't it?

👆 [Back to index](#the-big-old-ass-index)
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

👆 [Back to index](#the-big-old-ass-index)
<br><br>
### MELTING FACES, WATER DROPLETS AND ARTIFACTS
Are you getting renders with faces melting like wax? Or excesive, unsolicited water droplets like the characters are sweating as a fake witness would do?

**This isn’t caused by INT8, FP8, or the VAE.** The water droplets/melted skin artifacts come from the checkpoint’s own wet‑skin prior, which is common in **models trained on glamour or NSFW datasets**.

Qwen isn’t misinterpreting embeddings — it simply triggers that prior when the prompt includes ambiguous wording or lighting conditions the model statistically associates with moisture.

**Check your prompt for terms that activate specular highlights, or reinforce the opposite:** *matte skin*, *dry skin*, *no water droplets*, *studio lighting*. "Wet <female sexual organ>", "semen" or even "saliva" can surely trigger it, even if they're perfectly prompted and situated into the scene context.

*Let the prompt undo what the prompt has done* I always say 😁

👆 [Back to index](#the-big-old-ass-index)
<br><br>
### DON'T BE SO NEGATIVE: TRY TO SEE THE LIGHT

In ComfyUI's official Krea 2 t2i template the negative is automatically created based on the positive by a stranger process my brain was unable to decode and/or I simply don't care to find about. I didn't like this, so I separated the negative (clip text) to have full control. Do not overuse the negative but use it. Start with a base negative like:

    avoid flash photography. avoid cold white lighting.  avoid studio lighting.
    
**Yes**, **use "avoid" inside the negative clip**, it is not a double negation but a **reinforced semantic distance**.** A double negative creates a stronger semantic push away from the concept**, the same way "**fully** wearing" emphasizes the be wearing something.

A generalist negative prompt to get realistic, amateur looking renders, although diffusion models do not always make it easy when it comes to the lighting section. This happens on SDXL, Krea 2 and every other single encoder out there. LoRAs or no LoRAs (tried all options).

As with the nudes, this is also related on how models are trained: **the base pictures use maximum efficiency studio lightning to get 100% out the photographed character**. **It's only natural wanting to go back to the source material**, those inputs on which that model was trained on (this is why most models share the same faces/look-a-likes too).

As for the **lighting** issue itself, it's quite the **nightmare** if you want to work with nuances on dark/low light/moody scenarios. **Daylight scenarios are fairly easy, low-ISO's are a true challenge.**

I won't go deeper into lighting cause it's extremely complicated to prompt properly -for me at least- on the account other factors are taken into consideration by the models when receiving the embed; lighting embeds alone cannot always succeed. Prompting a camera angle can turn a useless lighting prompt into a masterpiece over a frame: models in general are very uncompliant to lighting on every shape, color and flavor.

**Be aware all Krea 2 models handle lighting differently**.  I always suggest trying several models until you find the one that fits the job as the main model while keeping the rest as secondary.  Sometimes a specific shot can only be achieved with a specific model.  You will get to know each model's cons and pros the more use you use them.

**Run the same prompt thru different models and compare results yourself. ** **Most, if not all, Krea 2 models are excellent** since they mostly use the same training material, only they adapt lighting and other elements to produce a custom styled product determined by each modeler.  **They all look the same, but they are not.**

👆 [Back to index](#the-big-old-ass-index)
<br><br>
### CUT!
[The camera section requires an article on its own, really hard to master and context dependent if you're looking for specific shots, but I will be adding some general prompting tips for simple camera setups later on - Pending]

👆 [Back to index](#the-big-old-ass-index)
<br><br>
### *NUTSHELLING* IT

Doing brain surgery with a jackhammer: SDXL felt like that.
Qwen/Krea 2 are closer to Star Wars: ahead of its time and to succeed just make sure the two proton torpedoes go into the two meters wide thermal exhaust port located in the station’s meridian trench. Remember: **detail**.
<br><br>
👆 [Back to index](#the-big-old-ass-index)

### FOOT NOTES / Q&A / RECOMMENDED MODELS

**What Krea 2 models work as they should?**<br>
Haven't tested that many, but these ones I found to be superb realistic models - in no order of preference:
- [Moody Krea 2 Mix](https://civitai.red/models/2731187/weekend-3days-blue-buzz-purchase-3buzz-moody-krea-2-mix-uncensored?modelVersionId=3209007)
- [CyberRealistic](https://civitai.red/models/2831028/cyberrealistic-krea-2)
- [GonzaLomo](https://civitai.red/models/2761943/gonzalomo-krea-2)
- [FinePorn](https://civitai.red/models/2762538/fineporn-v4-int8-or-nvfp4-or-bf16-or-fp8)
- [Sick Ollie](https://civitai.red/models/2676616/sick-ollie)

> A word of warning: Unchecked (uncensored) NSFW models **are** able to produce **illegal content**, thus communities like CivitAI are overrun by the kind of people who look for that kind of tools.  **A lot of Jeffrey Epsteins on CivitAI**.  Dangers of IA are very real, specially in the wrong hands.  **Use extreme caution when providing data such as creating a profile and/or sharing socials or even your own work**, it can all be reverse searched and you can easily fall victim to a stalker.  Avoid creating serious profiles and use the websites just to procure models, don't engage.

**"*What version should I download?*"**<br>
Here's a simple Krea 2 quantization guide by popular GPU model:

| Quantization | Approx. Size (Checkpoint) | Min. Recommended VRAM | Suggested GPUs (NVIDIA / AMD) | Quality vs. Speed Impact |
| :--- | :--- | :--- | :--- | :--- |
| **BF16** (Original) | ~24.5 GB | **24 GB - 48 GB+** | RTX 3090, RTX 4090, RTX 5090, A100, RTX 6000 Ada | **Maximum (100%)**. Lossless quality. May require VRAM offloading on 24GB GPUs due to the Text Encoder. |
| **INT8** (ConvRot) | ~12.6 GB | **12 GB - 16 GB** | RTX 3060 (12GB), RTX 4070 (12/16GB), RTX 4080 (16GB), RX 6800 / 7800 XT | **Excellent**. Virtually imperceptible quality loss and optimal generation speeds on Ampere/Ada architectures. |
| **MXFP8 / FP8** | ~12.2 GB - 12.6 GB | **12 GB - 16 GB** | RTX 3080 (12GB), RTX 4070 Ti, RTX 4080, RX 7900 GRE | **Very Good**. The standard balanced format, though INT8 often performs slightly faster in render times. |
| **NF4 / NVFP4** | ~7.1 GB | **6 GB - 8 GB** | RTX 2060 (6GB), RTX 3060 Ti (8GB), RTX 4060 (8GB), RX 6600 / 7600 | **Acceptable**. Minor loss in fine details and text rendering, but enables execution on entry-level GPUs. |

> **Note:** Running Krea 2 also requires loading an additional Text Encoder (such as *Qwen3-VL-4B*) and the VAE. If you are using an **8 GB VRAM** card, it is recommended to generate at base resolutions like 1024x1024 or 1536x1536 before upscaling.

**"*But my workflow is smart*"**<br>
Prompt enhancer, LLMs and the wonders of filtering the prompt by a light IA.
My opinion: If I'm to direct the orquestra remove middle management from it. I want to hear and control the instruments. These tips are meant for the **purist prompter**.

**LoRAs**<br>
Yeah, I don't use them. 

**Updates**<br>
On it. Will be adding tips and try to improve format.  I find markdown very unfriendly but hey, life is imperfect.

**Thanks to**<br>
GitHub for being weirdly awesome and not banning me after so many repetitive edits.  To all the people behind ComfyUI, model creators and coders that make the true miracle.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
End of guide.<br>
