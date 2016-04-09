# TCP connect timeout

## Windows

In Windows 7 and Windows Server 2008 R2, the TCP maximum SYN
retransmission (JH: MaxSynRetransmissions) value is set to 2, and is
not configurable. Because of the 3-second limit of the initial
time-out value (JH: InitialRTO), the TCP three-way handshake is
limited to a 21-second timeframe (`3 seconds + 2*3 seconds + 4*3
seconds = 21 seconds`).

Commands to change timeout settings in Windows 2012:

```
Get-NetTCPSetting
netsh interface tcp show global

netsh interface tcp set global MaxSynRetransmissions=2
netsh interface tcp set global InitialRto=3000
```

Command to test tcp connect 21 seconds timout, not for Windows 2012:

```
cmd /v:on /c "echo !TIME! & telnet 192.168.1.254 & echo !TIME!"
```

## Linux

`tcp_syn_retries (integer; default: 5; since Linux 2.2)`

The maximum number of times initial SYNs for an active TCP connection
attempt will be retransmitted.  This value should not be higher
than 255.  The default value is 5, which corresponds to approximately
189 seconds.

```bash
sysctl -a | grep tcp_syn_retries
net.ipv4.tcp_syn_retries = 5

sysctl -w net.ipv4.tcp_syn_retries=6
```

Command to test tcp connect timeout for linux, 192.168.1.254 should be
a non-existed host:

```bash
sysctl -w net.ipv4.tcp_syn_retries=1
date;telnet 192.168.1.254;date
```
