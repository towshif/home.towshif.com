---
title: "Configuring a Fresh Ubuntu Install for Coders"
date: 2017-11-12T23:01:21-08:00
categories:
- Ubuntu
- install
- programs
- configurations
tags:
- ubuntu
- code
- shell
- zsh
- vim
- git

keywords:
- ubuntu
- code
- shell
- zsh
- vim
- git
- netbeans
- vscode
- intelliJ idea
- guake

#thumbnailImage: //example.com/image.jpg
---
>Warning: Linux noob content below.

# How to create a sound coding environment after a fresh ubuntu install

While I have been a coding enthusiast and Ubuntu has been my choice of OS as a hobbyist, I have always wanted to have a consistent coding environment across my installations without having to hunt for settings and configurations over the internet. Here I laid out a simple script with basic install procedures of various packages which has helped me and I would recommend for newbies. Note that most of my input is based on my limited experience and mostly focussed on Web development with platforms like nodejs, java, mysql, mongodb, Google cloud and some backend platforms including python. I did throw in some basic installations for browser (chrome), choice of editors (VSCode, Netbeans, IntelliJ, Vim) and some static html generators for blogging like Hugo, Jekyll and Sphynx (Python doc). I also tried to add configurations for my terminal zsh shell (guake terminal) with fonts and powerline packages that I like while working with git repos.

I used the following desktop configuration for this install however I dont think this has anything to do with HW. Even you can try the same on a shared VM if you like which I tested and worked for me.

* OS : Ubuntu 16.04 LTS
* cpu: Xeon quad core 2.36 ghz
* Ram: 24 Gb

While I have researched these over the internet and compiled the following to suit my needs I feel this is still not an exhaustive one. I have added comments in this install script as guidance and you can choose to comment out or add packages that suits your needs.

```
# filename: after-ubuntu-installer.sh
# ubuntu after install packages
# update repository cache
sudo apt-get update

# install essential programs
sudo apt-get install samba gdebi gparted curl openssh-server htop geany synaptic git vim nano default-jdk gimp gimp-data gimp-plugin-registry gimp-data-extras vlc browser-plugin-vlc yum

# Install Python tools (2.7) - Pip
sudo apt-get install python-pip python-dev build-essential
sudo pip install --upgrade pip
sudo pip install --upgrade virtualenv

# install essential databases: mongodb, mysql
# configure each separately
sudo apt-get install mysql-server mongodb-server

# Install hugo, jekyll
sudo apt-get install hugo jekyll


# install apache OR nginx
# for Apache @ https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04
# post install: configure /etc/apache2/apache2.conf
# verify with sudo apache2ctl configtest
# sudo apt-get install apache2

# for Nginx
# post install configure: /etc/nginx/nginx.conf
sudo apt-get install nginx

# configure Vim with themes and features @ https://github.com/amix/vimrc
git clone --depth=1 git://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_basic_vimrc.sh


# install node js
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

# install nvm
sudo apt-get install build-essential libssl-dev
curl -sL  https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh | bash
chmod +x install_nvm.sh
# install nvm locally for user in ~/
./install_nvm.sh
# export NVM_NODEJS_ORG_MIRROR=http://nodejs.or./dist


# install chrome
sudo apt-get install libnss3
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo gdebi google-chrome-stable_current_amd64.deb

# install VS CODE
# OR download *.deb file from vscode website and install using
# sudo dpkg -i <file>.deb
# sudo apt-get install -f # Install dependencies

curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

sudo apt-get update
sudo apt-get install code # or code-insiders

# install Netbeans 8.2 EE - download from the netbeans website and install with the following:
# chmod +x <installer-file-name>.
# use sudo (below) if you want to install on /opt directory
# sudo ./<installer-file-name>
# search for java_home using: (need to input while installing)
# echo $(readlink -f /usr/bin/java | sed "s:bin/java::")

# install teamviewer [in progress]
#sudo apt-get update

# install teamviewer [in progress]
#sudo apt-get update

# install terminal awesome with guake, oh-my-zsh & powerline
sudo apt-get update && sudo apt-get install zsh
chsh -s $(which zsh)
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
# Install powerline font package
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
# edit .zsh.rc → ZSH_THEME="agnoster"
# fc-cache -f -v
# add DEFAULT_USER="towshif" or user username to hide user@domain$

# Configure Samba / users <towshif> / pass (with smbpass)
# Change /etc/samba/smb.conf
# [Downloads]
#  browseable = yes
#  path = /home/towshif/Downloads
#  read only = no

# Add password for your user <towshif>
sudo smbpasswd -a towshif

# VSCode Sync Settings - gist token for upload
# token <input ID from github>
# Public GIST to share settings: Gist id: <input ID from github>

# install vs code extensions; here is a list of all common ones for web devs
code --install-extension AlanWalk.markdown-toc
code --install-extension bungcip.better-toml
code --install-extension chiehyu.vscode-astyle
code --install-extension DavidAnson.vscode-markdownlint
code --install-extension dbankier.vscode-instant-markdown
code --install-extension donjayamanne.jquerysnippets
code --install-extension donjayamanne.python
code --install-extension formulahendry.code-runner
code --install-extension goessner.mdmath
code --install-extension koehlma.markdown-math
code --install-extension lior-chamla.google-fonts
code --install-extension luggage66.VBScript
code --install-extension mdickin.markdown-shortcuts
code --install-extension ms-vscode.csharp
code --install-extension ms-vscode.Theme-MarkdownKit
code --install-extension msjsdiag.debugger-for-chrome
code --install-extension mushan.vscode-paste-image
code --install-extension robertohuertasm.vscode-icons
code --install-extension searKing.preview-vscode
code --install-extension Shan.code-settings-sync
code --install-extension tht13.python
code --install-extension tushortz.python-extended-snippets
```