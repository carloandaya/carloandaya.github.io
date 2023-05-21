---
layout: post
title: ASUS Router Issue
date: 2023-05-21 12:00:00 -0800
categories: tech
---

My router started acting funny last Wednesday, 17 May 2023. My wired devices could not get on the internet. My wireless devices gave error messages stating, "Could not get an IP address."

The router would work for a little bit after restarting it but issues would arise after a day or so. 

I don't have a lot of devices. Certainly less than the 200 IP addresses the DHCP server can give out. I thought I had devices that would constantly ask for a new IP address. Checking the client list confirmed otherwise. 

I was able to get around the issue by reserving IP addresses for the devices that I have. However, I was concerned that the problem would reoccur. And I couldn't reserve an IP address for my iPhone because it uses randomized MAC addresses.

Clicking around the GUI of the router, I came across the system log where this message was on repeat:

`failed to write /var/lib/misc/dnsmasq.leases: No space left on device (retry in 60s)`

A quick Google search led me to [this forum post](https://rog-forum.asus.com/t5/gaming-network-products/rt-ac3200-dnsmasq-dhcp-271-failed-to-write-var-lib-misc-dnsmasq/td-p/930812) that had an [official response from ASUS](https://www.asus.com/ca-en/news/tp8wqrkgqbd4rrw7/?fbclid=IwAR3rJE5lnQ7FwmtH1akToRaSsgjCi-HDhAThDhslc42ZrWwe4yewJn-3bmU).

I was hesitant to [reset my router](https://www.asus.com/support/FAQ/1050464) to factory default but there was no way around it. It wasn't as bad as I thought. Backing up the config and restoring got my router back to proper working condition and the error message stopped showing up in the system logs.
