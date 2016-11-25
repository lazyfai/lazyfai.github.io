---
layout: post
title:  "在 Raspberry Pi 上使用 USB 音效咭播放音樂"
categories: RaspberryPi
tags: RaspberryPi, Audio, Music
---
## Pi 的 onboard 音效咭實在太差了，所以我決定用 [FiiO K1 USB][fiio-site] 音效咭代替它

1. 先更新 Pi kernel, 使用 "rpi-update" 指令即可

2. 安裝 pulseaudio

    ```
    apt-get install pulseaudio
    ```

3. 在 /etc/modprobe.d/ 中加一個 conf 檔案，把 snd_bcm2835 (RPi 的 onboard 音效咭)
例如 **/etc/modprobe.d/alsa.conf**

    ```
    blacklist snd_bcm2835
    ```

4. 在 **/etc/pulse/default.pa** 加入 TCP 支援

    ```
    load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;auth-anonymous=1
    ```

5. 在 **/etc/pulse/system.pa** 加入 TCP 支援

    ```
    load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;auth-anonymous=1
    ```
    ```
    load-module module-zeroconf-publish
    ```

6. 設定 pulseaudio 為開機服務

    ```
    systemctl enable pulseaudio
    ```

7. 重新開機，應該會偵測到 FiiO K1

    ```
    usb 1-1.2: Product: FiiO USB DAC K1
    ```
    ```
    usb 1-1.2: Manufacturer: FiiO
    ```
    ```
    input: FiiO FiiO USB DAC K1 as /devices/platform/soc/20980000.usb/usb1/1-1/1-1.2/1-1.2:1.0/0003:262A:100C.0001/input/input0
    ```
    ```
    hid-generic 0003:262A:100C.0001: input,hidraw0: USB HID v1.00 Device [FiiO FiiO USB DAC K1] on usb-20980000.usb-1.2/input0
    ```

8. 安裝 [mopidy][mopidy-site] 和一些有用的 plugins

    ```
    sudo pip install Mopidy-SoundCloud
    ```
    ```
    sudo pip install Mopidy-Youtube
    ```
    ```
    sudo pip install Mopidy-Tunein
    ```
    ```
    sudo pip install mopidy-musicbox-webclient
    ```
9. 修改 **/etc/mopidy/mopidy.conf** 把聲音輸出指向 127.0.0.1 和打開 HTTP 服務

    \[audio\] 段落

    ```output = pulsesink server=127.0.0.1```

    \[http\] 段落

    ```enabled = true```

    ```hostname = ::```

    ```port = 6680```

10. 打開 mopidy 服務，然後可以在 http://raspberry-pi-ip:6680/ 看到 Mopidy 界面

[fiio-site]: http://fiio.net/en/products/48
[mopidy-site]: http://mopidy.com/
