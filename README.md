# ONI Call System

Este proyecto nace como Trabajo de Fin de Grado, con el objetivo de estudiar y aprender cómo se realiza Spoofing Telefónico, Smishing y clonación de voces con IA. Aquí se recopilan los diferentes materiales utilizados para el trabajo.



# Índice
1. [Instalación de FreePBX en Raspberry Pi](#1-instalación-de-freepbx-en-raspberry-pi)
2. [Configuración adicional](#2-configuración-adicional)
3. [Licencia](#3-licencia)
4. [Contribuciones](#4-contribuciones)
5. [Créditos](5#-creditos)
6. [Contacto](#6-contacto)

## 1. Instalación de FreePBX en Raspberry Pi

Este script (`install`) y el archivo comprimido (`install.tar.gz`) permiten construir FreePBX (versiones 15, 16 o 17) junto con Asterisk (versiones 18, 20, 21 o 22) en un Raspberry Pi. También se instalarán **iptables**, **dnsmasq** y **exim4**.

**Tiempo estimado de instalación:** Aproximadamente 15 minutos en un Raspberry Pi 5.

### Requisitos

#### Hardware
- Raspberry Pi 5 (recomendado) o similar.
- Tarjeta SD de 8 GB o más.
- Conexión LAN mediante cable Ethernet.

#### Software
- Imagen Raspberry Pi OS Lite (32-bit/64-bit Bullseye o Bookworm).
- Herramientas para escribir la imagen en la tarjeta SD:
  - [Etcher](https://etcher.io/)
  - [imageUSB](http://osforensics.com/downloads/imageusb.zip)
- Cliente SSH como [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
- Cliente SCP como [WinSCP](https://winscp.net/eng/download.php).

### Proceso de instalación

#### Paso 1: Preparación del sistema
1. Descarga e instala la imagen Raspberry Pi OS Lite en una tarjeta SD.
2. Crea un archivo vacío llamado `ssh` en el directorio `/boot/`.
3. Crea un archivo de texto llamado `userconf` en el directorio `/boot/` con el siguiente contenido:

   ```plaintext
   pi:$6$c70VpvPsVNCG0YR5$l5vWWLsLko9Kj65gcQ8qvMkuOoRkEagI90qi3F/Y7rm8eNYZHW8CY6BOIKwMH7a3YYzZYL90zf304cAHLFaZE0
   ```
5. Inserta la tarjeta SD en la Raspberry Pi y conéctala a tu LAN.
6. Enciende la Raspberry Pi.

#### Paso 2: Copiar archivos de instalación
1. Copia los archivos `install` e `install.tar.gz` al directorio `/home/pi` de la Raspberry Pi usando WinSCP.

#### Paso 3: Ejecutar instalación
1. Accede a la Raspberry Pi vía SSH usando las credenciales `pi:raspberry`.
2. Haz el script ejecutable:
   
   ```bash
   chmod +x install
   ```
4. Ejecuta el script:
   
   ```bash
   sudo ./install
   ```
6. Sigue las indicaciones:
   - Configura la contraseña del usuario `pi` y del usuario `root`.
   - Selecciona las versiones de FreePBX y Asterisk.
   - Responde las opciones de Edge e IPv6 ("No" recomendado).
   - Configura el nombre de host, idioma, zona horaria y expande el sistema de archivos.
   - Selecciona "Finish" sin reiniciar.
7. Reinicia manualmente la Raspberry Pi.
8. Inicia sesión como `root` y completa la instalación.
9. Configura de forma básica FreePBX siguiendo los pasos establecidos en la interfaz web.
10. Acceder a Configuraciones -> Configuraciones Asterisk SIP -> Configuraciones NAT y la nuestra red local en la que se encuentra el servidor, sin ello los prefijos SIP no funcionarán.


## 2. Configuración adicional

### Configuración de GVSIP

1. Configura el servidor STUN: `stun.l.google.com:19302`.
2. Habilita los controladores de canales SIP adecuados (PJSIP).
3. Importa el certificado necesario para TLS/SSL desde la herramienta Certificate Manager.
4. Crea y edita el archivo `gvsip.dat` con la información de tu cuenta de Google Voice.
5. Copia `gvsip.dat` a `/etc/asterisk/pjsip_custom_post.conf`.
6. Reinicia FreePBX:
   
   ```bash
   fwconsole restart
   ```

### Configuración del servidor de fax HylaFAX

1. Ejecuta el script de instalación:
   
   ```bash
   ./install-fax
   ```
3. Añade extensiones de fax:
   
   ```bash
   ./add-fax-extension
   ```
5. Configura el cliente SendFax para enviar faxes desde Windows.


## 3. Licencia
Este proyecto se distribuye bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.


## 4. Contribuciones

Las contribuciones son bienvenidas. Por favor, abre un *issue* o envía un *pull request* con tus mejoras o correcciones.


## 5. Créditos
- El script de instalación (`install` e `install.tar.gz`) no es de mi autoría. Todo el crédito corresponde a **RonR**, quien diseñó e implementó este instalador automatizado para FreePBX en Raspberry Pi. Agradecimientos por su contribución a la comunidad.

## 6. Contacto
Si tienes alguna duda o problema, por favor abre un *issue* en este repositorio.
