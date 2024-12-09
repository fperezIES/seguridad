
### Cambiar nombre a la máquina

 Para cambiar el nombre de una máquina en Linux, sigue estos pasos:

1. Cambia el nombre de la máquina con el siguiente comando:

   ```sh
   sudo hostnamectl set-hostname nuevo_nombre
   ```

2. Edita el archivo `/etc/hosts` para reemplazar el viejo nombre por el nuevo:

   ```sh
   sudo nano /etc/hosts
   ```

3. Comprueba el cambio de nombre con el siguiente comando:

   ```sh
   hostname
   ```

### Abrir un puerto en Firewall

```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent

sudo firewall-cmd --reload
```
sudo

### Instalar Apache

Actualiza el sistema:

```
sudo dnf update
```

Instala apache:

```
sudo dnf install httpd
```

Inicia y habilita el servicio: 

```
sudo systemctl start httpd  
sudo systemctl enable httpd
```

Añade los puertos al firewall
```
sudo firewall-cmd --permanent --add-service={http,https}
sudo firewall-cmd --reload
```