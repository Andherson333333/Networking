# Configuración de VLANs

## Descripción general

Las Virtual LANs (VLANs) permiten segmentar una red física en múltiples redes lógicas aisladas. En esta implementación, las VLANs se utilizan para separar el tráfico por departamentos, mejorando la seguridad y el rendimiento de la red.

![Diagrama VLANs](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-vlans-topologi-1.png)

## Tabla de VLANs

| VLAN ID | Nombre | Descripción | Switch de acceso | Gateway VRRP |
|---------|--------|-------------|------------------|--------------|
| 10 | Administración | Departamento IT | SW-1 | 172.16.10.1 |
| 20 | Finanzas | Departamento Financiero | SW-1 | 172.16.20.1 |
| 30 | RRHH | Recursos Humanos | SW-2 | 172.16.30.1 |
| 40 | Ventas | Departamento Comercial | SW-2 | 172.16.40.1 |
| 50 | Producción | Entorno de producción | SW-3 | 172.16.50.1 |
| 60 | Desarrollo | Equipo de desarrollo | SW-3 | 172.16.60.1 |
| 999 | Management | VLAN nativa para trunks | Todos | N/A |
| 4094 | MLAG_PEER | Comunicación entre peers MLAG | DIST-1, DIST-2 | N/A |

## Configuración por dispositivo

### Distribution Layer

#### DISTRIBUTION-1
```
vlan 1
   state suspend

vlan 10
   name Administracion

vlan 20
   name Finanzas

vlan 30
   name RRHH

vlan 40
   name Ventas

vlan 50
   name Produccin

vlan 60
   name Desarrollo

vlan 999
   name Management

vlan 4094
   name MLAG_PEER
```

#### DISTRIBUTION-2
```
vlan 1
   state suspend

vlan 10
   name Administracion

vlan 20
   name Finanzas

vlan 30
   name RRHH

vlan 40
   name Ventas

vlan 50
   name Produccin

vlan 60
   name Desarrollo

vlan 999
   name Management

vlan 4094
   name MLAG_PEER
```

### Access Layer

#### SW-1
```
vlan 1
   state suspend

vlan 10
   name Administracion

vlan 20
   name Finanzas

vlan 999
   name Management

interface Ethernet1
   description PC_VLAN10
   switchport access vlan 10

interface Ethernet2
   description PC_VLAN20
   switchport access vlan 20
```

#### SW-2
```
vlan 1
   state suspend

vlan 30
   name RRHH

vlan 40
   name Ventas

vlan 999
   name Management

interface Ethernet1
   description PC_VLAN30
   switchport access vlan 30

interface Ethernet2
   description PC_VLAN40
   switchport access vlan 40
```

#### SW-3
```
vlan 1
   state suspend

vlan 50
   name Produccin

vlan 60
   name Desarrollo

vlan 999
   name Management

interface Ethernet1
   description PC_VLAN50
   switchport access vlan 50
   spanning-tree portfast

interface Ethernet2
   description PC_VLAN60
   switchport access vlan 60
   spanning-tree portfast
```

## Configuración de interfaces SVI

### DISTRIBUTION-1
```
interface Vlan10
   description Administracion
   ip address 172.16.10.2/24

interface Vlan20
   description Finanzas
   ip address 172.16.20.2/24

interface Vlan30
   description RRHH
   ip address 172.16.30.2/24

interface Vlan40
   description Ventas
   ip address 172.16.40.2/24

interface Vlan50
   description Producción
   ip address 172.16.50.2/24

interface Vlan60
   description Desarrollo
   ip address 172.16.60.2/24

interface Vlan4094
   description MLAG_PEER
   no autostate
   ip address 172.16.253.17/30
```

### DISTRIBUTION-2
```
interface Vlan10
   description Administracion
   ip address 172.16.10.3/24

interface Vlan20
   description Finanzas
   ip address 172.16.20.3/24

interface Vlan30
   description RRHH
   ip address 172.16.30.3/24

interface Vlan40
   description Ventas
   ip address 172.16.40.3/24

interface Vlan50
   description Producción
   ip address 172.16.50.3/24

interface Vlan60
   description Desarrollo
   ip address 172.16.60.3/24

interface Vlan4094
   description MLAG_PEER
   no autostate
   ip address 172.16.253.18/30
```

### Access Layer SVIs

#### SW-1
```
interface Vlan10
   description Administracion
   ip address 172.16.10.4/24

interface Vlan20
   description Finanzas
   ip address 172.16.20.4/24
```

#### SW-2
```
interface Vlan30
   description RRHH
   ip address 172.16.30.4/24

interface Vlan40
   description Ventas
   ip address 172.16.40.4/24
```

#### SW-3
```
interface Vlan50
   description Produccion
   ip address 172.16.50.4/24

interface Vlan60
   description Desarrollo
   ip address 172.16.60.4/24
```

## Configuración de troncales (trunks)

### DISTRIBUTION-1
```
interface Ethernet3
   description TRUNK_TO_SW1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk

interface Ethernet5
   description TRUNK_TO_SW2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk

interface Ethernet8
   description TRUNK_TO_SW3
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk

interface Ethernet1
   description TRUNK_TO_DIST2
   switchport trunk native vlan 999
   switchport mode trunk
```

### DISTRIBUTION-2
```
interface Ethernet3
   description TRUNK_TO_SW1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk

interface Ethernet4
   description TRUNK_TO_SW2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk

interface Ethernet5
   description TRUNK_TO_SW3
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk

interface Ethernet1
   description TRUNK_TO_DIST1
   switchport trunk native vlan 999
   switchport mode trunk
```

### SW-1
```
interface Ethernet3
   description TRUNK_TO_DIST2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk

interface Ethernet6
   description TRUNK_TO_DIST1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk
```

### SW-2
```
interface Ethernet4
   description TRUNK_TO_DIST2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk

interface Ethernet7
   description TRUNK_TO_DIST1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk
```

### SW-3
```
interface Ethernet5
   description TRUNK_TO_DIST2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk

interface Ethernet8
   description TRUNK_TO_DIST1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk
```

## Verificación

Para verificar la configuración de VLANs, utilice los siguientes comandos:

```
show vlan
show vlan brief
show interface trunk
show interface status
show ip interface brief
```

- Distribution-1 y 2
![VLANs-show](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-vlans-1.png)

- Sw-1 ,Sw2,Sw3
![VLANs-show](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-vlans-2.png)
![VLANs-show](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-vlans-3.png)

## Consideraciones de diseño

1. **Segmentación por departamentos**:
   - Cada VLAN corresponde a un departamento o función específica
   - Los departamentos relacionados se agrupan en el mismo switch de acceso
   - Esta segmentación mejora la seguridad y el rendimiento

2. **VLAN nativa**:
   - VLAN 999 se utiliza como VLAN nativa para todos los enlaces troncales
   - No se utiliza la VLAN 1 por defecto como medida de seguridad

3. **Suspensión de VLAN 1**:
   - La VLAN 1 está suspendida en todos los dispositivos (`state suspend`)
   - Esto es una buena práctica de seguridad para evitar ataques de VLAN hopping

4. **VLAN dedicada para MLAG**:
   - VLAN 4094 se utiliza exclusivamente para comunicación entre peers MLAG
   - Se configura con `no autostate` para mantenerla activa en todo momento

5. **Distribución de VLANs**:
   - SW-1: VLANs 10, 20 (Administración, Finanzas)
   - SW-2: VLANs 30, 40 (RRHH, Ventas)
   - SW-3: VLANs 50, 60 (Producción, Desarrollo)

6. **SVIs (Switch Virtual Interfaces)**:
   - Los switches de distribución tienen SVIs para todas las VLANs con direcciones IP
   - Los switches de acceso tienen SVIs solo para gestión
   - La configuración de VRRP, BFD y OSPF se añadirá posteriormente en sus respectivas secciones

7. **Trunk pruning**:
   - Se permite solo las VLANs necesarias en cada troncal
   - Esto reduce el tráfico innecesario y mejora la seguridad
