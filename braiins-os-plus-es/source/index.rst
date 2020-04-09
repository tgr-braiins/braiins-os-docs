.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*

---------------

############
Introducción
############

Braiins OS+ es un sistema operativo para mineros ASIC. Está basado en el producto `Braiins OS <https://braiins-os.com/community-edition>`_ y provee algoritmos adicionales propietarios para el autoajuste de
mineros. Cuando un usuario provee el consumo máximo de energía permitido en Vatios, el sistema optimizará automáticamente el proceso de minado para maximisar la tasa de hash. Este proceso funciona a través de un amplio espectro de entradas, permitiendole optimizar la mejor eficiencia posible o la tasa de hash máxima basada en consideraciones económicas. Pruebas internas muestran que para el Antminer S9, es posible alcanzar una eficiencia de 70J/THs o incluso mejor para el ajuste de pocos Vatios. Para consumo alto de energía, la tasa de hash puede mejorar 20% (comparado al Antminer S9, de serie a 13.5 TH/s ~ 94J/TH).

Los dispositivos actualmente soportados son Antminer S9, S9i y S9j de Bitmain. Soporte para Antminer S17 está planificado en el futuro próximo.

***************
Características
***************

 * Lo último en optimización de autoajuste para maximizar la tasa de hash o la eficiencia
 * Sistema operativo de código abierto
 * Implementación Stratum V2 con eficiencia de datos mejorada y prevención al secuestro de tasa hash
 * Reemplazo de CGminer (BOSminer) escrito desde cero en lenguaje Rust
 * Inicio rápido (5-7 segundos)
 * Sin bloqueos aleatorios por comportamiento indefinido
 * Instalación en masa
 * Actualizaciones automáticas con el sistema de paquetes estándar opkg
 * Control de ventilador completamente personalizable (permite el enfriamiento por inmersión)
 * Monitoreo avanzado para prevenir el sobrecalentamiento y otros problemas

******************
Contacto y Soporte
******************

¿Tiene preguntas?
Nuestros equipos de desarrollo y soporte siempre están disponibles para ayudar.

Únase a nuestro grupo en Telegram:

  * `EN group <https://t.me/BraiinsOS>`_
  * `ES group <https://t.me/BraiinsOS_ES>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

  También puede `enviar petición VIP <https://slushpool.kayako.com/en-us/conversation/new/11>`_ a nuestro equipo de soporte.


*********
Changelog
*********

20.03
---------------------------

  * Todos los tipos de hardware de minado

    * [característica] el archivo de configuración permite especificar un límite de energía para la PSU en
      el algoritmo de autoajuste que tomará en cuenta para maximizar los TH/W producidos por el dispositivo
      de minado.

  * Antminer S9

    * [característica] Autoajuste basado en limite de energía especificado por el usuario

*******************
Problemas Conocidos
*******************

A continuación se listan problemas que se sabe existen en la versión liberada.

20.03 (Actualizado 3/30/2020)
-----------------------------

  * GUI

   * La linea de referencia en el gráfico tasa de hash tiene un valor incorrecto para el
     promedio de tasa hash nominal. El problema solo aparece cuando hay menos de 3 cadenas
     hash funcionando.
   * La tasa de rechazo está multiplicada por 100. Por ejemplo cuando la tasa de rechazo es
     0.1%, muestra entonces 10%.

  * Configuración

    * La instalación por tarjeta SD reportará una llave de autenticación Stratum V2 faltante en la
      sección Miner/Configuration (Error: missing upstream authority key for securing stratum2+tcp
      connection in pool"). El usuario puede configurar la conexión (incluyendo la llave) en la
      configuración, o directamente en el archivo ``/etc/bosminer.toml``.
