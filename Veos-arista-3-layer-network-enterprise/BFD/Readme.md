# Configuración BFD

## Descripción general

Bidirectional Forwarding Detection (BFD) es un protocolo ligero diseñado para detectar fallos de enlace o rutas entre dispositivos de red. A diferencia de los mecanismos de detección integrados en protocolos como OSPF, BFD proporciona detección de fallos más rápida y consistente, permitiendo una convergencia más ágil de la red.

En esta implementación, BFD se utiliza junto con OSPF para proporcionar detección rápida de fallos en todas las conexiones críticas.

![Diagrama BFD](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-bfd-topologi-1.png)

## Principio de funcionamiento

BFD establece sesiones entre pares de dispositivos y envía paquetes de control periódicamente. Si un dispositivo deja de recibir paquetes BFD durante un período definido, se considera que el enlace o el dispositivo remoto ha fallado, permitiendo una reacción inmediata.

## Configuración por dispositivo

### Core Layer

#### CORE-1
```
interface Ethernet2
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet7
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet11
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd
```

#### CORE-2
```
interface Ethernet2
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet4
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet8
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd
```

### Distribution Layer

#### DISTRIBUTION-1
```
interface Ethernet4
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet11
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Vlan10
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan20
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan30
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan40
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan50
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan60
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd
```

#### DISTRIBUTION-2
```
interface Ethernet7
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Ethernet8
   bfd interval 500 min_rx 500 multiplier 5
   ip ospf neighbor bfd

interface Vlan10
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan20
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan30
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan40
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan50
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd

interface Vlan60
   bfd interval 100 min_rx 100 multiplier 5
   no ip ospf neighbor bfd
```

## Verificación de la configuración

Para verificar el estado de BFD, utilice los siguientes comandos:

```
show bfd neighbors
show bfd neighbors detail
show bfd counters
show ip ospf neighbor
```

## Consideraciones de diseño

1. **Intervalos BFD**:
   - Para enlaces físicos: Intervalo de 500ms, con un multiplicador de 5
     - Tiempo de detección total: 2.5 segundos (5 × 500ms)
   - Para interfaces VLAN: Intervalo más corto de 100ms, con un multiplicador de 5
     - Tiempo de detección total: 500ms (5 × 100ms)

2. **Integración con OSPF**:
   - BFD está habilitado para vecinos OSPF en enlaces físicos (`ip ospf neighbor bfd`)
   - En interfaces VLAN, BFD está configurado pero no se utiliza para OSPF (`no ip ospf neighbor bfd`)

3. **Beneficios**:
   - Detección de fallos más rápida que los mecanismos nativos de OSPF
   - Tiempo de convergencia reducido para la red
   - Comportamiento consistente independientemente del protocolo de enrutamiento
   - Menor sobrecarga de CPU en comparación con reducir los temporizadores de Hello de OSPF

4. **Consideraciones operativas**:
   - Intervalos muy cortos pueden aumentar la carga en el CPU
   - Es importante balancear entre tiempo de detección y estabilidad
   - Los multiplicadores más pequeños detectan fallos más rápido pero pueden causar falsos positivos
