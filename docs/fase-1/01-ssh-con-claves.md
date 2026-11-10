# 01 · SSH con claves y sin contraseñas

**Fecha:** 2026-11-10 · **Fase:** 1

## Objetivo

Administrar el servidor (`hp-server`) en remoto desde el PC, como se administra una máquina EC2 en AWS: por SSH, con clave y sin aceptar contraseñas.

## Punto de partida (inventario)

Antes de tocar nada hice inventario del portátil, como se haría con un servidor heredado:

| Comando | Resultado |
|---|---|
| `hostname -I` | IP en la red de casa (wifi) + `172.17.0.1` (red interna de Docker) |
| `lsb_release -a` | Linux Mint 22.3 "Zena" (base Ubuntu 24.04) |
| `sudo docker ps -a` | Docker instalado, sin contenedores |
| `systemctl status ssh` | Servidor SSH no instalado |

## Qué hice

1. **Instalé el servidor SSH:** `sudo apt update` y `sudo apt install openssh-server`.
2. **Verifiqué la huella del servidor** en la primera conexión, comparándola con la que muestra el propio servidor: `ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub`.
3. **Generé un par de claves en el PC:** `ssh-keygen -t ed25519 -C "arnau-pc"`, con passphrase.
4. **Copié la clave pública** al `~/.ssh/authorized_keys` del servidor (permisos `700` en la carpeta y `600` en el archivo).
5. **Endurecí SSH** con un archivo propio, `/etc/ssh/sshd_config.d/10-hardening.conf`:
   ```
   PasswordAuthentication no
   KbdInteractiveAuthentication no
   PermitRootLogin no
   ```
6. **Validé antes de aplicar** (`sudo sshd -t` y `sudo sshd -T | grep ...`) y reinicié con `sudo systemctl restart ssh`, manteniendo abierta una sesión de rescate.
7. **Probé como atacante:** `ssh -o PubkeyAuthentication=no ...` → `Permission denied (publickey)`.

## Por qué

- **SSH es la forma estándar de administrar servidores Linux**, también en la nube.
- **Las claves son más seguras que las contraseñas:** la clave privada nunca sale del PC y no se puede adivinar por fuerza bruta. Las EC2 de AWS funcionan así por defecto (*key pairs*).
- **La huella del servidor** protege contra ataques *man-in-the-middle*: hay que compararla por un segundo camino (pantalla del servidor, o el registro de arranque en AWS).
- **Archivo propio en `sshd_config.d/`** en lugar de editar `sshd_config`: las actualizaciones no lo pisan y queda claro qué he cambiado. En SSH **gana el primer valor leído**, por eso el prefijo `10-`.
- **Nada de root por SSH:** mínimo privilegio, igual que en AWS.

## Errores que cometí (y qué aprendí)

- **`git commit` desde `D:\`** → `not a git repository`. Git busca la carpeta `.git` en el directorio actual y en los de arriba.
- **Hice SSH del portátil al propio portátil** porque no miré el prompt. Por eso me pedía contraseña y la huella otra vez. **Lección: mirar siempre el prompt antes de pulsar Enter.**
- **`docker ps` → permission denied:** el comando habla con el servicio de Docker a través de `/var/run/docker.sock`, y mi usuario no tiene permiso. Pertenecer al grupo `docker` equivale a ser root, así que de momento uso `sudo`.
- **`cd imagenes` no funciona:** en Linux se distinguen mayúsculas y minúsculas (`Imágenes`).

## Relación con la certificación (SAA-C03)

- EC2 *key pairs* y acceso a instancias.
- Mínimo privilegio y endurecimiento de instancias (*Design Secure Architectures*).

## Pendiente

- Acceso desde el iPad (Termius) con su propia clave.
- Firewall (`ufw`): permitir solo SSH desde la red de casa.
- IP fija cuando el portátil esté junto al router por cable.
