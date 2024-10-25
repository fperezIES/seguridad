# Práctica de PKI en AlmaLinux: Configuración de una Autoridad Certificadora y Servidor Web Seguro

---

**Objetivo:**
Aprender a configurar una Infraestructura de Clave Pública (PKI) en AlmaLinux, incluyendo la creación de una Autoridad Certificadora (CA), emisión de certificados digitales y configuración de un servidor web Apache con soporte HTTPS utilizando los certificados generados.

---

**Requisitos Previos:**

- Sistema operativo AlmaLinux instalado.
- Acceso con privilegios de superusuario (root) o mediante `sudo`.
- Conexión a Internet.

---

### **Pasos a Seguir:**

#### **1. Actualizar el Sistema**

```bash
sudo dnf update -y
```

#### **2. Instalar OpenSSL y Apache**

```bash
sudo dnf install -y openssl httpd mod_ssl
```

#### **3. Configurar el Firewall**

Permitir el tráfico HTTP y HTTPS:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

#### **4. Iniciar y Habilitar el Servicio Apache**

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

#### **5. Crear Directorios para la CA**

```bash
sudo mkdir -p /root/ca/{certs,crl,newcerts,private,csr}
sudo chmod 700 /root/ca/private
sudo touch /root/ca/index.txt
echo 1000 | sudo tee /root/ca/serial
```

#### **6. Crear el Archivo de Configuración de OpenSSL**

Copiar el archivo de configuración por defecto:

```bash
sudo cp /etc/pki/tls/openssl.cnf /root/ca/openssl.cnf
```

Editar `/root/ca/openssl.cnf` y modificar las siguientes líneas:

- Establecer `dir = /root/ca` en la sección `[ CA_default ]`.
- Asegurarse de que las rutas de `certificate`, `private_key`, `new_certs_dir`, etc., apuntan al directorio `/root/ca`.

#### **7. Generar la Clave Privada de la CA**

```bash
cd /root/ca
sudo openssl genrsa -aes256 -out private/ca.key.pem 4096
sudo chmod 400 private/ca.key.pem
```

#### **8. Crear el Certificado Raíz de la CA**

```bash
sudo openssl req -config openssl.cnf \
  -key private/ca.key.pem \
  -new -x509 -days 3650 -sha256 -extensions v3_ca \
  -out certs/ca.cert.pem
sudo chmod 444 certs/ca.cert.pem
```

Proporciona la información solicitada (país, estado, organización, etc.) cuando se te pida.

#### **9. Generar la Clave Privada para el Servidor Web**

```bash
sudo openssl genrsa -out private/www.example.com.key.pem 2048
sudo chmod 400 private/www.example.com.key.pem
```

#### **10. Crear una Solicitud de Firma de Certificado (CSR) para el Servidor Web**

```bash
sudo openssl req -config openssl.cnf \
  -key private/www.example.com.key.pem \
  -new -sha256 -out csr/www.example.com.csr.pem
```

Al ingresar la información, asegúrate de que el "Common Name" (CN) coincide con el nombre de dominio o dirección IP del servidor.

#### **11. Firmar el Certificado del Servidor con la CA**

```bash
sudo openssl ca -config openssl.cnf \
  -extensions server_cert -days 750 -notext -md sha256 \
  -in csr/www.example.com.csr.pem \
  -out certs/www.example.com.cert.pem
sudo chmod 444 certs/www.example.com.cert.pem
```

Responde "y" cuando se te pregunte si deseas firmar el certificado.

#### **12. Configurar Apache para Usar el Certificado SSL**

Editar el archivo de configuración SSL de Apache:

```bash
sudo nano /etc/httpd/conf.d/ssl.conf
```

Modificar las siguientes líneas para apuntar a los certificados generados:

```apache
SSLCertificateFile /root/ca/certs/www.example.com.cert.pem
SSLCertificateKeyFile /root/ca/private/www.example.com.key.pem
SSLCertificateChainFile /root/ca/certs/ca.cert.pem
```

Asegúrate de que el módulo SSL está cargado y habilitado.

#### **13. Reiniciar el Servicio Apache**

```bash
sudo systemctl restart httpd
```

#### **14. Probar la Configuración**

- En un navegador web, visita `https://www.example.com` (reemplaza con tu dominio o dirección IP).
- Deberías ver una advertencia de seguridad debido a que el navegador no confía en la CA personalizada.
- Para eliminar la advertencia, instala el certificado raíz `ca.cert.pem` en el almacén de certificados de confianza del navegador.

#### **15. (Opcional) Revocar un Certificado y Actualizar la Lista de Revocación (CRL)**

- **Revocar el Certificado:**

  ```bash
  sudo openssl ca -config openssl.cnf -revoke certs/www.example.com.cert.pem
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


---

**Notas Adicionales:**

- **Seguridad:** En un entorno de producción, las claves privadas y certificados deben manejarse con extremo cuidado y almacenarse de forma segura.
- **Certificados de Autoridades Reconocidas:** Para evitar advertencias en los navegadores de los usuarios, se deben obtener certificados firmados por una CA reconocida públicamente.