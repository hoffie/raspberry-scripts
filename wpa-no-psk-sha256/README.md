# Background

Raspberry Pi OS based on bookworm uses NetworkManager.
When configuring NetworkManager to use WPA, it automatically enables WPA-PSK-SHA256.
This can cause problems in certain configurations (Model 3B's Wifi chip + WPA2/WPA3 mixed mode APs).
No successful association is possible in such cases.
NetworkManager aims to be clever and provides no way to disable WPA-PSK-SHA256.

The wpa_supplicant output looks like this:
```
wpa_supplicant[559]: wlan0: Trying to associate with SSID 'example'
wpa_supplicant[559]: wlan0: CTRL-EVENT-ASSOC-REJECT bssid=00:00:00:00:00:00 status_code=16
```

The script (and the related systemd unit) in this folder can be used to monitor the systemd journal for this error via python.
If the error is detected, the scripts uses wpa_supplicant's DBUS interface (via wpa_cli) to override the `key_mgmt` config value to plain `WPA-PSK` (no `SHA256`).
This allows proper connections.

The alternative would be to stop using NetworkManager and use wpa_supplicant with a static config instead.

Depdendency: python3-systemd
Related keywords: PMF, MPF, FT-PSK, 802.11w.

# Usage
- Copy wpa_supplicant-retry-status-16 to /usr/local/sbin
- Copy wpa_supplicant-retry-status-16.service to /etc/systemd/system
- `systemctl enable --now wpa_supplicant-retry-status-16.service`
- Profit
