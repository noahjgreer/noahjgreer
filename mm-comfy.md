# **MuffinMode**
[MuffinMode](http://muffinmode.net:8188) is a web server I run on my computer. :3 Its most common public use is for generating stable-diffusion images. It runs on fing called ComfyUI, which is a semi-user-friendly graphical user interface, which gives more control and faster generations compared to alternative generators.
## Index
- [**MuffinMode**](#muffinmode)
  - [Index](#index)
- [ComfyUI Image Generation](#comfyui-image-generation)
  - [Compared to Bing](#compared-to-bing)
    - [Pros:](#pros)
    - [Cons:](#cons)
  - [Notes and FAQ](#notes-and-faq)
  - [Getting Started Guide](#getting-started-guide)
    - [Loading the Default Workflow](#loading-the-default-workflow)
    - [Components of the Workflow](#components-of-the-workflow)
      - [Loader](#loader)
        - [The Checkpoint Model (ckpt\_name)](#the-checkpoint-model-ckpt_name)
        - [LoRA Model (lora\_name)](#lora-model-lora_name)
        - [Positive Prompt (Upper Text Box)](#positive-prompt-upper-text-box)
        - [Negative Prompt (Lower Text Box)](#negative-prompt-lower-text-box)
        - [Empty Latent Sizes (empty\_latent\_with AND empty\_latent\_height)](#empty-latent-sizes-empty_latent_with-and-empty_latent_height)
      - [Sampler](#sampler)
        - [Noise Seed (noise\_seed)](#noise-seed-noise_seed)
        - [Steps (steps)](#steps-steps)
        - [CFG (cfg)](#cfg-cfg)
        - [Sampler and Scheduler (sampler\_name AND scheduler)](#sampler-and-scheduler-sampler_name-and-scheduler)
    - [Generating Your Image](#generating-your-image)
  - [Crafting and Making Prompts](#crafting-and-making-prompts)
    - [Positive and Negative](#positive-and-negative)
      - [An excerpt on negative prompting](#an-excerpt-on-negative-prompting)
    - [How to write prompts](#how-to-write-prompts)
    - [Keyword strengthening](#keyword-strengthening)
    - [Detail and Formatting](#detail-and-formatting)
      - [Little Excerpt on Camera Positions](#little-excerpt-on-camera-positions)
    - [Bottom Line](#bottom-line)
    - [Extra Components](#extra-components)
      - [Using LoRAs](#using-loras)
      - [Using Embeddings](#using-embeddings)
    - [Prompts in Action](#prompts-in-action)
  - [Model Details](#model-details)
    - [Best Standard Furry Models](#best-standard-furry-models)
    - [Best SDXL Models](#best-sdxl-models)
    - [Other Standard Models](#other-standard-models)
- [Training LoRAs with Dreambooth](#training-loras-with-dreambooth)
  - [What is a LoRA?](#what-is-a-lora)
  - [⚠️ Big Warning!](#️-big-warning)
  - [Installing Dreambooth \& A1111](#installing-dreambooth--a1111)
  - [Preparing Your Dataset](#preparing-your-dataset)
  - [Preparing to Train](#preparing-to-train)
  - [Training the LoRA](#training-the-lora)
# ComfyUI Image Generation
## Compared to Bing
Compared to the Bing Image generator, there are lots of pros and cons to using MuffinMode.
### Pros:
- Fast Generation, in most cases
- No token limits (You can generate as many images as you like, all the time)
- No filters that block basic words.
- Custom Image Sizes!
### Cons:
- You have to be more detailed with your descriptions
- More options
- Slight Learning Curve
## Notes and FAQ
- Keep in mind, if you save a workflow using the "Save" button in the side panel, ComfyUI will set the seed in the sampler to be the previous seed as it is exported. While this is great for recreating the exact same image, it will lead to the same image every generation as explained in the [noise seed details section](#noise-seed-noise_seed), so make sure you set the sampler noise seed to -1 to get new images! (As a rule of thumb, if your workflow makes the exact same images every time, it is very likely because your seed is fixed to a specific value.)
- Q: Why are there multiple characters in my generations?

  A: Multiple characters occur when you don't specify for there to only be a single character. If you want just a single subject, place `solo` near the beginning of the positive prompt, and `multiple partners, multiple subjects` near the beginning of your negative prompt. Where you put your keywords/phrases in your prompts matters! The AI will focus on prompts in 75 word bursts of "interest".
- Q: Why am I getting an error that says "AttributeError: 'NoneType' object has no attribute 'decode'"

  A: The reason for this is because you are trying to generate an image using a "Baked VAE" setting, on a model that does not have a baked VAE. Simply go to your Efficient Loader node, (usually the red one) locate the row called "vae_name" and change it to a VAE based on this guide [here](). In most cases, this can be set to MoiseMix.vae.pt. So, if you're not sure, do that! If you are still having trouble, contact me on discord. :3 (@finnithefox)
## Getting Started Guide
To get started using MuffinMode's ComfyUI, click on [this](http://muffinmode.net:8188) link. You'll be brought to a page with a node-graph. :3 It will probably have the default graph loaded, but I would recommend using the simpler, more fine-tuned graph which I will go in depth about below.
### Loading the Default Workflow
To make furry art using MuffinMode, the simplest way is to go to the [link above](http://muffinmode.net:8188), and then download [this](https://github.com/noahjgreer/noahjgreer/blob/main/DefaultWorkflow.json) link, then navigate back to the MuffinMode GUI, and in the right sidebar, click the "Load" button. Navigate to your downloads folder, and open up that `DefaultWorkflow.json`. Your workflow page should look something like it does below:

![MuffinMode Workflow](https://i.imgur.com/2jrluEf.png)

If you ever want to load your own custom workflow, you can load it with the "Load" button in the sidebar, or you can drag and drop an image Created from comfyUI into the window, and it will load the image, as long as it's metadata hasn't been stripped from the file. Discord compresses images, so this might be a common effect.

### Components of the Workflow
#### Loader
The first component to focus in is the "Loader" node. The loader node is the big red node on the leftmost side of the workflow. Within that node, you have various input boxes and settings you can configure. Any options not explicitly mentioned below can be ignored, as their use isn't important for simple generation.
##### The Checkpoint Model (ckpt_name)
The checkpoint model is the model which the generator uses to make images. The main model that you should use for furry art is "FluffyKemono_V2" and should be selected by default. If your aiming for more detailed furry art, I suggest using "IndigoFurryMix" You can find other models online on sites such as [CivitAI](https://civitai.com). If you find a model you like, and you want me to add to the MuffinMode generator, please let me know! :3
##### LoRA Model (lora_name)
If you want to use a LoRA model, you can select one from the list. Keep in mind that to use character LoRA's, you must use a series of trigger words in your prompt in order to get them to work. Below is a list of currently existing LoRAs, along with their trigger words! Keep in mind that some LoRAs may have a specific checkpoint model in their name, but this is just the checkpoint model which they were trained on, and they can be used with any of the checkpoint models
| Character | Model | Description | Trigger Word |
|-----------|-------|-------------|--------------|
| Yuley | lora_Yuley-v1.1_3200.safetensors | A LoRA which generates Ulysses, sauce's fursona | `yuley dragon`
| Finni | lora_Finni-v1.0-NAI_indigo_7000.safetensors| A LoRA which generates Finni the fox! | `f1nni fox`
| Hyper | lora_Hyper-v1.0_IFM_10000.safetensors | A LoRA which generates Hyper the fox. :3 | `hyp3r fox`
##### Positive Prompt (Upper Text Box)
The positive prompt is similar to most AI image generators, in which you will type in what you wish to generate. 
##### Negative Prompt (Lower Text Box)
The negative prompt is something which you generally won't see in most widely used AI image generators which you can find on the web. To keep things simple, the negative prompt is basically anything you *don't* want to see. (`malformed limbs, extra fingers, merged legs`) etc. Negative Prompts can get very long, so I would suggest using an embedding, by typing in `embedding:FastNegativeV2` or `embedding:EasyNegative` instead, to keep you from losing your mind.
##### Empty Latent Sizes (empty_latent_with AND empty_latent_height)
This is the width and height, in pixels, which you want your image to generate at. These can't be anything you want, but the node will automatically round whatever you put in to the nearest 16th, so that it will be able to generate normally.
#### Sampler
The sampler is the second peice of the puzzle for generating images. This is basically the node which does the generating of your image. It's the node that is yellow, and found in the middle of the workflow.
##### Noise Seed (noise_seed)
The noise seed is basically the random noisemap that will be used to generate the image. In short, when an AI generates an image, it generates a big noisy image, similar to TV static, and then slowly adds on layers based on the model you selected, the prompt you inputted, and the settings you set. By default, this input field should be set to -1, so that it will generate a new image every time. If it is set to a seed, (e.g. 18924981982984), then it will make the exact same image every time, if you don't change the prompt text or settings. In some ways, this can be very useful! If you find an image generation you absolutely loved, you can lock the seed so that it will generate that same image, but then you can tweak the positive and negative prompt to further refine the image to your liking.
##### Steps (steps)
To put it simply, steps are the amount of time the generator should spend on generating the image. This should be set from anywhere between 20-100 steps. Any higher and it will be too slow, with little difference. More steps doesn't always mean it will look better!
##### CFG (cfg)
Generally, I would say don't touch this too much. Keep it between 7.0-8.0. What the CFG does is it determines how much the AI should listen to your prompt, or do it's own thing. Lower means it will listen to you less, higher means it will listen to you more. While listening to you more sounds like a good thing, it will go nuts if you set it too high.
##### Sampler and Scheduler (sampler_name AND scheduler)
The sampler and scheduler determine what methods should be used for the diffusion process of generating your image! ^w^ It's kind of up to you about what you think is best, but from my experience, the best ones are either a sampler of Euler set to a scheduler of Normal, or a DPMPP_SDE set to SGM_Uniform.
### Generating Your Image
When you've set everything to exactly how you like, you can generate your image by clicking "Queue Prompt" in the top right of your screen. Enjoy! 

If you have any questions, please let me know uwu

## Crafting and Making Prompts
Prompts are the most important part of creating AI-generated images because they define what data the generator will use to make the image!

In this section, we will breakdown what components make up a prompt. It's fairly simple, and it is lightly covered in the sections above. We'll take a more in-depth look here.

### Positive and Negative
First, is the text input itself. This is comprised of a **positive** prompt and a **negative** prompt. These two things are very important, and both of them determine how the final image will look! 

Simply put, the **positive** prompt is the text box where you'll put the things you want to see from the AI, and the **negative** prompt is where you will put concepts you don't want to see from the AI. 

![Prompt boxes](https://i.imgur.com/DuQFYc6.png)
> If you use the workflow that's listed way up above, you will have your positive and negative prompts combined into a single module. The positive prompt is on top, and the negative is on the bottom. If you decide to break everything up and do it from scratch by yourself, you may have these separated into two different nodes!

#### An excerpt on negative prompting
A little note on negative prompting! Most of the work of the prompt goes into the positive prompt, at least with how I do things, and the negative is only really used to eliminate and tweak undesirable things. For example, the negative prompt I use always starts out with:
```
nsfw, boring_e621_fluffyrock_v4, deformityv6, EasyNegative
```

This is the best way to get rid of anything bad, along with things that are ugly, which is what the embeddings `boring_e621_fluffyrock_v4` and `deformityv6` do. I would highly recommend using that as your base negative prompt as well, adding keywords or concepts as you see fit. It will save you lots of time, unless of course you want to sit down and spend quite a while making a really massive and confusing negative, with lackluster results. 

Let's look at the bare-bones concepts that make up a prompt.

### How to write prompts
There are schools of thought regarding how to write prompts. The first one is keyword based prompting, and the second being natural based prompting. Personally, I tend to use a mix of these, and I think that's the best way to do it. Let's take a look at these two:

First, keyword based prompting:
```
anthro, furry, fox, solo, sitting, happy, blush
```

Next, natural based prompting:
```
An anthropomorphic furry fox sitting by himself, blushing and smiling happily.
```

To most, it is pretty clear that these two prompts are describing the same thing. And they are! :3 However, to a computer, all of those extra articles, prepositions, and conjunctions such as "An," "by," "and," and even the pronoun "himself" can confuse the AI and make it generate something that deviates slightly from what you want to actually see. 

On the other hand, using only keywords may generate dry results that leave the computer up to using its own interpretation, and might also not lead exactly to what you want to see. 

Thus, we must find a perfect balance! :3 This takes a little tinkering, but I think I would lean toward doing something like this. This provides the AI with simple and concise prompting while still allowing for more natural language use where clarifying concepts matters:

```
solo, anthro, furry, fox, sitting in a forest, smiling happily, blushing
```

### Keyword strengthening
Another important concept is controlling the strength of the keyword. For each word in a prompt, you can control it's strength within the prompt by enclosing it in parenthesis, and adding a colon with a number after it. Here's an example of it using the word `muscular`:
```
(muscular:0.25)
```
The lower the decimal number, the less strength it will have in the image! By default, this is normally set to `1`, in which case you can simply type the word without any special formatting. But, if you want to make it more or less intense, then I would recommend changing it to fit your needs. Generally, I wouldn't go below 0 or above 2.25 or so, because then the image can begin to look deepfried. 

If this is the case and you still aren't getting what you hoped for, then I would suggest adding the opposite of what you want to see into the negative prompt. More on that in the sections below! For negative prompts, adjusting the strength of a keyword does the same thing as in the positive prompt, but inversed. For example, if you put `skinny` in the negative prompt and adjust it's strength as above, then the higher the strength, the more it will be eliminated from the image. 

A quick tip! If you want to adjust the keyword strength without typing the number or parenthesis, then simply highlight the word with your cursor, and then while pressing the `CTRL` key on your keyboard, press the up and down arrow keys to change the strength accordingly. This is what I usually do to save from tedius typing, and it helps!


### Detail and Formatting
Here we will take a look at the importance of using detail and proper formatting in your prompt. Say perhaps I want an image of an anthro fox in a forest. In greatest simplicity: in the positive prompt I would put:
```
anthro fox in a forest
```

While you could stop here, your results would vary greatly, and usually with AI, you want to start moving closer to what you *want* to see, eliminating things you don't want to see. Try describing the artstyle, the scenery, and the use of colors throughout the image. As you work on doing these things, split them up into sections based on their focus. AI pays attention to prompts in spurts interest every 75 words, and this can help better collate these focuses, and also make it easier to read.

Depending on how you want to lay out your focus sections is up to you, but I usually work my way from broad concepts inward. Here's an example of that:
```
(best quality:1.1), masterpiece, warm vivid colors, detailed shading, gorgeous lighting, best eyes, digital art, kemono style

outdoors, autumn, cozy, realistic fur, (chibi:0.45), paws, side shot

male, solo, anthro, (small:0.65), f1nni fox, white angelic wings, heterochromia with yellow and blue, (gold halo:1.15), (lean:0.85), cute, huge fluffy tail, short head hair, wearing a (red bandana:1.1), blue stripedsocks, cute pose, sitting in a pile of leaves, arms in the air, happy, mouth open, eyes closed, (blush:1.1)
```

Let's break down this pattern:
```
[OVERALL ASPECTS]

[BACKGROUND AND VAGUE CONCEPTS]

[SUBJECT]
```
So, the prompt begins with the overall aspects of the piece, such as how it will look and be displayed in the image. Next comes the background, along with some vague overarching concepts, like camera angles and such. As you can see, I also put paws in there because the image was generating hands and I didn't want that, but you can also just stick `hands` into the negative to fix this as well. 

#### Little Excerpt on Camera Positions
If you want to control the camera's position, or the type of shot you want, determining how the subject will fill the frame, try using one of these keywords in your prompt:
```
side shot, half-body shot, full shot, front shot, back shot, top down, bottom up, close up, far away, zoomed in, zoomed out, portrait, landscape, square, wide, tall, vertical, horizontal, diagonal, 3/4 view, 1/4 view, 1/2 view
```

### Bottom Line
In reality, there is no true proper way to prompt, and I am still learning myself! This is a great opportunity to tinker around with words, move things around, and learn how things work, too!

Since the AI has certain moments of focus, your positioning of words within the prompt can greatly affect how the image will look! If you want your subject to have the most attention, then maybe try flipping the section focusing on it's head, starting with the details and working out to the big picture.

### Extra Components
There are some extra things that you can put into your prompt to make it extra snazzy! 😎

While you can make a perfectly good prompt on your own with just text, you can add in the things below to add extra flare into your generations, and make life easier for you!

#### Using LoRAs
I made a little section on how to use Loras above, but this section here will focus on their use within the prompt a little more and re-cover the concepts while also building upon them.

First off, a LoRA is a small mini-model trained on subjects or concepts and can be used within generation to cleanly bundle together those things, while also retaining the original look or subject it was trained on.

For instance, in the prompt above in the section on Detail and Formatting, there were ***three*** loras at use that you might not have even noticed!: `f1nni fox`, `heterochromia with yellow and blue`, and `blue stripedsocks` were all taking advantage of LoRAs! Now, these can't just be used automatically, you have to load them in. Some LoRA's might not work with certain models; it depends on what model they were trained on. It might take some experimenting to find out! :3

By default, you can only load 1 lora at a time into the efficient loader nodule, but if you look at the top left of that same node, you can find a lora_stack bubble. If you click and drag from it, outside of it, you can select `LoRA Stacker`! Doing this will allow you to use multiple LoRAs at once. 

To load them in, simply click on the respective `lora_name` bubbles, and the select the LoRA you want. If you have a LoRA you want to add, please send me a DM @finnithefox on Discord! You can search for LoRAs on [CivitAI](https://civitai.com/search/models?modelType=LORA)! Make sure to also change the weight for each Lora, this will determine it's strength in the prompt. 

To use a LoRA, you must trigger it within your prompt. This is usually a word or two, or many, that tell the LoRA to kick in! You can find some of these trigger words in the [LoRAs section in the introduction.](#lora-model-lora_name)

#### Using Embeddings
Similar to LoRA's, there's embeddings. Embeddings are like LoRAs except you don't have to import them! Well, there's a little bit more of difference than that, but it's still pretty similar!

The only embeddings stored on MuffinMode right now are just the `boring_e621_fluffyrock_v4`, `EasyNegative`, and `deformityv6`, embeddings seen in the [excerpt on negative prompts](#an-excerpt-on-negative-prompting).

Since this list is always changing, I would recommend double clicking somewhere in the workflow, and searching for "Embedding Picker". This will allow you to see all of the available embeddings, and can prove to be very helpful when you're trying to find the right one for your prompt! :3 When it comes time to use them in your prompt, simply type their filename, without the `.pt`, either in the positive or negative prompt, and it will work!


### Prompts in Action
To cut to the chase, here's a prompt I made recently that generates some cute results. This prompting uses all the stuff we've talked about above, and it's a great example of how to use it all together! :3

> Positive Prompt:
```
(best quality:1.1), medium close up, side shot, warm vivid colors, best eyes, masterpiece, soft cozy lighting, indoors, autumn, cozy, realistic fur, (chibi:0.45), paws, side shot, soft shading

male, solo, anthro, (small:0.65), f1nni fox, white folded angelic wings, holding a cup of hot cocoa, heterochromia with yellow and blue, (gold halo:1.15), (lean:0.85), (twink:1.15), (femboy:1.2), cute, huge fluffy tail, no head hair, wearing a (red bandana:1.1), cute pose, looking outside, happy, blushing, wearing cozy maroon sweater, sitting on a pillow next to a window
```
> Negative Prompt
```
human hands, full body shot, head hair, nsfw, (masculine:0.25), boring_e621_fluffyrock_v4, deformityv6, EasyNegative
```
Here are the results! I generated 10 images, and these were my 8 favorites. Yes, there are a lot of discrepancies, but I did change the prompt after the first attempt, and then also through generation, and I feel happy with the results.

![Finni Generations](https://i.imgur.com/SKBkCYl.jpg)

Now that you've learned about prompting, go give it a try for yourself! Can't wait to see what you make! :3 🧡

## Model Details
MuffinMode offers a wide selection of models, and here I will provide their names and best uses. The best ones will be placed in their own section, here at the top! The other models will have their own table below it.

### Best Standard Furry Models
| Model Name | Identifier | Use | Example | 
|------------|------------|-----|---------|
| Indigo Furry Mix - Hybrid | `indigoFurryMix_v105Hybrid` | Best for detailed digital furry art | ![IFM-H Example](https://i.imgur.com/HiV9QRe.png) |
| Indigo Furry Mix - Realistic | `indigoFurryMix_v95Realistic` | Best for detailed realistic furry art | ![IFM-R Example](https://i.imgur.com/hUMERVh.png) |
| Indigo Furry Mix - Anime | `indigoFurryMix_v95Anime` | Best for detailed anime furry art | ![IFM-A Example](https://i.imgur.com/kRmO7T1.png) |
| Indigo Kemono Mix | `indigoKemonoMix_delta` | Best for the *cutest~* detailed kemono furry art | ![IKM Example](https://i.imgur.com/J8VhQwV.png) |
| Indigo Comic | `indigoComic_v10withvae` | Best for detailed comic furry art | ![IC Example](https://i.imgur.com/VEyBunQ.png) |
| Fluffy Kemono V2 | `fluffyKemono_v2` | Good for kemono furry art | ![FK Example](https://i.imgur.com/Gh84EsS.png) |
> For those wondering, here is the positive and negative for the images generated in the section above. All used the same seed and generation settings, the only thing that was different was the model. :3
>
> **Positive:**
> ```
> masterpiece, high quality, best eyes, absurd res, soft lighting, (front view:0.7), extreme close-up, (sideview:0.6), bust portrait, paws,
> 
> solo, anthro, (kemono:0.4), twink, fox, (cute:1.1), (orange body), big tail, red bandana, purple sweater, hotpants, looking aside, blushing, happy, sitting against rock,
>  
> field, cloud, mountains, daytime, grass, flower, (blurred background:0.6), 
> 
> panorama, portrait, 135mm, character focus. (detailed background, amazing background), outdoors, scenery, light particles
> ```
> **Negative:**
> ```
> rear view, multi tail, humanoid hands, nsfw, boring_e621_fluffyrock_v4, deformityv6, EasyNegative
> ```


### Best SDXL Models
| Model Name | Identifier | Use | Example |
|------------|------------|-----|---------|
| Furry Art Fantasy XL Mix | `furryArtFantasyXLMix_v03Beta` | A different approach to detailed furry art, I would still recommend using the standard models instead. | ![FAFXL Example](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d49069dd-f7e9-4280-99c7-ad44fc811745/original=true/00009-3630117716.jpeg) |
| Stable Diffusion XL | `sd_xl_base_1.0` | An model for anything, not particularly for furry art. | ![SDXL Example](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9b32c2c3-88e3-464f-a63a-959877975e25/original=true/ComfyUI_00341_.jpeg) |

### Other Standard Models
| Model Name | Identifier | Use |
|------------|------------|-----|
| Fluffyrock Kemono Fusion | `Fluffyrockkemonofusion` | Okay for kemono furry art, can be a little wonky. |
| AniReality-Mix | `anirealityMix_v1` | Good for anime art |
| Niji V5 Style | `nijiv5style_v10` | Good for anime art |
| Photon | `photon_v1` | A model for everything, not particularly for furry art. |
| Stable Diffusion v2.1 | `v2-1_768-nonema-pruned` | A model for everything, not particularly for furry art. Not as good as SDXL |

# Training LoRAs with Dreambooth
## What is a LoRA?
To put it simply, a LoRA is a small little model, built on a checkpoint's previous training, that is trained for specific uses. The most common cases for LoRA's are the mimicking of styles, objects, or characters.


A Lora is notably small compared to regular model checkpoints, which usually are 4-6 GB in size whereas LoRAs are only about 4MB in size! This makes them very lightweight and versatile for portability, while still retaining the same accuracy as a regular model.

To train a LoRA on your own, you first need to gather a dataset of images to train the LoRA on. Although these datasets can be as large as you would like, LoRAs still can look incredible with just being trained on 5-6 images! It's better to value images that accurately depict what you want the LoRA to copy, rather than images that might not be exactly what you'd want to see. 

## ⚠️ Big Warning!

**The stuff below is very complex and will consume lots of your time if you intend on doing this yourself. Training LoRAs isn't necessarily difficult, but unless you have a good GPU, it can take anywhere from 12-24 hours to train. That's why, if you'd like, you can simply send me your batch of images, and I can do the rest! This will save you the time and difficulty of hopping through hurdles to get you your LoRA. :3 If you'd like to save *me* some time but you don't want to train it on your own computer, you can do all the tough work of captioning each image! >w< You can read more about that below. If you wanna continue doing this all by yourself, get ready for lots of fun learning below!**

## Installing Dreambooth & A1111
> Quick side note, if you ever get lost in my rambling process, check out [this](https://www.youtube.com/watch?v=70H03cv57-o) tutorial as it's much better than my explaination. x3

For this process, if you want to do this *all* by yourself, you will need to get started on installing Dreambooth onto your computer. Keep in mind that if you don't have an NVIDIA GPU, this probably won't work for you. In this case, I'd recommend reading the warning above this section and sending me your images to train for you.

To install Dreambooth, you're going to need to install a repository called Automatic 1111's Stable Diffusion Web-UI. You can find a link to that [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui#automatic-installation-on-windows), along with the steps to install it. It's basically like an alternative to ComfyUI that doesn't use nodes, and isn't as fast. If there was a way to train LoRAs in Comfy, I'd be telling you how to do that instead! X3 Anyway, after you follow those installation instructions and you have A1111 all set-up, you should be able to view the webpage similarly as seen below:
![Automatic 1111 Web-UI (Sorry this image is so big I don't want to disrupt the markdown syntax and use HTML instead because of my OCD. 👀)](https://i.imgur.com/o8db1M2.png)

From there, as pictured, head to the extensions tab, and then the "Available" subtab. Then using CTRL+F find the extension titled "sd_dreambooth_extension" and install it. Afterward, head back to the "Installed" subtab, and then tap the orange button called "Apply and Restart UI"

Next, after it's all installed and restarted, you should have a new "Dreambooth" tab! Yayyy! 🧡 ^w^ Now, this is where the nitty-gritty starts.

## Preparing Your Dataset
Gather together 6-300 images and place them in a folder somewhere on your computer. Make sure you know where this folder is, as you'll be needing it later. Make sure that your images are all 512x512 in size, as this will help with make training easier. I'd recommend using a tool like [Birme](https://www.birme.net/) if you have a lot of images. It gets the job done quickly.
![Dataset](https://i.imgur.com/QS3E214.png)

Once all your images are prepared, head on over to the a1111 web-ui you installed earlier, and go to the "Train" tab. Next, go to the preprocess images sub-tab, this will help prepare your dataset for generation. Now, copy your directory from which your dataset was stored, (e.g. go into file explorer and open the folder, copying the location from the Window Location bar) and paste it into the first input box in the a1111 web-ui training preprocess images tab, "Source directory." Next, under destination directory, I would recommend probably pasting in the same directory, but adding a "2" at the end to keep things simple. Keep everything below the same, except check the "Use BLIP for caption." I prefer the deepbooru captioning, but it's sometimes less accurate and more~.. dirty. After all that, your window should look something like this: if it does, go ahead a hit "Preprocess" and wait for it to complete.
![Preprocess](https://i.imgur.com/lg3SRJ6.png)

Now, after it's all finished, navigate to the output folder that you created. Here you will find all of your images from before, along with text documents next to them. If you open one of these up, you will see AI's best guess at what your image was predicting. If your dataset isn't too large, I'd recommend going through and perfecting them to best match your image.

**VERY IMPORTANT!!:** When captioning your images, make sure you don't include *anything* about the character itself. It's fur patterns, it's wings, etc. Anything you put in this text document will be ignored as extra stuff by the AI. Only describe anything that is specific to that image, (e.g the type of artstyle, the background, stuff like that!) The only thing I would recommend putting in there is it's species, and "furry" as this will make it a little more flexible if you ever want your character to be a different animal.

Example of all my jibber-jabber:
![Captioning Images](https://i.imgur.com/SqsKxTY.png)

After you're all done fixing up your captions, it's time to move on to the next step!

## Preparing to Train
Back in the A1111 web-ui, click over to the dreambooth tab. You will see a ton of stuff. Don't worry, I'll walk you through it! :3 First, in the section under the "Model" heading, go over to create. 

> Side-Note! I forgot to mention before, but here is where you will need to have a checkpoint to train the AI on. Yeah.. that really big file I mentioned earlier! x3 Don't worry if you want to drop the process now, you can still send me all your images and captioning so far, and I can do the rest for you. If you still want to do this yourself, click on [this](https://civitai.com/api/download/models/209164?type=Model&format=SafeTensor&size=full&fp=fp32) link. It will download a model. From there, copy that model and go to your a1111 webui folder, then navigate to the models folder, and then the "Stable-diffusion" folder. Paste it in there. Okay, back to the fun stuff! x3

Under the create tab of the Model section in the Dreambooth page, type in a name for your model. I usually will do something along the lines of: `lora_YOURCHARACTER-v1.0-ABBR.OFTHECKPTMODEL` Next, under model type, select v1x, and then under source checkpoint, click on the loading wheel next to it, and then select the model you downloaded from the dropdown. If your page looks like the image below, you're ready to click "Create Model."

![Model Creation](https://i.imgur.com/Phz1nly.png)

After doing that, move your eyes over to the Input section, and make sure you're on the "Settings" tab. Go ahead and click the "Performance Wizard (WIP)" button. Now, there are a few little things you have to change after that's done doing it's thing. For the number of "Training Steps Per Image (Epochs)" I would recommend setting this based on how many images you have. If you only have like 6 images, I would recommend setting it to 250-300, but if you have like a hundred images, I'd set it to like~ 10 epochs. Also, make sure the "Use LORA" box is checked!

![Input Settings](https://i.imgur.com/Mv222hM.png)

After that, go to the "Concepts" tab right next to the settings tab you are just in. From here, paste in the directory which contains all your dataset images, plus your captions for those images, into the input box titled "Dataset Directory". Right underneath it, in the "Classification Dataset Directory" box, paste in the directory again, but this time with a "c" at the end, for simplicity. The classification dataset is basically an additional control group which generates images based on your captions, to help the AI train better. Underneath all that, you will have your filewords. Under the box that says "Instance token" put in a very special name for your character! This can't be something generic, it has to be special. This is the word which will trigger your LoRA during generation. My formula is this: If I am training a character called Hyper, I will put my instance token to be `hyp3r` for example. Now, underneath that, in the class token, type the species of your character. Underneath that, in the training prompts section, put `[filewords]` in both the Instance Prompt, and the Class Prompt. This will read from your caption files which you created. If your window looks like the image below, head on over to the "Saving" tab next to the tab you're currently in.

![Input Concepts](https://i.imgur.com/kwEYTTZ.png)

Under the saving tab, in the "Custom Model Name" section, put in the exact same name which you entered for your lora model a little bit ago. Then for the settings proceeding, match the below image. Afterward, move on over to the testing tab. We are almost there!

![Input Saving](https://i.imgur.com/lXNa0VR.png)

Finally under the Testing tab under the Input section, untick "Calculate Split Loss". Now you can go to the "Generate" Tab, which is right next to the testing tab, and click on "Generate Class Images" this process can take a while, but after it's done, you will be able to start training.

## Training the LoRA
After your class images have finished generating, you can click on the orange "Train" button located at the top of the Dreambooth Tab. This process will probably take a couple hours, but it varies depending on your GPU.

After your Lora has finished training, you're all done! ^w^ Now, to locate the file, go to File Explorer and open up your stable diffusion web-ui directory. Then, in the models folder, go to the "Lora" folder, and there you should find two files. Use the one *without* the "auto" at the end of it! For some reason, the auto one crashes stuff. x3 If you want to use your LoRA in comfyUI, you can send me the file via discord, and I will add it to MuffinMode so that you can use it on there! If you want to use it on your own computer, you can either use the a1111 web-ui to generate images that way, or you can install ComfyUI yourself and copy it over to the models/LoRA directory.

Hopefully this process helps you with generating a LoRA. If it's too mind boggling, please don't hesitate to let me help out. If you have any questions let me know! Thanks! ^w^ 🧡
