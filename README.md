# Cisco IOS Switch Initial Configuration Guide

This guide covers the initial configuration steps for a single Cisco IOS switch. Follow the steps below to set up your switch with essential network functionality, security, and management features.

## 1. Set Hostname
Set a unique hostname to identify the switch.
```shell
Switch> enable
Switch# configure terminal
Switch(config)# hostname COMPANY_SW1
```

## 2. User Setup and Enable Secret
Create a local user with privilege level and set a strong enable secret password.
```shell
Switch(config)# username admin privilege 15 secret VERYStrongUserPassword!
Switch(config)# enable secret AVERYStrongEnablePassword!
```

## 3. Configure Management IP Address
Configure an IP address for the management interface (typically VLAN 1).
```shell
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.122.2 255.255.255.0
Switch(config-if)# no shutdown
```

## 4. Set Default Gateway
Set the default gateway for the switch to communicate with devices outside its local network.
```shell
Switch(config)# ip default-gateway 192.168.122.1
Switch(config)# no shutdown
```

## 5. Enable SSH
Configure SSH for secure remote management.
```shell
Switch(config)# ip domain-name company.domain.com
Switch(config)# crypto key generate rsa modulus 1024
Switch(config)# ip ssh version 2
Switch(config)# ip ssh authentication-retries 3
Switch(config)# line vty 0 15
Switch(config-line)# transport input ssh
```

## 6. Set Strong Passwords and Enable Service Password Encryption
Configure strong passwords for console and VTY lines and enable password encryption.
```shell
Switch(config)# line console 0
Switch(config)# exec-timeout 4 5
Switch(config-line)# password myConsolePassword
Switch(config-line)# login local
Switch(config)# line vty 0 15
Switch(config-line)# password myVtyPassword
Switch(config-line)# login
Switch(config)# service password-encryption
```

## 7. Setup Access List for VTY Lines
Restrict access to the switch via VTY lines using an access list.
```shell
Switch(config)# access-list 10 permit 192.168.122.0 0.0.0.255
Switch(config)# line vty 0 15
Switch(config)# logging synchronous
Switch(config)# exec-timeout 4 4
Switch(config)# transport input ssh
Switch(config-line)# access-class 10 in
```

## 8. Configure VLANs
Set up VLANs to segment network traffic, including a quarantine VLAN for unused ports.
```shell
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config)# vlan 20
Switch(config-vlan)# name Engineering
Switch(config)# vlan 999
Switch(config-vlan)# name Quarantine
```

## 9. Configure Trunk Port
Set up trunk ports to carry multiple VLAN traffic between switches.
```shell
Switch(config)# interface G0/24
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)# switchport mode trunk
Switch(config-if)# no shutdown
```

## 10. Setup Spanning Tree and BPDU Guard
Enable and configure Spanning Tree Protocol and BPDU Guard to prevent network loops.
```shell
Switch(config)# spanning-tree mode rapid-pvst
Switch(config)# spanning-tree portfast default
Switch(config)# spanning-tree bpduguard enable
```

## 11. Save Configuration
Save the configuration to ensure it persists after a reboot.
```shell
Switch# copy running-config startup-config
```
## Conclusion
By following these steps, you will ensure that your Cisco IOS switch is properly configured for basic network functionality, security, and management.

