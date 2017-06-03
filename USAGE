# (Slightly more) advanced usage

## 1.setup 

### Select keyboard

There are a couple of provided presets for some keyboards in the DATA folder.
To use them simply source one to set idVendor and idProduct to mimic that keyboard:
````
source DATA/cfg-<type>
````

Or if you want to full controll you can export the environment variables yourself before running ./1.setup:
````
export MANUFACTURER="Evil corp."
export SERIAL="666"
export IDVENDOR="0x0525"
export IDPRODUCT="0xa4ac"
export PRODUCT="0"
export CONFIG_NAME="Configuration 1" # Only used localy on the Firefly-device, you probably won't need to change this
````
Setting variables to "0" will make 1.setup ignore them, an empty string will cause the deault values to be used.

## 2.payload

### Select PAYLOAD

You can select different payloads and the method they are delivered to the victim.
````
export PAYLOAD=linux/gnome-terminal/python
````

The payload name should be in the format <operating system>/<injection method>/[<type of payload>].

### Select remote shell/command

./2.payload takes an argument of which shell/command to start at the victim. (May not work on all payloads.)

````
export PAYLOAD=linux/gnome-terminal/python
./2.payload /usr/bin/tcsh
````

## 3.firefly

As default Firefly will act as if stdin/stdout is a tty, to make things prettier if you run a remote shell. If you want to disable it set NOTTY to 1.
````
export NOTTY=1
````

If you want to save a session you can set FLOG to the path of your logfile.
````
export FLOG=/tmp/firefly.log
````
