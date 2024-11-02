# Un entorno virtualizado para prácticas de seguridad y filtros en Redes IPv6 con Containerlab
Este laboratorio de pruebas tiene como objetivo proporcionar una topología de red basada en Containerlab para el análisis de seguridad del Protocolo IPv6 en el proceso de autoconfiguración de direcciones sin estado (SLAAC). RFC 4862, RFC 7527, RFC 4941.

En base al análisis de tráfico de red de ataques comunes y RFCs de referencia, se propone la definición de reglas de alerta basadas en Suricata IDS y la posterior mitigación de vulnerabilidades en base a la configuración de ACLs filter en un dispositivo Nokia SRL Linux.

## Descripción de la topología

* Nokia SR Linux router node
* Nokia SR Linux switch node  
* PC1, PC2: Kali Linux OS. Image based on kali-rolling with packages net-tools, iproute2, ipv6toolkit and Thc-Ipv6.
* PC3, PC4: Kali Linux OS. 

Nokia SRL Linux Router General Config:
* Eth1/1.0 IPv6 static global unicast address (2001:db8:aaaa:1::1/64) with Router Advertisement, (advertised prefix: 2001:db8:aaaa:1::/64).

Nokia SRL Linux Switch General Config:
* A network instance type mac-vrf is created with the name "lanswitch". Interfaces eth1 to eth4 are configured as type bridged and associated with this net instance.
* An IPv6 ACL filter is configured, named "ipv6ra". The action is specified as drop, with logging set to true. The filter matches packets with IPv6 header field "next-header:58", "icmp6 type:134", "code:0"
* The configured ACL is attached to the eth2 and eth3 interfaces for incoming traffic.
* The eth4 interface connecting to PC2 Kali Linux is unprotected. The purpose is to perform security assessments, by executing attacks for future ACL definitions.
* For more configuration details, consult the config.json file
## Getting Started

### Dependencies

Prerequisites, libraries, OS version, etc., needed before running lab.
* 64-bit kernel and CPU support for virtualization.
* KVM virtualization support.
* At least 4 GB of RAM.
* Ubuntu Server 22.04 LTS with Docker engine. (https://docs.docker.com/desktop/install/linux-install/).
* Containerlab. (https://containerlab.dev/install/).
* For packet capture: Edgeshark. (https://github.com/siemens/edgeshark). 
### Installing topology files

* Clone repository to a current working directory in ubuntu server: git clone https://github.com/ernestosv73/ipv6seclab.git

### Executing topology lab

* From directory /ipv6seclab, run the command: clab deploy -t ipv6sec.clab.yml

### Managing nodes

* To connect to a bash shell Kali Linux Docker (PC1 or PC2), run the command: docker exec -it clab-ipv6sec-PC1 /bin/bash
* To connect to a bash shell Alpine Linux (PC3), run the command: docker exec -it clab-ipv6sec-PC1 /bin/bash
* You can access the CLI SRL Linux using a SSH connection running the command: ssh admin@clab-ipv6sec-SRL1. Default credentials: admin:NokiaSrl1!
 

## Test IPv6 Security with Thc-IPv6 Toolkit

To check the IPv6 filter ACL, run these examples:
* Description: Announce yourself as a router and try to become the default router.
  * PC1#atk6-fake_router6 eth1 2001:db8:bbbb:1::/64
* Description: Flood the local network with router advertisements. Option -F (perform full RA guard evasion, disallows all other bypass options)
  * PC1#ifconfig eth1 mtu 1500 up
  * PC1#atk6-flood_router26 eth1 -F
* Description: Announce yourself as a router and try to become the default router. Option -E o (overlapping fragments for keep-last targets)
  * PC1#atk6-fake_router26 eth1 -E o -A 2001:db8:dddd:1::/64

For more details, consult: https://www.kali.org/tools/thc-ipv6/

## Author

MSc. Ernesto Sánchez. 

mail: esanchez@ucasal.edu.ar

linkedin: https://www.linkedin.com/in/ernestos%C3%A1nchez


## License

This project is licensed under the [MIT] License - see the LICENSE.txt file for details

## Acknowledgments

* Alejandro Guevara. alejandro.guevara@nokia.com
* Henri Alvesde Godoy. henri.godoy@fca.unicamp.br
* Silvio Lucas da Silva. silvio.lucas@ifpb.edu.br 
