# zabbix-pnetlab-monitoring
Network monitoring lab: Zabbix + SNMP on a PNETLab virtual topology

## Objective
Build a virtual network in PNETLab and monitor it end to end with Zabbix,
including alerts when devices or links fail.

## Topology
> Diagram coming soon (`topology/`)

## Environment
| Component      | Details                          |
|----------------|----------------------------------|
| Host OS        | Ubuntu (Linux)                   |
| Hypervisor     | VirtualBox / VMware              |
| PNETLab        | Version TBD                      |
| Zabbix server  | Ubuntu Server VM, Zabbix version TBD |
| Devices        | Routers / switches / Linux hosts |

## Progress
- [ ] PNETLab installed and topology built
- [ ] Basic routing configured (OSPF)
- [ ] Zabbix server installed
- [ ] SNMP monitoring of routers and switches
- [ ] Zabbix agent on Linux hosts
- [ ] Triggers and alerts
- [ ] Failure tests with screenshots
- [ ] Dashboard and network map

## Troubleshooting
Problems I ran into and how I solved them will go here.

## License
MIT
