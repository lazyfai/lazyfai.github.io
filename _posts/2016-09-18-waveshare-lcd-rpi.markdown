---
layout: post
title:  "Waveshare 3.5 吋 LCD 記錄"
categories: RaspberryPi
tags: RaspberryPi, Waveshare, LCD, GPIO
---
## It is much simpler than Kedei LCD as it is supported by fbtft and device overlay

1. Update RPi kernel to latest by "rpi-update"

2. Clone the waveshare device tree overlay git [repository][waveshare-dtoverlays]

3. Copy **waveshare-dtoverlays/waveshare35a-overlay.dtb** from the repositroy to **/boot/overlays/waveshare35a.dtbo**

4. Add **dtoverlay=waveshare35a** to **/boot/config.txt**

5. Ensure **SPI** is enabled in **/boot/config.txt**

    ```
    dtparam=spi=on
    ```
6. Reboot and use **/dev/fb1** as the device for graphical output

7. For console output, add to **/boot/cmdline**:

    ```
    console=ttyAMA0,115200 fbcon=map:10 fbcon=font:ProFont6x11 consoleblank=0
    ```

[waveshare-dtoverlays]: https://github.com/swkim01/waveshare-dtoverlays
