# zcu-kiv-psi-4

This is the fourth KIV/PSI project. A simple network configuration with real devices running inside the
[GNS3](https://en.wikipedia.org/wiki/Graphical_Network_Simulator-3).

## Topology

![Topology](topology.png)

## Configuration

### R1

```
configure terminal
```

```
interface gigabitEthernet 0/0
ip address dhcp
ip nat outside
no shutdown
exit
```

```
interface gigabitEthernet 1/0
ip address 192.168.123.253 255.255.255.252
ip nat inside
no shutdown
exit
```

```
ip route 192.168.123.0 255.255.255.0 gigabitEthernet 1/0
```

```
access-list 1 permit 192.168.123.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/0 overload
```

### R2

```
configure terminal
```

```
interface gigabitEthernet 0/0
ip address 192.168.123.1 255.255.255.224
no shutdown
exit
```

```
interface gigabitEthernet 1/0
ip address 192.168.123.254 255.255.255.252
no shutdown
exit
```

```
ip route 0.0.0.0 0.0.0.0 192.168.123.253
```

```
ip dhcp excluded-address 192.168.123.1 192.168.123.10
ip dhcp pool R2_LAN
network 192.168.123.0 255.255.255.224
default-router 192.168.123.1
dns-server 1.1.1.1 1.0.0.1
exit
```

### PC1

```
udhcpc
```

### PC2

```
udhcpc
```

## Testing

```
ping seznam.cz
```

```
PING seznam.cz (77.75.74.172): 56 data bytes
64 bytes from 77.75.74.172: seq=0 ttl=57 time=32.164 ms
64 bytes from 77.75.74.172: seq=1 ttl=57 time=51.741 ms
64 bytes from 77.75.74.172: seq=2 ttl=57 time=41.580 ms
64 bytes from 77.75.74.172: seq=3 ttl=57 time=50.964 ms
```
