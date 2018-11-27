# PiPres
Presence Detection and Home Automation scripts for the Raspberry Pi

# Disclaimer
Let me preface this with a few things. This code is primarily being written by me, for me. I have no plans to support any of this code for anyone else, but I do welcome feedback, suggestions, bug reports, or even ridiculing of coding mistakes (I'm not an incredibly experienced coder, just enough to do what I need).

However, I did have a little trouble finding concept code for some of these functions, so I'm providing it open source as I work on it in case it might help anyone else. In the spirit of open software, I'm labeling my project and files as GPL licensed. Feel free to use parts or all of it for your own projects if it's helpful.

That said, I am planning on building it to be modular and easy to set up and use for my own sanity when I move or change anything, and so that it might become something others can use. It may be a while, if ever, before it's a nice package you can install on your Pi, but it shouldn't be too difficult to adapt for your own use.

# About
After an unpleasant experience with a big name commercial camera that was supposed to have geofencing (that never worked right), I decided I wanted to build something myself. The initial goal was to work with Hue lights and next work on integration with ZoneMinder, but first was presence detection itself. I decided the extra Raspberry Pi 3 that I had was the perfect tool for the job.

After several failed attempts at using bluetooth as a detector of my phone being near, I decided that BT was the wrong way to do it, and ARP scanning for my phone on the network was far easier and more reliable. The script is a very simple loop that continually runs ARP scans and notes the presence of your phone. This was the first script, and it's just a simple shell script, but that may change in the near future.

The presence script can then call other scripts upon specific events such as Present->Absent and Absent->Present. For now, it just calls my lighting script, hue_control.py, but I plan on making that more modular and powerful soon.

The hue_control.py script holds all the Philips Hue API code. To start, it's just a few functions I wanted immediately and as proof of concept, but this will get structured better and built upon soon.

Finally, the pipres-listener is perhaps my favorite part. It's a very simple Node.js script that provides webhooks that can be used to control various functions from outside the network. The primary use for this to start is in conjunction with IFTTT to create Google Assistant voice commands. This is also very basic to start, but I have further plans for it. This will probably become a web frontend as well.

I have lots of ideas and plans to turn this into a full smart home brain. Presence is an integral part of building a SMART smart home brain. With this information, as well as other data I plan to add in the future, the system can learn patterns like a learning thermostat can (and yes, a learning thermostat is part of the plans as well!).

So read through, take what you want, and subscribe to the project if you'd like to see how it turns out!

# Future Plans
* First, remove static code and build out functions for everything that I don't have.
* Build out the hue_control script with more functionality to get and set light details, pair with hub, auto discover lights, etc.
* Investigate other presence detection methods and/or fine tune what I have. Initially I was arp scanning every 5 seconds and I think it was taking a toll on my wifi access point which dropped wifi a few times during the first week (though that could absolutely be unrelated, my access point is old and due for an upgrade). IF it was the arp scanning that was the issue, hard wiring the Pi might help.
* Create a scheduler to run the presence detection as well as create the ability to perform scheduled tasks, like lights on at a certain time, potentially just build an interface to cron, but it may be useful to build my own scheduler.
* Create ZoneMinder plugin to allow scheduling, presence based, and voice activated recording or alarming. Use presence detection and previous alarm times to learn when motion is or isn't expected. Send alerts for unusual activity.
* Create a web front end to get status of the home and change settings. Probably also the best place to put configuration editing in the future.
* Incorporate other sensors on the Pi device, temp, humidity, and light sensors for sure, perhaps even CO2 other other sensors to gather environmental data. This opens up all kinds of possibilities from turning on lights when light levels fall below a certain point when you're home, to completely building a learning thermostat.
* Party tricks?!

# Dependencies
This project was built on Raspbian 9 (Stretch). The following packages need to be installed via APT:
* arp-scan
* python3 and python-requests
* nodejs and npm (+express, http-auth, and python-shell packages for pipres-listener)

# Initial Release:
* Very basic, overly static scripts:
* Mostly proof of concept, but works almost perfectly.
* Only supports turning on and off certain hard-coded lights when you come or go, as well as providing webhooks for "night mode" which was something I created out of annoyance that it was difficult to control lights in different rooms with a single voice command. Night mode is used to turn on certain lights in the house when the sun sets.
