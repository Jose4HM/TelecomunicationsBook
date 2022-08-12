# Script de comandos de packet tracer

## Equipo `Switch`

### Entrar en modo EXEC privilegiado

Ejecutar el comando `enable`

```
Switch> enable
Swtich#
```

### Examinar la configuración actual del switch

Ejecutar el comando `show running-config`, un comando de modo habilitación para mostrar la configuración en ejecución para una interfaz específica
```
Switch> show running-config
```

    Switch> show running-config interface #interface slot/number

### Asignar un nombre al switch

Ejecutar los comandos `configure terminal` , una vez dentro del módulo de configuración ingresar el comando `hostname` seguido del nombre asignado al switch, una vez asignado al nombre ejecutar `exit` para salir del módulo de configuración del terminal.

```
Switch# configure terminal
Switch(config)# hostname S1
S1(config)# exit
S1>
```

### Proporcionar acceso seguro a la consola

Ejecutar el comando `configure terminal` seguido de `line console 0`, una vez dentro de la configuración de consola escribir `password` seguido de la contraseña a utilizar, ejecutar `login` una vez presionado enter y salir de las configuración tanto de la consola como del terminal con `exit`

```
S1# configure terminal
S1(config)# line console 0
S1(config-line)# password letmein
S1(config-line)# login
S1(config-line)# exit
S1(config)# exit
```

### Proporcionar acceso seguro al modo privilegiado

Establecer una contraseña al modo privilegiado, entrar a la configuración del terminal ingresando `configure terminal`, ejecutar `enable password` seguido de la contraseña a utilizar, salir de la configuración con `exit`

```
S1> enable
S1# configure terminal
S1(config)# enable password c1$c0
S1(config)# exit
S1#
```

### Configurar una contraseña cifrada para proporcionar acceso seguro al modo privilegiado

Ingresar `configure terminal` habilitar el comando `secret` junto con la contraseña, la misma reemplazaria a la creada anteriormente

```
S1> enable
S1# configure terminal
S1(config)# enable secret itsasecret
S1(config)# exit
S1#
```

### Cifrar las contraseña de consola y enable

```
S1> enable
S1# configure terminal
S1(config)# service password-encryption
S1(config)# exit
S1#
```

### Configurar mensaje del día `MOTD`

```
S1> enable
S1# configure terminal
S1(config)# banner motd "Message of the day"
S1(config)# exit
S1#
```
Alternativamente
```
S1>enable
S1# configure terminal
S1(config)# banner motd #
Enter TEXT message.  End with the character '#'.
Here you write your message. #
S1(config)# exit
```


### Guardar los cambios de la configuración en la NVRAM

```
S1> enable
S1# copy running-configuration starup-configuration
```

### Deshabilitar la busqueda de nombres de dominio

```
S1> enable
S1# configure terminal
S1(config)# no ip domain-lookup
S1(config)# exit
S1#
```

## Equipo `Router`

### Asignar direcciones IP a las distintas interfaces

```
R1> enable
R1# configure terminal
R1(config)# interface GigabitEthernet0/0 #o la interface a configurar
R1(config-if)#ip address 172.31.1.1 255.255.255.240
R1(config-if)#no shutdown
```

Interface serial

```
R2(config)#interface Serial0/0/0
R2(config-if)#ip address 172.31.1.78 255.255.255.240
R2(config-if)#no shutdown
```

Configurar IPv6 en un router

```
R1(config)# ipv6 unicast-routing
R1(config)#interface gigabitEthernet 0/0
R1(config-if)#ipv6 address 2001:DB8:ACAD:A::1/64
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#no shutdown
R1(config-if)#exit
```

```
R2(config)# ipv6 unicast-routing
R2(config)#interface gigabitEthernet 0/0
R2(config-if)#ipv6 address 2001:DB8:ACAD:B::1/64
R2(config-if)#ipv6 address FE80::2 link-local
R2(config-if)#no shutdown
R2(config-if)#exit
```

### Habilitar VTY Line para Acceso vía Telnet

```
Router(config)# line vty 0 4
Router(config-line)# login
Router(config-line)# password p4ssw0rd
Router(config-line)# exec-timeout <cantidad-minutos> <cantidad-segundos>
Router(config-line)# exit
```

```
Swtich(config)# line vty 0 15
Swtich(config-line)# login
Swtich(config-line)# password p4ssw0rd
Swtich(config-line)# exec-timeout <cantidad-minutos> <cantidad-segundos>
Swtich(config-line)# exit
```

Interface vlan en un switch

```
S3# configure terminal
S3(config)# interface Vlan1
S3(config-if)#ip address 172.31.1.34 255.255.255.240
S3(config-if)#no shutdown
S3(config)#ip default-gateway 172.31.1.33
```

## Enrutamiento

### Enrutamiento Estático

#### Enrutamiento con IPv4

Sintaxis del comando ip route

```
Router(config)# ip route <dirección de red> <máscara de subred> { <dirección ip> | <interfaz de salida> }
```

`dirección de red` Dirección de la red de destino de la red remota que será agregada de la tabla de enrutamiento.

`máscarade subred` Máscara de subred de la red remota que será agregada a la tabla de enrutamiento, puede modificarse para resumir un grupo de redes.

`dirección ip` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

#### Configuración de Ruta estatica del siguiente salto

```
R1(config)# ip route 172.16.2.0 255.255.255.0 172.16.2.2
```

#### Configuración de Ruta estatica del conectadas directamente

Desde el router uno salen los paquetes a estas tres redes a tráves de la interfaz `Serial 0/0/0`.

```
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/0/0
R1(config)# ip route 192.168.1.0 255.255.255.0 s0/0/0
R1(config)# ip route 172.168.2.0 255.255.255.0 s0/0/0
```

#### Ruta estática predeterminada

Sintaxis del opción de ruta estática predeterminada

```
Router(config)# ip route 0.0.0.0 0.0.0.0 { <dirección ip> | <interfaz de salida> }
```

`dirección de red -> 0.0.0.0` Coincide con cualquier direcciónd de red.

`máscarade subred -> 0.0.0.0` Coincide con cualquier mascara de red.

`dirección ip` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

#### Enrutamiento con IPv6

Sintaxis

```
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 route <dirección de red>/<prefijo> { <dirección ip> | <interfaz de salida> }
```

Tipos de ruta: (aplica para ipv4 e ipv6)

- Siguiente salto: Se especifica la dirección ipv6 del siguiente salto.
- Estática conectada directamente: Se especifica la interfaz de salida del router.
- Completamente especificada: Se especifica la dirección del siguiente salto y la interfaz de salida

#### Configuración de ruta estatica del siguiente salto

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 2001:DB8:ACAD:4::2
R1(config)# ipv6 route 2001:DB8:ACAD:5::/64 2001:DB8:ACAD:4::2
R1(config)# ipv6 route 2001:DB8:ACAD:3::/64 2001:DB8:ACAD:4::2
```

#### Configuración de ruta estatica conectada directamente

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0
R1(config)# ipv6 route 2001:DB8:ACAD:5::/64 s0/0/0
R1(config)# ipv6 route 2001:DB8:ACAD:3::/64 s0/0/0
```

### Configuración de ruta estatica completamente especificada

Router 1

- red 2001:DB8:ACAD:1::/64
- dirección ip link-local FE80::1 s0/0/0
- dirección ip 2001:DB8:ACAD:4::1/64 s0/0/0

Router 2

- red 2001:DB8:ACAD:2::/64
- dirección ip link-local FE80::2 s0/0/0
- dirección ip 2001:DB8:ACAD:4::2/64 s0/0/0

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0 FE80::2
```

ó

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0 2001:DB8:ACAD:4::2
```

### Configuración ruta predeterminada

Sintaxis

```
Router(config)# ipv6 route ::/0 { <dirección ip> | <interfaz de salida> }
```

```
R1(config)# ipv6 route ::/0 s0/0/0 2001:DB8:ACAD:4::2
```

`::/0` Coincide con cualquier prefijo IPv6 independiente de la lonjituds de prefijo (/x).

`dirección ipv6` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

### Enrutamiento Dinámico

### Enrutamiento con IPv4

#### Configuración de enrutameinto dinámico RIPv2

```
R1# enable
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 192.168.1.0 (Configure RIP para las redes que se conectan al R1)
R1(config-router)# network 192.168.2.0 (Configure RIP para las redes que se conectan al R1)
R1(config-router)# passive-interface g0/0 (Evita el envío de tablas de enrutamiento)
```

#### Propagación de rutas predeterminadas

```
R1(config)# ip route 0.0.0.0 0.0.0.0 s0/0/1 200.165.200.226
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# default-information originate
```

### Enrutamiento con IPv6

REDESCISCO es un indentificador único para el enrutamiento, este es selecciónado por el administrador.

```
R1(config)#int g0/0
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#int g0/1
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#int g0/0
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#end
```

## Configuración SSH

### Pasos para configurar ssh en routers/swtiches

1.- Dominio ejemplo: `duoc.cl`
2.- Establecer longitud mínima de 7 caracteres para las contraseñas
3.- Clave MD5 `enable secret Cisco123`
4.- Crear usuario, el cual será el `duoc.cl`, con clave `ciscocisco`.
5.- Utilizar llave RSA de 2048 bits, donde el acceso remoto debe ser en la terminal virtual para (3 sesiones).
6.- Mejorar el SSH, utilizando la versión 2, establecer un máximo de 3 reintentos, y un tiempo de espera de 30 segundos.

```
R1(config)#ip domain-name www.duoc.cl
R1(config)#service password-encryption
R1(config)#security passwords min-length 7
R1(config)#enable secret Cisco123
```

```
R1(config)# username duoc.cl secret ciscocisco
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh time-out 30
R1(config)# ip ssh authentication-retries 3
R1(config)# ip ssh version 2
```

```
R1(config)# line vty 0 2
R1(config)# transport input ssh
R1(config)# login local
R1(config)# exit
```

### DEFAULT GATEWAY SWITCH

```
S1(config)# interface vlan1
S1(config-vlan)# ip address 192.168.10.50 255.255.255.0.
S1(config-vlan)# no shut.
S1(config)# ip default-gateway 192.168.10.1.
```

### RESPALDO TFTP

```
R1#copy tftp: running-config

Address or name of remote host []? ("IP del Server TFTP")
Source filename []?RESPALDO-CENTRAL
Destination filename [running-config]?
Accessing tftp://10.66.64.10/backup_cfg_for_my_router...
Loading backup_cfg_for_router from 64.104.207.171 (via FastEthernet0/0): !
```

### Configuración Vlan Switchs

```
vlan ID # Numero
name Name # Texto
exit
```

#### Asociación de Interface con Vlan

```
interface fastethernet 0/3
switchport access vlan 30
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation restrict
switchport port-security maximum 1
switchport mode access
exit
```

Configuración de modo troncal para interfaces que estan conectadas entre swtches y routers

```
interface fastethernet 0/1
switchport mode trunk
exit
```

Encapsulación de vlan en una sub interfaz en el router

```
interface fastethernet 0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
exit
```

### Autenticación OSPF

```
router ospf 1
area 0 authentication message-digest
```

#### En la interfaz

caso: Configurar llave MD5 en la interfaz usando el password MD5pa55 con llave 1

```
R1(config)# interface s0/0/0
R1(config-if)# ip ospf message-digest-key 1 md5 MD5pa55
```

### Configuración enrutamiento con OSPF

```
router ospf 10
router-id 2.2.2.2
default-information originate # Comando para propagación de las rutas estaticas
passive-interface fastethernet 0/0
passive-interface fastethernet 0/1
passive-interface fastethernet 0/0.50
network 172.16.50.0 0.0.0.255 area 0
network 172.16.60.0 0.0.0.3 area 0
exit
```

### Configuracion DHCP IPv4

Exclusion de rango de ip dentro del router que hara de servidor DHCP

```
ip dhcp excluded-address 172.20.10.1 172.20.10.20
ip dhcp excluded-address 192.168.20.1 192.168.20.20
```

Configuracion de una red dentro del router

```
ip dhcp pool RED_A
network 172.20.10.0 255.255.255.192
default-router 172.20.10.62 # Puerta de enlace

ip dhcp pool RED_B
network 192.168.20.0 255.255.255.128
default-router 192.168.20.126
```

Configuracion de interfaz

```
interface FastEthernet0/0 # interfaz hacia hosts
ip address 172.20.10.62 255.255.255.192

interface FastEthernet0/1 # interfaz entre routers
ip address 172.16.30.21 255.255.255.252
```

Configuracion en segundo router

```
interface FastEthernet0/0
ip address 192.168.20.126 255.255.255.128
ip helper-address 172.16.30.21 # interfaz del router dhcp
```

### Redistribucion de protocolos de enrutamiento

#### IPV6

rip

```
ipv6 router rip RED_ID_NAME
redistribute ospf <num proceso> metric 2
end
```

OSPF v3

```
ipv6 router ospf <num proceso>
redistribute rip RED_ID_NAME subnets
redistribute static subnets # redes estaticas
default-information originate # estaticas por defecto
```

#### IPV4

OSPF

```
router ospf 3
redistribute rip subnets metric 10
exit
```

rip

```
router rip
redistribute ospf <num proceso> match internal metric 10
exit
```

# Configuración de EtherChannel

### Configuracion de interfaces

Apagar las interfaces involucradas y en cada grupo configurarlas

```
Switch(config)# interface range f0/1-2 (fastEthernet 1 y 2)
Switch(config-if-range)# shutdown
Switch(config-if-range)# switchport mode trunk (si no esta habilitada)
Switch(config-if-range)# switchport nonegotiate
Switch(config-if-range)# channel-protocol pagp
Switch(config-if-range)# channel-group :id mode desirable (id es un numero arbitrario designado por el administrador)
Switch(config-if-range)# no shutdown
Switch(config-if-range)# exit
```

### Revision de interfaces

```
Switch# show ethernetchannel
```

## Configuracion de vtp

### Configurar vlan server

```
Switch(config)# vtp mode server
Switch(config)# vtp domain <nombre de dominio>
Switch(config)# vtp password <password>
Switch(config)# vtp version <1 o 2>
Switch(config)# vtp prunning
```

### Configurar vlan cliente

```
Switch(config)# vtp mode client
Switch(config)# vtp version <la misma del cliente>
```

#### Configurar router como DHCP

```
ip dhcp excluded-address 192.168.20.1 192.168.20.10
network 192.168.20.0 255.255.255.0
ip dhcp pool wireless2
default-router 192.168.20.1
```

### Eliminar pares de llaves RSA

```
R1(config)# cryto key zeroize rsa
```

### Asignar direcciones IP a las distintas interfaces

```
R1> enable
R1# configure terminal
R1(config)# interface GigabitEthernet0/0 #o la interface a configurar
R1(config-if)#ip address 172.31.1.1 255.255.255.240
R1(config-if)#no shutdown
```

Interface serial

```
R2(config)#interface Serial0/0/0
R2(config-if)#ip address 172.31.1.78 255.255.255.240
R2(config-if)#no shutdown
```

Configurar IPv6 en un router

```
R1(config)# ipv6 unicast-routing
R1(config)#interface gigabitEthernet 0/0
R1(config-if)#ipv6 address 2001:DB8:ACAD:A::1/64
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#no shutdown
R1(config-if)#exit
```

```
R2(config)# ipv6 unicast-routing
R2(config)#interface gigabitEthernet 0/0
R2(config-if)#ipv6 address 2001:DB8:ACAD:B::1/64
R2(config-if)#ipv6 address FE80::2 link-local
R2(config-if)#no shutdown
R2(config-if)#exit
```

### Habilitar VTY Line para Acceso vía Telnet

```
Router(config)# line vty 0 4
Router(config-line)# login
Router(config-line)# password p4ssw0rd
Router(config-line)# exec-timeout <cantidad-minutos> <cantidad-segundos>
Router(config-line)# exit
```

```
Swtich(config)# line vty 0 15
Swtich(config-line)# login
Swtich(config-line)# password p4ssw0rd
Swtich(config-line)# exec-timeout <cantidad-minutos> <cantidad-segundos>
Swtich(config-line)# exit
```

Interface vlan en un switch

```
S3# configure terminal
S3(config)# interface Vlan1
S3(config-if)#ip address 172.31.1.34 255.255.255.240
S3(config-if)#no shutdown
S3(config)#ip default-gateway 172.31.1.33
```

## Enrutamiento

### Enrutamiento Estático

#### Enrutamiento con IPv4

Sintaxis del comando ip route

```
Router(config)# ip route <dirección de red> <máscara de subred> { <dirección ip> | <interfaz de salida> }
```

`dirección de red` Dirección de la red de destino de la red remota que será agregada de la tabla de enrutamiento.

`máscarade subred` Máscara de subred de la red remota que será agregada a la tabla de enrutamiento, puede modificarse para resumir un grupo de redes.

`dirección ip` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

#### Configuración de Ruta estatica del siguiente salto

```
R1(config)# ip route 172.16.2.0 255.255.255.0 172.16.2.2
```

#### Configuración de Ruta estatica del conectadas directamente

Desde el router uno salen los paquetes a estas tres redes a tráves de la interfaz `Serial 0/0/0`.

```
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/0/0
R1(config)# ip route 192.168.1.0 255.255.255.0 s0/0/0
R1(config)# ip route 172.168.2.0 255.255.255.0 s0/0/0
```

#### Ruta estática predeterminada

Sintaxis del opción de ruta estática predeterminada

```
Router(config)# ip route 0.0.0.0 0.0.0.0 { <dirección ip> | <interfaz de salida> }
```

`dirección de red -> 0.0.0.0` Coincide con cualquier direcciónd de red.

`máscarade subred -> 0.0.0.0` Coincide con cualquier mascara de red.

`dirección ip` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

#### Enrutamiento con IPv6

Sintaxis

```
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 route <dirección de red>/<prefijo> { <dirección ip> | <interfaz de salida> }
```

Tipos de ruta: (aplica para ipv4 e ipv6)

- Siguiente salto: Se especifica la dirección ipv6 del siguiente salto.
- Estática conectada directamente: Se especifica la interfaz de salida del router.
- Completamente especificada: Se especifica la dirección del siguiente salto y la interfaz de salida

#### Configuración de ruta estatica del siguiente salto

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 2001:DB8:ACAD:4::2
R1(config)# ipv6 route 2001:DB8:ACAD:5::/64 2001:DB8:ACAD:4::2
R1(config)# ipv6 route 2001:DB8:ACAD:3::/64 2001:DB8:ACAD:4::2
```

#### Configuración de ruta estatica conectada directamente

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0
R1(config)# ipv6 route 2001:DB8:ACAD:5::/64 s0/0/0
R1(config)# ipv6 route 2001:DB8:ACAD:3::/64 s0/0/0
```

### Configuración de ruta estatica completamente especificada

Router 1

- red 2001:DB8:ACAD:1::/64
- dirección ip link-local FE80::1 s0/0/0
- dirección ip 2001:DB8:ACAD:4::1/64 s0/0/0

Router 2

- red 2001:DB8:ACAD:2::/64
- dirección ip link-local FE80::2 s0/0/0
- dirección ip 2001:DB8:ACAD:4::2/64 s0/0/0

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0 FE80::2
```

ó

```
R1(config)# ipv6 route 2001:DB8:ACAD:2::/64 s0/0/0 2001:DB8:ACAD:4::2
```

### Configuración ruta predeterminada

Sintaxis

```
Router(config)# ipv6 route ::/0 { <dirección ip> | <interfaz de salida> }
```

```
R1(config)# ipv6 route ::/0 s0/0/0 2001:DB8:ACAD:4::2
```

`::/0` Coincide con cualquier prefijo IPv6 independiente de la lonjituds de prefijo (/x).

`dirección ipv6` Dirección IP del router del siguiente salto.

`interfaz de salida` Interfaz de salida de paquetes a la red, conocida tambien como **ruta estática conectada directamente**, suele usarse para conectarse a una configuración punto a punto.

### Enrutamiento Dinámico

### Enrutamiento con IPv4

#### Configuración de enrutameinto dinámico RIPv2

```
R1# enable
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 192.168.1.0 (Configure RIP para las redes que se conectan al R1)
R1(config-router)# network 192.168.2.0 (Configure RIP para las redes que se conectan al R1)
R1(config-router)# passive-interface g0/0 (Evita el envío de tablas de enrutamiento)
```

#### Propagación de rutas predeterminadas

```
R1(config)# ip route 0.0.0.0 0.0.0.0 s0/0/1 200.165.200.226
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# default-information originate
```

### Enrutamiento con IPv6

REDESCISCO es un indentificador único para el enrutamiento, este es selecciónado por el administrador.

```
R1(config)#int g0/0
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#int g0/1
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#int g0/0
R1(config-if)#ipv6 rip REDESCISCO enable
R1(config-if)#end
```

## Configuración SSH

### Pasos para configurar ssh en routers/swtiches

1.- Dominio ejemplo: `duoc.cl`
2.- Establecer longitud mínima de 7 caracteres para las contraseñas
3.- Clave MD5 `enable secret Cisco123`
4.- Crear usuario, el cual será el `duoc.cl`, con clave `ciscocisco`.
5.- Utilizar llave RSA de 2048 bits, donde el acceso remoto debe ser en la terminal virtual para (3 sesiones).
6.- Mejorar el SSH, utilizando la versión 2, establecer un máximo de 3 reintentos, y un tiempo de espera de 30 segundos.

```
R1(config)#ip domain-name www.duoc.cl
R1(config)#service password-encryption
R1(config)#security passwords min-length 7
R1(config)#enable secret Cisco123
```

```
R1(config)# username duoc.cl secret ciscocisco
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh time-out 30
R1(config)# ip ssh authentication-retries 3
R1(config)# ip ssh version 2
```

```
R1(config)# line vty 0 2
R1(config)# transport input ssh
R1(config)# login local
R1(config)# exit
```

### DEFAULT GATEWAY SWITCH

```
S1(config)# interface vlan1
S1(config-vlan)# ip address 192.168.10.50 255.255.255.0.
S1(config-vlan)# no shut.
S1(config)# ip default-gateway 192.168.10.1.
```

### RESPALDO TFTP

```
R1#copy tftp: running-config

Address or name of remote host []? ("IP del Server TFTP")
Source filename []?RESPALDO-CENTRAL
Destination filename [running-config]?
Accessing tftp://10.66.64.10/backup_cfg_for_my_router...
Loading backup_cfg_for_router from 64.104.207.171 (via FastEthernet0/0): !
```

### Configuración Vlan Switchs

```
vlan ID # Numero
name Name # Texto
exit
```

#### Asociación de Interface con Vlan

```
interface fastethernet 0/3
switchport access vlan 30
switchport port-security
switchport port-security mac-address sticky
switchport port-security violation restrict
switchport port-security maximum 1
switchport mode access
exit
```

Configuración de modo troncal para interfaces que estan conectadas entre swtches y routers

```
interface fastethernet 0/1
switchport mode trunk
exit
```

Encapsulación de vlan en una sub interfaz en el router

```
interface fastethernet 0/0.10
encapsulation dot1q 10
ip address 172.16.10.1 255.255.255.0
exit
```

### Configuración enrutamiento con OSPF

```
router ospf 10
router-id 2.2.2.2
default-information originate # Comando para propagación de las rutas estaticas
passive-interface fastethernet 0/0
passive-interface fastethernet 0/1
passive-interface fastethernet 0/0.50
network 172.16.50.0 0.0.0.255 area 0
network 172.16.60.0 0.0.0.3 area 0
exit
```

### Configuracion DHCP IPv4

Exclusion de rango de ip dentro del router que hara de servidor DHCP

```
ip dhcp excluded-address 172.20.10.1 172.20.10.20
ip dhcp excluded-address 192.168.20.1 192.168.20.20
```

Configuracion de una red dentro del router

```
ip dhcp pool RED_A
network 172.20.10.0 255.255.255.192
default-router 172.20.10.62 # Puerta de enlace

ip dhcp pool RED_B
network 192.168.20.0 255.255.255.128
default-router 192.168.20.126
```

Configuracion de interfaz

```
interface FastEthernet0/0 # interfaz hacia hosts
ip address 172.20.10.62 255.255.255.192

interface FastEthernet0/1 # interfaz entre routers
ip address 172.16.30.21 255.255.255.252
```

Configuracion en segundo router

```
interface FastEthernet0/0
ip address 192.168.20.126 255.255.255.128
ip helper-address 172.16.30.21 # interfaz del router dhcp
```

### Redistribucion de protocolos de enrutamiento

#### IPV6

rip

```
ipv6 router rip RED_ID_NAME
redistribute ospf <num proceso> metric 2
end
```

OSPF v3

```
ipv6 router ospf <num proceso>
redistribute rip RED_ID_NAME subnets
redistribute static subnets # redes estaticas
default-information originate # estaticas por defecto
```

#### IPV4

OSPF

```
router ospf 3
redistribute rip subnets metric 10
exit
```

rip

```
router rip
redistribute ospf <num proceso> match internal metric 10
exit
```

# Configuración de EtherChannel

### Configuracion de interfaces

Apagar las interfaces involucradas y en cada grupo configurarlas

```
Switch(config)# interface range f0/1-2 (fastEthernet 1 y 2)
Switch(config-if-range)# shutdown
Switch(config-if-range)# switchport mode trunk (si no esta habilitada)
Switch(config-if-range)# switchport nonegotiate
Switch(config-if-range)# channel-protocol pagp
Switch(config-if-range)# channel-group :id mode desirable (id es un numero arbitrario designado por el administrador)
Switch(config-if-range)# no shutdown
Switch(config-if-range)# exit
```

### Revision de interfaces

```
Switch# show ethernetchannel
```

## Configuracion de vtp

### Configurar vlan server

```
Switch(config)# vtp mode server
Switch(config)# vtp domain <nombre de dominio>
Switch(config)# vtp password <password>
Switch(config)# vtp version <1 o 2>
Switch(config)# vtp prunning
```

### Configurar vlan cliente

```
Switch(config)# vtp mode client
Switch(config)# vtp version <la misma del cliente>
```

#### Configurar router como DHCP

```
ip dhcp excluded-address 192.168.20.1 192.168.20.10
network 192.168.20.0 255.255.255.0
ip dhcp pool wireless2
default-router 192.168.20.1
```

### Configuración de Listas de acceso (ACL)

**ACL estándar**, donde solo tenemos que especificar una dirección de origen. (0-99)
**ACL extendida**, en cuya sintaxis aparece el protocolo y una dirección de origen y de destino. (100-199)
**ACL con nombre**, permite dar nombres en vez de números a las ACL estándar o extendidas.

ACL Estándar

Formato:

```
R1(config)# access-listn {permit|deny} source {source-mask}
```

Ejemplo:

```
R1(config)# access-list 1 deny 10.5.3.0 0.0.0.255
R1(config)# access-list 1 permit host 10.5.3.37
R1(config)# access-list 1 permit any
```

ACL Extendida

Formarto:

```
R1(config)# access-listn {permit|deny} protocol source {source-mask} destination {destination- mask} [eq destination-port]
```

Ejemplo:

```
R1(config)#access-list 105 permit 10.5.4.0 0.0.0.255
R1(config)#access-list 105 permit host 10.5.3.37 10.5.64.0 0.0.63.255
R1(config)#access-list 105 deny 10.5.3.0 0.0.0.255 any
```

Aplicar la lista a una interface

```
R1(config)#interface serial 0/0
R1(config-if)#ip access-group 100 out
```

Sintaxis

- Protocolo: ip | tcp | udp | icmp
- Comparación: gt | lt | eq
- gt = greater than, lt = lesser than, eq = equal
- Origen de una sola ip: host
- Origen de cualquier ip: any
- Máscara wildcard: el inverso de la máscara.

¿Dónde se aplican las ACL?

- Las listas de acceso estándar se deben colocar cerca del destino.
- Las listas de acceso extendidas se deben colocar cerca de la fuente.

Guía [link](http://atc2.aut.uah.es/~rosa/LabRC/Prac_5/Listas%20de%20Control%20de%20acceso.pdf)

### Configuración de Servicios

#### COndifuración de Servicio NTP

```
R1#show clock
R1(config)#ntp server 1.0.0.200
R1(config)#exit
```

Caso: Usando la llave 1 con password NTPpa55

```
R1(config)# ntp authenticate
R1(config)# ntp trusted-key 1
R1(config)# ntp authentication-key 1 md5 NTPpa55
```

### Configuración de AAA

- Configurar un usuario nivel 15 (valor por defecto)

```
R1(config)# username admin password cisco
```

- Habilitar aaa

```
R1(config)# aaa new-model
```

- Habilitar aaa autenticación

* aaa authentication login -> puede ser nombrar (WORD) o por defecto (default)

```
R1(config)# aaa authentication login default ? -> group (server group) -> local (username auth)
R1(config)# aaa authentication login default group tacacs+ group radius
```

- Habilitar aaa autorización

```
R1(config)# aaa authorization exec default group tacacs+ local
```

- Habilitar aaa accounting

```
R1(config)# aaa accounting exec default ? -> (start-stop) (stop)
```

- Habilitar el servicio el el servidor

* Hostname del cliente (router)
* Dirección IP del cliente
* Agregar clave
* Seleccionar el tipo de servidor
* Agregar usuarios

- Agregar el servidor Radius/Tacacs al router

```
R1(config)# radius-server host <ip-server> key <clave>
R1(config)# tacacs-server host <ip-server> key <clave>
```

- Agregar autenticacion al conexión remota

```
R1(config)# line vty 0 2
R1(config)# login authentication
R1(config)# exit
```

### Configuración de logs

```
R1(config)# logging host <ip-syslog server>
R1(config)#logging trap debugging
R1(config)# logging console
```
## Configuraciones básicas
|Comandos| Proposito |
|--|--|
| **enable** | Lo inicia en el modo de habilitación, que también se conoce como modo de ejecución de usuario o modo privilegiado |
| **configure termina** |Te registra en el modo de configuración|
| **interface** *fastetherner/number*|Entra en el modo de configuración de interfaz para la interfaz Fast Ethernet especificada|
|**reload**|Un comando de modo exec que reinicia un conmutador o enrutador Cisco|
|**hostname**|Establece un nombre de host para el dispositivo de red Cisco actual|
|**copy** *from-location to-location*|Un comando de modo habilitado que copia archivos de una ubicación de archivo a otra|
|**copy running-config startup-config**|Un comando de modo habilitado que guarda la configuración activa, reemplazando la configuración de inicio cuando se inicializa un dispositivo de red de Cisco|
|**copy startup-config running-config** |Un comando de modo habilitado que fusiona la configuración de inicio con la configuración actualmente activa en la RAM|
|**write erase** <br/> **erase startup-config**|Un comando de modo habilitado que elimina la configuración de inicio|
|**ip address** *ip-address mask*|Asigna una dirección IP y una máscara de subred|
|**shutdown** <br/> **no shutdown**|Se utiliza en el modo de configuración de la interfaz. "Apagar" apaga la interfaz, mientras que "no apagar" abre la interfaz.|
|**ip default-gateway** _ip_address_|Establece la puerta de enlace predeterminada en un dispositivo Cisco|
|**show running-config**|Un comando de modo habilitado que muestra la configuración actual|
|**description** _name-string_|Un comando de interfaz de configuración para describir o nombrar una interfaz|
|**show running-config interface** _interface slot/number_|Un comando de modo habilitado para mostrar la configuración en ejecución para una interfaz específica|
|**show ip interface** _[type number]_|Muestra el estado de usabilidad de las interfaces que están configuradas para IP|
|**ip name-server** _serverip-1 serverip-2_|Un comando de modo de configuración que establece las direcciones IP de los servidores DNS|
|**show version**| Muestra la versión de IOS junto con otra información útil|
|**show ip interface brief**| Revisar el estado de las interfaces conectadas|
## Comandos de solución de problemas
|Comando|Proposito|
|--|--|
| **ping** _{hostname/system-address} [source source-address]_ | Se utiliza en el modo de activación para diagnosticar la conectividad de red básica |
|**speed** _{10/100/1000/auto}_|Un comando de modo de interfaz que establece manualmente la velocidad en el valor especificado o lo negocia automáticamente|
|**duplex** _{auto/full/half}_|Un comando de modo de interfaz que establece manualmente dúplex en medio, completo o automático|
|**cdp run** <br/> **no cdp run**|Un comando de modo de configuración que habilita o deshabilita Cisco Discovery Protocol (CDP) para el dispositivo|
|**show mac address-table**|Muestra la tabla de direcciones MAC|
|**show cdp**|Muestra si CDP está habilitado globalmente|
|**show cdp neighbors** _[detail]_|Muestra información resumida sobre cada vecino conectado a este dispositivo; la opción "detalle" enumera información detallada sobre cada vecino|
|**show interfaces**|Muestra información detallada sobre el estado de la interfaz, la configuración y los contadores|
|**show interface status**|Muestra el estado de la línea de la interfaz|
|**show interfaces switchport**|Muestra una gran variedad de opciones de configuración y el estado operativo actual, incluidos los detalles de enlace troncal de VLAN.|
|**show interfaces trunk**|Muestra información sobre los enlaces troncales operativos actualmente y las VLAN admitidas por esos enlaces troncales|
|**show vlan** <br/> **show vlan brief**|Enumera cada VLAN y todas las interfaces asignadas a esa VLAN pero no incluye troncales|
|**show vtp status**|Muestra el estado actual del VTP, incluido el modo actual|

## Comandos de ruteo y VLAN
|Comando|Proposito|
|--|--|
|**ip route** _network-number network-mask {ip-address/interface}_|Establece una ruta estática en la tabla de enrutamiento IP|
|**router rip**|Habilita un proceso de enrutamiento del Protocolo de información de enrutamiento (RIP), que lo coloca en el modo de configuración del enrutador|
|**network** _ip-address_|En el modo de configuración del enrutador, asocia una red con un proceso de enrutamiento RIP|
|**version 2**|En el modo de configuración del enrutador, configura el software para recibir y enviar solo paquetes RIP versión 2|
|**no auto-summary**|En el modo de configuración del enrutador, deshabilita el resumen automático|
|**default-information originate**|En el modo de configuración del enrutador, genera una ruta predeterminada en RIP|
|**passive-interface** _interface_|En el modo de configuración del enrutador, establece solo esa interfaz en el modo RIP pasivo. En el modo RIP pasivo, la interfaz especificada acepta las actualizaciones de enrutamiento RIP, pero no las envía.|
|**show ip rip database**|Muestra el contenido de la base de datos de enrutamiento RIP|
|**ip nat** _[inside/outside]_|Un comando de modo de configuración de interfaz para designar que el tráfico que se origina o se destina a la interfaz está sujeto a NAT|
|**ip nat inside source** _{list{access-list-number/access-list-name}} interface type number[overload]_|Un comando de modo de configuración para establecer una traducción de fuente dinámica. El uso de la palabra clave "lista" le permite usar una ACL para identificar el tráfico que estará sujeto a NAT. La opción de "sobrecarga" permite que el enrutador use una dirección global para muchas direcciones locales.|
|**ip nat inside source static** _local-ip global-ip_|Un comando de modo de configuración para establecer una traducción estática entre una dirección local interna y una dirección global interna|
|**vlan**|Crea una VLAN e ingresa al modo de configuración de VLAN para obtener más definiciones|
|**switchport access vlan**|Establece la VLAN a la que pertenece la interfaz.|
|**switchport trunk encapsulation dot1q**|Especifica la encapsulación 802.1Q en el enlace troncal.|
|**switchport access**|Asigna este puerto a una VLAN|
|**vlan vlan-id** _[name vlan-name]_|Configura un nombre de VLAN específico (de 1 a 32 caracteres)|
|**switchport mode** _{ access/trunk }_|Configura el modo de membresía de VLAN de un puerto. El puerto de acceso está configurado para acceder incondicionalmente y funciona como una única interfaz VLAN sin enlace troncal que envía y recibe tramas no encapsuladas (no etiquetadas). Un puerto de acceso se puede asignar a una sola VLAN. El puerto troncal envía y recibe tramas encapsuladas (etiquetadas) que identifican la VLAN de origen. Un enlace troncal es un enlace punto a punto entre dos conmutadores o entre un conmutador y un enrutador.|
|**switchport trunk** _{encapsulation { dot1q }_|Establece las características del enlace troncal cuando la interfaz está en modo enlace troncal. En este modo, el conmutador admite tráfico etiquetado y sin etiquetar simultáneamente en un puerto.|
|**encapsulation dot1q vlan-id**|Un comando de modo de configuración que define los criterios de coincidencia para asignar el ingreso de tramas 802.1Q en una interfaz a la instancia de servicio adecuada|
|**switch#conf t<br/>switch(config)#int vlan1<br/>switch(config-if)#no shut<br/>switch(config-if)#end<br/>switch#copy run start**|Si una VLAN aparece como *Administratively down*. Esto debería elevar la interfaz y guardar ese estado para el siguiente reinicio del interruptor.|
## Comandos DHCP
|Comando|Proposito|
|--|--|
| **ip address dhcp** | Un comando de modo de configuración para adquirir una dirección IP en una interfaz a través de DHCP |
| **ip dhcp pool name** | Un comando del modo de configuración para configurar un grupo de direcciones DHCP en un servidor DHCP e ingresar al modo de configuración del grupo DHCP |
| **domain-name** _domain_ | Se usa en el modo de configuración del grupo DHCP para especificar el nombre de dominio para un cliente DHCP |
| **network** _network-number [mask]_ | Se utiliza en el modo de configuración del grupo de DHCP para configurar el número de red y la máscara para una subred primaria o secundaria del grupo de direcciones de DHCP en un servidor DHCP de Cisco IOS. |
| **ip dhcp excluded-address** _ip-address [last-ip-address]_ | Un comando de modo de configuración para especificar direcciones IP que un servidor DHCP no debe asignar a clientes DHCP |
| **ip helper-address** _address_ | Un comando de modo de configuración de interfaz para habilitar el reenvío de transmisiones UDP, incluido BOOTP, recibidas en una interfaz |
| **default-router** _address[address2 ... address8]_ | Se utiliza en el modo de configuración del grupo DHCP para especificar la lista de enrutadores predeterminados para un cliente DHCP |
## Comandos de seguridad
|Comando|Proposito|
|--|--|
| **password** _pass-value_ | Muestra la contraseña que se requiere si el comando de inicio de sesión (sin otros parámetros) está configurado |
| **username** _name_ **password** _pass-value_ | Comando global que define uno de los posibles nombres de usuario y contraseñas asociadas que se utilizan para la autenticación de usuarios. Se utiliza cuando se ha utilizado el comando de configuración de línea local de inicio de sesión. |
| **enable password** _pass-value_ | Un comando del modo de configuración que define la contraseña requerida cuando se usa el comando **enable** |
| **enable secret** _pass-value_ | Un comando de modo de configuración que establece esta contraseña de dispositivo de Cisco que se requiere para que cualquier usuario ingrese al modo de activación |
| **service password-encryption** | Un comando del modo de configuración que indica al software Cisco IOS que cifre las contraseñas, los secretos CHAP y datos similares guardados en su archivo de configuración |
| **ip domain-name** _name_ | Configura un nombre de dominio DNS |
| **crypto key generate rsa** | Un comando de modo de configuración que crea y almacena (en una ubicación oculta en la memoria flash) las claves que requiere SSH |
| **transport input** _{telnet/ssh}_ | Utilizado en el modo de configuración de línea vty, define si se permite el acceso Telnet o SSH a este conmutador. Ambos valores se pueden especificar en un solo comando para permitir el acceso Telnet y SSH (configuración predeterminada). |
| **access-list** _access-list-number {deny/permit} source [source-wildcard] [log]_ | Un comando de modo de configuración que define una lista de acceso IP estándar |
| **access-class** | Restringe las conexiones entrantes y salientes entre un vty en particular (en un dispositivo Cisco básico) y las direcciones en una lista de acceso |
| **ip access-list** _{standard | extended} {access-list-name/access-list-number}_ | Un comando del modo de configuración que define una lista de acceso IP por nombre o número |
| **permit source** _[source-wildcard]_ | Se utiliza en el modo de configuración de ACL para establecer condiciones que permitan que un paquete pase una ACL de IP con nombre. Para eliminar una condición de permiso de una ACL, use la forma "no" de este comando. |
| **deny source** _[source-wildcard]_ | Se utiliza en el modo de configuración de ACL para establecer condiciones en una ACL de IP con nombre que denegará paquetes. Para eliminar una condición de denegación de una ACL, use la forma "no" de este comando. |
| **ntp peer** _<ip-address>_ | Se utiliza en el modo de configuración global para configurar el reloj del software para sincronizar un par o para ser sincronizado por un par |
| **switchport port-security** | Se utiliza en el modo de configuración de la interfaz para habilitar la seguridad del puerto en la interfaz |
| **switchport port-security maximum maximum** | Se utiliza en el modo de configuración de la interfaz para establecer la cantidad máxima de direcciones MAC seguras en el puerto |
| **switchport port-security mac-address** _{mac-addr/{sticky [mac-addr]}}_ | Se utiliza en el modo de configuración de interfaz para agregar una dirección MAC a la lista de direcciones MAC seguras. La opción "sticky" configura las direcciones MAC como sticky en la interfaz. |
| **switchport port-security violation** _{shutdown/restrict/protect}_ | Se utiliza en el modo de configuración de la interfaz para establecer la acción que se debe realizar cuando se detecta una violación de la seguridad. |
| **show port security** _[interface interface-id]_ | Muestra información sobre las opciones de seguridad configuradas en la interfaz |
## Comandos de monitoreo y registro
|Comando|Proposito|
|--|--|
| **logging** _ip address_ | Configura la dirección IP del host que recibirá los mensajes de registro del sistema (syslog) |
| **logging trap level** | Se utiliza en el modo de configuración para limitar los mensajes que se registran en los servidores syslog en función de la gravedad. Especifique el número o el nombre del nivel de gravedad deseado en el que se deben registrar los mensajes. |
| **show logging** | Habilitar comando de modo que muestra el estado del registro del sistema (syslog) y el contenido del búfer de registro del sistema estándar. |
| **terminal monitor** | Un comando de modo habilitado que le indica a Cisco IOS que envíe una copia de todos los mensajes de syslog, incluidos los mensajes de depuración, al usuario de Telnet o SSH que emite este comando. |
