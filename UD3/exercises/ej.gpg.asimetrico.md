
# Ejercicio: Cifrado Asimétrico con GnuPG

Anteriormente, utilizamos GPG (GNU Privacy Guard) para realizar **cifrado simétrico** de mensajes. En esta práctica, vamos a utilizar la misma herramienta para **cifrado asimétrico**.

**Pretty Good Privacy (PGP)** es un programa desarrollado por Phil Zimmermann en 1991, diseñado para proteger la información transmitida por Internet mediante **criptografía de clave pública** y facilitar la autenticación de documentos a través de **firmas digitales**. Basado en el diseño de PGP, el estándar **OpenPGP** (RFC4880) fue desarrollado por la Internet Engineering Task Force. GPG, o GnuPG, es una implementación completa y gratuita de este estándar.

![Logo gpg](../img/gnupg_logo.png){:style="width: 50%;" class="center"}

PGP es un criptosistema híbrido que **combina criptografía simétrica y asimétrica**. Esto permite aprovechar las ventajas de ambos: **la criptografía simétrica es más rápida**, mientras que la asimétrica resuelve el problema de la distribución segura de claves y garantiza la **autenticidad** y el **no repudio** de los datos.

**Nota:** Aunque GPG permite el cifrado simétrico y asimétrico, **en esta práctica nos centraremos en el cifrado asimétrico**.

Usaremos un sistema operativo Linux.

## Pasos a seguir:

1) Genera un par de claves de criptografía asimétrica: una clave pública y una clave privada.

   Hay dos métodos para hacerlo:

   - Para acceder a todas las opciones disponibles, usa el comando:
     ```sh
     gpg --full-generate-key
     ```

   - O bien, utiliza RSA con una clave predeterminada de 3072 bits:
     ```sh
     gpg --gen-key
     ```

   **En esta práctica, usaremos la primera opción.** Durante el proceso, se te harán varias preguntas:

   - **a. Tipo de clave:** Selecciona el tipo de clave entre cuatro opciones, que corresponden a algoritmos asimétricos (similares a DES o AES en criptografía simétrica). Elegiremos la **opción 2 (DSA y ElGamal)**, ya que permite usar un par de claves para cifrar y otro para firmar.

   - **b. Tamaño de la clave DSA:** Acepta el tamaño por defecto (2048) pulsando **intro**.

   - **c. Periodo de validez de la clave:** Selecciona un periodo de **5 años** escribiendo `5y`. Esto limita la validez de la clave en caso de pérdida o compromiso.

   - **d. Identificación de la clave:** Ingresa un nombre, dirección de correo y escribe `SMR2` como comentario. **Nota:** La dirección de correo es solo para identificar la clave.

   - **e. Confirmación de datos:** Revisa los datos y confirma pulsando **V**ale.

   - **f. Clave de protección:** Ingresa una **clave fácil de recordar** que protegerá las claves asimétricas generadas.

   **Tras completar el proceso**, GPG generará las claves y las almacenará en el directorio oculto `.gnupg` en tu directorio `HOME`. Las claves públicas estarán en `pubring.gpg` y las privadas en `secring.gpg`.

2) Muestra la lista de claves generadas:

   ```sh
   gpg --list-keys
   ```

   **Nota:** Verás una clave primaria (DSA) y una subordinada (ElGamal).

3) Exporta tu clave pública para que otros puedan enviarte mensajes cifrados. Para ello, usa:

   ```sh
   gpg –a –o /tmp/tunombre.pub --export tu_email@dominio.com
   ```

   Los parámetros `-a` y `-o` especifican que el archivo estará en formato de texto y se guardará en `/tmp`. La clave pública generada puede ser compartida con cualquiera.

4) **Intercambia claves:** Comunica tu clave pública a un compañero y recibe la suya. Si estás fuera de clase, puedes enviarte mensajes a ti mismo iniciando sesión como otro usuario en el mismo ordenador:

   - Para crear un usuario nuevo:
     ```sh
     sudo adduser nombreUsuario
     ```
   - Para cambiar de usuario:
     ```sh
     su nombreUsuario
     ```

5) **Importa la clave pública de tu compañero** para poder cifrar mensajes destinados a él:

   ```sh
   gpg --import /tmp/clave_de_otro_alumno.pub
   ```

6) **Consulta las claves disponibles en tu llavero**:

   ```sh
   gpg --list-keys
   ```

7) Crea un archivo llamado `mensaje.txt`:

   ```sh
   nano mensaje.txt
   ```

8) Cifra el mensaje utilizando la clave pública del destinatario:

   ```sh
   gpg –v –a –o /tmp/mensaje.cifrado --encrypt --recipient cuenta@correo.es mensaje.txt
   ```

   El sistema te pedirá confirmar la identidad de la clave pública. Puedes verificar la **huella digital** del certificado del destinatario con el siguiente comando:

   ```sh
   gpg --fingerprint
   ```

9) Muestra el contenido del mensaje cifrado con el comando `cat`:

   ```sh
   cat /tmp/mensaje.cifrado
   ```

10) **Envía tu mensaje cifrado** al destinatario. Solo él podrá descifrarlo.

11) Para **descifrar un mensaje recibido**:

   ```sh
   gpg --decrypt /tmp/mensaje.cifrado
   ```

   **Nota:** El sistema pedirá la contraseña de la clave privada. Puedes optar por que el sistema recuerde la clave durante un tiempo limitado.

12) Descifra el mensaje y guarda el contenido en un archivo:

   ```sh
   gpg --decrypt /tmp/mensaje.cifrado > mensaje.descifrado
   ```

13) Prueba a **modificar el archivo cifrado** para simular un daño y verifica que se produce un error al intentar descifrarlo.

--

# Tareas adicionales:

14) **Comparar tiempos de cifrado/descifrado:** Realiza pruebas cifrando archivos más grandes (como un archivo de vídeo o imagen) y compara el tiempo que tarda GnuPG en cifrar y descifrar usando cifrado simétrico y asimétrico. Puedes usar el comando `time` para medirlo:
   
   ```bash
   time gpg --symmetric archivo_grande.mp4
   ```

```
   time gpg –v –a –o /tmp/archivo.cifrado --encrypt --recipient cuenta@correo.es archivo_grande.mp4
   ```
   


> **Pregunta final:** Después de usar ambos tipos de cifrado (simétrico y asimétrico), ¿qué ventajas observas en el cifrado simétrico frente al asimétrico?


## Bibliografía

- [The GNU Privacy Guard ](https://www.gnupg.org/)
* [Manual de GnuPG](https://www.gnupg.org/gph/es/manual.html)
* [Gpg4win](https://www.gpg4win.org/download.html)
* [RFC4880](https://tools.ietf.org/html/rfc4880)
* [PGP](https://es.wikipedia.org/wiki/Pretty_Good_Privacy)