# **Gaining Wi-Fi Access**

Aircrack-ng is a tool that comes pre-installed in Kali Linux and is used for Wi-Fi network security and hacking. Aircrack is an all in one packet sniffer, WEP and WPA/WPA2 cracker, analysing tool and a hash capturing tool. It is a tool used for Wi-Fi hacking. It helps in capturing the package and reading the hashes out of them and even cracking those hashes by various attacks like dictionary attacks. It supports almost all the latest wireless interfaces.

It mainly focuses on 4 areas:

- Monitoring: Captures cap, packet, or hash files.
- Attacking: Performs deauthentication or creates fake access points
- Testing: Checking the Wi-Fi cards or driver capabilities
- Cracking: Various security standards like WEP or WPA PSK.

```
airmon-ng
```
To list all network interfaces.

```
airmon-ng start wlan0
```

This is used to make the interface in monitor mode. Monitor mode is similar to putting a wired adapter into promiscuous mode. It allows us to see all of the wireless traffic that passes by us in the air.

```
airodump-ng wlan0mon
```

This command grabs all the traffic that your wireless adapter can see and displays critical information about it, including the BSSID (the MAC address of the AP), power, number of beacon frames, number of data frames, channel, speed, encryption (if any), and finally, the ESSID (what most of us refer to as the SSID).

Our next step is to focus our efforts on one AP, on one channel, and capture critical data from it. We need the BSSID and channel to do this. For that use command:

```
airodump-ng --bssid 08:86:30:74:22:76 -c 6 –write crack wlan0mon
```

In order to capture the encrypted password, we need to have the client authenticate against the AP. If they&#39;re already authenticated, we can de-authenticate them (kick them off) and their system will automatically re-authenticate, whereby we can grab their encrypted password in the process. For that use the command:

```
aireplay-ng --deauth 100 -a 08:86:30:74:22:76 wlan0mon
```

we bounced the user off their own AP, and now when they re-authenticate, airodump-ng will attempt to grab their password in the new 4-way handshake.

Now that we have the encrypted password in our file crack, we can run that file against aircrack-ng using a password file of our choice. We&#39;ll now attempt to crack the password using the following command:

```
aircrack-ng crack.cap -w wordlist
```

It will crack the password using brute force it will crack faster if the password list is good.
