# Configuración OSPF

Este documento contiene la configuración OSPF para todos los dispositivos de la red.

## Descripción general

Open Shortest Path First (OSPF) es un protocolo de enrutamiento de estado de enlace diseñado para redes IP que permite a cada router crear un mapa de la topología de red. En esta implementación, OSPF se utiliza para proporcionar enrutamiento dinámico entre todas las capas de la red, con una estructura de múltiples áreas para optimizar el rendimiento.

## Diseño de áreas OSPF

- **Área 0**: Core y enlaces de backbone
- **Área 0.0.0.1**: VLANs 10 y 20 (Administración y Finanzas)
- **Área 0.0.0.2**: VLANs 30 y 40 (RRHH y Ventas)
- **Área 0.0.0.3**: VLANs 50 y 60 (Producción y Desarrollo)

![Diagrama de áreas OSPF](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-ospf-topologi-1.png)

## Router IDs

- **CORE-1**: 1.1.1.1
- **CORE-2**: 2.2.2.2
- **DISTRIBUTION-1**: 3.3.3.3
- **DISTRIBUTION-2**: 4.4.4.4

## Configuración por dispositivo

### CORE-1
```
router ospf 1
   router-id 1.1.1.1
   max-lsa 12000
   default-information originate

interface Ethernet2
   ip ospf area 0.0.0.0

interface Ethernet7
   ip ospf area 0.0.0.0

interface Ethernet11
   ip ospf area 0.0.0.0
```

### CORE-2
```
router ospf 1
   router-id 2.2.2.2
   max-lsa 12000
   default-information originate

interface Ethernet2
   ip ospf area 0.0.0.0

interface Ethernet4
   ip ospf area 0.0.0.0

interface Ethernet8
   ip ospf area 0.0.0.0
```

### DISTRIBUTION-1
```
router ospf 1
   router-id 3.3.3.3
   max-lsa 12000

interface Ethernet4
   ip ospf area 0.0.0.0

interface Ethernet11
   ip ospf area 0.0.0.0

interface Vlan10
   ip ospf area 0.0.0.1

interface Vlan20
   ip ospf area 0.0.0.1

interface Vlan30
   ip ospf area 0.0.0.2

interface Vlan40
   ip ospf area 0.0.0.2

interface Vlan50
   ip ospf area 0.0.0.3

interface Vlan60
   ip ospf area 0.0.0.3
```

### DISTRIBUTION-2
```
router ospf 1
   router-id 4.4.4.4
   max-lsa 12000

interface Ethernet7
   ip ospf area 0.0.0.0

interface Ethernet8
   ip ospf area 0.0.0.0

interface Vlan10
   ip ospf area 0.0.0.1

interface Vlan20
   ip ospf area 0.0.0.1

interface Vlan30
   ip ospf area 0.0.0.2

interface Vlan40
   ip ospf area 0.0.0.2

interface Vlan50
   ip ospf area 0.0.0.3

interface Vlan60
   ip ospf area 0.0.0.3
```

## Verificación de la configuración

Para verificar el estado de OSPF, utilice los siguientes comandos:

```
show ip ospf
show ip ospf neighbor
show ip ospf interface brief
show ip ospf database
show ip route ospf
```

## Consideraciones de diseño

1. **Estructura multi-área**: Se implementa OSPF con múltiples áreas para:
   - Reducir el tamaño de las tablas de enrutamiento
   - Contener los cambios de topología a sus respectivas áreas
   - Reducir el procesamiento de CPU durante recálculos de rutas
   - Mejorar la escalabilidad

2. **Distribución de ruta por defecto**: Los routers de core (CORE-1 y CORE-2) utilizan `default-information originate` para inyectar sus rutas por defecto en el dominio OSPF, permitiendo que todos los dispositivos accedan a Internet.

3. **Límite de LSAs**: Se configura `max-lsa 12000` como medida de protección contra tormentas de enrutamiento.

4. **Router ID estático**: Se asignan IDs estáticos para facilitar la identificación y el troubleshooting.

5. **Integración con BFD**: OSPF utiliza BFD para detección rápida de fallos en enlaces.
