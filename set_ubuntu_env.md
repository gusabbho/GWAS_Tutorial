# Problem in WSL2 and Ubuntu

1. First, check if any distributions are registered:

     `wsl --shutdown
    `

     `wsl --uninstall
    `

2. restart your computer.

3. After restart, open PowerShell as administrator and run:

    `wsl --install -d Ubuntu-22.04
    `
    
# If the above steps didn't work do the followings:




1. First, check if any distributions are registered:
```powershell
wsl --list
```

2. Let's remove any potential broken installations. Run these commands:
```powershell
wsl --unregister Ubuntu
wsl --unregister Ubuntu-22.04
```

3. Then let's check your WSL version:
```powershell
wsl --status
```

4. Let's also try to update the WSL kernel:
```powershell
wsl --update
```

5. After that, try installing again:
```powershell
wsl --install -d Ubuntu-22.04
```



## installing miniconda 

I'll help you install Miniconda on your WSL Ubuntu. Here's the step-by-step process:

1. First, open your Ubuntu WSL and get the Miniconda installation script:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

2. Give execute permission to the script:
```bash
chmod +x Miniconda3-latest-Linux-x86_64.sh
```

3. Run the installation script:
```bash
./Miniconda3-latest-Linux-x86_64.sh
```

4. During installation:
   - Press Enter to review the license
   - Type "yes" to accept the license terms
   - Press Enter to confirm the installation location (or specify a different one)
   - When asked "Do you wish to initialize Miniconda3?", type "yes"

5. After installation, either:
   - Close and reopen your terminal, OR
   - Reload your bash profile:
   ```bash
   source ~/.bashrc
   ```

6. Verify the installation:
```bash
conda --version
```

7. Optional but recommended - update conda:
```bash
conda update conda
```

# now lets make an environment which has a gpu enabled Tensorflow.
### note you can follow these steps from this <div style="position: relative; width: 100%; padding-bottom: 56.25%">
<iframe src="https://www.youtube.com/embed/1HzYU2_t3yc" 
        title="Video" frameborder="0" allowfullscreen
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        style="position: absolute; width: 100%; height: 100%;">
</iframe>
</div>

1. install gcc for NVIDIA Cuda

`sudo apt install gcc --fix-missing`

2. Download NVIDA Cuda from [ here](https://developer.nvidia.com/cuda-11-8-0-download-archive)

3. Based on the instructions from the above website should enter these codes.

`wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin`

`sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
`

`wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb`

`sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/`

`sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
`
`sudo apt-get update
`

`sudo apt-get -y install cuda
`

### post installation 

got to the root directroy.

`cd ~`

`nano .bashrc
`

insert the

*export PATH=/usr/local/cuda-11.8/bin${PATH:+:${PATH}}*

at the end of the .bashrc file and the save the .bashrc and exit.

`source ~/.bashrc
`
you can find the added path with the followin command:

`echo $PATH
`

the install the cuda with:

`sudo apt install nvidia-cuda-toolkit
`


## make a virtual environment and tensorflow gpu. based on this website. [steps](https://www.tensorflow.org/install/pip?_gl=1*f8gqtv*_up*MQ..*_ga*MTg1MjY4OTMxMS4xNzMxMzM1MDMx*_ga_W0YLR4190T*MTczMTMzNTAzMS4xLjAuMTczMTMzNTAzMS4wLjAuMA..)

`conda create --name RNN python=3.10.0
`

`conda activate RNN
`

`pip install --upgrade pip
`

`pip install tensorflow[and-cuda]
`

`python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"`


# Get a backup of your Ubuntu distribution


