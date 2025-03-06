# Configuración VRRP

## Descripción general

Virtual Router Redundancy Protocol (VRRP) es un protocolo de redundancia que proporciona failover automático para gateways por defecto en una red IP. En esta implementación, VRRP se utiliza para proporcionar redundancia de gateway para los usuarios finales, permitiendo que los switches de distribución compartan direcciones IP virtuales que actúan como gateway predeterminado para cada VLAN.

![Diagrama VRRP](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-veos-vrrp-topologi-1.png)

## Principio de funcionamiento

VRRP crea un router virtual que representa una dirección IP compartida entre varios dispositivos físicos. Un router es elegido como "master" (basado en la prioridad) y maneja el tráfico destinado a esa IP, mientras que los otros permanecen como "backup". Si el router master falla, el backup con la siguiente prioridad más alta toma el control automáticamente.

## Configuración por dispositivo

### DISTRIBUTION-1 (Router primario)
```
interface Vlan10
   vrrp 10 priority-level 110
   no vrrp 10 peer authentication
   vrrp 10 ipv4 172.16.10.1

interface Vlan20
   vrrp 20 priority-level 110
   no vrrp 20 peer authentication
   vrrp 20 ipv4 172.16.20.1

interface Vlan30
   vrrp 30 priority-level 110
   no vrrp 30 peer authentication
   vrrp 30 ipv4 172.16.30.1

interface Vlan40
   vrrp 40 priority-level 110
   no vrrp 40 peer authentication
   vrrp 40 ipv4 172.16.40.1

interface Vlan50
   vrrp 50 priority-level 110
   no vrrp 50 peer authentication
   vrrp 50 ipv4 172.16.50.1

interface Vlan60
   vrrp 60 priority-level 110
   no vrrp 60 peer authentication
   vrrp 60 ipv4 172.16.60.1
```

### DISTRIBUTION-2 (Router secundario)
```
interface Vlan10
   vrrp 10 priority-level 90
   no vrrp 10 peer authentication
   vrrp 10 ipv4 172.16.10.1

interface Vlan20
   vrrp 20 priority-level 90
   no vrrp 20 peer authentication
   vrrp 20 ipv4 172.16.20.1

interface Vlan30
   vrrp 30 priority-level 90
   no vrrp 30 peer authentication
   vrrp 30 ipv4 172.16.30.1

interface Vlan40
   vrrp 40 priority-level 90
   no vrrp 40 peer authentication
   vrrp 40 ipv4 172.16.40.1

interface Vlan50
   vrrp 50 priority-level 90
   no vrrp 50 peer authentication
   vrrp 50 ipv4 172.16.50.1

interface Vlan60
   vrrp 60 priority-level 90
   no vrrp 60 peer authentication
   vrrp 60 ipv4 172.16.60.1
```

## Verificación de la configuración

Para verificar el estado de VRRP, utilice los siguientes comandos:

```
show vrrp
show vrrp brief
show vrrp interface vlanX
```

![Diagrama VRRP](https://github.com/Andherson333333/Networking/blob/main/Veos-arista-3-layer-network-enterprise/imagenes/Arista-show-vrrp-1.png)

## Consideraciones de diseño

1. **Asignación de prioridades**:
   - DISTRIBUTION-1: Prioridad 110 (principal)
   - DISTRIBUTION-2: Prioridad 90 (respaldo)
   
   Esta configuración hace que DISTRIBUTION-1 sea el router activo para todas las VLANs en condiciones normales.

2. **Identificadores VRRP**:
   - Se utiliza el número de VLAN como ID de grupo VRRP para facilitar la identificación y mantenimiento.

3. **Direcciones virtuales (VIP)**:
   - Cada VLAN tiene configurado su VIP con el primer host de la subred (.1)
   - Estas direcciones son las que los dispositivos finales utilizan como gateway por defecto

4. **Autenticación**:
   - La autenticación entre pares está deshabilitada (`no vrrp X peer authentication`).
   - En entornos de producción críticos, podría habilitarse con contraseñas seguras.

5. **Beneficios**:
   - Transparencia para los dispositivos finales
   - Conmutación por error automática
   - No requiere configuración especial en los hosts
   - Tiempo de convergencia rápido
   - Complementa perfectamente la redundancia proporcionada por MLAG
