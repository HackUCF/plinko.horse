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
host="game.team6.plinko.horse" res=failed acct!=hkeating
```
## Query 4
```splunk
host="game.team6.plinko.horse" addr!=172.16.255.221 addr!="::ffff:172.16.255.221"
```
## Query 5
```splunk
host="db.team6.plinko.horse" exe="/usr/bin/sudo"
```
