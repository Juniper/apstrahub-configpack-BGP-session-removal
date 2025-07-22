# BGP Session Removal Configlet for Apstra

This configlet automates the removal of BGP sessions for decommissioned devices in Juniper Apstra.

## Overview

The configlet performs three key tasks:
1. Identifies devices tagged for decommissioning
2. Locates BGP sessions associated with these devices
3. Generates configuration commands to remove the identified BGP sessions on neighboring devices

## Use Case

This configlet is designed to handle scenarios where a device (e.g., a spine switch) is being decommissioned from the network. When a device is undeployed in Apstra, the BGP sessions on neighboring devices are not automatically removed. This configlet fills that gap by generating the necessary deletion commands for those BGP sessions.

For example, if a spine switch is tagged with 'decomm' and this configlet is applied to all leaf switches, it will create Junos `delete` statements on each leaf switch for every BGP session (both IP and EVPN) that would have faced the decommissioned spine.

## Configlet

The configlet uses Jinja2 templating to generate the necessary BGP configuration deletions. It performs the following steps:

1. Collects IP addresses of interfaces connected to devices tagged with 'decomm'
2. Identifies BGP sessions associated with these IP addresses
3. Generates BGP configuration deletions for affected sessions on neighboring devices

## System Tagging

To use this configlet, you must tag the device that is to be decommissioned with the tag 'decomm' in your blueprint. This is crucial for the configlet to identify which BGP sessions should be removed on neighboring devices.

### Applying the System Tag

1. In the blueprint, go to Staged > Physical > Topology.
2. Select the device you want to decommission (e.g., a spine switch).
3. Click "Update Node Tags" and add the tag "decomm" to the device.
4. The configlet, when applied to other devices (e.g., leaf switches), will automatically generate the necessary BGP configuration deletions for sessions connected to the tagged device.

Note: Always review the generated configuration before applying it to your network to ensure it aligns with your decommissioning plans. This configlet should be used as part of a carefully planned decommissioning process.