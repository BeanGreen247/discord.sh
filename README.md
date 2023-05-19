# discord.sh
script for installing discord from tarball (tar.gz), can be used for updating as well

how to use?
```bash
bash discord.sh
```
discord.sh
```bash
#!/bin/bash
#vars
echo -n "Enter root password: "
read -s password
pass="$password"
discordLatestVer="$(curl -I --silent 'https://discord.com/api/download?platform=linux&format=tar.gz' | grep "location" | cut -d '/' -f 6)"

cd $HOME/Downloads/
rm -rf discord.tar.gz 2> /dev/null
echo "Latest version: "$discordLatestVer;
echo "Downloading..."
curl -# -L -o discord.tar.gz https://dl.discordapp.net/apps/linux/$discordLatestVer/discord-$discordLatestVer.tar.gz
echo "Installing..."
echo $pass | sudo -S tar -xvzf discord.tar.gz -C /opt
echo $pass | sudo -S ln -sf /opt/Discord/Discord /usr/bin/Discord
echo "[Desktop Entry]
Name=Discord
Comment=Free voice and text chat for gamers
StartupWMClass=discord
Version=1.0
Icon=/opt/Discord/discord.png
Exec=/usr/bin/Discord --enable-accelerated-mjpeg-decode --enable-accelerated-video --ignore-gpu-blacklist --enable-native-gpu-memory-buffers --enable-gpu-rasterization 
Type=Application
Terminal=false
StartupNotify=true
Categories=Network;InstantMessaging;" | tee -a temp.discord.desktop
echo $pass | sudo -S mv temp.discord.desktop /opt/Discord/discord.desktop
echo $pass | sudo -S cp -r /opt/Discord/discord.desktop /usr/share/applications
echo "Cleanup..."
rm -rf discord.tar.gz
cd $HOME
```
