# razerblade2017linux
Useful info from my experience installing Bodhi Linux on a Razer Blade 2017 model. Geared towards deep learning / rnn usage.

The latest release of Bodhi (as of July 2017) is built on Ubuntu 16.04.  It features Moshka as its window manager, which is a fork of Enlightenment E17.  It’s super lightweight and perfect if you are dual booting and want a fast window manager that is going to go easy on your CPU/GPU. 

Resize your existing Windows partition in Disk Management and allocate as much space as you’d like to give it.  Remember that the Nvidia drivers always seem to take up more room than they should.

The installation of Bodhi is straightforward.  Download the ISO from  http://www.bodhilinux.com and make a bootable flash drive from it.
This document assumes that you have some working linux knowledge and know how not to wipe your Windows partition when selecting your dual boot options during the prep of the linux disk.  Use at your own risk. :)

Nvidia drivers:
Run lspci to make sure it sees both the Intel and Nvidia card.  
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update && sudo apt install nvidia-381
Sudo apt-get install nvidia-modprobe

Run glxinfo | grep “OpenGL renderer” to make sure it’s seeing your card.

Run nvidia-smi and nvidia-settings to make sure you’re happy with what you see.

Reboot 

You are now ready to install docker-ce and nvidia-docker for torch-rnn

Install docker-ce first as nvidia-docker depends on docker engine.  First, add the key.

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce

Install nvidia-docker

wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb


And then test it by trying to run nvidia-smi in a container
nvidia-docker run --rm nvidia/cuda nvidia-smi

Pull the cuda8 torch-rnn docker image from xoryouyou/torch-rnn


Additional stuff for Razer:

Razerutils and Polychromatic -

sudo add-apt-repository ppa:terrz/razerutils

sudo apt update

sudo apt install python3-razer razer-kernel-modules-dkms razer-daemon razer-doc

sudo add-apt-repository ppa:lah7/polychromatic

sudo apt update

sudo apt install polychromatic

Disable touchpad when typing 
https://github.com/rolandguelle/razer-blade-stealth-linux/blob/master/etc/X11/xorg.conf.d/50-synaptics.conf

