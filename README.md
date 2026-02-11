# Getting-Started

### Installing Dependencies

You will need to install dependencies for DVMhost to run properly.
To intall these dependencies run `apt-get install libasio-dev libncurses-dev libssl-dev`

### Installing/Building DVMhost and MMDVM Hat Firmware

1. Clone the DVMhost repository in  `/opt/ `. `git clone https://github.com/DVMProject/dvmhost.git`
2. Switch into the "dvmhost" folde and Create a new folder named "build" and switch into it.
   ```
   1. cd dvmhost
   2. mkdir build
   3. cd build
   ```
3. Run CMake
   ```
   1. cmake /opt/dvmhost/build
   The shell should return -- Build files have been written to: dvmhost/build

   2. make
   ```
4. Run the following commands to download and install the hotspot firmware. Make sure you are in `/opt/dvmhost` when downloading the firmware.
   ```
   1. sudo git clone --recurse-submodules https://github.com/DVMProject/dvmfirmware-hs.git`
   2. cd /opt/dvmhost/dvmfirmware-hs
   3. sudo make -f Makefile.STM32FX mmdvm-hs-hat-dual
   4. (We recommend flashing the hat over the usb adapter board.) Set the adapter switch to "BOOT0" (if using the W3AXL board) or position B/‘ON’ (with the older "Chinese" boards), and plug the adapter board into the Pi
   5. Run sudo stm32flash -v -w dvm-firmware-hs_f1.bin /dev/ttyUSB0

5. Editing files
   1. Run `sudo nano /boot/firmware/cmdline.txt` and remove `console=serial0, 115200` then press Ctrl + S to save then Ctrl + X to close.
   2. Run `sudo nano /boot/firmware/config.txt` and add `dtoverlay=disable-bt` to the `[all]` section. Then press Ctrl + S to save then Ctrl + X to close.

      -If using a Pi3 add `dtoverlay=pi3-disable-bt` to the `[all]` section instead.
      -If using a Pi 5 using Bookworm or newer add the below text to the `[all]` section
      ```
      enable_uart=1
      dtoverlay=uart0,ctsrts
      ```

6. Disabling Services.
   1. Run the following as an individuall command.
   ```
    sudo systemctl disable serial-getty@ttyAMA0.service
    sudo systemctl disable hciuart.service
    sudo systemctl disable bluealsa.service
    sudo systemctl disable bluetooth.service
    sudo systemctl mask serial-getty@ttyAMA0.service
    sudo systemctl mask hciuart.service
    sudo systemctl mask bluealsa.service
    sudo systemctl mask bluetooth.service
   
### Running DVMhost
1. Contact a system admin for configuration files.
2. Run the config files using  `/opt/dvmhost/build/dvmhost -c /opt/dvmhost/configs/HNxx.yml` [Replace xx with CC or VC]
