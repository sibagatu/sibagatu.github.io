---
layout: post
title: My attempt to ping the world
---
![Mapping using the Basemap module](/img/figure_2.png)
The map above shows the result of my Python script that generates 100,000 of random IPs and plots the ones that are responsive to pinging and geocoding. 

I was really impressed by the maps produced in this [blog post](http://erikbern.com/2015/04/26/ping-the-world/) and became curious if a non-programmer can code something like this independently and without resorting to powerfull computing hardware. I've been thinking about this idea for a while but did not have the means for implementaton. I've decided now to start the implementation stage after becoming familiar with Python. The specs of the machine that I will be using: Intel i7 CPU 920 @ 2.67GHz with 12GB RAM. The internet speed at my home is: 8.26Mbps down and 2.88Mbs up at the time of writing this post. I am using the Anaconda 2.7 64-bit Python environment because I discovered that the Cartopy module is not working yet with Anaconda 3. 

OK, here is the plan:

1.  Randomly generate thousands/millions of IPs (Internet Protocol address in IPv4 version) so that they conform to the basic IP         naming conventions.
2.  Ping each one of those random IPs and record the respective response delay if a given IP is responsive. 
3.  Geocode (get approximate latitude and longitude) the responsive IPs.
4.  Graphically map all of the responsive and geocoded IPs.
5.  Build a contour map of the world that shows the latency delay everywhere.

Here are the modules that I included and functions that I wrote:

~~~
import random
import geocoder
import subprocess 
import re
import numpy
from random import randrange
from multiprocessing.dummy import Pool as ThreadPool 
import pickle
import time

#write a function to ping an ip with returned average response time
def ping(ip):
    ping_response = subprocess.Popen(["ping", ip, "-n", '1'], stdout=subprocess.PIPE).stdout.read()
    match=re.search('Average = (\d+)', str(ping_response))
    if match:
        return match.group(1)
        print("Avg delay=", match.group(1))
      
    else:
        return str(0)
       
#writing a function that takes index, geocodes the ip 
def new_geocode_ip(ipandresponse):
    i=0
    lat=[]
    lng=[]
    for ip in ipandresponse[:,0]:
        if i%1000==0:
            print("Geocoding stage: "+ str(i))
        if ipandresponse[i,1]!='0':
            g=geocoder.google(ip)
            time.sleep(1)                   #avoid Google service limit
            if g.ok: 
                lat.append(str(g.latlng[0]))
                lng.append(str(g.latlng[1]))
            else:
                lat.append("GC failed ")
                lng.append("GC failed")
        else:
            lat.append("Non-pingable, no GC")
            lng.append("Non-pingable, no GC")
        i=i+1
    return lat, lng

#writing a funciton that takes index, geocodes the ip individualy for multithreading 
def new_geocode_ip_ver2(ipandresponse):
        if ipandresponse[1]!='0':
            match=geolite2.lookup(ipandresponse[0])
            if match is not None: 
                lat, lng = match.location 
            else:
                lat="GC failed"
                lng="GC failed"
        else:
            lat="Non-pingable, no GC"
            lng="Non-pingable, no GC"
        
        return lat, lng
~~~
{: .language-python}
