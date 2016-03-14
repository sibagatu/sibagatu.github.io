---
layout: post
title: My attempt to ping the world.
---
I was really impressed by the maps produced in this [blog post](http://erikbern.com/2015/04/26/ping-the-world/) and I became curious if a non-programmer can code something like this independently. I've been thinking about this idea for a while but did not have the means for implementaton. I've decieded now to start the implementation stage after becoming familiar with Python. Here is the plan:
1.  Randomly generate thousands/millions of IPs (Internet Protocol address in IPv4 version) so that they conform to the basic IP naming conventions.
2.  Ping each one of those random IPs and record the respective response delay if a given IP is responsive. 
3.  Geocode (get approximate latitude and longitude) the responsive IPs.
4.  Graphically map all of the responsive and geocoded IPs
5.  Build a contour map of the world that shows the latency delay
