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

Bug#8
    FileNotFoundError: [Errno 2] No such file or directory: 'TerminalUi.ui'
    /usr/bin/esim: line 4: 82129 Aborted                 (core dumped) python3 Application.py
    Fix
        # Other variables
        base_dir = os.path.dirname(os.path.abspath(__file__))
        self.darkColor = True
        self.qProcess = qProcess
        self.args = args
        self.iconDir = os.path.abspath(os.path.join(base_dir, "../../images"))
        # self.darkColor = True
        # self.qProcess = qProcess
        # self.args = args
        # self.iconDir = "../../images"

        # Load the ui file
        uic.loadUi(os.path.join(base_dir, "TerminalUi.ui"), self)
        #uic.loadUi("TerminalUi.ui", self)

Bug#9
 ~$ esim
    Starting eSim......
    Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
    QTextBrowser: No document for file:///home/balaji/eSim-2.5/src/browser/library/browser/welcome.html
    Default workspace selected : /home/balaji/eSim-Workspace
    Traceback (most recent call last):
    File "/home/balaji/eSim-2.5/src/frontEnd/ProjectExplorer.py", line 138, in openProject
        lines = self.fopen.read()
    File "<frozen codecs>", line 325, in decode
    UnicodeDecodeError: 'utf-8' codec can't decode byte 0xae in position 464: invalid start byte
    /usr/bin/esim: line 4: 23869 Aborted                 (core dumped) python3 Application.py
Fix
   if (os.path.isfile(str(self.filePath))):
            # self.fopen = open(str(self.filePath), 'r')
            # lines = self.fopen.read()
            try:
                with open(str(self.filePath), 'r', encoding='utf-8') as f:
                    lines = f.read()
            except UnicodeDecodeError:
                QtWidgets.QMessageBox.warning(
                    self,
                    "Unsupported File Format",
                    f"Cannot open binary or non-UTF-8 file '{filename}' in the text editor."
                )
                return


Bug#10
    esim
    Starting eSim......
    Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
    QTextBrowser: No document for file:///home/balaji/eSim-2.5/src/browser/library/browser/welcome.html
    Default workspace selected : /home/balaji/eSim-Workspace
    Traceback (most recent call last):
    File "/home/balaji/eSim-2.5/src/frontEnd/ProjectExplorer.py", line 116, in openMenu
        if level == 0:
        ^^^^^
    UnboundLocalError: cannot access local variable 'level' where it is not associated with a value
    /usr/bin/esim: line 4: 25091 Aborted                 (core dumped) python3 Application.py
    balaji@U25:~$ 
Fix:
    indexes = self.treewidget.selectedIndexes()
        if not indexes: 
            return
    if len(indexes) > 0:
            level = 0
            index = indexes[0]
            while index.parent().isValid():
                index = index.parent()
                level += 1