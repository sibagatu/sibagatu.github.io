---
layout: post
title: My attempt to ping the world
---
![Mapping using the Basemap module](/img/figure_2.png)
I was really impressed by the maps produced in this [blog post](http://erikbern.com/2015/04/26/ping-the-world/) and became curious if a non-programmer can code something like this independently and without resorting to powerfull computing hardware. I've been thinking about this idea for a while but did not have the means for implementaton. I've decided now to start the implementation stage after becoming familiar with Python. The specs of machine that I will be using: Intel i7 CPU 920 @ 2.67GHz with 12GB RAM. The internet speed at my home is: 8.26Mbps down and 2.88Mbs up at the time of writing this post. I am using the Anaconda 2.7 64-bit Python environment because I discovered that the Cartopy module is not working yet with Anaconda 3. 

OK, here is the plan:

1.  Randomly generate thousands/millions of IPs (Internet Protocol address in IPv4 version) so that they conform to the basic IP naming      conventions.
2.  Ping each one of those random IPs and record the respective response delay if a given IP is responsive. 
3.  Geocode (get approximate latitude and longitude) the responsive IPs.
4.  Graphically map all of the responsive and geocoded IPs.
5.  Build a contour map of the world that shows the latency delay everywhere.

