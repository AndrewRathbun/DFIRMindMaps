# [tcpdump](https://www.tcpdump.org/manpages/tcpdump.1.html)

## Resources

### [tcpdump filters](http://alumni.cs.ucr.edu/~marios/ethereal-tcpdump.pdf)

### [tcpdump Cheat Sheet by comparitech](https://cdn.comparitech.com/wp-content/uploads/2019/06/tcpdump-cheat-sheet-1.pdf)

### [A tcpdump Tutorial with Examples — 50 Ways to Isolate Traffic](https://danielmiessler.com/study/tcpdump/)

## Purpose

### Capture, display, and filter network traffic

## Useful Commands

### Read Traffic Capture

- tcpdump -r pathtofile -n
- tcpdump -r pathtofile -n -A

### Traffic Capture

- tcpdump -i interface
- tcpdump -i interface -w file

## Filtering

### [Berkley Packet Filters (BPF)](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter)

- Qualifiers

	- Type

		- host
		- net
		- port
		- portrange

	- Direction

		- src
		- dst

	- Protocol

		- icmp
		- ip
		- tcp
		- udp
		- ether
		- fddi
		- ip6
		- ppp
		- radio
		- rarp
		- slip
		- wlan

- Subtopic 2

### Logical Operators
(with tcpdump syntax)

- AND

	- and
	- &&

- OR

	- or
	- ||

- EXCEPT

	- not
	- !

- LESS

	- <

- GREATER

	- >

## [Examples](https://www.tcpdump.org/manpages/tcpdump.1.html#lbAF)

### Print all packets arriving at or departing from host MAINFILESERVER

### Print traffic between MAINFILESERVER and either MAINDC or MAINAVSERVER

### Print all IP packets between MAINFILESERVER and any host except MAINDC

