# Un entorno virtualizado para prácticas de seguridad y filtros en Redes IPv6 con Containerlab
Este laboratorio de pruebas tiene como objetivo proporcionar una topología de red basada en Containerlab para el análisis de seguridad del Protocolo IPv6 en el proceso de autoconfiguración de direcciones sin estado (SLAAC). RFC 4862, RFC 7527, RFC 4941.

En base al análisis de tráfico de red de ataques comunes y RFCs de referencia, se propone la definición de reglas de alerta basadas en Suricata IDS y la posterior mitigación de vulnerabilidades en base a la configuración de ACLs filter en un dispositivo Nokia SRL Linux.

## Descripción de la topología

* Nokia SR Linux router node
* Nokia SR Linux switch node  
* PC1, PC2: Kali Linux OS. Image based on kali-rolling with packages net-tools, iproute2, ipv6toolkit and Thc-Ipv6.
* PC3, PC4: Kali Linux OS. Image based on kali-rolling from, https://hub.docker.com/r/bodane/exfil, with package IPv6teal.
* Suricata IDS: Ubuntu 22.04. https://hub.docker.com/repository/docker/esanchezv/suricatafilebeatv1/general
* Elasticsearch y Kibana from https://www.docker.elastic.co/ y https://github.com/srl-labs/srl-elk-lab 

Nokia SRL Linux Switch General Config:
* network instance type mac-vrf iname "lanswitch". 
* Se configuró IPv6 ACL filter "ipv6ra" y se aplicó a los puertos interface ethernet-1/2, 1/3, 1/5, 1/8.  
* Ver detalles en Webinar LACNIC: https://www.lacnic.net/7495/1/lacnic/
## Getting Started

### Dependencies

Prerequisitos, librerias, OS version, etc., necesarios para el deploy de la topología.
* 64-bit kernel and CPU support for virtualization.
* KVM virtualization support.
* At least 8 GB of RAM.
* Ubuntu Server 22.04 LTS with Docker engine. (https://docs.docker.com/desktop/install/linux-install/).
* Containerlab. (https://containerlab.dev/install/).
* For packet capture: Edgeshark. (https://github.com/siemens/edgeshark). 

## Author

MSc. Ernesto Sánchez. 

mail: esanchez@ucasal.edu.ar

linkedin: https://www.linkedin.com/in/ernestos%C3%A1nchez

## License

This project is licensed under the [MIT] License - see the LICENSE.txt file for details

## Acknowledgments

* Alejandro Acosta. alejandro@lacnic.net
* Henri Alvesde Godoy. henri.godoy@fca.unicamp.br
* Alejandro Guevara. alejandro.guevara@nokia.com
