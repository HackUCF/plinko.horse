---
title: "Queries"
layout: default
---

Written by Jeffrey DiVincent, CISO. For IHPL Eyes only.

##  Query 1

```splunk
host="WP"
```
## Query 2

```splunk
host="DNS" Account_Name=Administrator
```
## Query 3
```splunk
host="game.teamt.plinko.horse" res=failed acct!=hkeating
```
(Remember to replace "`teamt`" with your team's specific name)
## Query 4
```splunk
host="game.teamt.plinko.horse" addr!=172.16.255.221 addr!="::ffff:172.16.255.221"
```
(Remember to replace "`teamt`" with your team's specific name)
## Query 5
```splunk
host="db.teamt.plinko.horse" exe="/usr/bin/sudo"
```
(Remember to replace "`teamt`" with your team's specific name) 
