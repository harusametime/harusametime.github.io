---
title: 'How to install Fujixerox printer in ubuntu 16.04'
date: 2018-01-05
permalink: /posts/2018/01/install_printer_ubuntu/
tags:
  - Ubuntu
---

This is how to install Fujixerox C5000-d printer in ubuntu 16.04. Unfotunately, the official printer driver is too old to be compatibe with latest ubuntu.

## Step
1. Download "fxlinuxprint_1.1.0+ds.orig.tar.xz" in [Launchpad](https://launchpad.net/ubuntu/+source/fxlinuxprint/1.1.0+ds-2
)
2. Extract the tar.xz file.
```
tar Jxfv fxlinuxprint_1.1.0+ds.orig.tar.xz
```
3. Search and run "Printer" from "Dash" that is the top icon in launcher
4. Add 
5. Select "Network Printer" and Fuji Xerox Docuprint C5000d
6. Select AppSocket/HP JetDirect and go forward
7. Provide PPD file by specifying fxlinuxprint.ppd extracted from fxlinuxprint_1.1.0+ds.orig.tar.xz
8. Done 
