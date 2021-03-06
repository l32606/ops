#Management Interface Feature Test Cases

The following test cases verify Management interface configurations in :

- [IPv4 DHCP mode](#verifying-management-interface-configuration-test-cases-in-ipv4-dhcp-mode.)
- [Static IPv4 mode](#verifying-management-interface-configuration-test-cases-in-static-ipv4-mode.)
- [IPv6 DHCP mode](#verifying-management-interface-configuration-test-cases-in-ipv6-dhcp-mode.)
- [Static IPv6 mode](#verifying-management-interface-configuration-test-cases-in-static-ipv6-mode.)
- [System hostname](#verifying-system-hostname-configuration-testcases.)

## Verifying Management interface configuration test cases in IPv4 DHCP mode.##
### Objectives ###
These cases test:
- Configuring, reconfiguring, and unconfiguring the management interface.
- Verifying the expected behavior of the management interface with the DHCP IPv4 addressing mode.

### Requirements ###
The requirements for this test case are:

 - IPv4 DHCP Server.

### Setup ###
#### Topology Diagram ####
```ditaa
                                                           +-------------------+
              +------------------+                         | Linux workstation |
              |                  |eth0                eth0 |+-----------------+|
              |  AS5712 switch   |-----+         +---------||DHCP IPV4 Server ||
              |                  |     |         |         |+-----------------+|
              +------------------+     |         |         +-------------------+
                                       |         |
                                       v         v
                                 +---------------------+
                                 | port 1      port 2  |
                                 |                     |
                                 |      Switch         |
                                 +---------------------+
```
### Test case 1.01 : Verifying that the DHCP client has started. ###
#### Description ####
After booting the switch, verify that the dhcp client has started on the management interface by using the systemctl command `systemctl status dhclient@eth0.service`.
### Test Result Criteria ###
#### Pass Criteria ####
The test case is successful if the dhcpclient service is in running state.
#### Fail Criteria ####
The test case is a fails if the dhcpclient service is not in running state.

### Test case 1.02 - Verifying that the management interface is updated from the `image.manifest` file.###
#### Description ####
Verify that the management interface name is updated from the image.manifest file during boot.
### Test Result Criteria ###
#### Pass Criteria ####
The test case is successful if the `name=eth0` is present in the mgmt_intf column.
#### Fail Criteria ####
The test case is a failure if the `name=eth0` is missing from the mgmt_intf column.

### Test case 1.03 - Verifying management interface attributes in DHCP mode. ###
#### Description ####
Verify that the management interface attributes are configured in DHCP mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the following criterions are met:
- The `IPv4 address/subnet-mask`,`Default gateway IPv4`,`Primary Nameserver`, and `Secondary Nameserver` addresses are present in the `show interface mgmt` output .
- The dhcp client service is running.

#### Fail Criteria ####
The test fails if:
- The `IPv4 address/subnet-mask`,`Default gateway IPv4`,`Primary Nameserver`,`Secondary Nameserver` addresses are missing in the `show interface mgmt` output.
- The dhcp client is not running.


##   Verifying Management interface configuration test cases in Static IPv4 mode. ##
### Objectives ###
These cases test:
- Configuring, reconfiguring, and unconfiguring the management interface.
- Verifying the expected behavior of the management interface with the static IPV4 addressing mode.

### Requirements ###
No Requirements.
### Setup ###
#### Topology Diagram ####
```ditaa
              +------------------+                         +-------------------+
              |                  |eth0                eth0 |                   |
              |  AS5712 switch   |-----+         +---------| Linux Workstation |
              |                  |     |         |         |                   |
              +------------------+     |         |         +-------------------+
                                       |         |
                                       v         v
                                 +---------------------+
                                 | port 1      port 2  |
                                 |                     |
                                 |      Switch         |
                                 +---------------------+
```
### Test case 2.01 - Verifying that the static IPV4 address is configured on the management interface. ##
#### Description ####
Configure the static ipv4 address on the management interface using the management interface context.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `IPv4 address/subnet-mask` address is present in the `show interface mgmt` output and the `ifconfig` ouptut.
#### Fail Criteria ####
The test fails if the `IPv4 address/subnet-mask` address is missing in the `show interface mgmt` output and in the `ifconfig` ouptut.

### Test case 2.02 - Verifying that the default gateway is configured in static mode. ##
#### Description ####
Configure the static default IPv4 gateway in the management interface using the management interface context.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Default gateway IPv4` address is present in the `show interface mgmt` output and in the `ip route show` output.
#### Fail Criteria ####
The test fails if the `Default gateway IPv4` address is missing in the `show interface mgmt` or the `ip route show` output.

### Test case 2.03 - Verifying that the default gateway is removed in static mode. ###
#### Description ####
Remove the IPv4 default gateway in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Default gateway IPv4` address is missing in the `show int erface mgmt` or the `ip route show` output
#### Fail Criteria ####
The test fails if the `Default gateway IPv4` address is present in the `show interface mgmt` output and the `ip route show` output.


### Test case 2.04 - Verifying that the Primary DNS and secondary DNS are configured in static mode. ###
#### Description ####
Configure the IPv4 primary DNS and secondary DNS in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Primary Nameserver`, and `Secondary Nameserver` addresses are present in the `show interface mgmt` output and the `/etc/resolv.conf` file.
#### Fail Criteria ####
The test fails if the `Primary Nameserver`, and `Secondary Nameserver` addresses are missing in the `show interface mgmt` output or the `/etc/resolv.conf` file.


### Test case 2.05 - Verifying that the Primary DNS and secondary DNS are removed in static mode. ###
#### Description ####
Remove IPv4 primary DNS and secondary DNS in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Primary Nameserver`,and `Secondary Nameserver` addresses are missing in the `show interface mgmt` output and the `/etc/resolv.conf` file.
#### Fail Criteria ####
The test fails if the `Primary Nameserver`, and `Secondary Nameserver` addresses are present in the `show interface mgmt` output or the `/etc/resolv.conf` file.


## Verifying Management interface configuration test cases in IPv6 DHCP mode. ##
### Objectives ###
These cases test:
- Configuring, reconfiguring, and unconfiguring the management interface.
- Verifying the expected behavior of the management interface with the DHCP IPV6 addressing mode.

### Requirements ###
The requirements for this test case are:

 -  IPv6 DHCP Server.

### Setup ###
- #### Topology Diagram ####
```ditaa
                                                           +-------------------+
              +------------------+                         | Linux workstation |
              |                  |eth0                eth0 |+-----------------+|
              |  AS5712 switch   |-----+         +---------||DHCP IPV6 Server ||
              |                  |     |         |         |+-----------------+|
              +------------------+     |         |         +-------------------+
                                       |         |
                                       v         v
                                 +---------------------+
                                 | port 1      port 2  |
                                 |                     |
                                 |      Switch         |
                                 +---------------------+
```
### Test case 3.01 - Verifying that the default gateway is configurable in DHCP mode. ###
#### Description ####
Configure the ipv6 default gateway in dhcp mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the IPv6 default gateway is not configured.
#### Fail Criteria ####
The test is fails if the IPv6 default gateway is configured.


### Test case 3.02 - Verifying that the management interface attributes are updated in DHCP mode. ###
#### Description ####
Verify that the management interface attributes are configured in DHCP mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `IPv6 address/prefix`,`Default gateway IPv6`,`Primary Nameserver`,`Secondary Nameserver` addresses are present in the `show interface mgmt` output and the dhcpclient service is in a running state.
#### Fail Criteria ####
The test fails if the`IPv6 address/prefix`,`Default gateway IPv6`,`Primary Names server`,`and the Secondary Nameserver` addresses are missing and the `show interface mgmt` output or the dhcpclient is not in a running state.


## Verifying Management interface configuration test cases in Static IPv6 mode. ##
### Objectives ###
These cases test:
- Configuring, reconfiguring, and unconfiguring the management interface.
- Verifying the expected behavior of the management interface in static IPV6 mode.

### Requirements ###
No Requirements.

### Setup ###
- #### Topology Diagram ####
```ditaa
              +------------------+                         +-------------------+
              |                  |eth0                eth0 |                   |
              |  AS5712 switch   |-----+         +---------| Linux Workstation |
              |                  |     |         |         |                   |
              +------------------+     |         |         +-------------------+
                                       |         |
                                       v         v
                                 +---------------------+
                                 | port 1      port 2  |
                                 |                     |
                                 |      Switch         |
                                 +---------------------+
```

### Test case 4.01 - Verifying that the static IPv6 address is configured on the management interface. ##
#### Description ####
Configure the static IPv6 address on the management interface in management interface context.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `IPv6 address/prefix` address is present in the `show interface mgmt` output and the `ip -6 addr show dev eth0` output.
#### Fail Criteria ####
The test fails if the `IPv6 address/prefix` address is missing in the `show interface mgmt` output or in the `ip -6 addr show dev eth0` output.


### Test case 4.02 - Verifying that the default gateway IPv6 address is configured in static mode. ###
#### Description ####
Configure the static IPv6 default gateway on the management interface from the management interface context.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Default gateway IPv6` address is present in the `show int erface mgmt` output and the `ip route show` output.
#### Fail Criteria ####
The test also fails if the configured default gateway is not present in the `show interface mgmt` or the `ip route show` output.


### Test case 4.03 - Verifying that the default gateway IPv6 address is removed in static mode. ###
#### Description ####
Remove the IPv6 default gateway that is in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Default gateway IPv6` address is missing in the `show interface mgmt` output and the `ip route show` output.
#### Fail Criteria ####
The test fails if the `Default gateway IPv6` address is present in the `show interface mgmt` output or the `ip route show` output.

### Test case 4.04 - Verifying that the Primary and secondary DNS IPv6 addresses are configured in static mode. ###
#### Description ####
Configure IPv6 primary DNS and secondary DNS in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Primary Nameserver`, and `Secondary Nameserver` addresses are present in the `show interface mgmt` output and the `/etc/resolv.conf` file.
#### Fail Criteria ####
The test fails if the `Primary Nameserver`,and `Secondary Nameserver` addresses are missing in the `show interface mgmt` output or the `/etc/resolv.conf` file.


### Test case 4.05 -  Verifying that the Primary DNS and secondary DNS IPv6 addresses are removed in static mode. ###
#### Description ####
Remove the IPv6 primary DNS and secondary DNS in static mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the `Primary Nameserver`,and `Secondary Nameserver` addresses are missing in the `show interface mgmt` output and the `/etc/resolv.conf` file.
#### Fail Criteria ####
The test fails if the `Primary Nameserver`,and `Secondary Nameserver` addresses are present in the `show interface mgmt` output or the `/etc/resolv.conf` file.

## Verifying system hostname configuration testcases.  ##

### Objectives ###
   These cases test:
   - Configuring, reconfiguring and unconfiguring the system hostname.
   - Verifying the expected behavior of the system hostname.

### Requirements ###
The requirements for this test case are:

 -  DHCP Server.

### Setup ###
- #### Topology Diagram ####

                                                +-------------------+
              +------------------+              | Linux workstation |
              |                  |eth0     eth1 |+-----------------+|
              |  AS5712 switch   |--------------||   DHCP Server   ||
              |                  |              |+-----------------+|
              +------------------+              +-------------------+


### Test case 5.01 : Verifying that the system hostname is configured using CLI.  ###
#### Description ####
Test to verify whether hostname of the system changes to the value configured using CLI command "hostname new-name" in config mode.
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the configured value is present in `uname -n` output.
#### Fail Criteria ####
The test fails if  the configured hostname is not present in `uname -n` output.

### Test case 5.02 : Verifying that the system hostname is configured via DHCP Server.  ###
#### Description ####
Test to verify whether hostname of the system changes to the value configured by DHCP server via dhclient using option12 "option host-name".
### Test Result Criteria ###
#### Pass Criteria ####
The test is successful if the configured value is present in `uname -n` output.
#### Fail Criteria ####
The test fails if  the configured hostname is not present in `uname -n` output.
