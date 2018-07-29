---
layout: post
title: "Install IOBroker on Cubietruck"
categories: iobroker,cubietruck,mqtt,node.js
---

In order to automate my mqtt based home automation components i want to try iobroker. It allows blockly based "progamming", which may make it accessable to the rest of the family :)

**Installation of node.js**

I decided to try the current version 10, [according to post in the iobroker forum it might work :)][2]

On the [node js site][1] the recommended installation procedure is:

    curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
    sudo apt-get install -y nodejs

It adds their repository as trusted source. Next step: Do a smoke test to check the installation

    node -v
    v10.7.0
    nodejs -v
    v10.7.0
    npm -v
    6.1.0

**Installation of IOBroker**

    sudo mkdir /opt/iobroker
    sudo chmod 777 /opt/iobroker
    cd /opt/iobroker
    sudo npm install iobroker --unsafe-perm
    
    
This takes some minutes. 

The iobroker web interface is reachable under port 8081 - this collided with my calibre server on that machine. It can be changed using the web interface :)
I didnt find a way to set this using a config file - so i stopped the calibre server temporarily and changed the port in the [admin webinterface] of iobroker.


**Installation of adapters**

I have a Sonoff compatible Socket (from OBI, 10€) which i had reflashed with the open source alternative firmware tasmota.
IOBroker has a sonoff module. (unfortunatly it comes with an own mqtt server. It seems that it cant use an existing one.)
What a pity, i'd prefer using only mosquitto.

Blockly programming is enabled by the [js module][4]. 

**Result**

In the end all did work pretty well. I was able to "program" a simple rule which switches a lamp off, when my mobile is no longer in the wireless network.

However, the Blockly programming has some pitfalls. Normally the boolean comparision blocks reject incompatible types.
This failed when comparing the ping state with the string "true". It wasnt rejected but failed to compare.  The result type seems to be a boolean. The comparision works if
the boolean true is used.

An other Drawback is the huge memory footprint of IOBroker. For this setup it is 340 MB.
Each adapter consumes at least 40 MB
 


  [1]: https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions
  [2]: https://forum.iobroker.net/viewtopic.php?t=13754 
  [3]: https://github.com/ioBroker/ioBroker/wiki/ioBroker-Adapter-admin
  [4]: http://www.iobroker.net/docu/?page_id=5319&lang=de
