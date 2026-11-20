Bug #1

    ./install-eSim.sh: line 70: syntax error near unexpected token }' ./install-eSim.sh: line 70: }' Fix: 69 fi 70 } 
#FIX#1 closed if clause section

Bug #2
    >./install-eSim.sh --install 
    >Detected Ubuntu Version: 
    >Running script: /home/balaji/eSim-2.5/install-eSim-scripts/install-eSim-25.04.sh --install
    >Enter proxy details if you are connected to internet thorugh proxy
    >Is your internet connection behind proxy? (y/n): c
    >Please select the right option
    >./install-eSim.sh: line 62: /: Is a directory
    >./install-eSim.sh: line 63: /: Is a directory
    >./install-eSim.sh: line 64: /: Is a directory
    >./install-eSim.sh: line 65: /: Is a directory
    >./install-eSim.sh: line 66: /: Is a directory
    >./install-eSim.sh: line 67: /: Is a directory
    >./install-eSim.sh: line 68: /: Is a directory
Fix
    > FULL_VERSION=$(lsb_release -d | grep -oP '\d+\.\d+(\.\d+)?') #fix3 changed regex to show release version with or without point releases

Bug #3 
    >Installing KiCad...........................
    >Ubuntu 25.04 detected.
KiCad 8.0 is already installed.
Fix (install-eSim-25.04.sh)
    >136  return 0 #changed to return 0 to resume install

Bug #4
# Specific dependency for canberra-gtk modules
    echo "Installing Gtk Canberra modules..........................."
    sudo apt install -y libcanberra-gtk3-module 
Fix(install-nghdl-24.04.sh)
    >Removed libcanberra-gtk-module 

Bug #5
    Unhandled version llvm 20.1.2


    Error! Kindly resolve above error(s) and try again.

    Aborting Installation...
Fix

    echo "Installing LLVM........................................"
    sudo apt install -y llvm-19 llvm-19-dev #fix forcing llvm 19

Bug #6
    cannot run /usr/bin/llvm-config
Fix(install-nghdl-24.04.sh)
 107   ./configure --with-llvm-config=/usr/bin/llvm-config-19 #added version number

Bug#7

    Build machine is: x86_64-linux-gnu
    Unhandled version llvm 19.1.7


    Error! Kindly resolve above error(s) and try again.

    Aborting Installation...
Fix Try v18
