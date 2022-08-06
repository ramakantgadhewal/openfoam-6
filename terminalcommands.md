Install WSL in Windows.
Install Linux distro Ubuntu.

Install Geany.
Install Xming.

##
Open Xming Server in windows
Find the host adderess as "0.0"
Set DISPLAY IP using
->$export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
then type
install x11 clients
-> sudo apt-get install xbase-clients
->$geany
it should open the geany editor with the help of Xming server

# expired certificates of sourceforge.net so I was not able to create repository
->$sudo apt install ca-certificates

# Add the repository
->$curl https://dl.openfoam.com/add-debian-repo.sh | sudo bash

# Install preferred package. Eg,
->$sudo apt-get install openfoam2112-default

# Use the openfoam shell session. Eg,
->$openfoam2112

# to go to the openfoam tutorial location
-> cd $FOAM_TUTORIALS
or 
-> cd /usr/lib/openfoam/openfoam2112

## install libx11-xcb1
# for the error "Unable to launch paraview: Could not load the Qt platform plugin"
sudo apt-get install libx11-xcb1

# paraFoam to touch all files
paraFoam -touch-all

#Install Blender and swiftblock
# from https://openfoamwiki.net/index.php/Contrib/SwiftBlock
# https://github.com/flowkersma/swiftBlock
sudo apt install blender
sudo apt install python3-pip
pip3 install numpy
mkdir -p $HOME/.config/blender/2.xx/scripts/addons
cd $HOME/.config/blender/2.xx/scripts/addons
git clone https://github.com/flowkersma/swiftBlock/