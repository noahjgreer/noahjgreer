# **MuffinMode**
MuffinMode is a web server I run on my computer. :3 Its most common public use is for generating stable-diffusion images. It runs on fing called ComfyUI, which is a semi-user-friendly graphical user interface, which gives more control and faster generations compared to alternative generators.
## Index
- [ComfyUI Image Generation](#comfyui-image-generation)
  - [Compared to Bing](#compared-to-bing)
  - [Notes and FAQ](#notes-and-faq)
  - [Getting Started Guide](#getting-started-guide)
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
## Getting Started Guide
To get started using MuffinMode's ComfyUI, click on [this](http://muffinmode.net:8188) link. You'll be brought to a page with a node-graph. :3 It will probably have the default graph loaded, but I would recommend using the simpler, more fine-tuned graph which I will go in depth about below.
### Loading the Default Workflow
To make furry art using MuffinMode, the simplest way is to go to the [link above](http://muffinmode.net:8188), and then in the right sidebar, click the "Load Default" button. Your workflow page should look like it does below:

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
| Finni the Fox | lora_Finni-v1.0-NAI_indigo_7000.safetensors| A LoRA which generates finni the fox! | `f1nni fox`
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
