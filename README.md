# Kohya's GUI

This repository provides a Windows-focused Gradio GUI for [Kohya's Stable Diffusion trainers](https://github.com/kohya-ss/sd-scripts). The GUI allows you to set the training parameters and generate and run the required CLI commands to train the model.

If you run on Linux and would like to use the GUI, there is now a port of it as a docker container. You can find the project [here](https://github.com/P2Enjoy/kohya_ss-docker).

### Table of Contents

- [Tutorials](#tutorials)
- [Required Dependencies](#required-dependencies)
  - [Linux/macOS](#linux-and-macos-dependencies)
- [Installation and Upgrading](#installation-and-upgrading)
  - [Runpod](#runpod)
  - [macOS, Windows, Linux, BSD](#macos-windows-linux-bsd)
    - [Configuration](#configuration)
      - [Command Line Arguments](#command-line-arguments)
      - [Configuration File](#configuration-file)
    - [Running Kohya_SS](#running-kohya_ss)
    - [CUDNN 8.6](#optional--cudnn-86)
- [Upgrading](#upgrading)
  - [Windows](#windows-upgrade)
  - [Linux/macOS](#linux-and-macos-upgrade)
- [Launching the GUI](#starting-gui-service)
  - [Windows](#launching-the-gui-on-windows)
  - [Linux/macOS](#launching-the-gui-on-linux-and-macos)
  - [Direct Launch via Python Script](#launching-the-gui-directly-using-kohyaguipy)
- [Dreambooth](#dreambooth)
- [Finetune](#finetune)
- [Train Network](#train-network)
- [LoRA](#lora)
- [Troubleshooting](#troubleshooting)
  - [Page File Limit](#page-file-limit)
  - [No module called tkinter](#no-module-called-tkinter)
  - [FileNotFoundError](#filenotfounderror)
  - [Installation Issues](#installation-issues)
    - [General Installation Workflow](#general-installation-workflow)
- [Change History](#change-history)

## Tutorials

<details>
<summary>How to Create a LoRA Part 1: Dataset Preparation</summary>

[How to Create a LoRA Part 1: Dataset Preparation](https://www.youtube.com/watch?v=N4_-fB62Hwk):

[![LoRA Part 1 Tutorial](https://img.youtube.com/vi/N4_-fB62Hwk/0.jpg)](https://www.youtube.com/watch?v=N4_-fB62Hwk)

</details>

<br>

<details>
<summary>How to Create a LoRA Part 2: Training the Model</summary>

[How to Create a LoRA Part 2: Training the Model](https://www.youtube.com/watch?v=k5imq01uvUY):

[![LoRA Part 2 Tutorial](https://img.youtube.com/vi/k5imq01uvUY/0.jpg)](https://www.youtube.com/watch?v=k5imq01uvUY)

</details>

## Installation and Upgrading

### Required Dependencies

These dependencies are taken care of via `setup.sh` or `setup.ps1` in the installation section. 

No additional steps should be needed unless the scripts inform you otherwise.

- Install [Python 3.10](https://www.python.org/ftp/python/3.10.9/python-3.10.9-amd64.exe) 
  - make sure to tick the box to add Python to the 'PATH' environment variable
- Install [Git](https://git-scm.com/download/win)
- Install [Visual Studio 2015, 2017, 2019, and 2022 redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe)

### Runpod
Follow the instructions found in this discussion: https://github.com/bmaltais/kohya_ss/discussions/379

### macOS, Windows, Linux, BSD
To set up and install the application, use the provided setup scripts depending on your operating system:

Windows: Use <ins>setup.ps1</ins>. <ins>setup.bat</ins> is also available, but considered legacy.

Non-Windows: Use <ins>setup.sh</ins> or <ins>setup.ps1</ins> if you have pwsh available.

**Both setup scripts accept the same command-line arguments, and they will execute launcher.py with the same arguments.**

#### Running Kohya_SS

There are many configuration options which you can find just below this section. Here are some examples on how to run the scripts. They also have help functions.

Default Settings: 
```bash
# Windows
.\setup.ps1

# Linux / Non-Windows / Cygwin, Msys, etc
./setup.sh
```

Custom Settings:
```bash
# Windows
.\setup.ps1 -Listen 192.168.1.100 -Username myusername -Password mypassword -ServerPort 8000 -Interactive $true -RunPod $true `
-Branch mybranch -Dir "C:\path\to\kohya_ss" -GitRepo "https://github.com/myfork/kohya_ss.git"

# Linux / Non-Windows / Cygwin, Msys, etc
./setup.sh -l 192.168.1.100 -u myusername -p mypassword -s 8000 -i -r \
--branch mybranch --dir "/path/to/kohya_ss" --git_repo "https://github.com/myfork/kohya_ss.git"
```

<details>
<summary>Bypass all setup steps, installation checks, and Python validations and run the GUI directly</summary>

Bypass Python, git, and tk checks by running launcher.py:
```bash
# Windows
python .\launcher.py --listen 192.168.1.100 --username myusername --password mypassword --server_port 8000 --interactive --runpod `
--branch mybranch --dir "C:\path\to\kohya_ss" --git_repo "https://github.com/myfork/kohya_ss.git"

# Linux / Non-Windows / Cygwin, Msys, etc
python3 launcher.py --listen 192.168.1.100 --username myusername --password mypassword --server_port 8000 --interactive --runpod \
--branch mybranch --dir "/path/to/kohya_ss" --git_repo "https://github.com/myfork/kohya_ss.git"
```

</details>

<br>

<details>
<summary>Bypass all setup steps, installation checks, and Python validations and run the GUI directly</summary>

Bypass all setup steps, installation checks, and Python validations and run the GUI directly:
```bash
# Windows
python .\launcher.py --listen 192.168.1.100 --username myusername --password mypassword --server_port 8000 --exclude-setup

# Linux / Non-Windows / Cygwin, Msys, etc
python3 launcher.py --listen 192.168.1.100 --username myusername --password mypassword --server_port 8000 --exclude-setup
```

</details>

<br>

<details>
<summary>Permission Errors when running setup.ps1</summary>

If you get any errors about permissions running the setup.ps1 script on Windows try the following:
```pwsh
$Policy = Get-ExecutionPolicy -Scope CurrentUser; 
if ($Policy -eq "Restricted" -or $Policy -eq "AllSigned") { 
    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force 
}
```

This command does the following:

1. Retrieves the current execution policy for the current user.
2. If the policy is set to Restricted or AllSigned, it changes the policy to RemoteSigned for the current user only, allowing local unsigned scripts to run. The -Force flag is used to bypass the confirmation prompt.

</details>


#### Configuration

<details>
<summary>Command Line Arguments</summary>

##### Command Line Arguments

The following command-line arguments are supported by setup.ps1 and setup.sh:

```bash
-b BRANCH or --branch=BRANCH: Select which branch of kohya to check out on new installs.
-d DIR or --dir=DIR: The full path you want kohya_ss installed to.
-g REPO or --git_repo=REPO: You can optionally provide a git repo to check out for runpod installation. Useful for custom forks.
-h or --help: Show help information.
-i or --interactive: Interactively configure accelerate instead of using the default config file.
-n or --no-git-update: Do not update kohya_ss repo. No git pull or clone operations.
-p or --public: Expose public URL in runpod mode. Won't have an effect in other modes.
-r or --runpod: Forces a runpod installation. Useful if detection fails for any reason.
-s or --skip-space-check: Skip the 10Gb minimum storage space check.
-v or --verbose: Increase verbosity levels up to 3.
```

GUI Arguments
The following command-line arguments are passed through from setup.ps1 or setup.sh to launcher.py and then to kohya_gui.py. 
Use them in the same manner is the above arguments:

```bash
--listen or -l: The IP address to listen on (default: 127.0.0.1).
--username or -u: The username for the GUI (default: empty string).
--password or -p: The password for the GUI (default: empty string).
--server_port or -s: The server port for the GUI (default: 8080).
--inbrowser or -i: Launch the GUI in the default web browser (default: false).
--share or -r: Share the GUI over the network (default: false).
```

Launcher-specific Arguments
The following command-line arguments are passed through from setup.ps1 or setup.sh to launcher.py and then to kohya_gui.py. 
Use them in the same manner is the above arguments:

```bash
--listen or -l: The IP address to listen on (default: 127.0.0.1).
--username or -u: The username for the GUI (default: empty string).
--password or -p: The password for the GUI (default: empty string).
--server_port or -s: The server port for the GUI (default: 8080).
--inbrowser or -i: Launch the GUI in the default web browser (default: false).
--share or -r: Share the GUI over the network (default: false).
```

</details>
<br>
<details>
<summary>Configuration File</summary>

##### Configuration File
The setup scripts look for a configuration file called install_config.yaml in various locations to determine the command-line arguments for the installation process. The order in which the scripts search for this file is as follows:

The path specified by a variable in the script itself (if any).
For Windows users: the kohya_ss folder within your AppData or LocalAppData directories.
For non-Windows users: a hidden folder named .kohya_ss in your home directory.
The installation directory you've chosen for kohya_ss.
The kohya_ss folder within your user profile directory.
The same directory as the setup script.
The setup scripts will use the first install_config.yaml file they find in this order. This allows you to place your configuration file in a location that suits your needs, making it easy for you to customize the installation process. If you're not familiar with some of these locations, don't worry—simply placing the configuration file in the same directory as the setup script is a straightforward and effective option.

</details>

<br>

<details>
<summary>Optional: CUDNN 8.6</summary>

### Optional: CUDNN 8.6

This step is optional but can improve the learning speed for NVIDIA 30X0/40X0 owners. It allows for larger training batch size and faster training speed.

Due to the file size, I can't host the DLLs needed for CUDNN 8.6 on Github. I strongly advise you download them for a speed boost in sample generation (almost 50% on 4090 GPU) you can download them [here](https://b1.thefileditch.ch/mwxKTEtelILoIbMbruuM.zip).

To install, simply unzip the directory and place the `cudnn_windows` folder in the root of the this repo.

Run the following commands to install:

```
.\venv\Scripts\activate

python .\tools\cudann_1.8_install.py
```

Once the commands have completed successfully you should be ready to use the new version. MacOS support is not tested and has been mostly taken from https://gist.github.com/jstayco/9f5733f05b9dc29de95c4056a023d645

</details>

## Launching the GUI directly using kohya_gui.py

To run the GUI directly bypassing the wrapper scripts, simply use this command:

```
launcher.py -x 
```

## Dreambooth

You can find the dreambooth solution specific here: [Dreambooth README](train_db_README.md)

## Finetune

You can find the finetune solution specific here: [Finetune README](fine_tune_README.md)

## Train Network

You can find the train network solution specific here: [Train network README](train_network_README.md)

## LoRA

Training a LoRA currently uses the `train_network.py` code. You can create a LoRA network by using the all-in-one `gui.cmd` or by running the dedicated LoRA training GUI with:

```
.\venv\Scripts\activate

python lora_gui.py
```

Once you have created the LoRA network, you can generate images via auto1111 by installing [this extension](https://github.com/kohya-ss/sd-webui-additional-networks).

## Troubleshooting

<details>
<summary>Page File Limit</summary>

### Page File Limit

- X error relating to `page file`: Increase the page file size limit in Windows.

</details>

<details>
<summary>No module called tkinter</summary>

### No module called tkinter

- Re-install [Python 3.10](https://www.python.org/ftp/python/3.10.9/python-3.10.9-amd64.exe) on your system.

</details>

<details>
<summary>FileNotFoundError</summary>

### FileNotFoundError

This is usually related to an installation issue. Make sure you do not have any python modules installed locally that could conflict with the ones installed in the venv:

1. Open a new powershell terminal and make sure no venv is active.
2.  Run the following commands:

```
pip freeze > uninstall.txt
pip uninstall -r uninstall.txt
```

This will store your a backup file with your current locally installed pip packages and then uninstall them. Then, redo the installation instructions within the kohya_ss venv.

</details>

### Installation Issues

<details>
<summary>General Installation Workflow</summary>

#### General Installation Workflow
1. Run setup.ps1 on Windows or setup.sh on non-Windows operating systems with the desired command-line arguments.
2. The setup script will execute launcher.py with the same arguments.
3. launcher.py will pass the command-line arguments through to kohya_gui.py, which will use these arguments to configure the GUI and other settings according to your preferences.
Now the workflow is complete, and your application is set up and configured. 

You can run launcher.py whenever you want to launch the application with the specified settings.

</details>

<details>
<summary>Change History</summary>

## Change History

* 2024/04/01 (v21.4.0)
    - Improved linux and macos installation and updates script. See README for more details. Many thanks to @jstayco and @Galunid for the great PR!
    - Fix issue with "missing library" error.
* 2023/04/01 (v21.3.9)
    - Update how setup is done on Windows by introducing a setup.bat script. This will make it easier to install/re-install on Windows if needed. Many thanks to @missionfloyd for his PR: https://github.com/bmaltais/kohya_ss/pull/496
* 2023/03/30 (v21.3.8)
    - Fix issue with LyCORIS version not being found: https://github.com/bmaltais/kohya_ss/issues/481
* 2023/03/29 (v21.3.7)
    - Allow for 0.1 increment in Network and Conv alpha values: https://github.com/bmaltais/kohya_ss/pull/471 Thanks to @srndpty
    - Updated Lycoris module version
* 2023/03/28 (v21.3.6)
    - Fix issues when `--persistent_data_loader_workers` is specified.
        - The batch members of the bucket are not shuffled.
        - `--caption_dropout_every_n_epochs` does not work.
        - These issues occurred because the epoch transition was not recognized correctly. Thanks to u-haru for reporting the issue.
    - Fix an issue that images are loaded twice in Windows environment.
    - Add Min-SNR Weighting strategy. Details are in [#308](https://github.com/kohya-ss/sd-scripts/pull/308). Thank you to AI-Casanova for this great work!
        - Add `--min_snr_gamma` option to training scripts, 5 is recommended by paper.
        - The Min SNR gamma fiels can be found unser the advanced training tab in all trainers.
    - Fixed the error while images are ended with capital image extensions. Thanks to @kvzn. https://github.com/bmaltais/kohya_ss/pull/454
* 2023/03/26 (v21.3.5)
    - Fix for https://github.com/bmaltais/kohya_ss/issues/230
    - Added detection for Google Colab to not bring up the GUI file/folder window on the platform. Instead it will only use the file/folder path provided in the input field.
* 2023/03/25 (v21.3.4)
    - Added untested support for MacOS base on this gist: https://gist.github.com/jstayco/9f5733f05b9dc29de95c4056a023d645

    Let me know how this work. From the look of it it appear to be well tought out. I modified a few things to make it fit better with the rest of the code in the repo.
    - Fix for issue https://github.com/bmaltais/kohya_ss/issues/433 by implementing default of 0.
    - Removed non applicable save_model_as choices for LoRA and TI.
* 2023/03/24 (v21.3.3)
    - Add support for custom user gui files. THey will be created at installation time or when upgrading is missing. You will see two files in the root of the folder. One named `gui-user.bat` and the other `gui-user.ps1`. Edit the file based on your prefered terminal. Simply add the parameters you want to pass the gui in there and execute it to start the gui with them. Enjoy!
* 2023/03/23 (v21.3.2)
    - Fix issue reported: https://github.com/bmaltais/kohya_ss/issues/439
* 2023/03/23 (v21.3.1)
    - Merge PR to fix refactor naming issue for basic captions. Thank @zrma
* 2023/03/22 (v21.3.0)
    - Add a function to load training config with `.toml` to each training script. Thanks to Linaqruf for this great contribution!
        - Specify `.toml` file with `--config_file`. `.toml` file has `key=value` entries. Keys are same as command line options. See [#241](https://github.com/kohya-ss/sd-scripts/pull/241) for details.
        - All sub-sections are combined to a single dictionary (the section names are ignored.)
        - Omitted arguments are the default values for command line arguments.
        - Command line args override the arguments in `.toml`.
        - With `--output_config` option, you can output current command line options  to the `.toml` specified with`--config_file`. Please use as a template.
    - Add `--lr_scheduler_type` and `--lr_scheduler_args` arguments for custom LR scheduler to each training script. Thanks to Isotr0py! [#271](https://github.com/kohya-ss/sd-scripts/pull/271)
        - Same as the optimizer.
    - Add sample image generation with weight and no length limit. Thanks to mio2333! [#288](https://github.com/kohya-ss/sd-scripts/pull/288)
        - `( )`, `(xxxx:1.2)` and `[ ]` can be used.
    - Fix exception on training model in diffusers format with `train_network.py` Thanks to orenwang! [#290](https://github.com/kohya-ss/sd-scripts/pull/290)
    - Add warning if you are about to overwrite an existing model: https://github.com/bmaltais/kohya_ss/issues/404
    - Add `--vae_batch_size` for faster latents caching to each training script. This  batches VAE calls.
        - Please start with`2` or `4` depending on the size of VRAM.
    - Fix a number of training steps with `--gradient_accumulation_steps` and `--max_train_epochs`. Thanks to tsukimiya!
    - Extract parser setup to external scripts. Thanks to robertsmieja!
    - Fix an issue without `.npz` and with `--full_path` in training.
    - Support extensions with upper cases for images for not Windows environment.
    - Fix `resize_lora.py` to work with LoRA with dynamic rank (including `conv_dim != network_dim`). Thanks to toshiaki!
    - Fix issue: https://github.com/bmaltais/kohya_ss/issues/406
    - Add device support to LoRA extract.

</details>