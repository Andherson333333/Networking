# Diseño de Red Empresarial Redundante

Este repositorio contiene la configuración y documentación para una arquitectura de red empresarial completamente redundante basada en equipos Arista (EOS).

## Descripción general

La topología implementa un diseño de red de campus en tres capas (Core, Distribución, Acceso) con redundancia completa y segmentación de tráfico por departamentos.

![Network Topology Diagram](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos--topologi-1.png)

## Componentes

### Dispositivos
- 2 Routers Core (CORE-1, CORE-2)
- 2 Switches de Distribución (DISTRIBUTION-1, DISTRIBUTION-2)
- 3 Switches de Acceso (SW-1, SW-2, SW-3)
- 2 Simuladores de ISP (ISP-1, ISP-2)

### Requisitos de hardware y software
- EVE-NG
- Arista vEOS-lab 4.29.2F

### Protocolos implementados
- **[OSPF](./OSPF/readme.md)**: Enrutamiento dinámico para routing interno
- **[MLAG](./MLAG/readme.md)**: Agregación de enlaces multichassis entre distribución y acceso
- **[VRRP](./VRRP/readme.md)**: Redundancia de gateway para VLANs
- **[BFD](./BFD/Readme.md)**: Detección rápida de fallos en enlaces
- **[Spanning-Tree](./Spanning-Tree/readme.md)**: MSTP para redundancia en capa 2
- **[VLAN](./VLAN/readme.md)**: redes lógicas aisladas


## Estructura del repositorio
```
/
├── Topologia/          # Información detallada de la topología y direccionamiento IP
├── VLAN/               # Configuración de VLANs por departamento
├── Spanning-Tree/      # Configuración de STP/MSTP
├── MLAG/               # Configuración de Multi-Chassis Link Aggregation
├── VRRP/               # Configuración de Virtual Router Redundancy Protocol
├── OSPF/               # Configuración de OSPF y distribución de áreas
├── BFD/                # Configuración de Bidirectional Forwarding Detection
├── configs/            # Archivos de configuración por dispositivo
│   ├── CORE-1.conf
│   ├── CORE-2.conf
│   ├── DISTRIBUTION-1.conf
│   ├── DISTRIBUTION-2.conf
│   ├── SW-1.conf
│   ├── SW-2.conf
│   ├── SW-3.conf
│   ├── ISP-1.conf
│   └── ISP-2.conf
├── imagenes/           # Diagramas y capturas de pantalla
```
## Instalación y configuración

1. Importe los archivos de configuración en su entorno EVE-NG
2. Asegúrese de utilizar la versión correcta de Arista vEOS-lab (4.29.2F)
3. Implemente la topología siguiendo el diagrama proporcionado
    - 1 topologia
    - 2 Vlan
    - Vrrp
    - Mlag
    - ospf
    - bfd
    - monitor
5. Aplique las configuraciones para cada dispositivo según la documentación

## Características principales

- Redundancia completa en todas las capas
- Segregación de tráfico por departamentos mediante VLANs
- Rápida convergencia ante fallos mediante BFD
- Balanceo de carga mediante MLAG
- Redundancia de gateway mediante VRRP
- Distribución de rutas mediante OSPF












