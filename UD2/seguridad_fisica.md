# Ubicación y protección física de equipos

El primer paso para establecer la seguridad de un servidor o un equipo es decidir adecuadamente dónde vamos a instalarlo. Esta decisión puede parecer superflua, pero nada más lejos de la realidad: resulta vital para el mantenimiento y protección de nuestros sistemas.

Los planes de seguridad física se basan en proteger el hardware de los posibles desastres naturales, de incendios, inundaciones, sobrecargas eléctricas, robos y otra serie de amenazas.

Se trata, por tanto, de aplicar barreras físicas y procedimientos de control, como medidas de prevención y contramedidas para proteger los recursos y la información, tanto para mantener la seguridad dentro y alrededor del Centro de Cálculo como los medios de acceso remoto a él o desde él.

### Factores para elegir la ubicación

Cuando hay que instalar un nuevo centro de cálculo es necesario fijarse en varios factores. En concreto, se elegirá la ubicación en función de la disponibilidad física y la facilidad para modificar aquellos aspectos que vayan a hacer que la instalación sea más segura. Existen una serie de factores que dependen de las instalaciones propiamente dichas, como son:

* **El edificio.** Debemos evaluar aspectos como el espacio del que se dispone, cómo es el acceso de equipos y personal, y qué características tienen las instalaciones de suministro eléctrico, acondicionamiento térmico, etc. Igualmente, hemos de atender a cuestiones de índole física como la altura y anchura de los espacios disponibles para la instalación, si tienen columnas, cómo es el suelo, la iluminación, etc. Además atenderemos a la seguridad física del edificio contra incendios, inundaciones y otros peligros naturales que puedan afectar a la instalación.
  
* **Tratamiento acústico.** En general, se ha de tener en cuenta que habrá equipos, como los de aire acondicionado, necesarios para refrigerar los servidores, que son bastante ruidosos. Deben instalarse en entornos donde el ruido y la vibración estén amortiguados.
  
* **Suministro eléctrico propio del CPD.** La alimentación de los equipos de un centro de procesamiento de datos tiene que tener unas condiciones especiales, ya que no puede estar sujeta a las fluctuaciones o picos de la red eléctrica que pueda sufrir el resto del edificio. No suele ser posible disponer de toda una red de suministro eléctrico propio, pero siempre es conveniente utilizar un sistema independiente del resto de la instalación y elementos de protección y seguridad específicos, como sistemas de alimentación ininterrumpida.
  
* **Condiciones ambientales** que rodean al local donde vayamos a instalar el CPD. Los principales son los factores naturales (frío, calor, inundaciones, incendios o terremotos); los servicios disponibles, especialmente de energía eléctrica y comunicaciones (antenas, líneas telefónicas, etc.), y otras instalaciones de la misma zona; y la seguridad del entorno, ya que la zona donde vaya a situarse el CPD debe ser tranquila, pero no un sitio desolado. Otros factores que han de tenerse en consideración son el vandalismo, el sabotaje y el terrorismo.
  
###  ¿Dónde se debe instalar el CPD?

Atendiendo solo a estos factores ya podemos obtener las primeras conclusiones para instalar el CPD en una ubicación de características idóneas. Así pues, siempre que podamos, tendremos en cuenta que: 

* Deben evitarse áreas con fuentes de interferencia de radiofrecuencia, tales como transmisores de radio y estaciones de TV.
* El CPD no debe estar contiguo a maquinaria pesada o almacenes con gas inflamable o nocivo.
* El espacio deberá estar protegido ante entornos peligrosos, especialmente inundaciones. Se buscará descartar:
	- Zonas cercanas a paredes exteriores, planta baja o salas de espera, ya que son más propensas al vandalismo o los sabotajes.
	- Sótanos, que pueden dar problemas de inundaciones debido a cañerías principales, sumideros o depósitos de agua.
	- Última planta, evitando desastres aéreos, etc.
	- Encima de garajes de vehículos de motor, donde el fuego se puede originar y extender más fácilmente.

Según esto, la ubicación más conveniente se sitúa en las plantas intermedias de un edificio o en ubicaciones centrales en entornos empresariales.



### Control de acceso

De modo complementario a la correcta elección de la ubicación del CPD es necesario un férreo control de acceso al mismo. Dependiendo del tipo de instalación y de la inversión económica que se realice se dispondrá de distintos sistemas de seguridad, como los siguientes:

* Servicio de vigilancia, donde el acceso es controlado por personal de seguridad que comprueba la identificación de todo aquel que quiera acceder a una ubicación. En general, suele utilizarse en el control de acceso al edificio o al emplazamiento y se complementa con otros sistemas en el acceso directo al CPD.

* Detectores de metales y escáneres de control de pertenencias, que permiten “revisar” a las personas, evitando su acceso a las instalaciones con instrumentos potencialmente peligrosos o armas.

* Utilización de sistemas biométricos, basados en identificar características únicas de las personas cuyo acceso esté autorizado, como sus huellas digitalizadas,su iris, la voz o la dinámica de firma manuscrita.

* Protección electrónica, basada en el uso de sensores conectados a centrales de alarma que reaccionan ante la emisión de distintas señales. Cuando un sensor detecta un riesgo, informa a la central que procesa la información y responde según proceda, por ejemplo emitiendo señales sonoras que alerten de la situación.
  


### Sistemas de climatización

Además de instalar el CPD en la mejor localización posible, es imprescindible que se instalen en su interior sistemas de climatización, de protección contra incendios (PCI) y sistemas de alarma apropiados.

Los equipos de un CPD disipan mucha energía calorífica y hay que refrigerarlos adecuadamente para mantener las condiciones interiores de temperatura y humedad estables, ya que las altas temperaturas podrían dañar estos equipos

Podemos, por ejemplo, utilizar un ventilador que expulsa el aire caliente al exterior para refrigerar un servidor, pero debemos tener en cuenta que haya recirculación de aire y que éste atraviese el servidor *(figura 1)*.

Para un CPD con varios racks, podemos optar por el uso de equipos murales (splits) que darán directamente aire frío a los racks. *(figura 2)*.


![Climatización de espacios](imagenes/tema02_climatizacion.jpg)



### Sistemas contra incendios

Estos sistemas no son instalados por los responsables de seguridad informática, aunque sí es necesario conocer su funcionamiento:

* Sistema de detección: Por ejemplo el sistema de detección precoz, que realiza análisis continuos del aire, de modo que pueda observar un cambio de composición en el mismo, detectando un incendio incluso antes de que se produzca el fuego.
  
* Sistema de desplazamiento del oxígeno: Reduce la concentración de oxígeno, extinguiendo así el fuego sin necesidad de usar agua, que podría estropear los equipos. Para el uso de este sistema, es necesario que antes haya una evacuación de todo el personal, pues podría peligrar su integridad física.

### Recuperación en caso de desastre

Una opción a tener en cuenta es la de tener un centro de backup independiente, de modo que aunque los equipos del CPD queden fuera de servicio, la organización podrá seguir realizando su actividad con cierta normalidad recuperando sus datos. También es conveniente la realización de sistemas redundantes, como sistemas RAID y copias de seguridad.

En caso de que se produzca un desastre, el comité de crisis decidirá poner en marcha el plan de contingencia, recuperando en primer lugar las bases de datos y ficheros esenciales, así como desviar las comunicaciones más críticas al centro alternativo.


