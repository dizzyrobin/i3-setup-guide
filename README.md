# i3-setup-guide
A setup guide for i3 windows manager in Ubuntu

## Install i3

```
sudo apt install i3
```

Reboot your PC and select i3 in the login screen.

## Install dependences

```
sudo apt install \
gcc \
g++ \
clang \
sphinx-common \
python-dbus \
i3blocks \
feh \
scrot \
fonts-font-awesome \
acpi \
pulseaudio \
util-linux \
rofi \
i3lock \
compton
```

## Setup i3blocks configuration

Put this in `~/.config/i3blocks/config`:

```
full_text= 
align=left
separator=false
separator_block_width=12

[spotify]
label=
command=/home/dizzyrobin/.config/i3blocks/spotify.py
color=#81b71a
interval=5

[separator]

[volume-pulseaudio]
command=/home/dizzyrobin/.config/i3blocks/volume-pulseaudio
interval=once
signal=1

[separator]

[calendar]
command=/home/dizzyrobin/.config/i3blocks/calendar
interval=1
label=

[separator]

[battery2]
command=/home/dizzyrobin/.config/i3blocks/battery2
markup=pango
interval=30

[separator]
```

(you **MUST** change the username *dizzyrobin* for your own username)

After that, copy the scripts in this repo folder `i3blocks-scripts` in your i3blocks config folder. Then, give them execution rights with `chmod +x`.

## Setup i3 configuration

Add this to your i3 configuration:

```
# Lock screen
bindsym $mod+p exec i3lock -c 000000 -e -f & sleep 2 && xset dpms force off

# Remove window titlebar
for_window [class="^.*"] border pixel 2

# Keys for volume
bindsym XF86AudioRaiseVolume exec amixer -q -D pulse sset Master 5%+ && pkill -RTMIN+1 i3blocks
bindsym XF86AudioLowerVolume exec amixer -q -D pulse sset Master 5%- && pkill -RTMIN+1 i3blocks
bindsym XF86AudioMute exec amixer -q -D pulse sset Master toggle && pkill -RTMIN+1 i3blocks

# For the calendar in i3blocks
for_window [class="Yad"] floating enable

# Network manager
exec --no-startup-id "nm-applet"

# Wallpaper
exec --no-startup-id feh --bg-scale ~/Pictures/i3-blue.png

# Start compton
exec --no-startup-id compton --inactive-dim 0.3 --focus-exclude '_NET_WM_NAME@:s = "rofi"'

# Screenshot utility
bindsym --release Print exec scrot -s /home/dizzyrobin/Pictures/screenshots/%Y-%m-%d_%H-%M-%S.png

```

Change the application launcher:

```
# Comment this line
# bindsym $mod+d exec dmenu_run

# Add this line
bindsym $mod+d exec "rofi -show run -color-window '#222222, #555566, #b1b4b3' -color-normal '#222222, #b1b4b3, #222222, #005577, #b1b4b3'"
```

Change the bar content:

```
bar {
    # Set up status_command to this
    status_command i3blocks
    
    # Put this line if you want to keep i3blocks only in your primary monitor
    output primary
}
```

## Add padding to termintator

Create the file `~/.config/gtk-3.0/gtk.css` with this content:
```
VteTerminal, vte-terminal {
    padding: 10px;
}
```

## Setup backlight controller (only if using intel_backlight)

Follow the instructions in https://github.com/HalosGhost/enlighten

After that, execute:

```
cd /sys/class/backlight/intel_backlight/
sudo chgrp video max_brightness
sudo chgrp video brightness
sudo chmod g+w brightness
```

And add this to the end of your i3 config file `~/.config/i3/config`

```
# Screen brightness
bindsym XF86MonBrightnessUp exec enlighten +10% && pkill -RTMIN+1 i3blocks
bindsym XF86MonBrightnessDown exec enlighten -10% && pkill -RTMIN+1 i3blocks
```

And this to the end of your i3blocks config file `~/.config/i3blocks/config`

```
[brightness]
command=enlighten | cut -d"(" -f2 | cut -d")" -f 1
interval=once
signal=1
label=☀️
min_width=☀️ 100%

[separator]
```


## Done!

Now reboot and you're done!

Additionally, I like to install this other utilities:

```
sudo apt install okteta texlive texlive-lang-spanish texlive-latex-extra gcc g++ vlc wxmaxima thunderbird libreoffice gparted git audacity gdb evince vim geany terminator meld gimp make automake mtools pavucontrol trash-cli
```
