# Práctica: Configuración de una Autoridad Certificadora con OpenSSL y Servidor Web Seguro 

---

**Objetivo:**
Aprender a configurar una Infraestructura de Clave Pública (PKI) usando OpenSSL, incluyendo la creación de una Autoridad Certificadora (CA), emisión de certificados digitales y configuración de un servidor web Apache con soporte HTTPS utilizando los certificados generados.

---

**Requisitos Previos:**

- Sistema operativo AlmaLinux instalado.
- Acceso con privilegios de superusuario (root) o mediante `sudo`.
- Conexión a Internet.

---

### **Pasos a Seguir:**

### 1. Instalación de entorno gráfico

Hasta ahora no hemos usado entorno gráfico en Alma linux para instalarlo:

Comprobamos los "package groups" que hay disponibles en AlmaLinux:

```
dnf group list
```


Veremos que hay un grupo disponible llamado `Server with GUI.Para instalarlo:

```
dnf groupinstall "Server with GUI"
```


Podremos iniciar el modo gráfico con el comando:
```
startx
```


Podemos cambiar el modo de arranque a modo gráfico con el comando:
```
systemctl set-default graphical
```

En el próximo reinicio el sistema arrancará en modo gráfico directemente.

#### Guest Additions (Opcional)

Primero actualizamos e instalamos dependencias necesarias:

```shell
sudo dnf update -y
sudo dnf install kernel-headers kernel-devel gcc cpp perl make elfutils-libelf-devel
```

Insertamos la ISO en la unidad virttual: 

`Dispositivos->Insertar imagen CD de los complementos de invitado`

Nos preguntará si queremos ejecutar el CD, le diremos que sí. Si no se ejecutara automáticamente ejecutaríamos el siguiente comando:
`VBoxLinuxAdditions.run`

Una vez terminado, reinicia.

#### **2. Actualizar el Sistema**

```bash
sudo dnf update -y
```

#### **3. Instalar OpenSSL y Apache**

```bash
sudo dnf install -y openssl httpd mod_ssl
```

#### **4. Configurar el Firewall**

Permitir el tráfico HTTP y HTTPS:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

#### **5. Iniciar y Habilitar el Servicio Apache**

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Deberías poder ver la página web de prueba en:

```
http://localhost
https://localhost
```



En lugar de contratar y configurar un dominio en nuestros servidores DNS, simplemente añadiremos un nombre el fichero `hosts`

```
sudo nano /etc/hosts
```

Añade un nombre para tu host que siga este patrón: www.servidorincialapellido.com, debes cambiar apellido por tu propio apellido:

En mi caso queda así:

```
127.0.0.1   clienteperez www.servidorfperez.com clienteperez.localdomain cliente>
::1         clienteperez clienteperez.localdomain clienteperez6 clienteperez6.l>
```

Si lo has hecho bien podrás cargar la página de prueba en un navegador poniendo la siguiente url (tendrás que cambiarla para que coincida con tu inicial y tu apellido) en la barra de direcciones (solo funcionará desde el propio servidor):

```
http://www.servidorfperez.com
```


Muestra una captura del contenido de tu fichero `/etc/hosts`y de un navegador en el servidor intentando cargar `http://www.servidorfperez.com`

#### **6. Crear Directorios para la CA**

```bash
 mkdir -p ca/{certs,newcerts,private,csr}
```



#### **7. Generar la Clave Privada de la CA**

```bash
openssl genrsa -aes256 \
      -out private/rootCA.key.pem \
      -passout pass:ca_12345 4096

chmod 400 private/rootCA.key.pem

```

#### **8. Crear el Certificado Raíz de la CA**

```bash
openssl req -new -x509 \
      -days 3650 -sha256 \
      -key private/rootCA.key.pem \
      -passin pass:ca_12345 -out certs/rootCA.crt.pem
      
chmod 444 certs/rootCA.crt.pem
```


Proporciona la información solicitada (país, estado, organización, etc.) cuando se te pida. 
> Recuerda la organización introducida, toma una captura de la consola después de crear el certificado

#### **9. Generar la Clave Privada para el Servidor Web**

```bash
openssl genrsa -aes256 \
      -out private/serverCert.key.pem \
      -passout pass:server_12345 2048

chmod 400 private/serverCert.key.pem
```


#### **10. Crear una Solicitud de Firma de Certificado (CSR) para el Servidor Web**

Primero vamos a crear un archivo con los datos que queremos que contenga nuestro certificado, es especialmente importante `commonName` que será el dominio principal que validará el certificado, podemos añadir nombres adicionales en la sección de `list_of_alternative_names`.

```
cat > serverCert.conf <<EOF
[ req ]
prompt = no
distinguished_name = requested_distinguished_name
req_extensions = requested_extensions
x509_extensions = requested_extensions

[ requested_distinguished_name ]
countryName = ES
stateOrProvinceName = Alicante
localityName = San Vicente
organizationName = IES San Vicente
organizationalUnitName = ASIR
commonName = www.servidorfperez.com
emailAddress = fperez@iessanvicente.com

[ requested_extensions ]
basicConstraints = CA:FALSE
subjectAltName = @list_of_alternative_names

[ list_of_alternative_names ]
DNS.1 = www.servidorfperez.com
DNS.2 = servidorfperez.com
DNS.3 = 127.0.0.1
DNS.4 = clienteperez

EOF
```

Finalmente,  procedemos a generar la petición CSR:

```bash
openssl req -new -sha256 \
      -config serverCert.conf \
      -key private/serverCert.key.pem -passin pass:server_12345 \
      -out certs/serverCert.csr.pem
```

> Muestra el contenido de tu fichero `serverCert.conf`

#### **11. Firmar el Certificado del Servidor con la CA**

```bash
openssl x509 -req -days 750 -sha256 -in certs/serverCert.csr.pem \
    -CA certs/rootCA.crt.pem -CAkey private/rootCA.key.pem -CAcreateserial \
    -out certs/serverCert.crt.pem -passin pass:ca_12345 \
    -extensions requested_extensions -extfile serverCert.conf

chmod 444 certs/serverCert.crt.pem
```


### 12. Copia de Certificados al Directorio Adecuado

Copia el certificado y la clave privada a los directorios correspondientes en AlmaLinux:

```sh
sudo cp certs/serverCert.crt.pem /etc/pki/tls/certs/
sudo cp private/serverCert.key.pem /etc/pki/tls/private/
```

Asegúrate de establecer los permisos y propietarios correctos para la clave privada:

```sh
sudo chown root:root /etc/pki/tls/private/serverCert.key.pem
sudo chmod 400 /etc/pki/tls/private/serverCert.key.pem
```


#### **13. Configurar Apache para Usar el Certificado SSL**

Editar el archivo de configuración SSL de Apache:

```bash
sudo nano /etc/httpd/conf.d/ssl.conf
```

Modificar las siguientes líneas para apuntar a los certificados generados:

```apache
SSLCertificateFile /etc/pki/tls/certs/serverCert.crt.pem
SSLCertificateKeyFile /etc/pki/tls/private/serverCert.key.pem
SSLCertificateChainFile /etc/pki/tls/certs/serverCert.crt.pem
```

Asegúrate de que el módulo SSL está cargado y habilitado.

#### **14. Reiniciar el Servicio Apache**

```bash
sudo systemctl restart httpd
```

Al reiniciar el servicio **te pedirá la contraseña** simétrica que protege la clave privada, se indicó al generar la clave.


Es posible eliminar la contraseña de la clave privada para que el funcionamiento del servicio no dependa de la interacción humana:

1. Quitamos el cifrado de la clave (Pero guardamos una copia del fichero original):  
```bash
    sudo cp serverCert.key.pem serverCert.key.pem.old
    sudo openssl rsa -in serverCert.key.old -out serverCert.key.pem
```      

2. Nos aseguramos de que la clave sólo sea legible por root:  

```bash
	chmod 400 server.key
```


#### **14. Probar la Configuración**

- En un navegador web, visita `https://www.serverfperez.com` (reemplaza con tu dominio o dirección IP). 
- Deberías ver una advertencia de seguridad debido a que el navegador no confía en la CA personalizada.
- Para eliminar la advertencia, instala el certificado raíz `ca.cert.pem` en el almacén de certificados de confianza del navegador. En firefox: `Settings->Certificates` pulsar sobre el botón `View Certificates...`y en la ventana emergente usar el botón `import`para añadir nuestro certificado raíz.

**Muestra una captura en que se vea tu certificado importado en la configuración del navegador, aparecerá listando bajo el nombre de la organización que hayas puesto al crear el certificado raíz.**

<!--
#### **15. (Opcional) Revocar un Certificado y Actualizar la Lista de Revocación (CRL)**

- **Revocar el Certificado:**

  ```bash
  sudo openssl ca -config openssl.cnf -revoke certs/www.serverCert.crt.pem
  ```

- **Generar la CRL:**

  ```bash
  sudo openssl ca -config openssl.cnf -gencrl -out crl/ca.crl.pem
  ```

- **Actualizar la Configuración de Apache para Incluir la CRL:**

  Editar `/etc/httpd/conf.d/ssl.conf` y agregar:

  ```apache
  SSLCARevocationFile /root/ca/crl/ca.crl.pem
  ```

- **Reiniciar Apache:**

  ```bash
  sudo systemctl restart httpd
  ```

-->

---

**Notas Adicionales:**

-  **Seguridad de las Claves Privadas:** Es crucial proteger las claves privadas, estableciendo permisos adecuados y restringiendo el acceso solo a usuarios autorizados.
- **Entornos de Producción:** En entornos reales, los certificados deben ser emitidos por una CA reconocida públicamente para que los navegadores los consideren confiables sin necesidad de importarlos manualmente.
- **Actualización de Paquetes y Dependencias:** Siempre verifica que los paquetes y dependencias estén actualizados para garantizar la compatibilidad y seguridad del sistema.
- **Políticas de Certificados:** Si usas comodines en certificados SSL/TLS (EJ: `*.servidorfperez.com`), asegúrate de cumplir con las políticas y estándares establecidos para evitar problemas de seguridad.

# Bibliografía

[OpenSSL CA](https://dev.to/pauladj/create-self-signed-certificates-with-openssl-jpa)
[Configuración SSL Apache](https://httpd.apache.org/docs/2.4/ssl/ssl_faq.html)
[DigiCert tutoria CSR y configuración Apache ](https://www.digicert.com/kb/csr-ssl-installation/apache-openssl.htm)
[Tutorial Certificado OpenSSl](https://medium.com/@tbusser/creating-a-browser-trusted-self-signed-ssl-certificate-2709ce43fd15)
[Crear tu CA con OpenSSL](https://arminreiter.com/2022/01/create-your-own-certificate-authority-ca-using-openssl/)
