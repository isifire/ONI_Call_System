# Instalación de FreePBX en Raspberry Pi

Este script (`install`) y el archivo comprimido (`install.tar.gz`) permiten construir FreePBX (versiones 15, 16 o 17) junto con Asterisk (versiones 18, 20, 21 o 22) en un Raspberry Pi. También se instalarán **iptables**, **dnsmasq** y **exim4**.  
**Tiempo estimado de instalación:** Aproximadamente 15 minutos en un Raspberry Pi 5.

---

## Requisitos previos

1. **Descargar Raspberry Pi OS Lite** (32 bits o 64 bits, versiones Bullseye o Bookworm):  
   - [Descargar desde aquí](https://www.raspberrypi.com/software/operating-systems/).

2. **Escribir la imagen en una tarjeta SD de al menos 8 GB**:  
   - Herramientas recomendadas:  
     - [Etcher](https://etcher.io/)  
     - [imageUSB](http://osforensics.com/downloads/imageusb.zip)

3. **Preparar la tarjeta SD**:
   - Crea un archivo vacío llamado `ssh` en el directorio `/boot/`.  
     Comando en Windows: `type NUL > ssh`  
     Comando en Linux/Mac: `touch /boot/ssh`
   - Crea un archivo llamado `userconf` en el directorio `/boot/` con el siguiente contenido:  
     ```
     pi:$6$c70VpvPsVNCG0YR5$l5vWWLsLko9Kj65gcQ8qvMkuOoRkEagI90qi3F/Y7rm8eNYZHW8CY6BOIKwMH7a3YYzZYL90zf304cAHLFaZE0
     ```

4. Conecta el Raspberry Pi a la red local mediante un cable Ethernet.

5. Inserta la tarjeta SD y enciende el Raspberry Pi.

---

## Instalación

### Paso 1: Transferir los archivos al Raspberry Pi
1. Copia los archivos `install` y `install.tar.gz` al directorio `/home/pi` en el Raspberry Pi.
   - Herramienta recomendada: [WinSCP](https://winscp.net/eng/download.php).

### Paso 2: Conectarse al Raspberry Pi
1. Usa un cliente SSH para iniciar sesión en el Raspberry Pi con las credenciales:
   - Usuario: `pi`
   - Contraseña: `raspberry`
   - Herramienta recomendada: [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

### Paso 3: Ejecutar el script de instalación
1. Haz que el script sea ejecutable:
   ```bash
   chmod +x install
   ```
2. Ejecuta el script como superusuario:
   ```bash
   sudo ./install
   ```

---

## Opciones recomendadas durante la instalación

Durante la ejecución del script, se te pedirá configurar varios parámetros. Aquí están las opciones recomendadas:

1. **Contraseña del usuario `pi`:** Configura una contraseña segura.
2. **Contraseña del usuario `root`:** Configura una contraseña segura.
3. **Selecciona la versión de FreePBX:** Elige la última versión disponible.
4. **Selecciona la versión de Asterisk:** Elige la versión recomendada por el script (habitualmente la más reciente).
5. **Opción Edge:** Responde "No" si no necesitas características experimentales.
6. **Opción IPv6:** Responde "No" a menos que uses IPv6 en tu red.
7. **Revisar las selecciones:** Confirma que todas las configuraciones sean correctas.
8. **Configurar el nombre del host:** Cambia el nombre del host a `FreePBX` (opcional).
9. **Opciones de localización:**
   - **Idioma:** Configura la localización adecuada (por ejemplo, `es_ES.UTF-8` para español).
   - **Zona horaria:** Configura tu zona horaria (por ejemplo, `Europe/Madrid`).
10. **Expandir el sistema de archivos:** Selecciona esta opción para aprovechar todo el espacio de la tarjeta SD.
11. **Reiniciar:** Responde "No" para finalizar la configuración antes de reiniciar.

---

## Finalización de la instalación

1. **Reinicia el Raspberry Pi:**  
   Después de completar la instalación inicial, reinicia el sistema y vuelve a iniciar sesión como `root`.

2. **Verificar la instalación:**  
   La instalación se completará automáticamente y el sistema se reiniciará nuevamente.

3. **Scripts adicionales:**  
   Algunos scripts útiles se encuentran en el directorio `/root` para gestionar utilidades y configuraciones adicionales.

---

## Configuración de Google Voice SIP (GVSIP)

Para usar troncos de Google Voice con FreePBX:

1. Configura las opciones de SIP en FreePBX:
   - **Configuración avanzada > Dialplan y operación:**  
     - `SIP Channel Driver = both`
   - **Asterisk SIP Settings > General SIP Settings:**  
     - `STUN Server Address = stun.l.google.com:19302`
   - Configura los puertos de escucha en las pestañas correspondientes:
     - PJSIP (UDP): 5060
     - PJSIP (TLS): 5061
     - Chan SIP (UDP): 5160
     - Chan SIP (TLS): 5161

2. Carga y configura certificados:
   - Instala el módulo **Certificate Manager**.
   - Mueve los certificados a `/etc/asterisk/keys/`:
     ```bash
     mv /root/obihai.* /etc/asterisk/keys/
     chown asterisk:asterisk /etc/asterisk/keys/obihai*
     ```
   - Importa los certificados en FreePBX:
     - Ve a **Administración > Certificate Management > Importar Localmente**.

3. Configura el archivo `gvsip.dat` con tus credenciales de Google Voice y cópialo:
   ```bash
   cp gvsip.dat /etc/asterisk/pjsip_custom_post.conf
   ```

4. Reinicia Asterisk:
   ```bash
   fwconsole restart
   ```

---

## Notas finales

- **Acceso a la interfaz de FreePBX:**  
  Abre un navegador web y accede a la dirección IP del Raspberry Pi.

- **Mantenimiento:**  
  Utiliza los scripts disponibles en `/root` para gestionar y actualizar el sistema.

Si necesitas ayuda adicional, revisa la documentación oficial de FreePBX o comunícate con el administrador del sistema.

