# Peculiar Packet Capture:
So the first steps for a Packet Capture is to open WireShark, so i start by doing so.

I also know that most packets tend to be encrypted under the 802.11 protocol, you can read up more on this protocol here:
https://en.wikipedia.org/wiki/IEEE_802.11#:~:text=IEEE%20802.11%20is%20part%20of,6%20GHz%2C%20and%2060%20GHz

```
Open pcap in Wireshark
Almost all packets are encrypted under the 802.11 protocol and need to be decrypted
Note down the BSS ID and SSID from the first packet
Make sure that aircrack-ng is installed, and that you have rockyou.txt
run aircrack-ng -w [rockyou.txt location] -b [BSSID] ATLAS_Capture.cap
this command will find the WPA2 pasword to the network which when combined with the SSID in wireshark decrypts the encrypted 802.11 packets
In wireshark right click the first packet and click protocol preferences and then decryption keys
Add a new decryption key of type wpa-pwd  [password]:[SSID] no brackets
Export any files downloaded : click file, export objects, http
Flag is in the PDF at the bottom
```
