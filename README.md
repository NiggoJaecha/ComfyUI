ComfyUI
=======
A powerful and modular stable diffusion GUI.
-----------
![ComfyUI Screenshot](comfyui_screenshot.png)

This ui will let you design and execute advanced stable diffusion pipelines using a graph/nodes/flowchart based interface. For some workflow examples and see what ComfyUI can do you can check out:
### [ComfyUI Examples](https://comfyanonymous.github.io/ComfyUI_examples/)

## Features
- Nodes/graph/flowchart interface to experiment and create complex Stable Diffusion workflows without needing to code anything.
- Fully supports SD1.x and SD2.x
- Asynchronous Queue system
- Many optimizations: Only re-executes the parts of the workflow that changes between executions.
- Command line option: ```--lowvram``` to make it work on GPUs with less than 3GB vram.
- Can load both ckpt and safetensors models/checkpoints. Standalone VAEs and CLIP models.
- Embeddings/Textual inversion
- Loras
- Loading full workflows (with seeds) from generated PNG files.
- Saving/Loading workflows as Json files.
- Nodes interface can be used to create complex workflows like one for [Hires fix](https://comfyanonymous.github.io/ComfyUI_examples/2_pass_txt2img/) or much more advanced ones.
- [Area Composition](https://comfyanonymous.github.io/ComfyUI_examples/area_composition/)
- Starts up very fast.
- Works fully offline: will never download anything.

Workflow examples can be found on the [Examples page](https://comfyanonymous.github.io/ComfyUI_examples/)

# Installing

Git clone this repo.

Put your SD checkpoints (the huge ckpt/safetensors files) in: models/checkpoints

Put your VAE in: models/vae

At the time of writing this pytorch has issues with python versions higher than 3.10 so make sure your python/pip versions are 3.10.

### AMD
AMD users can install rocm and pytorch with pip if you don't have it already installed:

```pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/rocm5.2```

### NVIDIA

Nvidia users should install torch using this command:

```pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu117```

Nvidia users should also install Xformers for a speed boost but can still run the software without it.

#### Troubleshooting

If you get the "Torch not compiled with CUDA enabled" error, uninstall torch with:

```pip uninstall torch```

And install it again with the command above.

### Dependencies

Install the dependencies by opening your terminal inside the ComfyUI folder and:

```pip install -r requirements.txt```

After this you should have everything installed and can proceed to running ComfyUI.


### I already have another UI for Stable Diffusion installed do I really have to install all of these dependencies?

You don't. If you have another UI installed and working with it's own python venv you can use that venv to run ComfyUI. You can open up your favorite terminal and activate it:

```source path_to_other_sd_gui/venv/bin/activate```

or on Windows:

With Powershell: ```"path_to_other_sd_gui\venv\Scripts\Activate.ps1"```

With cmd.exe: ```"path_to_other_sd_gui\venv\Scripts\activate.bat"```

And then you can use that terminal to run Comfyui without installing any dependencies. Note that the venv folder might be called something else depending on the SD UI.


# Running

```python main.py```

### For AMD 6700, 6600 and maybe others

Try running it with this command if you have issues:

```HSA_OVERRIDE_GFX_VERSION=10.3.0 python main.py```

# Notes

Only parts of the graph that have an output with all the correct inputs will be executed.

Only parts of the graph that change from each execution to the next will be executed, if you submit the same graph twice only the first will be executed. If you change the last part of the graph only the part you changed and the part that depends on it will be executed.

Dragging a generated png on the webpage or loading one will give you the full workflow including seeds that were used to create it.

You can use () to change emphasis of a word or phrase like: (good code:1.2) or (bad code:0.8). The default emphasis for () is 1.1. To use () characters in your actual prompt escape them like \\( or \\).

To use a textual inversion concepts/embeddings in a text prompt put them in the models/embeddings directory and use them in the CLIPTextEncode node like this (you can omit the .pt extension):

```embedding:embedding_filename.pt```

### Colab Notebook

To run it on colab you can use my [Colab Notebook](notebooks/comfyui_colab.ipynb) here: [Link to open with google colab](https://colab.research.google.com/github/comfyanonymous/ComfyUI/blob/master/notebooks/comfyui_colab.ipynb)

### Fedora

To get python 3.10 on fedora:
```dnf install python3.10```

Then you can:

```python3.10 -m ensurepip```

This will let you use: pip3.10 to install all the dependencies.

## How to increase generation speed?

The fp16 model configs in the CheckpointLoader can be used to load them in fp16 mode, depending on your GPU this will increase your gen speed by a significant amount.

You can also set this command line setting to disable the upcasting to fp32 in some cross attention operations which will increase your speed. Note that this will very likely give you black images on SD2.x models.

```--dont-upcast-attention```

## Support and dev channel

[Matrix room: #comfyui:matrix.org](https://app.element.io/#/room/%23comfyui%3Amatrix.org) (it's like discord but open source).

# QA

### Why did you make this?

I wanted to learn how Stable Diffusion worked in detail. I also wanted something clean and powerful that would let me experiment with SD without restrictions.

### Who is this for?

This is for anyone that wants to make complex workflows with SD or that wants to learn more how SD works. The interface follows closely how SD works and the code should be much more simple to understand than other SD UIs.
