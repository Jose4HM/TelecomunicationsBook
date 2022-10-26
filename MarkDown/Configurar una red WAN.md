# Configurar una red WAN a través de internet
<div>
<p style = 'text-align:center;'>
<img src="https://user-images.githubusercontent.com/71269653/197926936-aaeed4da-c2e2-4851-9aff-8387aa8368eb.PNG" alt="topologia" width="800px">
</p>
</div>

## Interfaces de routers
Teniendo nuestra topologia debemos de configurar las **interfaces** de los routers, para ello:
```
R1>enable
R1#configure terminal
R1(config)#int {interfaces que tenemos(G0/0, f0/5, etc)}
R1(config)#ip address {dirección ip} {máscara de red}
R1(config)#no shutdown
```
Configuramos de igual manera el puerto serial, nota que ahora incluye el comando `clock rate`
```
R1>enable
R1#configure terminal
R1(config)#int {interfaz serial s0/0 o la que tengamos}
R1(config)#ip address {dirección ip} {máscara de red}
R1(config)#clock rate 128000
R1(config)#no shutdown
```
> Debemos configurar todos los router de esta manera, adaptando los códigos a cada situación.

## Configurando DHCP

Esto nos permitirá un routing dinámico en las PC's que tengamos conectadas a nuestra topología. Para esto debemos configurar el DHCP de la siguiente manera:
```
R1> enable
R1# configure terminal
R1(config)# ip dchp pool {nombre del pool}
R1(config-ip)# network {ip de la interfaz inferior} {máscara de red}
R1(config-ip)# default-router {ip de la interfaz inferior}
```
¿A qué nos referimos con ip de la interfaz inferior?
Sabemos que en un switch tenemos puertos que nos serviran para conectar elementos por arriba de este (puertos seriales) y por debajo de este (fast ethernet, giga ethernet, consola). En este caso solo tenemos conectados a la consola Gigabit Ethernet y Serie, por lo tanto nuestra ip que debemos colocar ahí es la de Gigabit Ethernet.

<div>
<p style = 'text-align:center;'>
<img src="https://user-images.githubusercontent.com/71269653/197926188-8796215a-429e-489e-861c-3c20d2bbe9a9.PNG" alt="IP" width="600px">
</p>
</div>

## Configuración de la nube

Debemos de ir a la nube y vamos a *Config>Interface>Serial0*. Atento en esta parte que debes seguir estos pasos:
1. Cada puerto serial que este conectado a la nube vamos a asiganrle un número (tendremos esto como referencia). Si tengo 2 router: Router 1 será 1, el Router 2 será 2, Router 3 será 3 y así sucesivamente (independientemente al puerto donde se ha conectado).
2. En la pestaña Serial0, escribimos en DLCI el número del origen y el número del destino separados por un 0. Por ejemplo, asumamos que en el puerto serial 0 de la nube tengo conectado el router R1, en el puerto serial 1 tengo router 2 y en el puerto serial 2 tengo router 3. Entonce escribo, en el DLCI 102 porque a través del serial 0 puedo comunicarme desde el router 1 hacia el router 2. Luego de ello le asignamos un nombre para identificarlo.
<div>
<p style = 'text-align:center;'>
<img src="https://user-images.githubusercontent.com/71269653/197926307-7ee0cb38-fdba-434e-8c69-442471b88f20.PNG" alt="Interface en la nube" width="600px">
</p>
</div>

4. Hacemos lo mismo con los demas Seriales. Como ayuda puedes ayudarte de esta oración *Del router A puedo llegar al router B por el serial C*, escribes: *A0B* dentro del *serial C*. Cuando lo tengas definido, no te olvides darle en *Add*
5. Una vez que tengamos todas las combinaciones, nos dirigimos a *Connections>Frame relay*. Aquí vamos a seleccionar el serial y nombre del DCLI, seleccionamos todas las combinaciones posibles; si la combinacion ya esta definifida te indicará con un mensaje. Las combaciones son recíprocas (es lo mismo A-B que B-A) así que si te sale un mensaje que no se pudo añadir es porque ya está definido. Añadimos cada uno pulsando *Add*
<div>
<p style = 'text-align:center;'>
<img src="https://user-images.githubusercontent.com/71269653/197926777-e196f497-a059-4f78-951e-57cd90d7c962.PNG" alt="Interface en la nube" width="600px">
</p>
</div>

## Routeo estático
Con eso configurado volvemos a los router para configurar los router y su ruteo estático:
```
R1> enable
R1# configure terminal
R1(config)# ip route {ip dirección de red de entrada} {máscara} {ip del salto, la IP por donde va salir}
R1(config)# int {interface serial del mismo router}
R1(config-if)#encapsulation frame-relay
```
> Nota que estamos configurando desde R1 pero las direcciones IP que vamos a colocar son las IP de los demás routers. La dirección de red de entrada es la que esta entre el router y el swich. La *ip de salto* es la siguiente IP (generalmente la del puerto serie). Recuerda que la *dirección de red* tiene un cero **al final**, mientras que las IP normales no lo tienen.

>Todas las configuraciones que se han hecho **deben ser realizadas** para todos routers.
