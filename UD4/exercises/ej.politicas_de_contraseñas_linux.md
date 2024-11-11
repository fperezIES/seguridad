
# Establecimiento de Políticas de Contraseñas y Gestión de Permisos en AlmaLinux 9

## Índice

1. [Introducción](#introducción)
3. [Establecimiento de Políticas de Contraseñas en AlmaLinux 9](#establecimiento-de-políticas-de-contraseñas-en-almalinux-9)
   - 2.1 [Introducción a la Seguridad de Contraseñas](#21-introducción-a-la-seguridad-de-contraseñas)
   - 2.2 [Configuración de Políticas con PAM](#22-configuración-de-políticas-con-pam)
   - 2.3 [Parámetros Clave de Configuración](#23-parámetros-clave-de-configuración)
   - 2.4 [Ejemplos de Configuración](#24-ejemplos-de-configuración)
   - 2.5 [Aplicación y Verificación de Políticas](#25-aplicación-y-verificación-de-políticas)
4. [Sistema de Permisos en Linux](#sistema-de-permisos-en-linux)
   - 3.1 [Fundamentos de Propietario, Grupo y Otros](#31-fundamentos-de-propietario-grupo-y-otros)
   - 3.2 [Permisos Básicos](#32-permisos-básicos)
   - 3.3 [Visualización de Permisos](#33-visualización-de-permisos)
   - 3.4 [Cambio de Propietario de Fichero](#34-cambio-de-propietario-de-fichero)
   - 3.5 [Cambio de Permisos](#35-cambio-de-permisos)
   - 3.6 [Utilidad y Gestión de Grupos](#36-utilidad-y-gestión-de-grupos)
5. [Conclusión](#conclusión)

---

## Introducción

En este documento, exploraremos cómo establecer políticas de contraseñas adecuadas en AlmaLinux 9 y comprenderemos el sistema de permisos en Linux. Estos conceptos son esenciales para garantizar la seguridad y el correcto funcionamiento de un sistema operativo Linux en entornos tanto personales como empresariales.

## Establecimiento de Políticas de Contraseñas en AlmaLinux 9

### 2.1 Introducción a la Seguridad de Contraseñas

Las contraseñas son la primera línea de defensa contra accesos no autorizados. Establecer políticas robustas de contraseñas es crucial para proteger los sistemas y datos sensibles. En AlmaLinux 9, la gestión de políticas de contraseñas se realiza principalmente a través de PAM (Pluggable Authentication Modules) y el archivo de configuración `pwquality.conf`.

### 2.2 Configuración de Políticas con PAM

PAM es un marco flexible que maneja la autenticación de usuarios en sistemas Linux. El módulo `pam_pwquality.so` permite imponer políticas de contraseñas como longitud mínima, complejidad y reutilización.

#### Ubicación de Archivos Clave

- **Archivo de configuración de políticas de contraseñas**: `/etc/security/pwquality.conf`
- **Configuración de PAM para contraseñas**: `/etc/pam.d/system-auth`

### 2.3 Parámetros Clave de Configuración

Al configurar políticas de contraseñas, los siguientes parámetros son esenciales:

- `minlen`: Longitud mínima de la contraseña.
- `dcredit`: Requisito de dígitos (números).
- `ucredit`: Requisito de letras mayúsculas.
- `lcredit`: Requisito de letras minúsculas.
- `ocredit`: Requisito de caracteres especiales.
- `difok`: Número mínimo de caracteres diferentes de la contraseña anterior.
- `remember`: Cantidad de contraseñas antiguas a recordar para evitar reutilización.
- `maxrepeat`: Máximo de caracteres repetidos consecutivos.

### 2.4 Ejemplos de Configuración

#### Establecer Longitud Mínima de Contraseña

Para requerir una longitud mínima de 12 caracteres, edite `/etc/security/pwquality.conf`:

```bash
minlen = 12
```

#### Exigir Complejidad en Contraseñas

Para requerir al menos una letra mayúscula, una minúscula, un dígito y un carácter especial:

```bash
ucredit = -1
lcredit = -1
dcredit = -1
ocredit = -1
```

#### Evitar Reutilización de Contraseñas

Para recordar las últimas 5 contraseñas:

Edite `/etc/pam.d/system-auth` y agregue en la sección `password`:

```bash
password requisite pam_pwhistory.so use_authtok remember=5
```

#### Configurar Caducidad de Contraseñas

Para establecer que las contraseñas caduquen cada 90 días, use:

```bash
chage -M 90 usuario
```

Para establecerlo globalmente, edite `/etc/login.defs`:

```bash
PASS_MAX_DAYS 90
```

### 2.5 Aplicación y Verificación de Políticas

Después de configurar las políticas, pruebe cambiando la contraseña de un usuario:

```bash
passwd usuario
```

Intente establecer una contraseña que no cumpla con las políticas para verificar que el sistema la rechaza.

## Sistema de Permisos en Linux

### 3.1 Fundamentos de Propietario, Grupo y Otros

Cada archivo y directorio en Linux tiene asociado un propietario, un grupo y permisos asignados a tres categorías:

- **Propietario (User)**: El usuario que posee el archivo.
- **Grupo (Group)**: Un grupo de usuarios con permisos comunes.
- **Otros (Others)**: Cualquier otro usuario no incluido en las categorías anteriores.

### 3.2 Permisos Básicos

- **Lectura (r)**: Permite leer el contenido.
- **Escritura (w)**: Permite modificar el contenido.
- **Ejecución (x)**: Permite ejecutar archivos o acceder a directorios.

### 3.3 Visualización de Permisos

Use `ls -l` para listar archivos con detalles:

```bash
ls -l
```

Salida de ejemplo:

```bash
-rw-r--r-- 1 juan desarrollo 1024 Oct 10 12:00 documento.txt
```

#### Interpretación:

- `-rw-r--r--`: Permisos del archivo.
  - `-`: Tipo de archivo (archivo regular).
  - `rw-`: Propietario puede leer y escribir.
  - `r--`: Grupo puede leer.
  - `r--`: Otros pueden leer.

### 3.4 Cambio de Propietario de Fichero

#### Uso del Comando `chown`

Sintaxis:

```bash
chown [opciones] propietario[:grupo] archivo
```

#### Ejemplos:

- Cambiar propietario:

  ```bash
  chown maria documento.txt
  ```

- Cambiar propietario y grupo:

  ```bash
  chown maria:ventas documento.txt
  ```

- Cambiar solo grupo:

  ```bash
  chown :ventas documento.txt
  ```

### 3.5 Cambio de Permisos

#### Uso del Comando `chmod`

Sintaxis:

```bash
chmod [opciones] modo archivo
```

#### Modos de Permisos

- **Simbólico**: Usa letras y signos.

  - Añadir permiso de ejecución al propietario:

    ```bash
    chmod u+x script.sh
    ```

  - Quitar permiso de lectura al grupo:

    ```bash
    chmod g-r documento.txt
    ```

- **Numérico (Octal)**: Usa números para representar permisos.

  - Valores:
    - Lectura (r): 4
    - Escritura (w): 2
    - Ejecución (x): 1

#### Ejemplos:

- Permisos completos al propietario, lectura y ejecución al grupo, sin permisos a otros:

  ```bash
  chmod 750 programa.bin
  ```

- Permisos de lectura y escritura al propietario y grupo, solo lectura a otros:

  ```bash
  chmod 664 informe.pdf
  ```

### 3.6 Utilidad y Gestión de Grupos

#### Creación de Grupos

Crear un nuevo grupo:

```bash
groupadd desarrolladores
```

#### Modificación de Grupos

Cambiar el nombre de un grupo:

```bash
groupmod -n nuevos_desarrolladores desarrolladores
```

#### Eliminación de Grupos

Eliminar un grupo existente:

```bash
groupdel antiguos_desarrolladores
```

#### Añadir Usuarios a Grupos

Agregar un usuario a un grupo secundario:

```bash
usermod -aG desarrolladores juan
```

> **Nota**: El parámetro `-a` es crucial para no eliminar al usuario de otros grupos secundarios.

#### Ver Grupos de un Usuario

Mostrar los grupos a los que pertenece un usuario:

```bash
groups juan
```

## Conclusión

La configuración adecuada de políticas de contraseñas y el entendimiento del sistema de permisos en Linux son fundamentales para mantener la seguridad y el control de acceso en AlmaLinux 9. A través de PAM y la gestión cuidadosa de propietarios, grupos y permisos, los administradores pueden proteger eficazmente los recursos del sistema y garantizar que solo los usuarios autorizados tengan acceso a información y funcionalidades sensibles.

---

**Referencias Adicionales:**

- Manual de PAM: `man pam`
- Manual de chown: `man chown`
- Manual de chmod: `man chmod`
- Manual de groupadd, groupmod, groupdel: `man groupadd`, `man groupmod`, `man groupdel`
- Documentación oficial de AlmaLinux: [https://almalinux.org](https://almalinux.org)