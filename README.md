# Stable-Diffusion-Latent-Space-Explorer

## General Information
<img align="right" width="300" height="" src="https://upload.wikimedia.org/wikipedia/commons/1/1e/Logo-Goethe-University-Frankfurt-am-Main.svg">

**Author:**
* Alen Smajic

**Advisor:**
* [Prof. Dr. Visvanathan Ramesh](http://www.ccc.cs.uni-frankfurt.de/people/), email: V.Ramesh@em.uni-frankfurt.de

**Institutions:**
  * **[Goethe University](http://www.informatik.uni-frankfurt.de/index.php/en/)**
  * **[AISEL - AI Systems Engineering Lab](http://www.ccc.cs.uni-frankfurt.de/)**


## Setup
### Installation
1. Clone this repository.

       git clone https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer
       
2. Create a virtual environment and activate it.

       python -m venv venv
       
3. Install PyTorch with CUDA ([follow this PyTorch installation](https://pytorch.org/get-started/locally/)).
       
4. Install [diffusers](https://huggingface.co/docs/diffusers/index) and [transformers](https://huggingface.co/docs/transformers/index) librariers.

       pip install diffusers["torch"] transformers
       
5. Optional: Install [xFormers](https://github.com/facebookresearch/xformers) for efficient attention.

       pip install xformers
       
### Tested Versions
If you face any issues, you should try to match the version of the installed packages to these versions:

- torch 2.0.0
- cuda 11.8
- diffusers 0.14.0
- xformers 0.0.16
       
### Model Weights
You can download the model weights using [git-lfs](https://git-lfs.com/).

        git lfs install
        git clone https://huggingface.co/stabilityai/stable-diffusion-2-1
        
The command above will create a local folder called ```./stable-diffusion-2-1``` on your disk.

The code in this repo was tested on: 

:heavy_check_mark: [Stable Diffusion 2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1) (native resolution 768x768)

:heavy_check_mark: [Stable Diffusion 2 Inpainting](https://huggingface.co/stabilityai/stable-diffusion-2-inpainting) (native resolution 512x512)


## Instructions
Below is a list of experiments that are currently supported. Each entry is linked to a tutorial of the specified experiment.

### Experiments
1. [Single Inference](#1-single-inference)
2. [Visualize Diffusion](#2-visualize-diffusion)
3. [Random Walk](#3-random-walk)
4. [Interpolation](#4-interpolation)
5. [Diffevolution](#5-diffevolution)

### How to run an experiment
In order to run an experiment, you first need to define the experiment parameters using one of the experiment configuration files. Once this is done, you can run an experiment by calling the following script:

        python run_sd_experiment.py --exp_config ./configs/experiments/{path to your config} 
        
The script ```run_sd_experiment.py``` expects an argument ```--exp_config```, which is the path to an experiment configuration file (e.g. ```./configs/experiments/txt2img/single_inference.yaml```). 

### Experiment Results

Each experiment run will produce a results folder with a unique name consisting of the date and time of execution, the model identifier and the experiment identifier. The subfolders of the results folder include:
 - ```configs``` here will be a copy of the configuration file, which was used to start the experiment.
 - ```images``` stores the images generated by Stable Diffusion.
 - ```embeddings``` stores the unique latents for each generated image. You can reference these files to load the prompt embeddings and latent noise.
  - ```gifs``` stores the gifs generated by the experiment (each frame of the gif is contained within the ```images``` folder). 
  
 ### Some general tips
 :information_source: You can find more information on schedulers [here](https://huggingface.co/docs/diffusers/using-diffusers/schedulers). Morover, if you are unfamiliar with any concept from the ```Model Configurations``` you can refer to the [diffusers documentation](https://huggingface.co/docs/diffusers/index)., diffusion steps, guidance scale
 :information_source: Note that the strength parameter scales the specified amount of diffusion steps. That is the reason why the output folder only contains 20 images (for 20 diffusion steps) even though we specified 25 diffusion steps in the config file. It starts the diffusion process from step 5.


## Tutorials

All experiment runs in this tutorial have the same model configuration. [Stable Diffusion 2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1) was used for ```txt2img``` and ```img2img``` experiments. For ```inpaint``` was [Stable Diffusion 2 Inpainting](https://huggingface.co/stabilityai/stable-diffusion-2-inpainting) used. The most relevant parameters from the model configuration are listed below.

#### Model Configurations

:page_facing_up: 	```scheduler```: DPMSolverMultistepScheduler

:arrows_counterclockwise: ```diffusion steps```: 25

:control_knobs: ```guidance_scale```: 9.5


### 1. Single Inference
In this tutorial we will use the ```single_inference.yaml``` configuration files for ```txt2img```, ```img2img``` and ```inpaint```. 

We will start by configuring ```txt2img``` to generate 4 images that adhere to a prompt describing an astronaut riding a horse on the moon (you can find the configuration [here](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/txt2img/single_inference.yaml)).

#### Prompt Configurations

:keyboard: ```prompt```: *"A photograph of an astronaut riding a horse on the moon."*, ```negative prompt```: *"black and white, blurry, painting, drawing"*

#### Latent Noise Configuration

:seedling: ```rand_seed```: 0, ```height```: 768, ```width```: 768, ```images per prompt```: 4

<img width="" height="" src="resources/images_for_readme/txt2img_single_inference.png">
:information_source: Note that your results may differ from the ones presented here, even with identical configurations and random seeds. 

You can find [here](https://huggingface.co/docs/diffusers/using-diffusers/reproducibility) more information on reproducibility.

Next, we will configure ```img2img``` to generate 4 variations of the image ```output-3_diffstep-25.png``` from the previous experiment. The variations will
depict an astronaut riding a donkey on the moon (you can find the configuration [here](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/img2img/single_inference.yaml)).

#### Prompt Configurations

:keyboard: ```prompt```: *"A photograph of an astronaut riding a donkey on the moon."*, ```negative prompt```: *"black and white, blurry, painting, drawing"*

#### Latent Noise & Image Configuration

:seedling: ```rand_seed```: 42, ```height```: 768, ```width```: 768, ```images per prompt```: 4

:framed_picture: ```image```: *".experiments/2023-03-27_18-43-02_txt2img_single-inference/images/output-3_diffstep-25.png"* 

:mechanical_arm: ```strength```: 0.8

<img width="" height="" src="resources/images_for_readme/img2img_single_inference.png">

Finally, we use ```inpaint``` to generate 4 variations of the image ```output-0_diffstep-25.png``` from the previous experiment, which depicts a donkey instead of a horse. The final 4 variations will be replacing the astronaut with a humanoid roboter using a [manually generated mask](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/resources/astronaut_mask.png) (you can find the configuration [here](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/inpaint/single_inference.yaml)).

#### Prompt Configurations

:keyboard: ```prompt```: *"A humanoid roboter astronaut."*, ```negative prompt```: *"black and white, blurry, painting, drawing, watermark"*

#### Latent Noise & Image Configuration

:seedling: ```rand_seed```: 0, ```height```: 768, ```width```: 768, ```images per prompt```: 4

:framed_picture: ```image```: *".experiments/2023-03-27_21-40-35_img2img_single-inference/images/output-0_diffstep-25.png"* 

:black_square_button: ```mask```: *".resources/astronaut_mask.png"*

<img width="" height="" src="resources/images_for_readme/inpaint_single_inference.png">

### 2. Visualize Diffusion
In this tutorial we will use the [```visualize_diffusion.yaml```](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/inpaint/visualize_diffusion.yaml) configuration file for ```inpaint```. This experiment decodes the latent noise tensor after each diffusion step and creates a visualization of the whole diffusion process.

We can easily reuse textual embeddings and latent noise tensors to recreate images that were created by a previous experiment run. 

#### Prompt Configurations

:keyboard: :floppy_disk: ```load_prompt_embeds```: *"./experiments/2023-03-27_21-46-30_inpaint_single-inference/embeddings/output-1_diffstep-25.pt"*

#### Latent Noise & Image Configuration

:seedling: :floppy_disk: ```load_latent_noise```: *"./experiments/2023-03-27_21-46-30_inpaint_single-inference/embeddings/output-1_diffstep-25.pt"*

:framed_picture: ```image```: *".experiments/2023-03-27_21-40-35_img2img_single-inference/images/output-0_diffstep-25.png"* 

:black_square_button: ```mask```: *".resources/astronaut_mask.png"*

<div style="display: flex;">
  <img src="resources/images_for_readme/inpaint_visualize_diffusion.gif" alt="Image 1" width="47%">
  <img src="resources/images_for_readme/inpaint_visualize_diffusion_grid.png" alt="Image 2" width="52%">
</div>

:information_source: The results of this experiment are not in this repository due to their size.

### 3. Random Walk
In this tutorial we will use the [```random_walk.yaml```](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/txt2img/random_walk.yaml) configuration file for ```txt2img```. We will create a visualization by performing a random walk within both the textual and image latent space starting from an initial image. 

To make our visualization more appealing, we will extend the image width from 768 to 1200. 

#### Experiment Configurations

:ballot_box_with_check: ```prompt_rand_walk```: True, ```noise_rand_walk```: True

:test_tube: ```walk_directions```: 3, ```walk_steps```: 50, ```step_size```: 0.0095

#### Prompt Configurations

:keyboard: ```prompt```: *"A beautiful painting of a pirate ship."*, ```negative prompt```: *"low quality, blurry, low resolution"*

#### Latent Noise Configuration

:seedling: ```rand_seed```: 0, ```height```: 768, ```width```: 1200, ```images per prompt```: 4

https://user-images.githubusercontent.com/63591221/228901991-aa6ccd79-c82b-4956-8b5e-ca1234a667ac.mp4

:information_source: The results of this experiment are not in this repository due to their size.

### 4. Interpolation
In this tutorial we will use the [```interpolation.yaml```](https://github.com/alen-smajic/Stable-Diffusion-Latent-Space-Explorer/blob/main/configs/experiments/txt2img/interpolation.yaml) configuration file for ```txt2img```. We will create a visualization by interpolating text embeddings and latent noise tensors, which are loaded from a pre-defined list. The list contains 8 prompts and 8 latent noise entries. Besides listing raw text prompts and random seeds, one can directly reference an embeddings file from a previous expeeriment. When doing so for latent noise tensors, it is important to take care that the image resolution matches for all items of the inter_noises parameter (random seed entries will use the heigh and width parameters, which are defined below).

As the second entry of the ```inter_prompts``` and ```inter_noises``` lists, we will link to the embeddings of the initial image from the previous tutorial depicting a painting of a pirate ship. Since the loaded latent noise embeddings are configured for 700x1200 image resolution, we will have to set this as the base resolution for the experiment.

#### Experiment Configurations

:test_tube: ```interpolation_steps```: 30, ```interpolation_method```: slerp

#### Prompt Configurations

:keyboard: ```inter_prompts```: 
 - *"A photograph of a dog with a funny hat"*
 - *"./experiments/2023-03-30_19-53-28_txt2img_random-walk/embeddings/output-0_direction-0_randwalkstep-start.pt"*
 - *"A digital illustration of a steampunk library with clockwork machines, 4k, detailed, trending on artstation, fantasy vivid colors"*
 - A beautiful castle beside a waterfall in the woods, by Josef Thoma, matte painting, trending on artstation HQ|
 - A digital illustration of a medieval town, 4k, detailed, trending on artstation, fantasy|low quality, low detail
 - A Hyperrealistic photograph of ancient Paris architectural ruins in a flooded apocalypse landscape of dead skyscrapers, eiffel tower, lens flares, 
 - A Hyperrealistic photograph of a landscape with ancient human ruins, lens flares,
 - A Hyperrealistic photograph of a futuristic city with cyberpunk skyscrapers, lens flares, 


#### Latent Noise Configurations

:seedling: ```inter_noises```: [2, *"./experiments/2023-03-30_19-53-28_txt2img_random-walk/embeddings/output-0_direction-0_randwalkstep-start.pt"*, 1, 0, 0, 2, 1, 0], ```height```: 768, ```width```: 1200
 
https://user-images.githubusercontent.com/63591221/229148028-0a951529-4555-4750-a649-d3dda2b04a40.mp4

:information_source: The results of this experiment are not in this repository due to their size.

### 5. Diffevolution
