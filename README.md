## MJPG-streamer installer

Installing MJPG-streamer for multiple cameras can be simplified by using the scrips written https://www.repetier-server.com/setting-webcam-repetier-server-linux/. This was teste on Ubuntu for debian jessie, but should work on other current distributions as well. This solution will install mjpg-streamer service for one or multiple cameras. I took the pi configuration out since this is dedicated to pc platforms but you could easily add it back in. In order for this to work you should install mjpg-streamer as follows so that things are in the correct directory. Note: this is set to run under user "pi" as this is meant to work with Octoprint.

**1. Install build dependencies**

`sudo apt-get updatesudo apt-get install libjpeg8-dev imagemagick libv4l-dev v4l-utils make gcc git cmake g++`

**2. Download MJPG-streamer**

`git clone https://github.com/jacksonliam/mjpg-streamer.git`

**3. Open directory**

`cd mjpg-streamer/mjpg-streamer-experimental/`

**4. Build MJPG-streamer**

`cmake -G &quot;Unix Makefiles&quot;make`

**5. Install MJPG-streamer**

`sudo make install`

**6. Add user to video service**

`sudo usermod -a -G video pi`

**7. MJPG-streamer service installation**

```
sudo su
cd /usr/local 
git clone https://github.com/john-clark/mjpg-streamer-setup.git
cd mjpg-streamer-setup
./installWebcams install
exit
```

Now installed webcams are running and can be viewed. Raspi-webcam is reachable over port 8090 while the usb webcams 0-9 are reachable from ports 5050 to 5059. You can also control the service as the pi user using the `installWebcams` command.

**Settings:**

`nano etc/webcam.conf` or `./installWebcams setParm`

_Edit the configuration to your situation. The last 3 lines should match where you installed mjpg\_streamer and should match if you followed the instructions. Frame rate and resolution be changed if you think it is required. But keep in mind that larger images and higher frequencies mean more traffic and that can get a problem. So check if images get submitted with only small delay. If delay builds up you will get problems and should reduce traffic. Also note that some usb webcams do not support mjpg – these are supported but then the boards needs to compress images to jpg increasing the load. So best is to avoid these webcams or use them with low resolutions only. Once you are satisfied with the setup, store configuration and proceed:_

The following command will list available video formats
`ffmpeg -f video4linux2 -list_formats all -i /dev/video0`

Still testing the `WEBCAM_USB_PARAMS` variable in the config with settings from this post
https://3dprinting.stackexchange.com/questions/3261/octoprint-mjpg-streamer-configuration

**OctoPrint Note** 

the URL to type into octoprint are:

`http://{url.or.ip}:5051/?action=stream`
`http://{url.or.ip}:5051/?action=snapshot`

More info here - http://docs.octoprint.org/en/master/configuration/config_yaml.html#webcam

**TODO**

make this into a real service and configs for each camera
