# Report:eSim Upgradation Task 4 -  Winter Internship 2026

| Field | Details |
| :--- | :--- |
| **Name** | Murugeshan Velappann Balaji |
| **Email** | mvbalaji200@gmail.com |

Test Environment:
Static hostname: U25
Icon name: computer-laptop
Chassis: laptop
Boot ID: 482aaf9c0f8e4679a677812c0a223580
Operating System: Ubuntu 25.04
Kernel: Linux 6.14.0-37-generic
Architecture: x86-64
Hardware Vendor: ASUSTeK COMPUTER INC.
Hardware Model: ZenBook UX325SA_UM325SA

## Bugs Encountered

### Script Bugs

| # | Script / File | Bug & Log | Fix |
| :--- | :--- | :--- | :--- |
| 1 | [install-eSim.sh](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/install-eSim.sh) | Open if clause section at line 70 in ([debug1.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug1.log)) | Closed open if clause section at line 70 |
| 2 | [install-eSim.sh](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/install-eSim.sh) | Implement updated regex to show Ubuntu versions with point releases,Fix echo Ubuntu Version in terminal output([debug2.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug2.log)) | Implemented updated regex to show Ubuntu versions with point releases,Fixed script to echo Ubuntu Version in terminal output (`\d+\.\d+(\.\d+)?`) |
| 3 | [install-eSim-25.04.sh](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/install-eSim-scripts/install-eSim-25.04.sh) | Script was terminating once KiCad was being installed([debug3.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug3.log)) | Fix changed exit 0 to return 0 at line 136 to resume install |
| 4 | [install-nghdl-24.04.sh](https://github.com/mvbalaji200/eSim/blob/installers/nghdl/install-nghdl-scripts/install-nghdl-24.04.sh) | Installer was terminating at libcanberra install as libcanberra-gtk-module isnt a part of 25.04 libraries([debug5.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug5.log)) | Fix:added libcanberra-gtk3-module at line 69 |
| 5 | [install-nghdl-24.04.sh](https://github.com/mvbalaji200/eSim/blob/installers/nghdl/install-nghdl-scripts/install-nghdl-24.04.sh) | Installer stops after nghdl installation due to llvm version error as 25.04 defaults to v20 ([debug6.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug6.log)) , tried llvm 19 result [debug8.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug8.log)) | Fix: Set script to install llvm-18 at line 58|
| 6 | [install-nghdl-24.04.sh](https://github.com/mvbalaji200/eSim/blob/installers/nghdl/install-nghdl-scripts/install-nghdl-24.04.sh) | Installer stops due to generic llvm directory name [debug7.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug7.log) | Fix added correct llvm-18 specific path at line 107 leading to successful install [debug9.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug9.log) |

###GUI Bugs
