# Ejercicio: Firma Digital con GPG

En esta práctica se usará la herramienta GPG para firmar documentos digitalmente en **AlmaLinux**.

![Logo gpg](../img/firma.png){:style="width: 40%;" class="center"}

#### Objetivos de la práctica

1. Comprender el proceso de creación y gestión de claves GPG.
2. Aprender a firmar digitalmente un documento.
3. Verificar la autenticidad de una firma digital en un archivo.
4. Comprender la importancia de la firma digital en la integridad y autenticidad de la información.


### Materiales necesarios

- Un equipo con **Linux** (con GPG instalado).
- Conexión a internet (para instalar GPG si fuera necesario).

---

### Paso 1: Instalación de GPG

1. **Verificar si GPG ya está instalado:**
   - Abre una terminal y escribe:
     ```bash
     gpg --version
     ```
   - Si aparece la versión de GPG, ya está instalado. De lo contrario, sigue los pasos de instalación.

2. **Instalación (si no está instalado):**
   - **AlmaLinux:** Puedes instalar GPG con el siguiente comando:
     ```bash
     sudo dnf install gnupg2
     ```
     *Nota:* En algunas versiones, `gnupg` puede estar ya instalado. `gnupg2` es la versión más actualizada.

---

### Paso 2: Crear un par de claves GPG

1. **Generar el par de claves:**
   - En la terminal, escribe:
     ```bash
     gpg --full-generate-key
     ```
   - Selecciona las opciones según las instrucciones. Usa la clave de tipo **RSA y RSA** y un tamaño mínimo de **2048 bits**. Asegúrate de configurar una **fecha de caducidad** (por ejemplo, 1 año) y proporciona tu nombre y correo electrónico.

2. **Confirmación de creación de la clave:**
   - Una vez completada la configuración, GPG generará el par de claves (privada y pública).
   - Anota el **ID de la clave** que GPG generará, ya que lo usarás en los pasos siguientes.

---

### Paso 3: Exportar la clave pública

1. **Exportar tu clave pública** para compartirla con otras personas:
   - Ejecuta el siguiente comando, reemplazando `TU_ID_DE_CLAVE` por el ID que has anotado:
     ```bash
     gpg --armor --export TU_ID_DE_CLAVE > clave_publica.asc
     ```
   - Esto creará un archivo `clave_publica.asc` que contiene tu clave pública en formato ASCII.

2. **Envía el archivo `clave_publica.asc`** a uno de tus compañeros para que pueda verificar las firmas que generes. Tendrá que importar la clave pública a su llavero con el comando:
     ```bash
     gpg --import clave_publica.asc
     ```

---

### Paso 4: Firmar un archivo

1. **Crea un archivo de texto para firmar** (por ejemplo, `documento.txt`):
   ```bash
   echo "Este es un documento importante." > documento.txt
   ```

2. **Firmar el archivo**:
   - Ejecuta el siguiente comando para firmar digitalmente `documento.txt`:
     ```bash
     gpg --output documento.txt.sig --sign documento.txt
     ```
   - Esto generará un archivo `documento.txt.sig` que contiene la firma digital.

3. **Opcional:** También puedes generar una **firma separada** con el comando:
   ```bash
   gpg --output documento.txt.sig --detach-sig documento.txt
   ```

---

### Paso 5: Verificar una firma digital

1. **Envía el archivo `documento.txt` y su firma `documento.txt.sig`** a uno de tus compañeros.

2. **Verificar la firma recibida:**
   - El compañero que reciba el archivo puede verificar la firma con el siguiente comando:
     ```bash
     gpg --verify documento.txt.sig documento.txt
     ```
   - Si la firma es válida, GPG confirmará que el archivo fue firmado por el propietario de la clave correspondiente y no ha sido modificado.

---

### Paso 6: Revocar una clave (en caso de necesidad)

1. Si deseas revocar una clave (por ejemplo, en caso de pérdida de la clave privada), puedes **generar un certificado de revocación**:
   ```bash
   gpg --output revocacion.asc --gen-revoke TU_ID_DE_CLAVE
   ```
2. **Almacena este archivo `revocacion.asc` en un lugar seguro.** Solo necesitarás usarlo en caso de que necesites revocar tu clave.

---

>  **Preguntas**:
>    - ¿Qué garantiza la firma digital de un archivo?
>    - ¿Por qué es importante que las claves tengan una fecha de caducidad?
>    - Explica el proceso de verificación de la firma desde el punto de vista de la seguridad.


```

## Bibliografía

- [The GNU Privacy Guard ](https://www.gnupg.org/)
* [Manual de GnuPG](https://www.gnupg.org/gph/es/manual.html)
* [Gpg4win](https://www.gpg4win.org/download.html)
* [RFC4880](https://tools.ietf.org/html/rfc4880)
* [PGP](https://es.wikipedia.org/wiki/Pretty_Good_Privacy)