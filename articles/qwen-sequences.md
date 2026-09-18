# Qwen/Krea 2 on ComfyUI
### The Totally Unrequested Prompting Tips - Rated R for language
#### For the purist prompter

[![](https://github.com/gershu-ar/gershu-ar/blob/1a76545fc4263706b4555df94f6163861354c7b3/articles/img/cover_qwen_krea2.jpg)](https://github.com/gershu-ar/gershu-ar/blob/1a76545fc4263706b4555df94f6163861354c7b3/articles/img/cover_qwen_krea2.jpg)

------------
## THE BIG OLD ~~ASS~~ INDEX
1. [The 101](#the-101)<br>
2. [Bootcamp](#bootcamp)<br>
2.1. [QWEN vs SDXL](#qwen-vs-sdxl)<br>
2.2. [Prompting on Qwen](#prompting-on-qwen)<br>
2.2.1 [Structure, structure, structure](#structure-structure-structure)<br>
2.2.2. [One -or a few- word to rule them all](#one--or-a-few--word-to-rule-them-all)<br>
2.3 [Remember](#remember)<br>
2.4. [Every great lie has a lot of detail](#every-great-lie-has-a-lot-of-detail)<br>
2.5. [Mark Twain yourself thru Qwen](#mark-twain-yourself-thru-qwen)<br>
2.6. [Melting faces, water droplets and artifacts](#melting-faces-water-droplets-and-artifacts)<br>
2.7. [Don'T be so Negative: try to see the Light](#dont-be-so-negative-try-to-see-the-light)<br>
2.8. [Cut!](#cut)<br>
2.9. [*Nutshelling* it](#nutshelling-it)<br>
2.10. [Foot notes](#foot-notes)

------------
Disclaimer: Yes, there are probably a ton of errors in this article.  Suggestions are always welcome, drop me a line!

------------

<br>

## The 101
Prompt → Qwen (encode) → Krea 2 (denoise) → VAE (decode)<br>
Congratulations, you're all set for BOOTCAMP.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
## Bootcamp
#### **QWEN VS SDXL**
Qwen does not fill in the empty spaces. If you have not described it, it will not be there. No prompt, no tokenization, no embed. As for SDXL's CLIP ViT-L it was created to be light, tolerant, and to guess, not to follow orders.

If you're coming from SDXL you will notice that Qwen/Krea 2 is a whole new ball game in prompting. Takes a few days but the results are exponentially better, Krea 2 has a lot of potential.

These tips are not noticeable on small prompts but the richer the prompt gets, the more characters are part of it, that's when word precision becomes a must.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### **PROMPTING ON QWEN**
##### Structure, structure, structure

A suggested prompting technique involves separating the prompt in categories, most specially for complex scenes:

    Subject details: Uniquely name and describe each character on the scene.
    
    Attire context: Using the names you defined above, describe how are they dressing.
    
    Action: Describe the actions your characters are doing, calling them by their given name.
    
    Camera: As name suggests, this is where your camera stands.  Hard to master.
    
    Environment: Where are the characters in? Describe it as detailed as possible.  Time of day, objects around, furniture, background, setting.
    
    Lighting: Same as camera, the lighting prompting is hard to master (models force light by default and prompt needs to tackle it wisely).
    
    Textures: Skins, fabrics, liquids, etc.

**EXAMPLE #1**<br>
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


**EXAMPLE #2**<br>
Slightly modifying the same scene by adding "Dog is setting paws on Man" (only changing the dog action), rest remains the same:

    Action: Man is walking Dog on the pathway. Dog is setting paws on Man.
    
Output:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/5f38dbf7652fb661decd3cef2d09fd50.png)


Qwen/Krea 2 did what I ask of them. After contextualizing the scene by creating the embeds, it filled the gaps and presented the intention with accuracy, even repeating the same background and scenery. It prioritized the big picture over small detail, and this is where we need to come in.

As you see on the examples both renders are almost the same. Liberties are visible. Since I did not specify about sneakers' colors, they're different. Grey for the t-shirt?, yes, but different shades of grays. The leash just "orange" is not detailed enough, needs refinement. Pants are not the name.

Also pay attention how it ignores ugly and fit. That dude does not stand out: his human face is what is expected to be. See, Ugly by itself has no meaning, it needs to be described: long nose, deformed cheeks, whatever. Same for the unfit. What is unfit? Unless you describe it, unfit is a shallow concept free of meaning. Unfit can refer to several things not just to a body type description.

Ugly, unfit, ... like using relaxed. There's no universal explanation for what relaxed is supposed to be in any given situation. If you want somebody with a natural pose and "relaxed", do so by exactly prompting it as such: "The person has a naturally distended body pose, being at ease, resting shoulders and arms, and a face expression of tranquility and calmness".

Oh, Qwen, you cheeky, adorable bastard.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### One -or a few- word to rule them all
So, to recap: "Woman standing with a bath robe" will produce a woman with an open bath robe showing the underneath nude body. Why is she nude? Because the default statistical prior of a robe is not to be closed/buttoned. 

"Long pants are seen over the floor" will render a pair of perfectly positioned pants. Why? Because that's the default statistical prior of the pants: straight, perfectly in order, shape and position for the camera, as if presented for sale or in a drawer. The perfect image of pants vs the image of realistic pants.


Be detailed. Qwen is expecting the closed for the bath robe and messy scenario for the pants for a realistic output. A woman wearing a closed bath robe or pants on the floor that somebody threw on the floor provides the extra detail. For Qwen that distinction is everything and so is for you: the pants, or robe or whatever you're prompting, will maintain coherence in between renders.

Beware: a single misplaced word can ruin your entire scene.

"Wearing summer clothes" might give you somebody on a bathing suit, but chances are you will get a naked person. Why? Because "summer clothes" is not an item, is a group of items, a generic term Qwen does not properly understand. Maybe if there's a "beach" or a "summer" word on the rest of the prompt you might get somebody wearing a bathing short because there is context but avoid it: describe the bathing suit: "Bikini top and bikini bottom" are not the same as just "Bikini". "Bikini" alone refers to the bottom, not to a bikini set (bottom and top).

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### REMEMBER

This is not SDXL:
using...

    (pants:1.5)

...won't get you anywhere, there's no embed weighing on Qwen. Every word is as important as the rest. What matters in Qwen is the entire context. Thus, a single word has Qwen-sequences and it can ruin everything.

Same goes for LoRAs or whatever embeddings you used or want to use:

`<LORA:XXX bla bla>`

Don't.  There is not even..

`// Hey, this is a comment`

... on Qwen.  SDXL used to ignore `// COMMENT`, Qwen will give it some interpretation based on context and create an embedding confusing the model.  



👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### EVERY GREAT LIE HAS A LOT OF DETAIL

You prompt for "Pants".

Qwen has mercy: No item color specified? Pants colors will randomly change... or not: if the prompt has been repeated long enough along the +1 seed trend, modifying the scene (and as long as you don't mess with the pants), will maintain the random color it adopted by its own the first time. This is playing with an uncertainty and a false security: the pants color will change randomly on every X render only to return to the series predominant color. But avoid that, use: "color pants" (color=black, white, green, etc). That will guarantee the exact same pants on each frame.

Extend it even more for detail and coherence: "Black sports pants with laces with a white tip". You will be getting the right color, type (pants), subtype (sports) and details (laces with a white tip).

Details allow the repeating results effortlessly and provide the base for any realistic render: detail. Life has detail, clothing fabric is not perfect, light is always terrible and shit happens. Models are trained on the basis of perfection, so Qwen is waiting to encode the terrible reality of life: don't be afraid to add extra details about a character's background or their intention within the context of the scene.

If your character is about to jump off a plane, add to the context: "The character is jumping off a plane for the first time in his life". Besides jumping off a plane, a sense of wonder, fear, curiosity will be added to the character's posture and even face expression if you haven't done so by prompting. Qwen won't be filling a blank but adapting to the situation.

Keep in mind the software you're using and the cache's circuit/purges. Instant results might not be visible from one render to the other: it might take 5/6 renders to start seeing results and a settled prompt into the cache (at least on ComfyUI). The more renders you run, the more you will see how the produced render takes the exact shape as you prompted it.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### MARK TWAIN YOURSELF THRU QWEN
One thing Qwen and Krea 2 are EXCELLENT at is text. Wildly good at. In between getting proper text while working with SDXL and winning the lottery, winning the lottery always has more chances.

Two simple modifications to action from the example:

    Action: Man is holding a professionally designed sign that reads "I love dogs and I'm a drunk".  Dog is standing next to Man staring at Man.  Dog has a small sign hanging from the beck that reads: "This cat is crazy".

Output:
[![](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)](https://github.com/gershu-ar/gershu-ar/blob/f38b44276cb19ce55d3e168fa9f2667890301ddc/articles/img/0bc8527e11b8cbf13a8c445e639a1d9e.png)

`Action: Man is holding a professionally designed sign that reads "I love dogs and I'm a drunk".  Dog is standing next to Man staring at Man.  Dog has a small sign hanging from the beck that reads: "This cat is crazy".`

Produces exactly that. The more you describe the medium (paper, sign, wall advertising, graph, etc.) the more realistic it will be:

The face and the shirt, yeah. I can smell it from here, amigo. Dog knows.

Again: being drunk is super general, not everybody looks or behaves the same when drunk. In this case, Qwen/Krea 2 did their best to represent the scenario. Man looks more sad than drunk, but if I were to add "Red face, glassy eyes, sloppy grin, vacant stare" the result would surely differ. Dog, on the other hand, strangely looks like its smiling, doesn't it?

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### NSWF VS SFW

You need to render a character wearing a shirt. But even as you prompt the word "shirt" like a lunatic, the character appears time and time again with the shirt up, exposing chest. Why? Models learn the human body as a base structure, and clothing is learned as an additional layer. When context is ambiguous, the model may fall back to the underlying anatomical prior. The base model, the layer is essentially a nude body.

This is not about Qwen so much as the model: a SFW model would avoid that from the base.

"But my NSFQ renders dressed people to me--", yeah, I get it. Characters are not drawn nude by default, yet they do when the scene context plays in. Some prompts tend to sex and nudity to a point that getting people naked is the obvious so in fact dressing them is the contrary. Creating context to fight the context in a way.

If I'm placing a man and a woman on a bed, and the action involves caressing, kissing, with an intimate environment and low-key dim warm lighting with a fireplace on the back chances are Qwen will interpret that as clothes off scenario. There's a sexualized context going on and will try to get one or both characters as dispossessed of clothing as possible. If clothes are not specified or the clothing is easily/highly removable (boxers, bras, bikini bottom,...) Qwen goes for it: I know I would ;)

Since prompting is everything, go for something like fully wearing a XXX. Stressing the action will create a stronger embed.

- fully wearing the t-shirt and the pants
- fully wearing a buttoned work shirt
- fully wearing bra and bottom underwear
- etc.

Same goes for the negative prompting (below). Go for the detail about the clothing, the room, the face, the body, the light, camera, everything.

Detail -if coherent- is what Qwen feeds about.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### MELTING FACES, WATER DROPLETS AND ARTIFACTS
Are you getting renders with faces melting like wax? Or excesive, unsolicited water droplets like the characters are sweating as a fake witness would do?

This isn’t caused by INT8, FP8, or the VAE. The water droplets/melted skin artifacts come from the checkpoint’s own wet‑skin prior, which is common in models trained on glamour or NSFW datasets.

Qwen isn’t misinterpreting embeddings — it simply triggers that prior when the prompt includes ambiguous wording or lighting conditions the model statistically associates with moisture.

Check your prompt for terms that activate specular highlights, or reinforce the opposite: matte skin, dry skin, no water droplets, studio lighting. "Wet <female sexual organ>", "semen" or even "saliva" can surely trigger it, even if they're perfectly prompted and situated into the scene context.

Let the prompt undo what the prompt has done I always say 😁

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### DON'T BE SO NEGATIVE: TRY TO SEE THE LIGHT

In ComfyUI's official Krea 2 t2i template the negative is automatically created based on the positive by a stranger process my brain was unable to decode and/or I simply don't care to find about. I didn't like this, so I separated the negative (clip text) to have full control. Do not overuse the negative but use it. Start with a base negative like:

    avoid flash photography. avoid cold white lighting.  avoid studio lighting.
    
Yes, use "avoid" inside the negative clip, it is not a double negation but a reinforced semantic distance. A double negative creates a stronger semantic push away from the concept, the same way "fully wearing" emphasizes the be wearing something.

A generalist negative prompt to get realistic, amateur looking renders, although diffusion models do not always make it easy when it comes to the lighting section. This happens on SDXL, Krea 2 and every other single encoder out there. LoRAs or no LoRAs (tried all options).

As with the nudes, this is also related on how models are trained: the base pictures use maximum efficiency studio lightning to get 100% out the photographed character. It's only natural wanting to go back to the source material, those inputs on which that model was trained on (this is why most models share the same faces/look-a-likes too).

As for the lighting issue itself, it's quite the nightmare if you want to work with nuances on dark/low light/moody scenarios. Daylight scenarios are easy, low-ISO's are a challenge. 

I won't go deeper into lighting cause it's extremely complicated to prompt properly -for me at least- on the account other factors are taken into consideration by the models when receiving the embed; lighting embeds alone cannot always succeed. Prompting a camera angle can turn a useless lighting prompt into a masterpiece over a frame: models in general are very uncompliant to lighting on every shape, color and flavor.

So -> Be aware some Krea 2 models are much better at handling lighting than others.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
#### CUT!
[The camera section requires an article on its own, really hard to master and context dependent if you're looking for specific shots, but I will be adding some general prompting tips for simple camera setups later on - Pending]

👆 [Back to index](#the-big-old-ass-index)
<br><br>
##### *NUTSHELLING* IT

Brain surgery with a jackhammer<br>
SDXL felt like that. Krea 2 is closer to Star Wars: ahead of its time and to succeed just make sure the two proton torpedoes go into the 2 meters wide thermal exhaust port located in the station’s meridian trench. Remember: detail.
<br><br>
👆 [Back to index](#the-big-old-ass-index)

#### FOOT NOTES
**"But my workflow is smart"**<br>
Yeah, prompt enhancer, LLMs and the wonders of filtering the prompt by a light IA.
My opinion: If I'm to direct the orquestra remove middle management from it. I want to hear and control the instruments. These tips are meant for the purist prompter.

**LoRAs**<br>
Yeah, I don't use them. 

**Updates**<br>
On it. Will be adding tips.

👆 [Back to index](#the-big-old-ass-index)
<br><br>
End of guide.
(C) gershu.ar
