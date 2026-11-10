# 02 · Firewall con ufw

**Fecha:** 2026-11-10 · **Fase:** 1

## Objetivo

Que al servidor solo le puedan llegar las conexiones que yo decida: **todo cerrado salvo lo que abra explícitamente**. Es el mismo modelo que un *security group* de AWS.

## Qué hice

Desde la sesión SSH en `hp-server`, **en este orden**:

```bash
sudo ufw status verbose                      # inventario: estaba inactivo
sudo ufw default deny incoming               # cerrar todo lo que entra
sudo ufw default allow outgoing              # dejar salir (actualizaciones, etc.)
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp comment 'SSH desde LAN'
sudo ufw enable                              # encender (también al arrancar)
sudo ufw status verbose                      # comprobar
```

Resultado:

```
Estado: activo
Acceso: on (low)
Predeterminado: deny (entrantes), allow (salientes), deny (enrutados)

Hasta        Acción     Desde
22/tcp       ALLOW IN   192.168.1.0/24    # SSH desde LAN
```

Después comprobé desde una sesión SSH nueva en el PC que seguía pudiendo entrar.

## Por qué

- **Denegar por defecto:** si mañana instalo algo que abre un puerto sin darme cuenta, no queda expuesto.
- **El orden importa:** si activo `deny incoming` sin haber permitido antes SSH, corto mi propia conexión. En la nube no hay teclado físico para arreglarlo.
- **SSH solo desde la LAN (`192.168.1.0/24`)** y no desde cualquier sitio: mínimo privilegio también en red.
- **Registro (`Acceso: on`)**: las conexiones bloqueadas quedan registradas, útil para ver intentos de acceso.

## Conceptos

- **Puerto:** número que identifica un servicio dentro de una máquina (SSH = 22, HTTP = 80, HTTPS = 443).
- **CIDR:** `192.168.1.0/24` = las IPs de `192.168.1.0` a `192.168.1.255`. El `/24` dice cuántos bits son fijos. En AWS, las VPC y subredes se definen con CIDR (p. ej. `10.0.0.0/16`).
- **Stateful:** ufw (como los security groups) recuerda las conexiones salientes y deja pasar sus respuestas sin necesitar una regla de entrada.

## Equivalencia con AWS

| ufw (home lab) | AWS |
|---|---|
| `default deny incoming` | Un security group nuevo no permite nada entrante |
| `default allow outgoing` | Regla de salida por defecto: todo permitido |
| `allow from 192.168.1.0/24 port 22` | Regla de entrada: SSH (22) desde un CIDR concreto, nunca `0.0.0.0/0` |

## A tener en cuenta

- **Docker se salta ufw:** los puertos que publica un contenedor (`-p 8080:80`) quedan accesibles aunque ufw no los permita, porque Docker modifica las reglas de red por su cuenta. Lo trataré cuando monte los primeros contenedores.
