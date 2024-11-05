---
title: Auditoría de seguridad de servidores Linux
---

<!-- Tomada de curso seguridad del CEFIRE 2020 -->

# Introducción

Linux es considerado un sistema operativo más seguro que Windows. Esto es así principalmente porque existen menos malware creados para atacar las múltiples distribuciones que hay. Sin embargo eso no significa que no haya problemas que afecten a la privacidad, seguridad o provoquen errores con las aplicaciones instaladas.

Lynis es una auditoría muy completa que podemos utilizar en Linux. Está disponible para distribuciones tan populares como Ubuntu, Fedora, Debian y OpenSUSE. Lo que hace básicamente es analizar todo el software instalado en nuestro equipo y busca posibles problemas que debamos corregir. De esta forma sabremos si tenemos que realizar cambios, instalar actualizaciones, etc.


![Logo CISOFY](img/lynis/lynis.png){width=60%}


Lynis es una herramienta muy interesante para ayudarnos a auditar y fortificar nuestro servidor. Es muy sencilla de usar y una vez descargada, tan solo debemos ejecutarla para que Lynis analice el software instalado en busca de problemas de configuración o de seguridad, haciéndonos recomendaciones. Se distribuye bajo licencia GPL v3, por lo que cualquiera puede utilizarlo para analizar su distribución sin ningún coste.

Cuando se está ejecutando, nos muestra por pantalla información acerca de nuestro sistema en aspectos como:

* Resumen del sistema y actualizaciones pendientes
* Gestor de arranque
* Servicios que se inician con el sistema
* Memoria y procesos
* Usuarios, grupos y sistemas de autenticación disponibles
* Shells del sistema
* Sistemas de archivos y almacenamiento
* Servicio de nombres
* Gestores de paquetes
* Impresoras
* Servidores de correo electrónico
* Estado del firewall
* Servicios instalados (web, ssh, snmp, etc.)
* Servicios de loging como syslog
* Tareas programadas (cron y at)
* Presencia de SELinux o AppArmor
* Sistemas de integridad de archivos instalados
* Escaneadores de malware y de rootkits instalados

Finalmente se nos presenta un completo informe de todos los pasos realizados con el fin de que tomemos las medidas necesarias para endurecer la seguridad de nuestro servidor. Es también capaz de encontrar vulnerabilidades y configuraciones por defecto y para ello emplea colores llamativos, para llamar nuestra atención.

# Enunciado

Analiza la seguridad de un servidor Linux en el que esté habilitado como mínimo un servidor SSH y un servidor Apache ulitilzando la herramienta Lynis.

**Revisa la bibliografía antes de empezar.**

Debes crear una memoria para la práctica que contenga:

1. Cómo has instalado Lynis 
3. Cómo has ejecutado Lynis
3. El informe generado por Lynis
4. Un análisis de del informe obtenido en el que expliques, bajo tu punto de vista, los problemas más importantes que se han encontrado.


# Bibliografía

* https://cisofy.com/documentation/lynis/get-started/
- [Tutorial Lynis  Almalinux](https://www.techrepublic.com/article/how-to-run-a-security-audit-on-almalinux-with-lynis/)

* https://packages.cisofy.com/community/#debian-ubuntu
* https://www.redeszone.net/tutoriales/seguridad/lynis-auditoria-seguridad-linux/



