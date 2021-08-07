## linux-wireguard
Ansible Galaxy role to create side-to-side or mulit-side wireguard setups.

## Examples:
```
                +--------+                +--------+
+-------+       |        |                |        |       +-------+
| Local |       |   wg   |                |   wg   |       | Local |
|  Lan  +<----->+ server +                + server +<----->+  Lan  |
|   #1  |       |   #1   |                |   #2   |       |   #2  |
+-------+       |        |                |        |       +-------+
                +--------+                +--------+
                          \              /
                           \            /
                            +----------+
                            |          |
                            + Internet +
                            |          |
                            +----------+



                +--------+                +--------+
+-------+       |        |                |        |       +-------+
| Local |       |   wg   |                |   wg   |       | Local |
|  Lan  +<----->+ server +                + server +<----->+  Lan  |
|   #1  |       |   #1   |                |   #2   |       |   #2  |
+-------+       |        |                |        |       +-------+
                +--------+                +--------+
                          \              /
                           \            /
                            +----------+
                            |          |
                            + Internet +
                            |          |
                            +----------+
                           /
                          /                 
                +--------+                
+-------+       |        |
| Local |       |   wg   |
|  Lan  +<----->+ server +
|   #3  |       |   #3   |
+-------+       |        |
                +--------+                
               


                +--------+                +--------+
+-------+       |        |                |        |       +-------+
| Local |       |   wg   |                |   wg   |       | Local |
|  Lan  +<----->+ server +                + server +<----->+  Lan  |
|   #1  |       |   #1   |                |   #2   |       |   #2  |
+-------+       |        |                |        |       +-------+
                +--------+                +--------+
                          \              /
                           \            /
                            +----------+
                            |          |
                            + Internet +
                            |          |
                            +----------+
                           /            \
                          /              \                 
                +--------+                +--------+
+-------+       |        |                |        |       +-------+
| Local |       |   wg   |                |   wg   |       | Local |
|  Lan  +<----->+ server +                + server +<----->+  Lan  |
|   #3  |       |   #3   |                |   #4   |       |   #4  |
+-------+       |        |                |        |       +-------+
                +--------+                +--------+
```
## Variables

The following variables must be set via the inventory file or via host vars:

* wireguard_tunnel_interface_ip
* local_network

## Inventory

inventory
```yaml
# inventory

[locationA_wireguardservers]
wg1.domain.tld ansible_user=root ansible_port=22 ansible_python_interpreter="/usr/bin/python3" wireguard_tunnel_interface_ip=192.168.101.1/32 local_network=172.18.10.0/24

[locationB_wireguardservers]
wg2.domain.tld ansible_user=root ansible_port=22 ansible_python_interpreter="/usr/bin/python3" wireguard_tunnel_interface_ip=192.168.102.1/32 local_network=172.18.20.0/24

[locationC_wireguardservers]
wg3.doamin.tld ansible_user=root ansible_port=22 ansible_python_interpreter="/usr/bin/python3" wireguard_tunnel_interface_ip=192.168.103.1/32 local_network=172.18.30.0/24


[wireguardservers:children]
locationA_wireguardservers
locationB_wireguardservers
locationC_wireguardservers


[locationA:children]
locationA_wireguardservers

[locationB:children]
locationB_wireguardservers

[locationC:children]
locationC_wireguardservers
```

## Playbook

wireguardservers.yml
```yaml
---

- hosts: wireguardservers
  roles:
   - linux_wireguard
```

## Project dependencies

### Python
* [netaddr](https://pypi.org/project/netaddr/)

```bash
pip install netaddr
```