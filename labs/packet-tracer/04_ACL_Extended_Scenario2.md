---
title: "Packet Tracer – ACL Extendida con Nombre (Escenario 2)"
author: "Diego Ramón Claros"
version: "1.0"
date: "2025-10-26"
---

# 🧩 Packet Tracer – Configurar ACL Extendida con Nombre (Escenario 2)

## 🎯 Objetivo
Configurar una **ACL extendida con nombre** en el router **RT1** para restringir servicios específicos desde la LAN hacia los servidores externos.

---

## 🖧 Topología y Direccionamiento

| Dispositivo | Interfaz | Dirección IP | Máscara | Gateway |
|--------------|-----------|--------------|----------|----------|
| RT1 | G0/0 | 172.31.1.126 | 255.255.255.224 | — |
| RT1 | S0/0/0 | 209.165.1.2 | 255.255.255.252 | — |
| PC1 | NIC | 172.31.1.101 | 255.255.255.224 | 172.31.1.126 |
| PC2 | NIC | 172.31.1.102 | 255.255.255.224 | 172.31.1.126 |
| PC3 | NIC | 172.31.1.103 | 255.255.255.224 | 172.31.1.126 |
| Server1 | NIC | 64.101.255.254 | 255.255.255.0 | — |
| Server2 | NIC | 64.103.255.254 | 255.255.255.0 | — |

---

## ⚙️ Configuración del Router RT1

```bash
enable
configure terminal

! Configuración de interfaces
interface GigabitEthernet0/0
 description LAN Local
 ip address 172.31.1.126 255.255.255.224
 no shutdown

interface Serial0/0/0
 description Enlace WAN a Internet
 ip address 209.165.1.2 255.255.255.252
 no shutdown

! Configuración de ACL extendida con nombre
ip access-list extended ACL

! PC1 – Bloquear HTTP y HTTPS hacia Server1 y Server2
 deny tcp host 172.31.1.101 host 64.101.255.254 eq 80
 deny tcp host 172.31.1.101 host 64.101.255.254 eq 443
 deny tcp host 172.31.1.101 host 64.103.255.254 eq 80
 deny tcp host 172.31.1.101 host 64.103.255.254 eq 443

! PC2 – Bloquear FTP hacia Server1 y Server2
 deny tcp host 172.31.1.102 host 64.101.255.254 eq 21
 deny tcp host 172.31.1.102 host 64.103.255.254 eq 21

! PC3 – Bloquear ICMP (ping) hacia Server1 y Server2
 deny icmp host 172.31.1.103 host 64.101.255.254
 deny icmp host 172.31.1.103 host 64.103.255.254

! Permitir el resto del tráfico
 permit ip any any

exit

! Aplicar ACL en la interfaz LAN de salida
interface GigabitEthernet0/0
 ip access-group ACL out

end
write memory
