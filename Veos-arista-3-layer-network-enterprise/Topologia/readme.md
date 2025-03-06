# Topología de Red

## Arquitectura de Tres Niveles (Three-Tier)

La arquitectura de red implementada sigue el modelo de tres niveles (Three-Tier), que divide la infraestructura en tres capas funcionales: Core, Distribution y Access. Este diseño jerárquico proporciona escalabilidad, redundancia y una clara separación de funciones.

### Características de cada capa

#### Capa de Core (Núcleo)
- **Función principal**: Proporcionar transporte óptimo y rápido entre sitios y hacia Internet
- **Dispositivos**: CORE-1 y CORE-2
- **Características**: Alta velocidad, bajo retardo, conexiones redundantes a ISPs
- **Protocolos**: OSPF, BFD, rutas estáticas a Internet

#### Capa de Distribution (Distribución)
- **Función principal**: Implementar políticas, agregar conexiones y segmentar dominios de broadcast
- **Dispositivos**: DISTRIBUTION-1 y DISTRIBUTION-2
- **Características**: Enrutamiento inter-VLAN, agregación de enlaces, redundancia de gateway
- **Protocolos**: OSPF, MLAG, VRRP, BFD

#### Capa de Access (Acceso)
- **Función principal**: Proporcionar conectividad a dispositivos de usuario final
- **Dispositivos**: SW-1, SW-2 y SW-3
- **Características**: Switching L2, VLANs por departamento
- **Protocolos**: STP, LACP

## Diagrama físico

![Network Topology Diagram](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-1.png)

## Diagrama Logico

![Network Topology Diagram](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos--topologi-1.png)

## Direccionamiento IP por Dispositivo

### Tabla 1: ISP Layer

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara |
|-------------|----------|-------------|--------------|---------|
| ISP-1 | Ethernet1 | Link-to-distribution-1 | 203.0.113.1 | 255.255.255.252 |
| ISP-1 | Loopback1 | Google-DNS | 8.8.8.8 | 255.255.255.255 |
| ISP-1 | Loopback2 | CloudFlare DNS Simulation | 1.1.1.1 | 255.255.255.255 |
| ISP-2 | Ethernet1 | Link-to-distribution-2 | 198.51.100.1 | 255.255.255.252 |
| ISP-2 | Loopback0 | Google-DNS | 8.8.8.8 | 255.255.255.255 |
| ISP-2 | Loopback1 | Level3 DNS Simulation | 2.2.2.2 | 255.255.255.255 |

### Tabla 2: Core Layer

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

### Tabla 3: Distribution Layer

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

### Tabla 4: Access Layer (Switches)

| Dispositivo | Interfaz | Descripción | Dirección IP | Máscara |
|-------------|----------|-------------|--------------|---------|
| SW-1 | Vlan10 | Administración | 172.16.10.4 | 255.255.255.0 |
| SW-1 | Vlan20 | Finanzas | 172.16.20.4 | 255.255.255.0 |
| SW-2 | Vlan30 | RRHH | 172.16.30.4 | 255.255.255.0 |
| SW-2 | Vlan40 | Ventas | 172.16.40.4 | 255.255.255.0 |
| SW-3 | Vlan50 | Producción | 172.16.50.4 | 255.255.255.0 |
| SW-3 | Vlan60 | Desarrollo | 172.16.60.4 | 255.255.255.0 |

## Segmentación por VLANs

| VLAN ID | Nombre | Descripción | Gateway VRRP |
|---------|--------|-------------|--------------|
| 10 | Administración | Departamento IT | 172.16.10.1 |
| 20 | Finanzas | Departamento Financiero | 172.16.20.1 |
| 30 | RRHH | Recursos Humanos | 172.16.30.1 |
| 40 | Ventas | Departamento Comercial | 172.16.40.1 |
| 50 | Producción | Entorno de producción | 172.16.50.1 |
| 60 | Desarrollo | Equipo de desarrollo | 172.16.60.1 |
| 999 | Management | VLAN nativa para trunks | N/A |
| 4094 | MLAG_PEER | Comunicación entre peers MLAG | N/A |

## Flujos de tráfico

### Tráfico Norte-Sur (Usuarios a Internet)
1. El tráfico de usuario entra por el switch de acceso (SW-X)
2. Sube a través de los enlaces MLAG hacia los switches de distribución
3. El tráfico es enrutado hacia los routers de core (CORE-1 o CORE-2)
4. Sale hacia Internet a través del ISP correspondiente (ISP-1 o ISP-2)

### Tráfico Este-Oeste (Entre VLANs)
1. El tráfico entre VLANs en el mismo switch de acceso sube a la capa de distribución
2. Los switches de distribución realizan routing entre VLANs (interfaces SVI)
3. El tráfico regresa al switch de acceso a través de los enlaces MLAG
4. No es necesario que el tráfico atraviese la capa de core

## Consideraciones de diseño

- Redundancia completa en todos los niveles
- Ningún punto único de fallo
- Rápida detección de fallos mediante BFD
- Capa de distribución activa-activa mediante MLAG y VRRP
- Segregación de tráfico por departamentos para mejorar seguridad y desempeño
- Múltiples áreas OSPF para optimizar el número de LSAs y la convergencia
