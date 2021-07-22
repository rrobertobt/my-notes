# Fedora Linux

Here you'll find some post-install tips and must do's for the Fedora Linux distribution.

## Post-install

### Get better download speeds when using DNF

Some users might get significantly slow download speeds when using the **dnf**  package manager, but it can be improved by configuring it by modifying its config file.

You can use a terminal-based text editor like `vi`:
Run `sudo vi /etc/dnf/dnf.conf`

or a GUI text editor like `gedit`:  Run `sudo gedit /etc/dnf/dnf.conf`

And then add the following lines to the end of the file:
```
max_parallel_downloads=10
fastestmirror=True
```

And then just save it. No need to logout or reboot, these new settings will be applied the next time you run `dnf`



### Add [rpmfusion](https://rpmfusion.org/) repositories for additional software

Run the following command on a terminal:

```shell
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

After installing the additional repositories, you can install more useful software that's not available on the official repositories.

### Install multimedia codecs
Fedora Linux only provides open source software out-of-the-box, and therefore, lacks some multimedia codecs.

To install the complements multimedia packages needed by gstreamer enabled applications, run:

```shell
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

The following command will install the sound-and-video complement packages needed by some applications:

```shell
sudo dnf groupupdate sound-and-video
```

### Add [flathub](https://flathub.org/home) and flathub-beta repositories for flatpak applications
Fedora provides its own flatpak repository by default, but doesn't provide as much applications as flathub.

To add flathub repositories run:

- Stable
```shell
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
- Beta
```shell
flatpak remote-add --if-not-exists flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
```
**(Optional) Install [Flatseal](https://flathub.org/apps/details/com.github.tchx84.Flatseal)**

Flatseal lets you easily manage permissions of Flatpak applications (access to devices, user's files) using a GUI instead of having to deal with long commands on the terminal. To install it run:

```shell
flatpak install flathub com.github.tchx84.Flatseal
```

### NVIDIA graphics cards drivers

In case you have and NVIDA graphics card on your system, it's recommended to install the proprietary drivers to get better performance.

To install the driver (kernel module) run:

```shell
sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs xorg-x11-drv-nvidia-libs.i686
```

Optional, but recommended: To install CUDA, and get CUDA/NVENC/NVDEC support run:

```shell
sudo dnf install xorg-x11-drv-nvidia-cuda
```

After finishing the installation, run:

```sudo akmods --force``` and ```sudo dracut --force``` and then ```reboot```

For setting the NVIDIA card as primary (only on supported cards), refer to: [this page and post](https://ask.fedoraproject.org/t/optimus-setting-the-nvidia-gpu-as-primary-rpmfusion-in-fedora-32-workstation/6550/6)

### Free fonts replacments

**[Better fonts for Fedora](https://github.com/silenc3r/fedora-better-fonts)** provides free substitutions for popular proprietary fonts from Microsoft and Apple operating systems. And also provides an optional package to enable Subpixel antialiasing (Can be enabled on GNOME Tweaks too)

**Installation:**

1. Enable [COPR](https://copr.fedorainfracloud.org/) repository:
		`sudo dnf copr enable dawid/better_fonts -y`
2. Install packages:
- For font replacements: `sudo dnf install fontconfig-font-replacements -y`
- For enabling Subpixel antialiasing: `sudo dnf install fontconfig-enhanced-defaults -y`
		
And then log out and log in again or reboot to see the effects.

### RAR archives support

Fedora doesn't provide RAR archives support by default, but thanks to rpmfusion it can be installed with the command:

```shell
sudo dnf install unrar
```

### GUI for managing compressed archives

To install GNOME's archive manager (file-roller) run:

```shell
sudo dnf install file-roller
```

### Add support for virtual video devices

Programms like OBS and DroidCam, lets you use alternative video sources, as a video capture device and use it on other programms (like replacing your webcam's video for another video source)

To enable this capability, you need to install the v4l2loopback kernel module which allows to create V4L2 loopback devices

To install it, run:

```shell
sudo dnf install v4l2loopback
```
