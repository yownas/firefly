# Firefly:

# 1.setup

Before the usb-gadget is activated the victim-machine tries to connect to the Raspberry PI, causing errors in the /var/log/messages on the host.

## 2.payload

The inject-scripts should really be done more like a Rubber Ducky. But since I didn't want to make a bad implementation of a Rubber Ducky clone I went with the option of simply doing a bad implemenation.

## 3.firefly

Make sure all the caps-/num-/scroll-lock keys are off before running. You might get stuck in a weird state otherwise.

If the client get out of sync with the victim you can get stuck in a state where you can't exit without killing the process.

### CentOS 7 (and probably other linuxes as well):

Don't forget that the python payload needs the python-xlib package to be installed.

nautilus doesn't seem to like firely and give a lot of errors:
````
gnome-session: (nautilus:30070): Gtk-CRITICAL **: gtk_widget_event: assertion 'WIDGET_REALIZED_FOR_EVENT (widget, event)' failed
````

Xorg runs with high cpu load for a while even after the firefly-client has disconnected.

### Windows 10:

Powershell can manipulate the keyboard leds, but I can't seem to make it work reliably.


