# Raspberry Pi


## Remote Access

### Getting the IP address of your Raspberry Pi

#### If you have a screen hooked up to your raspberry pi:
1. (in a terminal on your Raspberry Pi) enter the command `hostname -I`

#### If you don't (ie you're running headless)
See [this page for more info](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).

1. (from another computer on the same network as the pi) use nmap

### SSH from a Linux/Unix machine

1. Get the IP of your raspberry pi that you want to access 
2. (in a terminal on your Linux/Unix machine) enter the command `ssh pi@<IP address of the target raspberry pi>`

### Remote access of files

To remotely access files on your raspberry pi via SFTP (Secure File Transfer Protocol), 
1. Get the IP of your raspberry pi that has the files you want to access
2. (in a browser) go to `sftp://<IP address of the target raspberry pi>` (should look like `sftp://123.456.7.89`)


## Camera Stuff

links
* [`raspistill` docs](https://www.raspberrypi.org/documentation/usage/camera/raspicam/raspistill.md)