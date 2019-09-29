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

Costs
-----

At the time of writing, Amazon was selling a bundle of a Raspberry Pi 3 B+, a power supply, and a 32GB microSD memory card for about $60, and a Grandstream GS-HT802 ATA for about $35. 

For this project, I'm using https://telnyx.com as the ITSP; phone numbers there cost about $1/month plus usage. Usage costs vary by country, but can be less than $0.01 per minute.

For comparison, typical "full service" home phone providers tend to charge at least $25/month per line.


