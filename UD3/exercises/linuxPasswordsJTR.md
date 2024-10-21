
# Ejercicio: Fortaleza de contraseñas en Linux

## Introducción

En esta práctica utilizaremos la herramienta **John the Ripper** (JtR) para comprobar la fortaleza de las contraseñas de las cuentas de usuario en un sistema Linux.

Para entender el proceso, estudiaremos cómo se almacenan las contraseñas y los usuarios en un sistema operativo basado en Linux, y luego utilizaremos JtR para intentar descifrar las contraseñas de usuarios que hemos creado previamente.

![JtR logo](../img/John-the-Ripper-Logo.jpg){:style="width: 50%;" class="center"}

**John the Ripper** ([https://www.openwall.com/john/](https://www.openwall.com/john/)) es una herramienta de código abierto para la auditoría de seguridad y recuperación de contraseñas. Su versión Jumbo admite una gran variedad de tipos de cifrado y hash en múltiples plataformas, incluidos sistemas Linux, macOS, Windows, aplicaciones web y muchos más. A diferencia de otras herramientas, JtR no se limita a utilizar ataques de fuerza bruta, sino que emplea varios modos más avanzados, como el **single crack**, que permite un análisis más rápido e inteligente de las contraseñas más comunes e inseguras.

## Preparativos: Configuración de AlmaLinux

Usaremos **AlmaLinux** en esta práctica, ya que es una distribución basada en RHEL (Red Hat Enterprise Linux), que es muy común en entornos empresariales.

## Gestión de usuarios en AlmaLinux

A continuación, repasamos algunos comandos útiles que utilizaremos durante la práctica:

* Para añadir un usuario en AlmaLinux:

  ```sh
  sudo adduser nombreUsuario
  ```

* Para cambiar la contraseña del usuario actual:

  ```sh
  passwd
  ```

* Para cambiar la contraseña de otro usuario:

  ```sh
  sudo passwd nombreUsuario
  ```

* Para forzar a un usuario a cambiar la contraseña en su próximo inicio de sesión:

  ```sh
  sudo chage -d 0 nombreUsuario
  ```

## Almacenamiento de usuarios y contraseñas en AlmaLinux

Al igual que en otras distribuciones Linux, AlmaLinux almacena la información de usuarios y contraseñas en los archivos `/etc/passwd` y `/etc/shadow`. Veamos en detalle el contenido y propósito de estos archivos.

1. Crea los siguientes usuarios con las contraseñas indicadas:
   - Usuario **pablo** con contraseña **123456**.
   - Usuario **pedro** con contraseña **pedro123**.

   Estos usuarios tienen contraseñas comunes y vulnerables, lo que nos permitirá probar JtR más fácilmente.

2. Verifica los usuarios existentes mostrando el contenido del archivo `/etc/passwd`:

   ```sh
   cat /etc/passwd
   ```

   > **Tareas**:
   > 
   > - Muestra una captura en la que se vea que los usuarios creados aparecen en el archivo `/etc/passwd`.
   > - Investiga el significado de cada campo en una línea del archivo `/etc/passwd`.
   > - Verifica los permisos de los archivos `/etc/passwd` y `/etc/shadow` con el comando `ls -l`. Explica las diferencias en los permisos y el motivo de su existencia.
   > - Muestra una captura del archivo `/etc/shadow` para explicar cómo se almacenan las contraseñas (formato hash). ¿Qué tipo de cifrado se utiliza? (Apoya tu respuesta con bibliografía).

3. **Hash de contraseñas**: Investiga qué es una función **hash** y su utilidad en la seguridad de contraseñas.

4. **Rainbow Tables**: ¿Qué son las **rainbow tables** y cómo pueden utilizarse para descifrar contraseñas?

5. Crea dos usuarios (`user1` y `user2`) con la misma contraseña (`flower`). Verifica si el hash generado para ambos usuarios es el mismo o diferente, y explica por qué ocurre esto.

6. **Salted hash**: Investiga qué es un **salted hash** y por qué es importante en la seguridad de contraseñas.

## Instalación de John the Ripper en AlmaLinux

No disponemos de un paquete, así que tenemos que descargar el código y compilarlo.

1. Descargamos, descomprimimos, compilamos y probamos el código:

```sh
# Descargamos desde repositorio oficial
git clone --depth=1 https://github.com/openwall/john
# Instalamos requisitos
sudo dnf install -y openssl-devel
sudo dnf install make
sudo dnf groupinstall "Development Tools"
# Compilamos el código
cd john/src
./configure
make
# Comprobamos si funciona: hacemos pruebas de rendimiento
cd ../run/
./john --test
```

   > **Tareas**:
   > 
   > - Muestra el rendimiento de diferentes tipos de cifrado en tu máquina.

## Crackeando contraseñas

Para crackear las contraseñas de los usuarios, primero combinaremos los contenidos de `/etc/passwd` y `/etc/shadow` en un único archivo con el comando `unshadow`, que encontrarás en la misma carpeta que el comando `john`:

```sh
sudo ./unshadow /etc/passwd /etc/shadow > hashes.txt
```

1. Ejecuta el comando y verifica el contenido del archivo `hashes.txt` para los usuarios `pablo` y `pedro`, edita el fichero y deja únicamente las líneas de los usuarios que te interesan.

2. Inicia el proceso de cracking con **John the Ripper**:

   ```sh
   sudo ./john hashes.txt
   ```

   Este comando ejecuta varios modos de cracking de contraseñas de forma secuencial.

3. Si la contraseña de un usuario es débil, **John the Ripper** la descifrará rápidamente. Si es fuerte, tardará más tiempo.

### Consultar las contraseñas crackeadas

Para ver las contraseñas que ha descifrado John the Ripper, utilizamos el siguiente comando:

```sh
sudo ./john --show hashes.txt
```

> **Tareas**:
> 
> - Muestra una captura en la que se vean las contraseñas obtenidas.

## Conclusiones

Al finalizar esta práctica, deberías ser capaz de responder a las siguientes preguntas:

1. ¿Qué es John the Ripper y para qué se utiliza?
2. ¿Por qué se utiliza el archivo `/etc/shadow` para almacenar contraseñas?
3. ¿Cómo se almacena una contraseña en dicho archivo? Explica el formato de almacenamiento.

### Consideraciones legales y éticas:

Es importante recordar que el uso de **John the Ripper** para crackear contraseñas sin autorización es ilegal y antiético. Solo debes utilizar estas técnicas en entornos controlados, como pruebas de penetración autorizadas o en entornos educativos con los permisos adecuados.

## Bibliografía

- [Página oficial de John the Ripper password Cracker](https://www.openwall.com/john/)
- [Página the GitHub de John the Ripper](https://github.com/openwall/john)
	- [Documentación GitHub John the Ripper](https://github.com/openwall/john/tree/bleeding-jumbo/doc)
		- [Instrucciones de instalación](https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL)
- [Tutorial Jtr ccatyberciti](https://www.cyberciti.biz/faq/unix-linux-password-cracking-john-the-ripper/)
- [Tutorial de uso de RedesZone.net](https://www.redeszone.net/seguridad-informatica/john-the-ripper/)
- [Tutorial de uso de FreeCodeCamp](https://www.freecodecamp.org/news/crack-passwords-using-john-the-ripper-pentesting-tutorial/)
- [Formato de shadow](https://linuxize.com/post/etc-shadow-file/)
- [Chuleta de uso de Git](https://education.github.com/git-cheat-sheet-education.pdf)

<!--
https://gist.github.com/goffinet/83565ebec963fed0c74d
-->

