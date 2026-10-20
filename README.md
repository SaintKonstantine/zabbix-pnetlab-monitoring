# zabbix-pnetlab-monitoring
Network monitoring lab: Zabbix + SNMP on a PNETLab virtual topology.

## Objective
Build a virtual network in PNETLab and monitor it end to end with Zabbix,
including detecting when devices or links fail.

## Topology
![Topology](topology/topology.png)

The Zabbix server sits outside the lab and reaches every device through a
**single link** to the PNETLab management cloud (Cloud0), attached to R5.
All other devices (R6, SW1, SW2) are polled through R5 using static routes.

| Device | Interface | Address | Role |
|--------|-----------|---------|------|
| R5 | e0/0, e0/1, e0/2 | 192.168.1.254/24, 10.0.0.1/30, 192.168.139.51/24 | Router, attached to Cloud0 |
| R6 | e0/0, e0/1 | 10.0.0.2/30, 192.168.2.254/24 | Router |
| SW1 | Vlan1 | 192.168.1.253/24 | L2 switch, gateway 192.168.1.254 |
| SW2 | Vlan1 | 192.168.2.253/24 | L2 switch, gateway 192.168.2.254 |
| VPC1 | eth0 | 192.168.1.1/24 | End host on SW1, gateway 192.168.1.254 |
| VPC2 | eth0 | 192.168.2.1/24 | End host on SW2, gateway 192.168.2.254 |
| Zabbix appliance | eth0 | 192.168.139.129/24 | Monitoring server |

## Environment
| Component      | Details                                          |
|----------------|--------------------------------------------------|
| Host OS        | Linux Mint                                       |
| Hypervisor     | VMware Workstation (NAT, vmnet8)                 |
| PNETLab        | Version: 4.2.10                                  |
| Zabbix server  | Zabbix 7.4.15 VMware appliance                   |
| Devices        | Cisco IOL routers and L2 switches, VPCS hosts    |
| Protocol       | SNMPv2c, template "Cisco IOS by SNMP"            |


### Test 1: interface shutdown on R6
![Before](screenshots/01before.png)
![Injection](screenshots/02-test1-injection.png)
![Link down problem](screenshots/03-test1-link-down.png)
![SW2 down](screenshots/04-test1-sw2-down.png)
![Recovery](screenshots/05-test1-recovery.png)
![Recovered](screenshots/06-test1-recovered.png)

### Test 2: link cut on R5
![Before](screenshots/01-before.png)
![Injection](screenshots/07-test2-injection.png)
![R6 unavailable](screenshots/08-test2-r6-unavailable.png)
![R6 and SW2 down](screenshots/09-test2-r6-sw2-down.png)
![Recovery](screenshots/10-test2-recovery.png)
![Recovered](screenshots/11-test2-recovered.png)

## Troubleshooting
Full write-up with symptoms, causes, diagnosis and fixes:
[docs/Zabbix-PNETLab-Lab-Report.pdf](docs/Zabbix-PNETLab-Lab-Report.pdf)

Main problems solved:
- Appliance could not reach routers: VMware promiscuous mode / `/dev/vmnet8` permission
- Switch ignored `ip default-gateway` because IP routing was enabled
- Appliance needed static routes into the lab subnets
- Zabbix database "Connection refused" after an appliance restart


## License
MIT
