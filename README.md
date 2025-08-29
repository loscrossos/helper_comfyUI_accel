# helper comfy accelerator for CUDA

The ridiculously easy guide.

---
**edit: AUG30  pls see latest update and use the  https://github.com/loscrossos/crossOS_acceleritor  project with the 280 file.**
you can follow this guide but for comfyUI you can use "acceleritor_python312torch280cu129_lite.txt". Stay tuned for another massive update soon.
---

This setup will:

- install the correct Triton, Flash-Attention and Sage Attention on your PC.
- All accelerators are custom compiled and work on ALL modern CUDA cards: 30xx(Ampere), 40xx(Lovelace), 50xx (Blackwell).
- works on Windows and Linux

You can use only Flash-attention or Sage-Attention at the same time. Since Sage is the best we will only activate that.

### News
- 2025.07.03: **Sageattention2++: v.2.2.0** (Pytorch 2.7.0 and 2.7.1). Read new chapter "the right file" for details.

## Pre-Requisites

Lets accelerate ComfyUI!


First (applies to all guides!):

### BACK UP YOUR INSTALL

before doing anything backup your install. 
You dont need to backip the whole comfy folder (which contains your models and what not)
You only need to backup the folder of your virtual environment. this will be some 6-10GB depending on your setup.


**on desktop install:**

its the folder: 

`%USERPROFILE%\Documents\ComfyUI\.venv`

**on portsble install:** 

its the `python_embeded`  folder. make a copy of it. if things go wrong delete the original and put the copy back in.

**on manual install**

its the virtual environment folder  you created when installing comfyUI. its usually named ".venv" or similar.

### The right file

For optimal results we need to find out your pytorch version. open a command window in your comfyUI folder and paste this:

```
python_embeded\python -m pip show torch
```
you will see somethin like this

````
d:\ComfyUI_windows_portable>python_embeded\python -m pip show torch

Name: torch
Version: 2.7.1+cu128
Summary: Tensors and Dynamic neural networks in Python with strong GPU acceleration
Home-page: https://pytorch.org/
Author: PyTorch Team
Author-email: packages@pytorch.org
License: BSD-3-Clause
Location: d:\gh_test\ComfyUI_windows_portable\python_embeded\Lib\site-packages
Requires: filelock, fsspec, jinja2, networkx, setuptools, sympy, typing-extensions
Required-by: accelerate, flash_attn, kornia, spandrel, torchaudio, torchsde, torchvision, xformers
````

You need the Version value: here `2.7.1`. the `+cu128` (or any other value with `cu`) says you are using the cuda enabled version.

If you have Torch `2.7.0` you can use this project.

If you have `2.7.1` then use my other project which is going to be the main project in the future: https://github.com/loscrossos/crossOS_acceleritor



### Needed Software

- you need python 3.12 installed. not conda python but the real python. 
    - On windows:
        - install it on your own from https://www.python.org/
        -  or download my one click installer:
            - `installer_win_python312.bat` from https://github.com/loscrossos/crossos_setup/tree/main/win/installers
    - Linux: you likely have it already if you already have comfy ui installed.

- on windows; you will need msvc installed. if not already installed you can install it like so:
on an admin console run (this is like 3gb download)

```
%userprofile%\AppData\Local\Microsoft\WindowsApps\winget install --id=Microsoft.VisualStudio.2022.BuildTools --force --override "--wait --passive --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 --add Microsoft.VisualStudio.Component.Windows11SDK.26100 --add  Microsoft.VisualStudio.Component.VC.CMake.Project"  -e  --silent --accept-package-agreements --accept-source-agreements
```


Install it and reboot your PC. this is the only step where you need admin rights. All of the following can (and should) be done from a non-admin account.


download `accelerated_270_312.txt` from this repository. Thats the only file you need. Put it somewhere where you can find it. This file is Cross-OS: it works under Windows and Linux. 

Depending on which install type you have you choose one of the three following guides:



# ComfyUI manual install (Windows Linux)

## First install

when first installing comfyUI you can set it right from the getgo. lets do it.

1. Download `accelerated_270_312.txt` file from my repository and put it in your comfyUI folder.
2. Open a terminal in your comfyUI directory.
3. Backup your old virtual environment (the ".venv" folder NOT the whole comfyUI directory!). We will recreate it. You will need to reinstall your node requirements afterwards if you had any. You can delete it if everything went right.

4. Create and activate a python virtual environment  

task       |  Windows                  | Linux
---        |  ---                      | ---
create venv|`py -3.12 -m venv .env_win`|`python3.12 -m venv .env_lin`
activate it|`.env_win\Scripts\activate`|`. ./.env_lin/bin/activate`


At this point you should see at the left of your prompt the name of your environment (e.g. `(.env_win)`)


5. Install the accelerator libraries (CrossOS):
```
pip install -r accelerated_270_312.txt
```

6. now install the comfy UI libraries

```
pip install -r requirements.txt
```

DONE!

now you can run comfyUI with the starter command (on your activated virtual environment):


```
python main.py --use-sage-attention 
python main.py  --fast fp16_accumulation --use-sage-attention 
```


## Update

if my files have been updated, the update process is easy: its the same as the initial process but we just dont create the virtual environment:


1. Download the new `accelerated_270_312.txt` from my repository and put it in your comfyUI folder.
2. Open a terminal in your comfyUI directory.
3. activate the python virtual environment 

task       |  Windows                  | Linux
---        |  ---                      | ---
activate it|`.env_win\Scripts\activate`|`. ./.env_lin/bin/activate`


At this point you should see at the left of your prompt the name of your environment (e.g. `(.env_win)`)


5. Install the accelerator libraries (CrossOS):
```
pip install -r accelerated_270_312.txt
```

6. now install the comfy UI libraries

```
pip install -r requirements.txt
```


enjoy





























# Comfy Portable (Windows)

quick video demo of the instructions below for portable install:

https://youtu.be/XKIDeBomaco?si=3ywduwYne2Lemf-Q


locate virtual env: Open a terminal console (no admin rights needed) in the folder where you extracted comfyUI portable.


The procedure is the same for the first time you use this file or if you update it with newer versions of the file.

### ensure the right version pre-requisites

**Check Python**: run this and ensure you get a value starting with "3.12". If not: do NOT proceed with this guide.

```
.\python_embeded\python.exe --version
```
e.g. `Python 3.12.10`

**Check torch**:
Now lets check torch is using CUDA 12. 

```
.\python_embeded\python.exe -m pip show torch
```
the line with the version should have the value "+cu12x".  


e.g.

`Version: 2.7.0+cu128`

### Patch comfyUI

```
.\python_embeded\python.exe -m pip install -r accelerated_270_312.txt

```

This will take a little after a while (it will dowload up to some 4GB of data).



Now we can check it worked:

```
.\python_embeded\python.exe -m pip show sageattention

```
you should see some output saying you have "Version: 2.1.1"




### enable starter

With any text editor open `run_nvidia_gpu.bat` and `run_nvidia_gpu_fast_fp16_accumulation.bat` add this at the end of the command:

```
 --use-sage-attention 
```

it should look like this:

**`run_nvidia_gpu.bat`**
```
.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build --use-sage-attention
```



**`run_nvidia_gpu_fast_fp16_accumulation.bat`**
```
.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build --fast fp16_accumulation --use-sage-attention
```


test it by starting any of them and your command line should say:

```
Using sage attention
```

thats it. enjoy.


 

















# ComfyUI Desktop (Windows)

quick vieeo demo for the instructions below:

https://youtu.be/Mh3hylMSYqQ?si=obbeq6QmPiP0KbSx

### ensure the right version pre-requisites
open a normal command terminal and run this. Ensure you get a value starting with "3.12". If not: do NOT proceed with this guide. You can however update comfyUI to the latest version, which comes with 3.12. then restart the guide.

```
%USERPROFILE%\Documents\ComfyUI\.venv\Scripts\python --version
```

run this and ensure the version has a cu12x (e.g. cu124) at the end. if it has 11x (e.g. cu118) then stop this guide and update your comfyUI.
```
%USERPROFILE%\Documents\ComfyUI\.venv\Scripts\pip3 show torch
```




### patch comfy

**locate virtual env**

Run this and ensure your prompt appears with a `(ComfyUI)` in the next line. If this step fails: do not proceed with this guide.

```
%USERPROFILE%\Documents\ComfyUI\.venv\Scripts\activate
```

now run this command to update the installation.

```
pip3 install -r accelerated_270_312.txt
```



```
%USERPROFILE%\Documents\ComfyUI\.venv\Scripts\pip3 show sageattention
```
you should see some output saying you have "Version: 2.1.1"


3) activate accelerator

open the comfy config:

```
notepad %USERPROFILE%\Documents\ComfyUI\user\default\comfy.settings.json
```


Find: `"Comfy.Server.LaunchArgs": {}` (should be line 8)

replace with this:

```
"Comfy.Server.LaunchArgs": {"use-sage-attention": ""},
```


If you have multiple entries ensure a comma "," is at the end of each except the last one.


test it by starting your command line should say:

```
Using sage attention
```


enjoy!


# Troubleshooting

if any of the checks fail then do not proceed with the guide.  You can however update your comfyUI installation to the latest version. It comes with python 3.12 and CUDA 12. then  restart the guide.

- if you have problems with sageattention just remove the activation entry "use-sage-attention" to the way it was before and your comfy will simply not use it. It will work with the normal pytorch attention just as before.

- Sage-Attention: For people who might be getting these errors in the console: 
    - tcc and includes
    - something along "python.h missing"
    - "Failed to compile. cc_cmd" 
    
    this is due to you not having Python properly installed. This comes from having e.g. comfyportable which brings its own python but its missing files.
    - you need to install python 3.12 on your system (not from conda!)
    - alternatively you can get the headers and copy them in the venv then it should work fine. you can take it from the main python 312 install. installing as described above should suffice.
    - you can follow this guide: https://github.com/woct0rdho/triton-windows?tab=readme-ov-file#8-special-notes-for-comfyui-with-embeded-python




- if you have problems post an issue with as much of the console errors as possible, your system info and ideally an example workflow so i can reproduce the error. will do my best to help