Triggers shutdown on button press and manages the power LED.

# Configuration
## Script
*Warning:* The GPIO pins are hardcoded in the script. Verify that they are correct for your setup before starting the script for the first time. Failing to do so may damage the hardware in your specific setup.

## Other helpful configuration
### OnOffShim support
If you are using an OnOffShim to cut power, put the following into /boot/firmware/config.txt
(With 4 being the OnOffShim's trigger GPIO pin):
```
dtoverlay=gpio-poweroff,gpiopin=4,active_low=1
```

No further scripts (cleanshutdown) are required (and as of 2024-03 the official script does not work on bookwarm at all due to /sys usage for GPIO control).

### Enable power LED on early boot
*Warning:* Check for proper GPIO pin before using!
/boot/firmware/config.txt:
```
gpio=13=op,dh
```

# Usage
- Copy shutdown-manager to /usr/local/sbin/
- Copy shutdown-manager.service to /etc/systemd/system/
- `systemctl enable --now shutdown-manager.service`
