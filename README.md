# junos-sys-check
Juniper Device System Checker


Suitable to be run by Level 1 techs and On-call members.
The intention if this script is to be a 'first port of call' when beginging your troubleshoot process...
It will in most cases, point out any unusual states that should be investigated or may likely be related to the problem you are seeing. 
This script is less affective on systems that are not well maintained, 
i.e. legacy code/interfaces/BGP/VPN tunnels not cleaned up will affect results.

NOTE: Not to be provided to customers or external parties. 
In these cases you should still ask for an RSI and /var/logs Archive.

The scripts basic functions are:
* Checks for Chassis Alarms
* Checks for failed hardware such as fans/mpcs/psu.
* Checks for any system alarms
* Reports on system Uptime
* Reports on connected users
* Reports on the last configuration change and by which users
* Reports on the JunOS Version running and the device type etc
* Checks in the Routing Engines are healthy or under load & cpu tempurature.
* Checks if any software licenses have expired or are oversubscribed.
PROTOCOLS
* Checks the following Protocols for status and neigbourship failures, etc
** LDP
** OSPF
** BGP
** RSVP
INTERFACES
* Checks for VPN tunnel failures.
* Checks if any configured interfaces are operationally down. (may not be so useful on a switch)


To run the script, on any Internet connected Juniper Device...
Execute the following command on the Operational mode prompt (>)
op url https://files.charter.ca/index.php/s/CesF4jFmznnxzRk

If you find any bugs or have suggestions for improvements/additional checks please forward them to <hidden>

