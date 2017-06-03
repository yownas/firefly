# Firefly

![LOGO](https://github.com/yownas/firefly/blob/master/DATA/firefly.png)

Firefly is a proof-of-concept tool to exfiltrate data from a computer by communicating via light, just like real life fireflies.

Well, not really, it may not actually communicate with light but it acts like a usb keyboard and use the state of the caps-, num- and scroll-lock leds to get signals sent back to the device. It is not fast but what you get is a remote shell over wifi to a workstation that may not have network or that have blocked any other usb-devices than mice and keyboards.

While this attack has been explored before...
````
https://techblog.vsza.hu/posts/Leaking_data_using_DIY_USB_HID_device.html
````
... and using devices to act as keyboards has been around for a while...
````
https://www.arduino.cc/en/Reference/MouseKeyboard
https://hakshop.com/products/usb-rubber-ducky-deluxe
https://hakshop.com/products/bash-bunny
https://www.youtube.com/watch?v=FPBzOaLbWhM
````
...I never seen any tool using the keyboard leds to get an interactive shell on the victim... So I made one.

# Installation

Download and install Raspbian Lite on a Raspbery PI Zero W.
https://www.raspberrypi.org/downloads/raspbian/

Setup wifi. If you need to do this headless you can add an empty file called ssh.txt at the root of your Raspbian sd-card and a file called wpa_supplicant.conf that contain your wifi-settings. (Needs to have Linux line breaks)
````
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
````

(Don't forget the ssh.txt file.)

Boot up and log in and get a proper root-shell.
````
sudo -i
````

Update and install useful commands:
apt update
apt upgrade
apt install git rpi-update

Current kernel, after an "apt upgrade" (2017-05-15), has some problem with hid-gadgets.
If running "1.setup" later cause trouble you might need to downgrade to 4.4.49+ by running:
````
rpi-update 2ca627126e49c152beb1bf7abd7122ce076dcc65
````

Enable dwc2 overlay and add modules:

````
echo "dtoverlay=dwc2" >> /boot/config.txt
echo "dwc2" >> /etc/modules
echo "libcomposite" >> /etc/modules
````

Download Firefly.

````
cd
git clone <url>
cd firefly
````

Done...

(Reboot before you try the attack.)

# Attack

(This has been tested on a laptop with a clean Cent OS 7 + python-xlib installed.)

Connect Firefly to the victim workstation. Connect the usb-cable to the usb-port marked "USB", not the one marked "PWR". Wait until it boots and then log in on it and change to the firefly folder.

````
cd ~/firefly
````

## Create usb hid gadget device.
(If this cause a segfault and errors in the kernel, downgrade to 4.4.49+. (see above))
````
./1.setup
````
On your Firefly-device you should get /dev/hidg0 and the victim should see something like this in /var/log/messages.
````
May 15 21:09:14 vince kernel: usb 2-1: new high-speed USB device number 95 using ehci-pci
May 15 21:09:14 vince kernel: usb 2-1: New USB device found, idVendor=0525, idProduct=a4ac
May 15 21:09:14 vince kernel: usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
May 15 21:09:14 vince kernel: usb 2-1: Product: Firefly keyboard
May 15 21:09:14 vince kernel: usb 2-1: Manufacturer: Yownas
May 15 21:09:14 vince kernel: usb 2-1: SerialNumber: b8:27:eb:12:34:56
May 15 21:09:14 vince kernel: input: Yownas Firefly keyboard as /devices/pci0000:00/0000:00:1d.7/usb2/2-1/2-1:1.0/input/input20
May 15 21:09:14 vince kernel: hid-generic 0003:0525:A4AC.0007: input,hidraw0: USB HID v1.01 Keyboard [Yownas Firefly keyboard] on usb-0000:00:1d.7-1/input0
````

## Push payload.
````
./2.payload
````
This needs to be done while the victim leaves the screen unlocked and isn't looking. This takes ~10 seconds.


## Run firefly-client
````
./3.firefly
````
If everything works fine you should get a (very slow) shell on the victim.

Ctrl-Q to exit the client, remote shell keeps on running. You may reconnect later.
If you exit the remote shell or kill the process firefly started, the payload will also exit.

# "Protocol"

The protocol used is rather simple, see pseudo-code below. It is rather naive and have no error checking. But it works ok for a PoC. Server and client use the same protocol, only difference is that the victim machine starts with the listening part instead of send.

````
If there is data to send
  Get one byte
  Send each bit as num-lock (0) or caps-lock (1)
Send a scroll-lock to signal end of transmission

Listen for num-lock and caps-lock until we see a scroll-lock
If we have 8 bits
  assemble character and print/parse it

Return to start
````

# Target audience 

Who is this tool created for? Mainly it was to get an old idea I had stuck in my head out. Can you really use the caps-lock light to exfiltrate data? ...apparently you can.

If you are a penetration-tester that finds Firefly useful when having a demonstration for customers, I would love to get an e-mail telling me how it went and how they reacted. (yownas@gmail.com) (Merch is also ok, coffee-mugs, t-shirts (size XL) and stickers.) :)

If you are an evil government agency using this to steal secret documents and/or launch-codes I understand that sending me an e-mail might be against regulations, but maybe an anonymous post-card is ok? (If you really are an evil government agency you probably already know who I am and where I live.)
