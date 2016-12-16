How to setup a AP with hostapd on Ubuntu-14.04
==============================================

This is pretty simple to do, but you need to configure several different daemons to get it working right.

# 1. Check your wifi card
You will need a wifi card that supports master mode, if you are going to create an access point with it.
First, figure out what your wifi card is named (look for one starting with 'wlan')

## 1.1 ifconfig


```
$ ifconfig -a

wlan0     Link encap:Ethernet  HWaddr 14:cf:92:xx:xx:xx
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

## 1.2  Next, check if "AP" mode is supported using iw
   and it looks for something like:

```
$ iw list
...
...
Wiphy phy0
        Band 1:
                Capabilities: 0x1862
                        HT20/HT40
                        Static SM Power Save
                        RX HT20 SGI
                        RX HT40 SGI
                        No RX STBC
                        Max AMSDU length: 7935 bytes
                        DSSS/CCK HT40
        Available Antennas: TX 0 RX 0
        Supported interface modes:
                 * IBSS
                 * managed
                 * AP
                 * AP/VLAN
                 * monitor
                 * mesh point
                 * P2P-client
                 * P2P-GO
...
...
```


# 2. Setup hostapd
  Assuming your wifi card supports master mode, the next step is to setup hostapd

```
$ sudo apt-get install hostapd
```

  Now create the file /etc/hostapd/hostapd.conf with the follow content: (if you want to use 5Ghz instead of 2.4Ghz, use hw_mode=a and channel=149 instead)


```
#/etc/hostapd/hostapd.conf
interface=wlan0
driver=nl80211
ssid=AP
hw_mode=g
channel=1
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=12345678
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Finally, edit /etc/default/hostapd to have the line:

```
DAEMON_CONF=/etc/hostapd/hostapd.conf
```

# 3. Setup dnsmasq
Now, it’s time to setup dnsmasq to handle DHCP and DNS on our wifi network, otherwise your clients won’t be able to get an IP address.


```
$ sudo apt-get install dnsmasq
```

Next, edit the dnsmasq configuration file to include this:

```
interface=wlan0
dhcp-range=10.0.0.2,10.0.0.10,255.255.255.0,12h
no-hosts
addn-hosts=/etc/hosts.dnsmasq
```

We set 'no-hosts' to avoid including all the entries in your hosts file in the DNS server, and instead set a separate file that will configure the DNS mapping for the machine hosting the AP. Make sure to create the file /etc/hosts.dnsmasq with the name of your computer:

```
10.0.0.1 my_hostname
```

# 4. Modify /etc/network/interfaces
Add these lines to your /etc/network/interfaces file, to give it a static IP address:


```
auto wlan0
iface wlan0 inet static
address 10.0.0.1
netmask 255.255.255.0
```

That is all, here your AP will work.


# 5. Reference

* Using hostapd on Ubuntu to create a wifi access point [cmberner](https://www.cberner.com/2013/02/03/using-hostapd-on-ubuntu-to-create-a-wifi-access-point/) for more details.


