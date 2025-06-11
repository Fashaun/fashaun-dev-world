---
{"dg-publish":true,"permalink":"/fashaun-dev-world/dev-military/","noteIcon":""}
---


# Edge-core

> Hotel Guest WiFi Hotspot Solution

## WiFi Controller (WLC)
- https://www.cisco.com/c/en/us/products/wireless/wireless-lan-controller/what-is-wlan-controller.html
- https://wifi.edge-core.com/wlan-gateway-controller/

## WiFi AP (LWAP)

## Concept of "AAA"
-  Authentication
- Authorization
- Accounting

- https://www.fortinet.com/resources/cyberglossary/aaa-security
- https://www.cisco.com/c/en/us/td/docs/security/asa/asa92/configuration/general/asa-general-cli/aaa-overview.pdf
## Concept of "Service Zone"
Service Zones are virtual partitions of the physical LAN side of a Controller. Similar to VLANs, Service Zones can be separately managed and defined with their own user landing pages, network interface settings, DHCP servers, authentication options, policies, security settings, and so on. By associating Service Zones with a unique VLAN Tag (when using tag-based mode) and an SSID, administrator can flexibly separate wired and wireless networks with ease.

- Service Zone features include: 
	- (1) VLAN, Isolation, NAT/Router Mode 
	- (2) DHCP Server Option 
	- (3) Authentication Settings 
	- (4) Page Customization


## Concept of "Complete Tunnel"

- Complete Tunnel uses the CAPWAP protocol to communicate with an Access Point so that all management traffic, authentication traffic and data traffic from the service area Access Point provided are transmitted back to the Controller, before forwarding data traffic to the internet. The Controller is able to implement role-based policies over Layer 3 networks, with user access control available in the remote sites. This feature allows the Controller to fully support centralized Access Point management and user management.

![](/img/user/fashaun-dev-world/attachments/DevMilitaryCompleteTunnel.png)


## Concept of "Split Tunnel"
- For Split tunnel, only user authentication related traffic will be directed back to the controller. For authenticated users, data traffic will go to the Internet through the local network directly. The user data can be transmitted with a shorter path and the network load of the controller can also be reduced.

![](/img/user/fashaun-dev-world/attachments/DevMilitarySPTunnel.png)

### Application Scenario

![](/img/user/fashaun-dev-world/attachments/DevMilitarySPApplicationScenario.png)



## Concept of Cloud Controller 

### Related Works

- https://www.fortinet.com/resources/cyberglossary/vpn-split-tunneling
- https://tailscale.com/learn/what-is-split-tunneling-secure-critical-data-vpn
- https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/configure-warp/route-traffic/split-tunnels/
- https://surfshark.com/zh-tw/features/split-tunneling?srsltid=AfmBOooeVieG9jWOb8tR0DbgWeg0bWTSUahTL8WJ2cXi8A-IaFELPABa


# Tags
#edgecore, #capwap
#dpdk, #sr-iov

# Ref
- https://www.edge-core.com/_upload/files/Service_Zone_Concept_20180627.pdf
- https://www.youtube.com/@edgecorewifi4615