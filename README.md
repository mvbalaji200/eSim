# Report:eSim Upgradation Task 4 -  Winter Internship 2026

| Field | Details |
| :--- | :--- |
| **Name** | Murugeshan Velappann Balaji |
| **Email** | mvbalaji200@gmail.com |

Test Environment:
  >Static hostname: U25
  >Icon name: computer-laptop
  >Chassis: laptop
  >Boot ID: 482aaf9c0f8e4679a677812c0a223580
  >Operating System: Ubuntu 25.04
  >Kernel: Linux 6.14.0-37-generic
  >Architecture: x86-64
  >Hardware Vendor: ASUSTeK COMPUTER INC.
  >Hardware Model: ZenBook UX325SA_UM325SA

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

### GUI bugs ([Fixes](https://github.com/FOSSEE/eSim/compare/master...mvbalaji200:eSim:frontend))

### GUI Bug #1: Core Dump Right-Clicking an Empty Space in Project Explorer
* **Log:** [debug13.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug13.log)
* **File:** `src/frontEnd/ProjectExplorer.py` (lines 108–116)
* **How to Reproduce:**
  1. Launch `esim`.
  2. Right-click inside the empty white space of the left-hand Project Explorer panel.
  3. **Result:** `UnboundLocalError: cannot access local variable 'level' where it is not associated with a value` -> `Aborted (core dumped)`.
* **Cause:** In `ProjectExplorer.openMenu()`, the `level` variable is only created when a tree item is selected (`len(indexes) > 0`). If a user right clicks an empty space, `selectedIndexes()` is empty, so `level` is never defined before `if level == 0:` runs, raising an `UnboundLocalError` crashing the GUI.
* **Fix (`src/frontEnd/ProjectExplorer.py`, lines 108–114):** Added an early return check `if not indexes: return` to exit safely when no item is selected.

---

### GUI Bug #2: Core Dump When Opening Binary `.raw` Files
* **Log:** [debug12.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug12.log)
* **File:** `src/frontEnd/ProjectExplorer.py` (lines 136–138)
* **How to Reproduce:**
  1. Launch `esim`.
  2. Expand the project in the left-hand Project Explorer tree and double-click or right-click -> **Open** `.raw` file in the project.
  3. **Result:** `UnicodeDecodeError: 'utf-8' codec can't decode byte 0xae ... invalid start byte` -> `Aborted (core dumped)`.
* **Cause:** `ProjectExplorer.openProject()` opens every selected file in plain UTF-8 text mode (`open(..., 'r')`) without error handling or closing the file handle. Double-clicking a file such as a `.raw` file triggers an unhandled `UnicodeDecodeError` and crashes the application.
* **Fix (`src/frontEnd/ProjectExplorer.py`, lines 136–138):** Used a `with` block to manage the file handle safely and added `try...except UnicodeDecodeError` error handling to display a warning popup for non-UTF-8/binary files instead of crashing.

---

### GUI Bug #3: Core Dump When Simulating After Modelica Converter (`TerminalUi.py`)
* **Log:** [debug11.log](https://github.com/mvbalaji200/eSim/blob/installers/Ubuntu/logs/debug11.log)
* **How to Reproduce:**
  1. Launch `esim` and open project (`Examples/Half_Adder`).
  2. Open the Ngspice to Modelica Converter tab and click **Convert**.
  3. Immediately click **Simulate**.
  4. **Result:** `FileNotFoundError: [Errno 2] No such file or directory: 'TerminalUi.ui'` -> `Aborted (core dumped)`.
* **Cause:** Running Modelica conversion hits an unhandled `KeyError` (`'adc_bridge'` missing from the Modelica mapping) and leaves Python's current working directory (`os.getcwd()`) outside `src/frontEnd/`. Because `TerminalUi.__init__()` loads `"TerminalUi.ui"` and `"../../images"` using relative paths, it fails to find the `.ui` file and crashes the GUI.
* **Fix (`src/frontEnd/TerminalUi.py`, lines 21–28):** Resolved both `TerminalUi.ui` and `iconDir` dynamically relative to `os.path.abspath(__file__)` so path lookups no longer depend on the active working directory.
