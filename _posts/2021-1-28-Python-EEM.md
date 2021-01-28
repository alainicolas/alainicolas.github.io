---
layout: post
title: Mixing Python and EEM on XR
---

## Mixing Python and EEM on XR 
### Introduction

Embedded Event Manager scripts are used since ages to capture outputs, or make action right after an external trigger. Unfortunately, they are still limited to the good old TCL language. Nowadays, the majority of engineers prefer modern languages, like Python. So let's mix things up!

Examples were taken on IOS-XR 6.3.3.


### Python on XR?
eXR platforms are powered with Linux kernel, and includes Python. Let's start slowly, and call a show clock in python. To find the system version of your favorite IOS-XR regular command, you can use the command "describe" :

`
RP/0/RP0/CPU0:NCS#describe show clock 
The command is defined in iosclock.parser


User needs ALL of the following taskids:

        basic-services (READ) 

It will take the following actions:
Thu Jan 28 14:56:01.414 UTC
  Spawn the process:
        iosclock -e 0x0 

`
And then, in running context :


RP/0/RP0/CPU0:NCS#run
[xr-vm_node0_RP0_CPU0:~]$iosclock -e 0x0
14:59:07.834 UTC Thu Jan 28 2021


Or in python :

[xr-vm_node0_RP0_CPU0:~]$python
Python 2.7.3 (default, Nov 16 2019, 21:56:43) 
[GCC 4.9.1] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> from subprocess import call
>>> call(['iosclock', '-e', '0x0'])
15:06:47.103 UTC Thu Jan 28 2021
0

For some commands we may have a lot of options and arguments, so these ones should be passed as list.

Directly from a script :


[xr-vm_node0_RP0_CPU0:~]$cat test.py 
from subprocess import call
output = call(['iosclock', '-e', '0x0'])
print output
[xr-vm_node0_RP0_CPU0:~]$exit
logout

RP/0/RP0/CPU0:par-th2-pb1-nc5#run python test.py
Thu Jan 28 15:09:36.308 UTC
15:09:36.588 UTC Thu Jan 28 2021
0


### Conclusion



