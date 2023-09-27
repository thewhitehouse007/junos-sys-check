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
* Checks the system snapshots are up to date and performs a snapshot if required
* Checks if any software licenses have expired or are oversubscribed.
#### PROTOCOLS
* Checks the following Protocols for status and neigbourship failures, etc
  * OSPF
  * MPLS
    * LDP
    * RSVP
  * BGP
#### VPN
* Checks for VPN tunnel failures. (For SRX Only)
#### INTERFACES
* Checks if any configured interfaces are operationally down. (may not be so useful on a switch)


To run the script, on any Internet connected Juniper Device...
Execute the following command on the Operational mode prompt (>)
op url https://raw.githubusercontent.com/thewhitehouse007/junos-sys-check/main/junos-sys-checks.slax

### Options:
## Mode
There are several "modes" that the script can operate in, if you wish to pick specific tests e.g
Using `./junos-sys-checks.slax mode <mode>` you can choose from the following options...
* `full` - The default, runs all tests available, this can take a couple in minutes
* `base` - Runs only Chassis and System checks, this is best used for ZTP testing
* `proto` - Runs only Protocol Checks, this is a fast check.
* `conn` - Runs only VPN (if SRX) and Interface checks, quick recheck option

## Snapshots
The default operation is to perform a system snapshot if the existing snapshot version does not match
the version of code running on the device. You may override this to prevent the snapshot operation 
being performed. (Note: This will still validate if the snapshots are up to date.)
Example: ``./junos-sys-checks.slax snap no`

If you find any bugs or have suggestions for improvements/additional checks please forward them to <hidden>

### Example Console Output:
`./junos-sys-checks.slax mode <mode> snap no`
NOTE: Output is also logged to SYSLOG
```
-------------------------------------------------------------------------------------------------------
                      Welcome admin                         Mode: full
-------------------------------------------------------------------------------------------------------
         ** This is the output of the sys_check script for <SERIAL_NUMBER> on srx320 **
-------------------------------------------------------------------------------------------------------
CHASSIS:   **  PASS  ** : There are no active chassis alarms
CHASSIS:   **  PASS  ** : Environment component checks are all OK
CHASSIS:   *** FAIL *** : FPC - 1 is Offline
-------------------------------------------------------------------------------------------------------
SYSTEM:    **  WARN  ** : Minor ALARM - License for feature idp-sig(29) is about to expire
SYSTEM:    *   INFO   * : System Uptime is 103 days, 22:10
SYSTEM:    *   INFO   * : admin is currently logged in from 192.168.1.8 since 10:11AM
SYSTEM:    *   INFO   * : Last commit was 2023-09-26 12:06:08 EST by: admin
SYSTEM:    *   INFO   * : Device is a srx320 running 21.4R3-S3.4
SYSTEM:    *** FAIL *** : Device srx320 is not running target Version of Junos, (21.4R3-S3)
SYSTEM:    **  PASS  ** : Routing Engine is OK
SYSTEM:    **  PASS  ** : System temperature is normal!
SYSTEM:    **  WARN  ** : System load is high!!! (User: 52% System: 20% Background: 0%)
SYSTEM:    **  WARN  ** : Disk Partition / is currently at  78% used space.
SYSTEM:    *** INFO *** : Checking system snapshots... wait...
SYSTEM:    **  PASS  ** : System Snapshots match current Junos version (21.4R3-S3.4)
SYSTEM:    *** FAIL *** : The feature license idp-sig is oversubscribed!
-------------------------------------------------------------------------------------------------------
OSPF:      *   INFO   * : OSPF is not configured
-------------------------------------------------------------------------------------------------------
MPLS:      *   INFO   * : MPLS is not configured
-------------------------------------------------------------------------------------------------------
BGP:       *   INFO   * : There are 5 BGP neighbor sessions UP
BGP:       *** FAIL *** : Peer  @ 42.54.13.13 is DOWN
BGP:       *** FAIL *** : Peer  @ 19.24.22.11 is DOWN
-------------------------------------------------------------------------------------------------------
VPN:       *   INFO   * : There are  VPN tunnels UP
VPN:       *** FAIL *** : Tunnel use1-aws-01 @ 65.12.45.18 is DOWN
                          Reason: IKE SA rekey successfully completed
VPN:       *** FAIL *** : Tunnel use1-aws-02 @ 42.65.31.29 is DOWN
                          Reason: IKE SA rekey successfully completed
-------------------------------------------------------------------------------------------------------
INTERFACE: *** FAIL *** : Configured interface ge-0/0/2.0 is Admin Up but Operationally DOWN
INTERFACE: *** FAIL *** : Configured interface ge-0/0/3.0 is Admin Up but Operationally DOWN
-------------------------------------------------------------------------------------------------------

```
