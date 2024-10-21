
# **Práctica: Descarga y verificación de ISO utilizando huella hash**

### **Objetivos**

- Verificar la integridad del archivo mediante la comparación de la huella hash (checksum) usando herramientas disponibles en Windows y Linux.

### **Parte 1: Descargar la ISO de AlmaLinux**

1. Accede al siguiente enlace desde tu navegador:  
    [AlmaLinux ISO](https://almalinux.org/get-almalinux/)
    
2. Descarga la ISO que mejor se ajuste a tus necesidades, por ejemplo, la versión más reciente de AlmaLinux.
    
3. Durante la descarga, también debes descargar el archivo de checksum asociado. En el mismo sitio web, busca la opción para obtener los checksums ("Download CHECKSUM").     

---

### **Parte 2: Verificación de la huella hash en Windows**

1. **Acceder a la terminal de comandos (PowerShell)**    
    - Haz clic derecho sobre el botón de inicio de Windows y selecciona "Windows PowerShell" o abre la búsqueda y escribe "PowerShell".
2. **Navegar a la carpeta donde se descargó la ISO**
    
    - Utiliza el siguiente comando para moverte a la carpeta de descargas:   
	    ```shell
        cd C:\Usuarios\TuUsuario\Descargas
        ```
3. **Comprobar el hash de la ISO usando SHA256**
    
    - Para obtener el hash de la ISO, usa el siguiente comando:
                        
     ```shell        
        Get-FileHash .\nombre-del-archivo.iso -Algorithm SHA256
        ```
        
        Nota: Reemplaza `nombre-del-archivo.iso` con el nombre real del archivo descargado.
4. **Comparar el hash**
    
    - Compara el valor que has obtenido con el que figura en el archivo `SHA256SUMS` descargado. Si coinciden, la imagen es íntegra y no ha sido alterada.

---

### **Parte 3: Verificación de la huella hash en Linux**

1. **Abrir la terminal**
    
    - En tu distribución Linux, abre una terminal (puedes buscarla como "Terminal" en el menú de aplicaciones).
2. **Navegar a la carpeta donde se descargó la ISO**
    
    - Usa el siguiente comando para moverte a la carpeta de descargas (por defecto suele ser `Descargas`):
        
	 ```bash       
    cd ~/Descargas
    ```
        
3. **Comprobar el hash de la ISO usando SHA256**
    
    - Ejecuta el siguiente comando para calcular la huella hash:
        
    ```bash        
        sha256sum nombre-del-archivo.iso
    ```
        
	Nota: Reemplaza `nombre-del-archivo.iso` con el nombre real del archivo descargado.
4. **Comparar el hash**    
    - Al igual que en Windows, compara el valor que has obtenido con el que figura en el archivo `SHA256SUMS`. Si ambos coinciden, la imagen descargada es correcta y no ha sido modificada.

## Repite el proceso con otros archivos

Repite el proceso, pero ahora descarga una imagen ISO de Debian, presta atención al tipo de hash proporcionado.

Descarga la ISO desde [el sitio web oficial](https://www.debian.org/download) y comprueba su integridad.