philterphone
============

A minimalist home phone system with filtering to reduce spam calls.

Goal
----

With the ubiquity of cell phone, most folks have gotten away from having a "home phone".  I still like having one for at least a couple of reasons -- it is nice to have a backup when/if your cell reception is spotty and it can be cheaper. A significant drawback are spam calls, and I thought it would be an interesting project to build a system that could filter calls from automated systems using a prompt requesting the caller enter a given number in order to complete the call.

Does it work? We'll see; I'll update this as I go.

Requirements
------------

There's a bunch of jargon to learn if interested:
* [VoIP](https://en.wikipedia.org/wiki/Voice_over_IP)
* [SIP](https://en.wikipedia.org/wiki/Session_Initiation_Protocol)
* [ATA](https://en.wikipedia.org/wiki/Analog_telephone_adapter)
* [PBX](https://en.wikipedia.org/wiki/Business_telephone_system#Private_branch_exchange)
* [ITSP](https://en.wikipedia.org/wiki/Internet_telephony_service_provider)
* [PSTN](https://en.wikipedia.org/wiki/Public_switched_telephone_network)

That said, it is sufficient to know that: 
* VoIP represents the idea of sending voice communication over the Internet
* SIP is a very commonly used protocol for VoIP
* An ATA is an "analog telephone adapter", a device for connecting a standard phone to a VoIP network. There are VoIP phones which do not require an ATA, but they tend to be much more expensive than analog phones.
* A PBX manages routing phone calls, be it to/from the phones or to voicemail.
* An ITSP routes calls from the PBX to/from elsewhere on the Internet or to the PSTN
* The PSTN represents the world's various telephone networks.

For this project we'll be using:

* "Asterisk", which is a software PBX 
* A RaspberryPi to run Asterisk
* A Grandstream ATA to connect our home phone to the PBX 
* An account with an ITSP to connect our PBX to the rest of the world.

Assuming you already have an analog phone, the only hardware you will need is:
* a RaspberryPi
* a power supply for the RaspberryPi
* a memory card for the RaspberryPi
* an ATA

If you don't already have a microSD reader/writer for you computer, you will need one of those as well; [this one by Transcend](https://www.amazon.com/gp/product/B009D79VH4) is an inexpensive one that I've used successfully.

While the RaspberryPi can be configured for wireless network access, the ATA can not (at least not the ones I have looked at); so you will need access to a network switch for that.

Technically, you don't need to buy a RaspberryPi if you already have a computer at home that you can run Linux on instead, but these instructions assume you are using a Pi.

Costs
-----

At the time of writing, Amazon was selling a bundle of a Raspberry Pi 3 B+, a power supply, and a 32GB microSD memory card for about $60, and a Grandstream GS-HT802 ATA for about $35. 

For this project, I'm using https://telnyx.com as the ITSP; phone numbers there cost about $1/month plus usage. Usage costs vary by country, but can be less than $0.01 per minute.

For comparison, typical "full service" home phone providers tend to charge at least $25/month per line.

Initialize the Pi
-----------------

While you can use practially any Linux distribution for this, I'm going to use Raspian, which is the officially supported OS for the RaspberryPi. As we don't need a desktop environment (we're not going to connect the Pi to a monitor), we can get the "Lite" version of the OS.

You can see the various versions of Raspian available at https://www.raspberrypi.org/downloads/raspbian/ .  Download the ZIP file for [Raspbian Lite](https://downloads.raspberrypi.org/raspbian_lite_latest). It will be < 500MB. 

Next, you will need to install this on the memory card. Links to guides on how to do that for various host operating systems can be found [here](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). If you happen to be running Linux, insert the card, run `lsblk` to determine which device it is ( of the format /dev/sdX ), and `unzip -p 2019-07-10-raspbian-buster-lite.zip | sudo dd of=/dev/sdX bs=4M conv=fsync` . There's no progress indicator, and it takes a couple of minutes to complete. Standard "I assume no responsibility if you enter the command incorrectly and wipe out your local hard disk" clause applies.

Once the OS is on the microSD card, we need to add a couple of things.
On the boot partition of the card:
1. If you want to connect to your local network via WiFi, create a file called `wpa_supplicant.conf`. This file will tell the Pi how to connect to your local WiFi network on boot. The contents of this file should look like:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
	ssid="YourWifiNetworkName"
	psk="YourWifiNetworkPassword"
}
```
2. Create an emtpy file called `ssh` . This will instruct the Pi to enable the SSH server on boot, so we can login to the Pi from our computer.

Now you can put microSD card in the Pi and power it up. After a minute or so, you should be able to connect to it by doing `ssh pi@raspberrypi.local` , assuming you are on the same local network. 

The default password for the `pi` account is `raspberry`; once you login, you should change that to something else by using the `passwd` command.

Setup ITSP account
------------------

This specifics of this part is going to vary wildly depending on the ITSP you choose, but generally speaking, you need to:
1. Configure credentials (a user name and password) for accessing the the account via SIP. This is very likely NOT the same as credentials you use to log into the web site.
2. Obtain a phone number.
3. You may need to configure outbound calling by designating which countries can be called.

If you are using telnyx.com , this takes the form of:
1. Configure a "connection" (which allows you to setup the necessary credentials). 
2. Purchase a phone number and attach it to that "connection".
3. Configure an "outbound profile" to allow for outbound calls, and attach it to the same "connection".

Connect the ATA
---------------

The ATA needs to be attached to your local network (wired; not WiFi) and to your analog telephone. Once it is powered, you should be able to hear a dial tone on your analog phone. Dial `***` and enter `02` at the prompt. This instructs the unit to speak the current IP address of the device. If you enter this IP address in your web browser, it should bring up the ATA management interface. 

We'll come back to this once Asterisk is setup.

Setup Asterisk
--------------

In your pi SSH terminal, install asterisk by doing `sudo apt-get install asterisk` .

Asterisk is configured using files located in `/etc/asterisk` . There are GUI frontends available for Asterisk ( [FreePBX](https://www.freepbx.org/) being a popular one ), but I wanted to get my hands dirty. 

More to come once I flesh out the config files and AGI scripts.
