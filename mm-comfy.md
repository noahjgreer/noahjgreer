# **MuffinMode**
MuffinMode is a web server I run on my computer. :3 Its most common public use is for generating stable-diffusion images. It runs on fing called ComfyUI, which is a semi-user-friendly graphical user interface, which gives more control and faster generations compared to alternative generators.
## Index
- [ComfyUI Image Generation](#comfyui-image-generation)
  - [Compared to Bing](#compared-to-bing)
  - [Notes and FAQ](#notes-and-faq)
  - [Getting Started Guide](#getting-started-guide)
- [Training LoRAs with Dreambooth](#training-loras-with-dreambooth)
  - [What is a LoRA?](#what-is-a-lora)
  - [Big Warning!](#%EF%B8%8F-big-warning)
  - [Installing Dreambooth & A1111](#installing-dreambooth--a1111)
  - [Preparing Your Dataset](#preparing-your-dataset)
  - [Preparing To Train](#preparing-to-train)
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
## Getting Started Guide
To get started using MuffinMode's ComfyUI, click on [this](http://muffinmode.net:8188) link. You'll be brought to a page with a node-graph. :3 It will probably have the default graph loaded, but I would recommend using the simpler, more fine-tuned graph which I will go in depth about below.
### Loading the Default Workflow
To make furry art using MuffinMode, the simplest way is to go to the [link above](http://muffinmode.net:8188), and then in the right sidebar, click the dropdown arrow next to the "Load" button. It might be cut off slightly, but select the one called: "Default-Clean." Your workflow page should look something like it does below:

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
