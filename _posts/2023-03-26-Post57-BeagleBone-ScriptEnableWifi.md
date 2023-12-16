---
layout: post
title: "A script to start connman wifi service at boot using a Systemd service"
author: wilmer
categories: [BeagleBone]
tags: [Arduino]
image: assets/images/Post57/CoverPost.png
description: "How to write a script to enable wifi with connman"
featured: false
hidden: false
rating: 5
date:   2023-12-16 08:00:00 -0600
---
In this post, I show a script to enable the wifi service automatically at boot in the BeagleBone using connman.

<style>
  /* Three image containers (use 25% for four, and 50% for two, etc) */
  .column {
    float: left;
    width: 46%;
    padding: 2%;
  }

  /* Clear floats after image containers */
  .row::after {
    content: "";
    clear: both;
    display: table;
  }

  img {
    border-radius: 1%;
  }

  video {
    border-radius: 1%;
  }

  p {
    text-align: justify;
  }

  figure {
    text-align: justify;
  }

  .text table, 
  .text th, 
  .text td {
    width: 100%; 
    margin-left: auto; 
    margin-right: auto;
    border: 1px solid black;
    border-collapse: collapse;
    padding-left: 15px;
    padding-right: 15px;
    text-align: center;
  }
</style>

## Introduction
<p>
  In the Debian Buster IoT image 2020-04-06 the wifi is not enabled automatically at boot. The first solution is to disconnect and connect again the USB wifi dongle adapter, unless it is not so comfortable. The second solution is to write a script to disable and enable the wifi service for you when the BeagleBone starts. This is possible through the use of systemd.  
</p>

## Create the file for the commands to enable wifi
First of all, a file with the instructions, commands or program to be executed needs to be created. To do that the next instruction can be used:

`nano /home/debian/bin/enableWifi.sh`

In that file, the next commands need to be copied:

```bash
#!/bin/bash
connmanctl disable wifi
connmanctl enable wifi
```

## Create the file for the systemd service
The location of the systemd service will be in `/etc/systemd/system`. In our case, the service will be named as **enableWifi.service**. The command to create the file for the service is:

`sudo nano /etc/systemd/system/enableWifi.service`

In that service file, the next instructions can be copied. The relevant command is the `ExecStartPre=/bin/sleep 30` in the Service part. This command will add a delay of 30 seconds before the service will be executed, ensuring that the network and connman interface are loaded and the service will be executed without errors.

```bash
[Unit]
Description=Enable wifi after connman service has started
After=multi-user.target

[Service]
ExecStartPre=/bin/sleep 30
Type=idle
User=debian
Group=root
ExecStart=/home/debian/bin/enableWifi.sh
Restart=on-failure
bash

[Install]
WantedBy=multi-user.target
```


## Start and enable the systemd service
The new service can be started with the next command:

`sudo systemctl start enableWifi.service`

To verify that the script runs correctly:

`sudo systemctl status enableWifi.service`

Next, to execute it automatically at boot, execute the next command:

`sudo systemctl enable enableWifi.service`