

# GNS3 OSPF Lab - Single Area (Area 0)

<img width="737" height="238" alt="image" src="https://github.com/user-attachments/assets/f7b559fb-308a-470d-b831-ad8d4a2489ab" />


This lab demonstrates a basic OSPF configuration across three routers in a point-to-point topology using GNS3 and Cisco IOS (c7200).

---

## Topology

```
[R1] f0/0 -------- f0/0 [R2] g1/0 -------- f0/0 [R3]
```

---

## IP Addressing

| Router | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| R1 | f0/0 | 10.0.12.1 | 255.255.255.0 |
| R2 | f0/0 | 10.0.12.2 | 255.255.255.0 |
| R2 | g1/0 | 10.0.23.1 | 255.255.255.0 |
| R3 | f0/0 | 10.0.23.2 | 255.255.255.0 |

---

## OSPF Design

| Parameter | Value |
|-----------|-------|
| Protocol | OSPFv2 |
| Process ID | 1 |
| Area | 0 (Backbone) |
| Networks | 10.0.12.0/24, 10.0.23.0/24 |

---

## Router Configurations

### R1

```
hostname R1
!
interface FastEthernet0/0
 ip address 10.0.12.1 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.0.12.0 0.0.0.255 area 0
```

### R2

```
hostname R2
!
interface FastEthernet0/0
 ip address 10.0.12.2 255.255.255.0
 no shutdown
!
interface GigabitEthernet1/0
 ip address 10.0.23.1 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.0.12.0 0.0.0.255 area 0
 network 10.0.23.0 0.0.0.255 area 0
```

### R3

```
hostname R3
!
interface FastEthernet0/0
 ip address 10.0.23.2 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.0.23.0 0.0.0.255 area 0
```

---

## Verification Commands

```
show ip ospf neighbor          # Confirm neighbor adjacencies
show ip route                  # Verify OSPF learned routes (O prefix)
ping 10.0.23.2                 # End-to-end reachability from R1 to R3
show ip ospf interface brief   # Check OSPF interface states
```

---

## Expected Output

On R2, `show ip ospf neighbor` should show:

```
Neighbor ID     Pri   State       Dead Time   Address       Interface
10.0.12.1         1   FULL/DR     00:00:34    10.0.12.1     FastEthernet0/0
10.0.23.2         1   FULL/DR     00:00:34    10.0.23.2     GigabitEthernet1/0
```

---

## Tools Used

- GNS3 (local server)
- Cisco IOS c7200 — adventerprisek9-mz.153-3.XB12
- Dynamips engine

---

## Skills Demonstrated

- Interface IP addressing and verification
- OSPFv2 single-area configuration
- Neighbor adjacency troubleshooting
- Route table verification
- End-to-end connectivity testing
