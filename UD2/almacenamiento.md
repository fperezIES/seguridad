# Almacenamiento de la Información

La información es uno de los activos más valiosos para cualquier empresa, por lo que su seguridad es primordial. En este tema nos centraremos en cómo se almacena físicamente la información y cómo protegerla adecuadamente.

Los cuatro aspectos más importantes que debemos tener en cuenta cuando tratamos la seguridad física de la información son:

* **Capacidad:** Cantidad de información que pueden almacenar los dispositivos. Es esencial para planificar el almacenamiento a corto y largo plazo.

* **Rendimiento:** Velocidad a la que puede ser guardada y recuperada la información. Depende de la tecnología utilizada en el dispositivo de almacenamiento y afecta directamente a la eficiencia operativa.

* **Disponibilidad:** Capacidad de los sistemas para funcionar con las menores interrupciones posibles. Los sistemas tolerantes a fallos y redundantes aumentan este aspecto, asegurando que la información esté accesible cuando se necesite.

* **Acceso a la información:** Tiempo necesario para acceder a los datos. Por ejemplo, no es lo mismo una unidad que permite la lectura aleatoria de datos que otra que solo permite la lectura secuencial.

Existen numerosas tecnologías y técnicas que nos pueden ayudar a optimizar estos aspectos, como los sistemas RAID y las arquitecturas SAN y NAS.

## Tipos de Almacenamiento

* **Unidades físicas de almacenamiento de información:**
    * Discos Duros (HDD)
    * Unidades de Estado Sólido (SSD)
    * Cintas Magnéticas

* **Configuraciones RAID:**
    * Combinaciones de discos que permiten aumentar el rendimiento, la disponibilidad o ambas.

* **Almacenamiento en Red (centralización del almacenamiento):**
    * NAS (Network Attached Storage)
    * SAN (Storage Area Network)

* **Almacenamiento en la Nube:**
    * Deslocalización del almacenamiento que ofrece escalabilidad y accesibilidad remota.

## Unidades de Almacenamiento

### Características Principales

* **Capacidad:** Medida en bytes y sus múltiplos, determina cuánto puede almacenar la unidad.
* **Velocidad:** Incluye tanto la velocidad de lectura/escritura secuencial como aleatoria.
* **Precio:** Factor importante en la planificación de recursos y presupuesto.
* **Fiabilidad:**
    * **MTBF (Mean Time Between Failures):** Tiempo medio entre fallos.
    * **AFR (Annualized Failure Rate):** Tasa anualizada de fallos.

### Tipos de Almacenamiento Físico

* **Discos Duros (HDD):** Utilizan platos magnéticos y cabezales móviles.
* **Unidades de Estado Sólido (SSD):** Basadas en memoria flash, sin partes móviles.
* **Cintas Magnéticas:** Utilizadas principalmente para copias de seguridad y archivado a largo plazo.

### Capacidad: Múltiplos Binarios

Para medir la capacidad de las unidades de almacenamiento se utilizan múltiplos binarios. Es importante distinguir entre los prefijos binarios y decimales:
Se pueden consultar en [Múltiplos binarios](https://es.wikipedia.org/wiki/Prefijo_binario)

| Múltiplos de [bytes](https://es.wikipedia.org/wiki/Byte "Byte")                                                                        |                                                                                                             |                                                                                           |         |
| -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------- |
| [Sistema Internacional (decimal)](https://es.wikipedia.org/wiki/Sistema_Internacional_de_Unidades "Sistema Internacional de Unidades") |                                                                                                             | [ISO/IEC 80000-13 (binario)](https://es.wikipedia.org/wiki/ISO/IEC_80000 "ISO/IEC 80000") |         |
| Múltiplo (símbolo)                                                                                                                     | [SI](https://es.wikipedia.org/wiki/Prefijos_del_Sistema_Internacional "Prefijos del Sistema Internacional") | Múltiplo (símbolo)                                                                        | ISO/IEC |
| _[kilobyte](https://es.wikipedia.org/wiki/Kilobyte "Kilobyte")_ (kB)                                                                   | 103                                                                                                         | _[kibibyte](https://es.wikipedia.org/wiki/Kibibyte "Kibibyte")_ (KiB)                     | 210     |
| _[megabyte](https://es.wikipedia.org/wiki/Megabyte "Megabyte")_ (MB)                                                                   | 106                                                                                                         | _[mebibyte](https://es.wikipedia.org/wiki/Mebibyte "Mebibyte")_ (MiB)                     | 220     |
| _[gigabyte](https://es.wikipedia.org/wiki/Gigabyte "Gigabyte")_ (GB)                                                                   | 109                                                                                                         | _[gibibyte](https://es.wikipedia.org/wiki/Gibibyte "Gibibyte")_ (GiB)                     | 230     |
| _[terabyte](https://es.wikipedia.org/wiki/Terabyte "Terabyte")_ (TB)                                                                   | 1012                                                                                                        | _[tebibyte](https://es.wikipedia.org/wiki/Tebibyte "Tebibyte")_ (TiB)                     | 240     |
| _[petabyte](https://es.wikipedia.org/wiki/Petabyte "Petabyte")_ (PB)                                                                   | 1015                                                                                                        | _[pebibyte](https://es.wikipedia.org/wiki/Pebibyte "Pebibyte")_ (PiB)                     | 250     |
| _[exabyte](https://es.wikipedia.org/wiki/Exabyte "Exabyte")_ (EB)                                                                      | 1018                                                                                                        | _[exbibyte](https://es.wikipedia.org/wiki/Exbibyte "Exbibyte")_ (EiB)                     | 260     |
| _[zettabyte](https://es.wikipedia.org/wiki/Zettabyte "Zettabyte")_ (ZB)                                                                | 1021                                                                                                        | _[zebibyte](https://es.wikipedia.org/wiki/Zebibyte "Zebibyte")_ (ZiB)                     | 270     |
| _[yottabyte](https://es.wikipedia.org/wiki/Yottabyte "Yottabyte")_ (YB)                                                                | 1024                                                                                                        | _[yobibyte](https://es.wikipedia.org/wiki/Yobibyte "Yobibyte")_ (YiB)                     | 280     |

Los fabricantes suelen indicar las capacidades de las unidades usando los múltiplos decimales (base 10) para que los números parezcan más grandes, mientras que los sistemas operativos y aplicaciones utilizan los múltiplos binarios (base 2), lo que puede generar confusiones al interpretar el espacio disponible.

### Fiabilidad

La fiabilidad es un aspecto crítico en el almacenamiento de información. Las dos medidas más comunes son:

#### MTBF (Mean Time Between Failures)

Es el tiempo medio que transcurre entre fallos en horas de servicio. Sin embargo, es una medida estadística basada en poblaciones grandes de dispositivos y no garantiza que un disco individual cumpla ese tiempo sin fallos.

#### AFR (Annualized Failure Rate)

Es la tasa anualizada de fallos, que estima el porcentaje de unidades que fallarán en un año. Al igual que el MTBF, es una medida estadística y se utiliza para evaluar la fiabilidad esperada de un lote de dispositivos.

### Velocidad

La velocidad de una unidad de almacenamiento afecta directamente al rendimiento del sistema. Los factores que influyen en la velocidad incluyen:

* **Tipo de interfaz:** SATA, SAS (Serial Attached SCSI), NVMe, etc.
* **RPM en HDD:** Revoluciones por minuto; mayores RPM suelen indicar mayor velocidad en discos duros.
* **Tecnología de memoria en SSD:** NAND Flash, SLC, MLC, TLC, QLC.

### Discos Duros (HDD)

Los discos duros tradicionales utilizan platos magnéticos que giran a altas velocidades y cabezales de lectura/escritura móviles. Son más económicos en términos de coste por gigabyte, pero tienen velocidades de acceso más lentas y son más susceptibles a daños físicos debido a sus partes móviles.

### Unidades de Estado Sólido (SSD)

Las SSD utilizan memoria flash para almacenar datos, lo que permite velocidades de lectura y escritura mucho más rápidas y menores tiempos de acceso. Al no tener partes móviles, son más resistentes a impactos y vibraciones, pero suelen tener un coste por gigabyte más alto y una vida útil limitada en términos de ciclos de escritura.

### Cintas Magnéticas

Las cintas magnéticas se utilizan principalmente para copias de seguridad y almacenamiento a largo plazo. Ofrecen una alta capacidad de almacenamiento a un coste relativamente bajo, pero tienen tiempos de acceso más lentos y suelen utilizarse para operaciones de lectura/escritura secuenciales.

### Estadísticas SMART

La tecnología SMART (Self-Monitoring, Analysis and Reporting Technology) permite la auto-monitorización de los dispositivos de almacenamiento para alertar al usuario sobre posibles fallos inminentes.

* **Monitoreo de Atributos:** Rastrea varios parámetros como errores de lectura, sectores reasignados, tiempo de arranque, temperatura, entre otros.
* **Limitaciones:** No es infalible; algunos fallos pueden ocurrir sin previo aviso.
* **Acción Preventiva:** Si se recibe una alerta SMART, es recomendable hacer una copia de seguridad inmediata y considerar el reemplazo del dispositivo.

Según un estudio de fiabilidad de discos duros publicado por Google ([Failure Trends in a Large Disk Drive Population](https://research.google/pubs/pub32774/)), el 36% de los discos que fallaron no mostraron ningún error SMART previo, lo que indica que, aunque útil, no es una garantía absoluta de detección de fallos.

## Almacenamiento Redundante y Distribuido (RAID)

Un **RAID** (Redundant Array of Independent Disks) es un sistema que utiliza múltiples discos duros o SSD para mejorar el rendimiento y/o la tolerancia a fallos mediante la distribución y/o replicación de datos.

Los sistemas RAID pueden ser implementados por **hardware** (controladoras RAID dedicadas) o por **software** (gestión del RAID por el sistema operativo). La implementación por hardware suele ofrecer mejor rendimiento y funcionalidades avanzadas como intercambio de discos en caliente (hot swapping).

Además, en entornos empresariales es común utilizar discos **hot spare**, que son discos de reserva que se activan automáticamente en caso de fallo de uno de los discos activos, sin necesidad de intervención manual.

### Ventajas del RAID

* **Mayor Capacidad:** Combinación de múltiples discos para aumentar el espacio de almacenamiento disponible.
* **Mayor Tolerancia a Fallos:** Redundancia de datos que permite continuar operando incluso si uno o varios discos fallan.
* **Mayor Seguridad:** Protección de datos mediante redundancia, aumentando la disponibilidad y la integridad.
* **Mayor Velocidad:** Distribución de datos que permite realizar operaciones de lectura/escritura en paralelo.

### Niveles de RAID Comunes

#### RAID 0 (Striping)

* **Características:**
    * Datos divididos equitativamente entre dos o más discos sin redundancia.
    * Mejora significativa en el rendimiento de lectura y escritura.
    * **No tolerante a fallos:** Si un disco falla, se pierde toda la información.
* **Uso Recomendado:** Situaciones donde el rendimiento es crítico y la pérdida de datos es aceptable o se cuenta con copias de seguridad frecuentes.

![RAID 0](img/02/Raid0.png){:class="center"}

#### RAID 1 (Mirroring)

* **Características:**
    * Copia exacta de datos en dos o más discos.
    * **Alta tolerancia a fallos:** Si un disco falla, el sistema continúa operando con el disco espejo.
    * Reducción del espacio de almacenamiento efectivo al 50%.
* **Uso Recomendado:** 
	* Entornos donde la disponibilidad y la integridad de los datos son críticos.
	* Común en servidores de sistema operativo y aplicaciones que requieren una rápida recuperación en caso de fallo de disco.

![RAID 1](img/02/Raid1.png){:class="center"}

#### RAID 5 (Striping con Paridad Distribuida)

* **Características:**
    * Datos y paridad distribuidos entre tres o más discos.
    * **Tolerante a fallos:** Puede soportar la falla de un disco sin pérdida de datos.
    * Rendimiento de lectura mejorado, pero el rendimiento de escritura puede verse afectado debido al cálculo de paridad.
* **Uso Recomendado:** Aplicaciones de almacenamiento compartido donde se requiere un equilibrio entre rendimiento, capacidad y tolerancia a fallos.

![RAID 5](img/02/Raid5.png){:class="center"}

#### RAID 6 (Striping con Doble Paridad)

* **Características:**
    * Similar al RAID 5 pero con dos bloques de paridad.
    * **Alta tolerancia a fallos:** Puede soportar la falla de hasta dos discos simultáneamente.
    * Requiere al menos cuatro discos.
* **Uso Recomendado:** Entornos donde la alta disponibilidad es crucial y se necesita protección adicional contra fallos múltiples.

![RAID 6](img/02/Raid6.png){:class="center"}

### RAID Anidados

#### RAID 10 (RAID 1+0)

* **Características:**
    * Combina las ventajas del RAID 1 y RAID 0.
    * Se realiza mirroring de los datos y luego striping entre los conjuntos espejados.
    * **Alta tolerancia a fallos** y **alto rendimiento**.
    * Requiere al menos cuatro discos.
* **Uso Recomendado:** Aplicaciones críticas que requieren alta disponibilidad y rendimiento, como bases de datos y servidores de aplicaciones.

![RAID 10](img/02/Raid10.png){:class="center"}

#### RAID 0+1

* **Características:**
    * Se realiza striping de los datos y luego mirroring del conjunto.
    * Ofrece alto rendimiento y tolerancia a fallos, pero es menos robusto que RAID 10 en caso de múltiples fallos.
    * Requiere al menos cuatro discos.
* **Uso Recomendado:** Similar al RAID 10, pero con diferencias en cómo se manejan los fallos múltiples.

![RAID 0+1](img/02/Raid01.png){:class="center"}

## Almacenamiento en Red

El almacenamiento en red permite centralizar la gestión y el acceso a los datos, facilitando la escalabilidad y la administración.

### NAS (Network Attached Storage)

* **Características:**
    * Dispositivos de almacenamiento conectados directamente a la red.
    * Proporcionan servicios de archivos a través de protocolos como NFS, SMB/CIFS.
    * Fácil de implementar y administrar.
* **Uso Recomendado:** Pequeñas y medianas empresas que necesitan compartir archivos entre múltiples usuarios y sistemas.

### SAN (Storage Area Network)

* **Características:**
    * Red dedicada de dispositivos de almacenamiento accesible a través de protocolos como Fibre Channel o iSCSI.
    * Permite acceso de nivel de bloque a los dispositivos de almacenamiento.
    * Escalabilidad y rendimiento elevados.
* **Uso Recomendado:** Grandes organizaciones con necesidades de almacenamiento masivas y requerimientos de alta velocidad y baja latencia.

### Convergencia SAN y NAS

La convergencia entre SAN y NAS ha sido posible gracias al uso de tecnologías basadas en IP y Ethernet. Esto ha permitido que dispositivos SAN ofrezcan funcionalidades típicas de NAS y viceversa, facilitando la gestión unificada y la flexibilidad en entornos de almacenamiento.

## Almacenamiento en la Nube

El almacenamiento en la nube ofrece soluciones de almacenamiento deslocalizadas, accesibles a través de internet, que permiten escalabilidad bajo demanda y modelos de pago por uso.

* **Ventajas:**
    * Escalabilidad y flexibilidad.
    * Reducción de costes en infraestructura.
    * Accesibilidad global.
* **Desafíos:**
    * Seguridad y privacidad de los datos.
    * Dependencia de la conectividad a internet.
    * Cumplimiento de regulaciones y normativas.

## Borrado y Recuperación de Datos

### Borrado de Datos

Cuando se borra un archivo en un sistema operativo, normalmente solo se elimina el puntero al archivo, marcando los sectores como disponibles. Los datos permanecen en el disco hasta que se sobrescriben, lo que permite su recuperación con herramientas especializadas.

Para eliminar información confidencial o sensible, es necesario utilizar métodos de borrado seguro que sobrescriban los datos originales, impidiendo su recuperación.

#### Métodos de Borrado Seguro

* **Sobrescritura Múltiple:** Escribir datos aleatorios sobre el espacio del archivo varias veces.
    * **Método de Gutmann:** Sobrescribe el área con 35 patrones diferentes. Es considerado uno de los métodos más seguros pero también el más lento.
    * **Estándar DoD 5220.22-M:** Estándar del Departamento de Defensa de EE.UU. que especifica sobrescrituras múltiples con diferentes patrones.
* **Herramientas de Software:**
    * **Eraser:** Software libre que implementa varios métodos de borrado seguro.
    * **SDelete:** Herramienta de Sysinternals que cumple con estándares de borrado seguro.
    * **Comando `dd` en Linux:** Puede usarse para sobrescribir un disco completo con ceros o datos aleatorios.
    * **`secure-delete` en Linux:** Paquete que incluye utilidades para borrar archivos, espacio libre, memoria RAM y swap de forma segura.
* **Destrucción Física:** Métodos como triturar, desmagnetizar o incinerar los discos, utilizados cuando el borrado lógico no es suficiente.

### Importancia del Borrado Seguro

El borrado seguro es una medida de seguridad activa que previene el acceso no autorizado a información confidencial. Es especialmente relevante al desechar dispositivos de almacenamiento o al reutilizar medios en entornos con información sensible.

### Recuperación de Datos

A pesar de los esfuerzos de borrado, existen técnicas avanzadas de recuperación de datos que pueden extraer información residual. Por ello, en casos donde la seguridad es crítica, se recomiendan métodos de destrucción física complementarios.
