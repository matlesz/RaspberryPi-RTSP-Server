
# RaspberryPi RTSP Server

There repository guide you through setting up lightweight Real Time Streming Server on your RaspberryPi.




## Installation

We need RaspberryPi with Raspbian OS already installed.
Official RaspberryPi camera.


1) We need to enable camera interface in the RPi configuration file.
Type: 
```bash
  sudo raspi-config
```
and then navigate to Interfacing Options >> P1 Camera >> Yes

2) Install v4l2rtspserver
Now navigate to Documents and install server using this command
```bash
  sudo apt-get install cmake liblog4cpp5-dev libv4l-dev git -y
```
Once the package is downloaded we can build it:
```bash
  git clone https://github.com/mpromonet/v4l2rtspserver.git ; cd v4l2rtspserver/ ; cmake . ; make ; sudo make install
```

3) Start video strem
```bash
  v4l2rtspserver -W 640 -H 480 -F 15 -P 8554 /dev/video0
```
4) Test video stream
Once everything is set up you can open VLC on your main computer and navigate to File >> Open network and pase this:

```bash
  rtsp://<raspberry-pi-ip>:8554/unicast
```
4a) If the camera position is off you can correct the orientation of the camera:
```bash
  v4l2-ctl --set-ctrl vertical_flip=1

  v4l2-ctl --set-ctrl horizontal_flip=1
```
5) Launch on boot
Firstly we need to change service file:
```bash
  sudo nano /lib/systemd/system/v4l2rtspserver.service
```
edit this line:
```bash
  ExecStart=v4l2rtspserver -W 640 -H 480 -F 15 -P 8554 /dev/video0
```
presss Ctrl+X to save and quit nano and then:
```bash
  sudo systemctl enable v4l2rtspserver
  sudo reboot
```

## Resources
I was using these sources to put together these short tutorial:

https://www.youtube.com/watch?v=34qj3b6AK4w&t=281s

https://www.youtube.com/watch?v=1AoIVQVjaEI

https://siytek.com/raspberry-pi-rtsp-to-home-assistant/