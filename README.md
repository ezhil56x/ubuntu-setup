# Ubuntu 24.04 Setup

**Note**: Docker Desktop doesn't seem to work on Ubuntu 24.04 for now, will update when it's ready  
#### Update and Upgrade
```
sudo apt update && sudo apt upgrade
```

#### Install Chrome Browser

1. Download the .deb file from [https://www.google.com/intl/en_in/chrome/](https://www.google.com/intl/en_in/chrome/)
2. Install chrome browser with dpkg
```
sudo dpkg -i ~/Downloads/google-chrome-stable_current_amd64.deb
```

#### Remove Firefox
```
sudo snap remove --purge firefox
```

#### Install Basic Tools

```
sudo apt install git htop neofetch
```

#### Install NVIDIA Graphics Drivers
1. Find your nvidia-driver-xx (Eg: `nvidia-driver-535`)
```
nvidia-detector
```

2. Install the driver
```
sudo apt install nvidia-driver-535
```

#### Setup Ubuntu Pro
**Note**: If you remove snap from Ubuntu Livepatch won't work
Free, personal subscription for 5 machines for you or any business you own, or 50 machines for active [Ubuntu Community members](https://ubuntu.com/community/membership)

Login to your [Ubuntu One Account](https://ubuntu.com/pro/dashboard)

```
sudo pro attach <your_token_here>
```

#### Install Brave Browser
Follow this guide [https://brave.com/linux/](https://brave.com/linux/)

#### Install VS Code

1. Download the .deb file from [https://code.visualstudio.com/download](https://code.visualstudio.com/download)
2. Install vs code with dpkg
```
sudo dpkg -i ~/Downloads/code_x.xx.x-xxxxxxxxxx_amd64.deb
```

#### Install Discord
1. Download the .deb file from [https://discord.com/download](https://discord.com/download)
2. Install discord with dpkg
```
sudo dpkg -i ~/Downloads/discord-x.x.xx.deb
```

#### Install Obsidian
1. Download the .deb file from [https://obsidian.md/download](https://obsidian.md/download)
2. Install obsidian with dpkg
```
sudo dpkg -i ~/Downloads/obsidian_x.x.xx_amd64.deb
```

#### Install Flameshot
```
sudo apt install flameshot
```

#### Install OpenJDK 21

```
sudo apt install openjdk-21-jdk
```

#### Install Flatpak
[https://flatpak.org/setup/Ubuntu](https://flathub.org/en)

1. Install Flatpak and add Flathub repository
```
sudo apt install flatpak
```

```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

3. After installing flatpak, go to [https://flathub.org/](https://flathub.org/) and install the applications you use

**Note**: Some are official applications and some are community maintained. So do your research before installing any application for security reasons

#### Install Pyenv (Python Version Manager)
[https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)

1. Install the required packages
```
sudo apt-get install libncurses-dev libreadline-dev libssl-dev libsqlite3-dev tk-dev
```
2. Install Pyenv with this Automatic Installer
```
curl https://pyenv.run | bash
```

#### Install NVM (Node Version Manager)
[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

or 

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

#### Install oh-my-posh

```
mkdir ~/bin
```
1. Install oh-my-posh
```
curl -s https://ohmyposh.dev/install.sh | bash -s -- -d ~/bin
```
2. Add the executable to path and setup oh-my-posh by adding the following to `~/.bashrc`
```
export PATH=$PATH:/home/ezhil/bin
eval "$(oh-my-posh init bash --config ~/.cache/oh-my-posh/theme_name.omp.json)"
```

#### Battery Charging Limit
1. Check the BATx
```
ls /sys/class/power_supply/
```
The above command will display the BAT device name (For Eg: BAT0, BAT1)
In my case, it was BAT0

2. Check if your battery supports to limit battery charging
```
ls /sys/class/power_supply/BAT0
```
If you could see `charge_control_start_threshold`, then you battery supports it else skip this

3. Creating a service for Battery Charging

```
sudo nano /etc/systemd/system/battery-charge.service
```

Paste the following lines `Ctrl + Shift + v`
```
[Unit]
Description=Set Battery Charge Maximum Limit
After=multi-user.target
StartLimitBurst=0

[Service]
Type=oneshot
Restart=on-failure
ExecStart=/bin/bash -c 'echo 60 > /sys/class/power_supply/BAT0/charge_control_end_threshold'

[Install]
WantedBy=multi-user.target
```

4. Reload the daemon and start the service

```
systemctl enable battery-charge.service
```

```
systemctl daemon-reload
```

```
systemctl start battery-charge.service
```

#### Install auto-cpufreq
[https://github.com/AdnanHodzic/auto-cpufreq](https://github.com/AdnanHodzic/auto-cpufreq)

```
git clone https://github.com/AdnanHodzic/auto-cpufreq.git
cd auto-cpufreq && sudo ./auto-cpufreq-installer
```

#### ROG Machines
1. Physical mute mic button (M3) in ROG doesn't seem to work which can be fixed

```
sudo nano /etc/udev/hwdb.d/90-nkey.hwdb
```

2. Paste the following lines `Ctrl + Shift + V`

```
evdev:input:b0003v0B05p19B6*
  KEYBOARD_KEY_ff31007c=f20 # x11 mic-mute
```

3. Update hwdb
```
sudo systemd-hwdb update
```

```
sudo udevadm trigger
```


#### Apps I use from Flatpak

1. [Telegram](https://flathub.org/apps/org.telegram.desktop)
2. [OBS](https://flathub.org/apps/com.obsproject.Studio)
4. [Resources](https://flathub.org/apps/net.nokyan.Resources)


