# helper comfy accelerator for CUDA

The ridiculously easy guide.



This setup will:

- install the correct Triton, Flash-Attention and Sage Attention on your PC.
- All accelerators are custom compiled and work on ALL modern CUDA cards: 30xx(Ampere), 40xx(Lovelace), 50xx (Blackwell).
- works on Windows and Linux

You can use only Flash-attention or Sage-Attention at the same time. Since Sage is the best we will only activate that.


## Pre-Requisites

Lets accelerate ComfyUI!


First (applies to all guides!):
- you need python 3.12 installed. not conda python but the real python. 
    - On windows:
        - install it on your own from https://www.python.org/
        -  or download my one click installer:
            - `installer_win_python312.bat` from https://github.com/loscrossos/crossos_setup/tree/main/win/installers
    - Linux: you likely have it already if you already have comfy ui installed.

Install it and reboot your PC. this is the only step where you need admin rights. All of the following can (and should) be done from a non-admin account.


download `accelerated_270_312.txt` from this repository. Thats the only file you need. Put it somewhere where you can find it. This file is Cross-OS: it works under Windows and Linux. 

Depending on which install type you have you choose one of the three following guides:



# ComfyUI manual install (Windows Linux)

1. Download `accelerated_270_312.txt` from my repository and put it in your comfyUI folder.
2. Open a terminal in your comfyUI directory.
3. delete your old virtual environment (the ".venv" folder NOT the whole comfyUI directory!). We will recreate it.

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

































# Comfy Portable (Windows)



locate virtual env: Open a terminal console (no admin rights needed) in the folder where you extracted comfyUI portable.


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
the line with the version should have the value "+cu12x". It does not matter if its 2.7.0 or 2.7.1 or other values.

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