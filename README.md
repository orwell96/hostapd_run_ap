# hostapd_run_ap

This is a very basic script to run a Wi-Fi (WLAN) hotspot from your GNU/Linux computer, sharing the LAN connection. It works out-of-the-box. No external configuration is required.

This utility is supposed to be used for simple on-the-fly internet sharing, like the Android Mobile Hotspot feature. It is NOT supposed to be used in environments where security or reliability is important. It is not supposed or designed to work as a daemon.

The script is based on https://nims11.wordpress.com/2012/04/27/hostapd-the-linux-way-to-create-virtual-wifi-access-point/.

## Dependencies

runAP depends on the following software packages:

- hostapd
- dnsmasq
- iptables

Additionally, the system must use the "ip" command to manage network interfaces.

The script is designed to work with NetworkManager as the utility that manages network interfaces. If NetworkManager is present, it will take care of unmanaging/managing the wireless interface. If you do not have any network manager, it should also work, except that the `nmcli` commands will fail. You might need to take extra steps and/or extend the script to work with other network managers.

## Usage

### Configuration

Open "runAP" using a text editor. At the top, there are some variable assignments. Put your desired values there.

The "wif" and "wan" settings are network interface identifiers. You can find these out by typing "ip link list" or "ifconfig" into a terminal.

### Running

To run the hotspot, execute the script as root, like:
```
sudo ./runAP
```
It will then create a "hostapd.conf" and a "dnsmasq.conf" file in the working directory, set the wif interface as unmanaged in NetworkManager, set appropriate iptables rules and start dnsmasq(as DHCP server) and hostapd.

When the message
```
wlp2s0: AP-ENABLED
```
appears, your hotspot is running and accepting connections.

To close the hotspot and terminate the script, press Ctrl-C in the terminal. Alternatively, send a SIGTERM to the hostapd process (NOT the runAP process!). The script then terminates dnsmasq and sets the wif interface managed again by NetworkManager.

If you (accidentally or coincidentally) terminate the runAP script itself, these cleanup operations will NOT be carried out.

### Common mistakes and errors

#### Failed to set beacon parameters
When this message appears, your hotspot will probably not/no longer work. When this happens, something inside hostapd went wrong. Try to quit and restart the script, if it still doesn't work restart your computer.

#### Permission denied
If you see lots of "Operation not permitted" or "Permission denied" errors, make sure the script runs as root.

#### nl80211: Could not configure driver mode
In most cases, this error means that some other program is operating on your network card. Make sure that no network manager is accessing the wireless device.

There might also be another cause for this error. I can not provide further assistance in this case. Try to find advice by googling.

## runForward
The runForward script is a copy of runAP that does the work of forwarding network traffic without actually opening a Wi-Fi hotspot. I use it to give older computers without a WiFi card access to the internet by connecting it using LAN and logging into the home Wi-Fi.

It works like the runAP script except that it does neither require nor invoke hostapd.

## License and disclaimer

 Copyright (C) 2019 orwell96 <orwell@bleipb.de>

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    This work is licensed under GNU GPL v3. The license can be viewed at https://www.gnu.org/licenses/gpl-3.0.txt.

