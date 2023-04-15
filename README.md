# Juniper Device System Checker


Suitable to be run by Level 1 techs and On-call members.
The intention if this script is to be a 'first port of call' when beginging your troubleshoot process...
It will in most cases, point out any unusual states that should be investigated or may likely be related to the problem you are seeing. 
This script is less effective on systems that are not well maintained.
i.e. legacy code/interfaces/BGP/VPN tunnels not cleaned up will affect results.

NOTE: Not to be provided to customers or external parties. 
In these cases you should still ask for an RSI and /var/logs Archive.

The scripts basic functions are:

#### CHASSIS
* Checks for Chassis Alarms
* Checks for failed hardware such as fans/mpcs/psu
* Checks that all attached PICs are online
#### SYSTEM
* Checks for any system alarms
* Reports on system Uptime
* Reports on connected users
* Reports on the last configuration change and by which user
* Reports on the JunOS Version running and the device type etc
* Compares the running version against the configured list of target versions
* Checks in the Routing Engines are healthy or under load & cpu tempurature
* Checks on Disk usage for important Partitions
* Checks if any software licenses have expired or are oversubscribed.
#### PROTOCOLS
* Checks the following Protocols for status and neigbourship failures, etc
  * LDP
  * OSPF
  * RSVP
  * BGP
#### VPN
* Checks for VPN tunnel failures. (For SRX Only)
#### INTERFACES
* Checks if any configured interfaces are operationally down. (may not be so useful on a switch)


To run the script, on any Internet connected Juniper Device...
Execute the following command on the Operational mode prompt (>)
op url https://raw.githubusercontent.com/thewhitehouse007/junos-sys-check/main/junos-sys-checks.slax

If you find any bugs or have suggestions for improvements/additional checks please forward them to <hidden>

### Example Console Output:
NOTE: Output is also logged to SYSLOG
```
-------------------------------------------------------------------------------------------------------
                                       Welcome admin
-------------------------------------------------------------------------------------------------------
    ** This is the output of the sys_check script for PBK-SW1 on Fri Oct  9 13:33:48 2020 **
-------------------------------------------------------------------------------------------------------
CHASSIS:   **  PASS  ** : There are no active chassis alarms
-------------------------------------------------------------------------------------------------------
SYSTEM:    **  PASS  ** : There are no active system alarms
SYSTEM:    *   INFO   * : System (fpc0) Uptime is 96 days,  3:20
SYSTEM:    *   INFO   * : admin is currently logged in from 172.31.227.8 since 30Sep20
SYSTEM:    *   INFO   * : Last commit was 2020-10-01 11:46:58 AEST by: mist
SYSTEM:    *   INFO   * : PBK-SW1(fpc0) is a ex2300-c-12p running 18.2R3-S2.9
SYSTEM:    *   INFO   * : RE0 is OK and master and has been up for 96 days, 3 hours, 20 minutes, 4 seconds since 0x1:power cycle/failure
SYSTEM:    **  WARN  ** : System load is high!!! (User: 60% System: 33% Background: 0%)
SYSTEM:    **  WARN  ** : Partition /.mount on fpc0 is currently at  64% used space.
-------------------------------------------------------------------------------------------------------
OSPF:      **  PASS  ** : There are 0 OSPF neighbor sessions UP
-------------------------------------------------------------------------------------------------------
LDP:       **  PASS  ** : There are 0 LDP neighbor interfaces UP (inc lo0.0)
LDP:       **  PASS  ** : There are 0 neighbor sessions UP
-------------------------------------------------------------------------------------------------------
RSVP:      **  PASS  ** : There are 0 RSVP interfaces UP
-------------------------------------------------------------------------------------------------------
BGP:       **  PASS  ** : There are 0 BGP neighbor sessions UP
-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------
INTERFACE: *** FAIL *** : Configured interface ge-0/0/2.0 is Admin Up but Operationally DOWN
INTERFACE: *** FAIL *** : Configured interface ge-0/0/3.0 is Admin Up but Operationally DOWN
-------------------------------------------------------------------------------------------------------

```
