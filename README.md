MicroPython for the BBC micro:bit
=================================

This is the source code for MicroPython running on the BBC micro:bit!

To get involved with the micro:bit community join the Slack channel by signing up here:
https://tech.microbit.org/get-involved/where-to-find/

Various things are in this repository, including:
- Source code in source/ and inc/ directories.
- Example Python programs in the examples/ directory.
- Tools in the tools/ directory.

The source code is a yotta application and needs yotta to build, along
with an ARM compiler toolchain (eg arm-none-eabi-gcc and friends).

Ubuntu users can install the needed packages using:
```
sudo add-apt-repository -y ppa:team-gcc-arm-embedded
sudo add-apt-repository -y ppa:pmiller-opensource/ppa
sudo apt-get update
sudo apt-get install cmake ninja-build gcc-arm-none-eabi srecord libssl-dev
pip3 install yotta
```

Once all packages are installed, use yotta and the provided Makefile to build.
You might need need an Arm Mbed account to complete some of the yotta commands,
if so, you could be prompted to create one as a part of the process.

- Use target bbc-microbit-classic-gcc-nosd:

  ```
  yt target bbc-microbit-classic-gcc-nosd
  ```

- Run yotta update to fetch remote assets:

  ```
  yt up
  ```

- Start the build:

  ```
  make all
  ```

The resulting firmware.hex file to flash onto the device can be
found in the build/ directory from the root of the repository.

The Makefile provided does some extra preprocessing of the source,
adds version information to the UICR region, puts the resulting
firmware at build/firmware.hex, and includes some convenience targets.

Vittascience MicroPython
=================================

In Vittascience we provide our users with a lot of kits and sensors. MicroPython for the BBC micro:bit does not come up with these modules. Thus, on this repository we added all the frozen custom modules to the MicroPython source code. 

## How to add custom modules to MicroPython.
Follow this procedure step by step to generate a micropython run time hex with your modules included.

1. Write your set of modules and put them in a folder, (for instance `/your_folder_path`).
2. Use [make-frozen.py tool](tools/make-frozen.py) to transform your `.py` modules to a `.c` file. `python make-frozen.py your_folder_path > frozen_module.c`
3. Open frozen_module.c and check its the module name, you could probably change the module name in the file.
4. Clone micropython for microbit and move your frozen_module.c to the source folder.

        git clone https://github.com/vittascience/micropython
        mv frozen_module.c source/py/
5. Find the mpconfigport.h in micropython/inc/microbit/, open it and add 2 lines at line 30.

        #define MICROPY_MODULE_FROZEN  (1)
        #define MICROPY_MODULE_FROZEN_STR  (1)

> If you clone from vittascience's micropython repoistory this should be already there.
6. Build your microbit micropython following these [steps](https://github.com/vittascience/micropython#micropython-for-the-bbc-microbit).

> Make sure your GCC version is 6+ with c++11 standard or later. C++98 standard won't work.
7. Find your `.hex` file in `micropython/build/xxxxxxxxxx/source/`
8. Send it to microbit. Then open the REPL and type: `help('modules')`, you should see your module listed below if you do it right.

