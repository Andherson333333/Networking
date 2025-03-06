# Configuración Spanning Tree

## Descripción general

Spanning Tree Protocol (STP) es un protocolo de capa 2 diseñado para prevenir bucles en redes con múltiples caminos. En esta implementación se utiliza Multiple Spanning Tree Protocol (MSTP), que proporciona una instancia independiente de spanning-tree por cada VLAN o grupo de VLANs, optimizando el uso de los enlaces.

Aunque MLAG proporciona la redundancia principal en la red, STP actúa como una capa adicional de protección contra bucles.

![Diagrama Spanning Tree](../imagenes/stp_diagram.png)

## Principio de funcionamiento

MSTP construye un árbol de spanning-tree que define un único camino entre cualquier par de dispositivos, bloqueando enlaces redundantes para evitar bucles. En caso de fallo de un enlace, STP recalcula automáticamente la topología y desbloquea enlaces alternativos.

## Configuración por dispositivo

### Core Layer

#### CORE-1
```
spanning-tree mode mstp
```

#### CORE-2
```
spanning-tree mode mstp
```

### Distribution Layer

#### DISTRIBUTION-1
```
spanning-tree mode mstp
spanning-tree mst 0 priority 8192
```

#### DISTRIBUTION-2
```
spanning-tree mode mstp
spanning-tree mst 0 priority 8192
```

### Access Layer

#### SW-1
```
spanning-tree mode mstp
```

#### SW-2
```
spanning-tree mode mstp
```

#### SW-3
```
spanning-tree mode mstp

interface Ethernet1
   description PC_VLAN50
   switchport access vlan 50
   spanning-tree portfast

interface Ethernet2
   description PC_VLAN60
   switchport access vlan 60
   spanning-tree portfast
```

## Verificación de la configuración

Para verificar el estado de Spanning Tree, utilice los siguientes comandos:

```
show spanning-tree
show spanning-tree mst
show spanning-tree mst detail
show spanning-tree interface
show spanning-tree blockedports
```
Core-1 y 2
![Diagrama Spanning Tree](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-stp-3.png)

Distribution-1 y 2
![Diagrama Spanning Tree](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-stp-2.png)

Sw-1 y Sw-2
![Diagrama Spanning Tree](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-stp-3.png)

## Consideraciones de diseño

1. **Modo MSTP**:
   - MSTP se utiliza en todos los dispositivos para mayor flexibilidad y eficiencia
   - Permite mapear VLANs a instancias específicas de spanning-tree
   - Mejor rendimiento que PVST+ con múltiples VLANs

2. **Prioridad de Bridge**:
   - Ambos switches de distribución tienen la misma prioridad (8192)
   - Esto los hace raíces preferidas del spanning-tree
   - Al tener la misma prioridad, la decisión final se basa en la MAC más baja

3. **Optimizaciones**:
   - Portfast está habilitado en interfaces de acceso (SW-3) que conectan a dispositivos finales
   - Esto permite que los dispositivos accedan inmediatamente a la red sin esperar la convergencia de STP

4. **Interacción con MLAG**:
   - MLAG ya proporciona redundancia sin bucles para los enlaces agregados
   - STP actúa como protección secundaria y para enlace no-MLAG
   - La VLAN de peer MLAG (4094) no participa en STP (`no autostate`)

5. **Beneficios**:
   - Protección contra bucles físicos accidentales
   - Convergencia automática ante fallos
   - Complementa las protecciones de MLAG
   - PortFast mejora el tiempo de acceso para dispositivos finales
