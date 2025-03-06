Aquí tienes la información separada en tres tablas distintas para mayor claridad:

# Direccionamiento IP por Dispositivo

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
