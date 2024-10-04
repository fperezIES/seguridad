


**Práctica: Comprobación del estado SMART de las unidades de disco**

---

**Objetivo:**

Aprender a verificar el estado SMART (Self-Monitoring, Analysis, and Reporting Technology) de las unidades de disco para anticipar posibles fallos y garantizar la integridad de los datos.

---

**Introducción:**

SMART es una tecnología incorporada en los discos duros y unidades SSD que permite monitorizar diversos indicadores de fiabilidad y rendimiento. Al analizar estos datos, es posible predecir fallos y tomar medidas preventivas antes de que ocurran pérdidas de datos.

---

**Materiales Necesarios:**

- Un ordenador con sistema operativo Windows o Linux.
- Acceso a internet (opcional, para descargar herramientas).
- Herramientas de software:
  - **Windows:** Símbolo del sistema, PowerShell o programas como *CrystalDiskInfo*.
  - **Linux:** Utilidad `smartctl` del paquete *smartmontools*.

---

**Procedimiento:**

### **Para usuarios de Windows:**

#### **1. Utilizando el Símbolo del sistema:**

a) Abre el **Símbolo del sistema** como administrador:
- Haz clic derecho en el menú Inicio y selecciona "Símbolo del sistema (Administrador)".

b) Ejecuta el siguiente comando:
```
wmic diskdrive get status
```

c) Observa el estado de cada unidad. Un resultado "OK" indica que no se han detectado problemas.

#### **2. Utilizando PowerShell:**

a) Abre **PowerShell** como administrador:
- Haz clic derecho en el menú Inicio y selecciona "Windows PowerShell (Administrador)".

b) Ejecuta el siguiente comando:
```
Get-WmiObject -namespace root\wmi -class MSStorageDriver_FailurePredictStatus
```

c) Revisa el valor de **PredictFailure** para cada unidad:
- **False:** No se predicen fallos.
- **True:** Se predicen posibles fallos; se recomienda hacer copias de seguridad y considerar el reemplazo de la unidad.

#### **3. Utilizando CrystalDiskInfo:**

a) Descarga e instala **CrystalDiskInfo** desde su [sitio oficial](https://crystalmark.info/en/software/crystaldiskinfo/).

b) Ejecuta el programa y observa el estado de salud de cada disco:
- **Buen estado:** Indica que la unidad funciona correctamente.
- **Precaución o Malo:** Señala problemas que requieren atención inmediata.

c) Analiza los detalles de los atributos SMART para identificar parámetros críticos.

---

### **Para usuarios de Linux:**

#### **1. Instalación de smartmontools:**

a) Abre una terminal.

b) Instala el paquete *smartmontools*:
- **Debian/Ubuntu:**
  ```
  sudo apt-get install smartmontools
  ```
- **Fedora/CentOS:**
  ```
  sudo dnf install smartmontools
  ```

#### **2. Identificación de las unidades de disco:**

a) Ejecuta:
```
sudo fdisk -l
```
b) Anota los identificadores de las unidades (ejemplo: `/dev/sda`).

#### **3. Verificación del estado SMART:**

a) Activa el soporte SMART en la unidad (si no lo está):
```
sudo smartctl -s on /dev/sdX
```
*(Reemplaza `/dev/sdX` por el identificador de tu unidad).*

b) Ejecuta un test rápido:
```
sudo smartctl -H /dev/sdX
```

c) Observa el resultado:
- **PASSED:** La unidad está en buen estado.
- **FAILED:** Se han detectado problemas.

#### **4. Obtención de un informe detallado:**

a) Ejecuta:
```
sudo smartctl -a /dev/sdX
```

b) Analiza los atributos SMART para detectar valores anómalos.

---

**Análisis de Resultados:**

- **Interpretación de Atributos SMART Clave:**
  - **Reallocated Sector Count:** Indica sectores reasignados; valores altos pueden ser preocupantes.
  - **Current Pending Sector Count:** Sectores pendientes de reasignación.
  - **Offline Uncorrectable Sector Count:** Sectores que no pueden ser corregidos; pueden provocar errores de lectura/escritura.

- **Acciones Recomendadas:**
  - Si se detectan valores anormales, realizar copias de seguridad inmediatas.
  - Considerar el reemplazo de la unidad afectada.

---

**Conclusiones:**

- La monitorización regular del estado SMART permite anticipar fallos y proteger los datos.
- Herramientas como `smartctl` y *CrystalDiskInfo* facilitan la obtención y análisis de esta información.
- Es fundamental interpretar correctamente los indicadores para tomar decisiones informadas.

---

**Cuestionario:**

1. **¿Qué es SMART y cuál es su función principal en las unidades de disco?**

2. **¿Cuál es la diferencia entre un test rápido y un informe detallado en la comprobación SMART?**

3. **Menciona al menos tres atributos SMART críticos y explica su importancia.**

4. **¿Qué medidas debes tomar si tu unidad de disco muestra fallos en los tests SMART?**

5. **¿Por qué es importante activar el soporte SMART en algunas unidades antes de realizar las pruebas?**

---

**Bibliografía y Recursos Adicionales:**

- **Documentación de smartmontools:** [www.smartmontools.org](https://www.smartmontools.org/)
- **CrystalDiskInfo:** [crystalmark.info](https://crystalmark.info/en/software/crystaldiskinfo/)
- **Información sobre SMART:** [Wikipedia - S.M.A.R.T.](https://es.wikipedia.org/wiki/S.M.A.R.T.)

---

**Nota:** Siempre maneja con cuidado los comandos y herramientas que interactúan a bajo nivel con el hardware para evitar daños o pérdidas de datos.

---



# SMART en Windows

En Windows, una herramienta comúnmente utilizada para ver los datos S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology) de un disco es **CrystalDiskInfo**. Esta aplicación gratuita te permite monitorizar el estado de los discos duros y unidades SSD, proporcionando información detallada sobre los atributos S.M.A.R.T., como la temperatura, el número de sectores reasignados, errores de lectura y escritura, entre otros.

### Pasos para usar CrystalDiskInfo:

1. **Descarga e instalación**: Descarga CrystalDiskInfo desde su sitio oficial.
2. **Ejecución**: Inicia la aplicación tras la instalación.
3. **Visualización de los datos S.M.A.R.T.**: CrystalDiskInfo mostrará una lista de los discos conectados y su estado de salud general (por ejemplo, "Good", "Caution", o "Bad"). También puedes ver los atributos detallados, como la cantidad de horas en funcionamiento, la cantidad de errores, y otros parámetros relacionados con el rendimiento del disco.
# SMART en Linux

En Linux, puedes usar varias herramientas para consultar los datos S.M.A.R.T. de un disco. Una de las más comunes y completas es **smartmontools**, que incluye la utilidad **smartctl** para gestionar y verificar el estado S.M.A.R.T. de los discos.

### Instalación de smartmontools:

La mayoría de las distribuciones de Linux incluyen smartmontools en sus repositorios, por lo que puedes instalarlo fácilmente con los siguientes comandos:

- **Ubuntu/Debian**:
    
    bash
    
    Copiar código
    
    `sudo apt-get install smartmontools`
    
- **Fedora**:
    
    bash
    
    Copiar código
    
    `sudo dnf install smartmontools`
    
- **Arch Linux**:
    
    bash
    
    Copiar código
    
    `sudo pacman -S smartmontools`
    

### Usar smartctl para ver los datos S.M.A.R.T.:

1. **Verificar el estado general del disco**: Primero, debes identificar el nombre del dispositivo (por ejemplo, `/dev/sda`, `/dev/nvme0n1` para discos NVMe, etc.). Luego puedes ejecutar el siguiente comando para obtener un resumen básico del estado S.M.A.R.T.:
    
    bash
    
    Copiar código
    
    `sudo smartctl -H /dev/sda`
    
    Este comando te mostrará si el disco está en buen estado o si presenta problemas.
    
2. **Mostrar todos los atributos S.M.A.R.T.**: Para obtener un informe más detallado, puedes usar:
    
    bash
    
    Copiar código
    
    `sudo smartctl -A /dev/sda`
    
    Esto te mostrará una lista completa de los atributos S.M.A.R.T., incluyendo el recuento de sectores defectuosos, la temperatura, los ciclos de encendido/apagado, entre otros.
    
3. **Realizar un test S.M.A.R.T.**: Puedes iniciar un test corto o largo del disco para verificar más a fondo su estado:
    
    - **Test corto** (rápido, generalmente dura unos minutos):
        
        bash
        
        Copiar código
        
        `sudo smartctl -t short /dev/sda`
        
    - **Test largo** (más exhaustivo, puede durar varias horas dependiendo del tamaño del disco):
        
        bash
        
        Copiar código
        
        `sudo smartctl -t long /dev/sda`
        
    
    Después de ejecutar estos tests, puedes revisar los resultados con:
    
    bash
    
    Copiar código
    
    `sudo smartctl -l selftest /dev/sda`
    

### Otras herramientas gráficas:

Si prefieres herramientas con interfaz gráfica en lugar de línea de comandos, también puedes utilizar **GSmartControl**, que es una interfaz gráfica para smartmontools.

1. **Instalación en Ubuntu/Debian**:
    
    bash
    
    Copiar código
    
    `sudo apt-get install gsmartcontrol`
    
2. **Ejecución**: Simplemente ejecuta `gsmartcontrol` desde el terminal o desde el menú de aplicaciones, y podrás ver la lista de discos conectados, su estado S.M.A.R.T., y ejecutar pruebas directamente desde la interfaz gráfica.
    

Estas herramientas son bastante fiables y te proporcionan una visión clara del estado de tus discos en Linux.
