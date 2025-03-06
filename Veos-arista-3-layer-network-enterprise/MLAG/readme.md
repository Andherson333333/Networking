# Configuración MLAG

## Descripción general

Multi-Chassis Link Aggregation (MLAG) es una tecnología que permite la agregación de enlaces de red a través de múltiples chasis físicos. En esta implementación, MLAG se utiliza para proporcionar redundancia y balanceo de carga entre los switches de distribución y acceso, eliminando las limitaciones del Spanning Tree Protocol.

MLAG permite que los switches de acceso vean a los dos switches de distribución como un único dispositivo lógico, permitiendo así utilizar todos los enlaces simultáneamente sin crear bucles en la red.

![Diagrama MLAG](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-mlag-topologi-1.png)

## Componentes principales del MLAG

- **MLAG Peer Link**: Enlace de alta capacidad entre los switches de distribución
- **VLAN Peer**: VLAN dedicada para comunicación de control entre peers (VLAN 4094)
- **MLAG Domain**: Identificador común que agrupa los dispositivos MLAG (DISTRIBUTION)
- **MLAG ID**: Identificador único por cada port-channel que participa en MLAG

## Configuración por dispositivo

### DISTRIBUTION-1
```
mlag configuration
   domain-id DISTRIBUTION
   local-interface Vlan4094
   peer-address 172.16.253.18
   peer-link Port-Channel100

interface Vlan4094
   description MLAG_PEER
   no autostate
   ip address 172.16.253.17/30

interface Port-Channel100
   description MLAG_PEER_LINK
   switchport trunk native vlan 999
   switchport mode trunk

interface Ethernet1
   description MLAG_PEER_LINK_TO_DIST2
   channel-group 100 mode active

interface Ethernet2
   description MLAG_PEER_LINK_TO_DIST2
   channel-group 100 mode active

interface Port-Channel10
   description MLAG_TO_SW1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk
   mlag 10

interface Port-Channel20
   description MLAG_TO_SW2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk
   mlag 20

interface Port-Channel30
   description MLAG_TO_SW3
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk
   mlag 30
```

### DISTRIBUTION-2
```
mlag configuration
   domain-id DISTRIBUTION
   local-interface Vlan4094
   peer-address 172.16.253.17
   peer-link Port-Channel100

interface Vlan4094
   description MLAG_PEER
   no autostate
   ip address 172.16.253.18/30

interface Port-Channel100
   description MLAG_PEER_LINK
   switchport trunk native vlan 999
   switchport mode trunk

interface Ethernet1
   description MLAG_PEER_LINK_TO_DIST1
   channel-group 100 mode active

interface Ethernet2
   description MLAG_PEER_LINK_TO_DIST1
   channel-group 100 mode active

interface Port-Channel10
   description MLAG_TO_SW1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk
   mlag 10

interface Port-Channel20
   description MLAG_TO_SW2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk
   mlag 20

interface Port-Channel30
   description MLAG_TO_SW3
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk
   mlag 30
```

### SW-1
```
interface Port-Channel10
   description TO_DISTRIBUTION
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk

interface Ethernet3
   description TO_DISTRIBUTION-2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk
   channel-group 10 mode active

interface Ethernet6
   description TO_DISTRIBUTION-1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 10,20
   switchport mode trunk
   channel-group 10 mode active
```

### SW-2
```
interface Port-Channel20
   description TO_DISTRIBUTION
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk

interface Ethernet4
   description TO_DISTRIBUTION-2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk
   channel-group 20 mode active

interface Ethernet7
   description TO_DISTRIBUTION-1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 30,40
   switchport mode trunk
   channel-group 20 mode active
```

### SW-3
```
interface Port-Channel30
   description TO_DISTRIBUTION-2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk

interface Ethernet5
   description TO_DISTRIBUTION-2
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk
   channel-group 30 mode active

interface Ethernet8
   description TO_DISTRIBUTION-1
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,60
   switchport mode trunk
   channel-group 30 mode active
```

## Verificación de la configuración

Para verificar el estado de MLAG, utilice los siguientes comandos:

```
show mlag
show mlag detail
show mlag interfaces
show port-channel summary
```

![Diagrama MLAG](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-mlag-1.png)
![Diagrama MLAG](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-mlag-2.png)

## Consideraciones de diseño

1. **Peer Link**: Se utiliza un port-channel con dos interfaces físicas para el peer-link, proporcionando redundancia y mayor ancho de banda.

2. **VLAN de Peer**: La VLAN 4094 se usa exclusivamente para la comunicación entre peers MLAG. Se configura con `no autostate` para mantenerla activa incluso cuando otras interfaces están inactivas.

3. **VLANs por switch**: Cada switch de acceso maneja diferentes VLANs:
   - SW-1: VLANs 10 y 20 (Administración, Finanzas)
   - SW-2: VLANs 30 y 40 (RRHH, Ventas)
   - SW-3: VLANs 50 y 60 (Producción, Desarrollo)

4. **LACP**: Se utiliza LACP en modo activo (`channel-group X mode active`) para permitir la negociación dinámica de los enlaces.

5. **Beneficios**:
   - Eliminación del bloqueo de enlaces por STP
   - Utilización de todos los enlaces simultáneamente
   - Balanceo de carga a nivel de flujo
   - Resistencia a fallos de enlace o dispositivo completo
   - Mantenimiento sin interrupción del servicio
