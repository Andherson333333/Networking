# Diseño de Red Empresarial Redundante

Este repositorio contiene la configuración y documentación para una arquitectura de red empresarial completamente redundante basada en equipos Arista (EOS).

## Descripción general

La topología implementa un diseño de red de campus en tres capas (Core, Distribución, Acceso) con redundancia completa y segmentación de tráfico por departamentos.

![Network Topology Diagram]

## Componentes

### Dispositivos
- 2 Routers Core (CORE-1, CORE-2)
- 2 Switches de Distribución (DISTRIBUTION-1, DISTRIBUTION-2)
- 3 Switches de Acceso (SW-1, SW-2, SW-3)
- 2 Simuladores de ISP (ISP-1, ISP-2)

### Requisitos de hardware y software

- Arista vEOS 4.29.2F 
- 

### Protocolos implementados
- **OSPF**: Enrutamiento dinámico para routing interno
- **MLAG**: Agregación de enlaces multichassis entre distribución y acceso
- **VRRP**: Redundancia de gateway para VLANs
- **BFD**: Detección rápida de fallos en enlaces
- **Spanning-Tree**: MSTP para redundancia en capa 2

### Direccionamiento IP por Dispositivo

## Tabla 1: ISP Layer

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara |
|-------------|----------|-------------|--------------|---------|
| ISP-1 | Ethernet1 | Link-to-distribution-1 | 203.0.113.1 | 255.255.255.252 |
| ISP-1 | Loopback1 | Google-DNS | 8.8.8.8 | 255.255.255.255 |
| ISP-1 | Loopback2 | CloudFlare DNS Simulation | 1.1.1.1 | 255.255.255.255 |
| ISP-2 | Ethernet1 | Link-to-distribution-2 | 198.51.100.1 | 255.255.255.252 |
| ISP-2 | Loopback0 | Google-DNS | 8.8.8.8 | 255.255.255.255 |
| ISP-2 | Loopback1 | Level3 DNS Simulation | 2.2.2.2 | 255.255.255.255 |

## Tabla 2: Core Layer

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara |
|-------------|----------|-------------|--------------|---------|
| CORE-1 | Ethernet1 | ISP-1 | 203.0.113.2 | 255.255.255.252 |
| CORE-1 | Ethernet2 | CORE-2 | 172.16.254.9 | 255.255.255.252 |
| CORE-1 | Ethernet7 | DISTRIBUTION-2 | 172.16.253.9 | 255.255.255.252 |
| CORE-1 | Ethernet11 | DISTRIBUTION-1 | 172.16.253.1 | 255.255.255.252 |
| CORE-2 | Ethernet1 | ISP-2 | 198.51.100.2 | 255.255.255.252 |
| CORE-2 | Ethernet2 | CORE-1 | 172.16.254.10 | 255.255.255.252 |
| CORE-2 | Ethernet4 | DISTRIBUTION-1 | 172.16.253.5 | 255.255.255.252 |
| CORE-2 | Ethernet8 | DISTRIBUTION-2 | 172.16.253.13 | 255.255.255.252 |

## Tabla 3: Distribution Layer

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara | VRRP |
|-------------|----------|-------------|--------------|---------|------|
| DISTRIBUTION-1 | Ethernet4 | CORE-2 | 172.16.253.6 | 255.255.255.252 | - |
| DISTRIBUTION-1 | Ethernet11 | CORE-1 | 172.16.253.2 | 255.255.255.252 | - |
| DISTRIBUTION-1 | Vlan4094 | MLAG_PEER | 172.16.253.17 | 255.255.255.252 | - |
| DISTRIBUTION-1 | Vlan10 | Administración | 172.16.10.2 | 255.255.255.0 | 172.16.10.1 (Pri 110) |
| DISTRIBUTION-1 | Vlan20 | Finanzas | 172.16.20.2 | 255.255.255.0 | 172.16.20.1 (Pri 110) |
| DISTRIBUTION-1 | Vlan30 | RRHH | 172.16.30.2 | 255.255.255.0 | 172.16.30.1 (Pri 110) |
| DISTRIBUTION-1 | Vlan40 | Ventas | 172.16.40.2 | 255.255.255.0 | 172.16.40.1 (Pri 110) |
| DISTRIBUTION-1 | Vlan50 | Producción | 172.16.50.2 | 255.255.255.0 | 172.16.50.1 (Pri 110) |
| DISTRIBUTION-1 | Vlan60 | Desarrollo | 172.16.60.2 | 255.255.255.0 | 172.16.60.1 (Pri 110) |
| DISTRIBUTION-2 | Ethernet7 | CORE-1 | 172.16.253.10 | 255.255.255.252 | - |
| DISTRIBUTION-2 | Ethernet8 | CORE-2 | 172.16.253.14 | 255.255.255.252 | - |
| DISTRIBUTION-2 | Vlan4094 | MLAG_PEER | 172.16.253.18 | 255.255.255.252 | - |
| DISTRIBUTION-2 | Vlan10 | Administración | 172.16.10.3 | 255.255.255.0 | 172.16.10.1 (Pri 90) |
| DISTRIBUTION-2 | Vlan20 | Finanzas | 172.16.20.3 | 255.255.255.0 | 172.16.20.1 (Pri 90) |
| DISTRIBUTION-2 | Vlan30 | RRHH | 172.16.30.3 | 255.255.255.0 | 172.16.30.1 (Pri 90) |
| DISTRIBUTION-2 | Vlan40 | Ventas | 172.16.40.3 | 255.255.255.0 | 172.16.40.1 (Pri 90) |
| DISTRIBUTION-2 | Vlan50 | Producción | 172.16.50.3 | 255.255.255.0 | 172.16.50.1 (Pri 90) |
| DISTRIBUTION-2 | Vlan60 | Desarrollo | 172.16.60.3 | 255.255.255.0 | 172.16.60.1 (Pri 90) |

## Tabla 4: Access Layer (Switches)

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara |
|-------------|----------|-------------|--------------|---------|
| SW-1 | Vlan10 | Administración | 172.16.10.4 | 255.255.255.0 |
| SW-1 | Vlan20 | Finanzas | 172.16.20.4 | 255.255.255.0 |
| SW-2 | Vlan30 | RRHH | 172.16.30.4 | 255.255.255.0 |
| SW-2 | Vlan40 | Ventas | 172.16.40.4 | 255.255.255.0 |
| SW-3 | Vlan50 | Producción | 172.16.50.4 | 255.255.255.0 |
| SW-3 | Vlan60 | Desarrollo | 172.16.60.4 | 255.255.255.0 |

## Estructura del repositorio


## Instalación y configuración


## Protocolos

### OSPF
- **Áreas**:
  - Area 0: Core y enlaces de backbone
  - Area 0.0.0.1: VLANs 10 y 20
  - Area 0.0.0.2: VLANs 30 y 40
  - Area 0.0.0.3: VLANs 50 y 60

  - **Routers ID**:
  - CORE-1: 1.1.1.1
  - CORE-2: 2.2.2.2
  - DISTRIBUTION-1: 3.3.3.3
  - DISTRIBUTION-2: 4.4.4.4

### MLAG
- **Dominio**: DISTRIBUTION
- **Peer Link**: Port-Channel100 (2x10G)
- **Peer VLAN**: 4094
- **Peer IPs**:
  - DISTRIBUTION-1: 172.16.253.17/30
  - DISTRIBUTION-2: 172.16.253.18/30

### VRRP
- Configurado en las VLANs 10-60
- DISTRIBUTION-1 como router primario (prioridad 110)
- DISTRIBUTION-2 como router secundario (prioridad 90)
- VIP configurado como .1 en cada VLAN
  
### BFD
- Habilitado en todas las interfaces OSPF
- Interval: 500ms, min_rx: 500ms, multiplier: 5
- Interval en VLANs: 100ms, min_rx: 100ms, multiplier: 5














