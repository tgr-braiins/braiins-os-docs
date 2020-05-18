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

Braiins OS es un sistema operativo completamente código abierto para mineros ASIC. Fue el primer firmware
en implementar overt AsicBoost en 2018, y ahora caracteriza la implementación de un nuevo protocolo de
minado, Stratum V2. Adicionalmente, BraiinsOS trabaja en conjunto con nuestro nuevo componente de
software, BOSminer, el cual hemos escrito desde cero en lenguaje Rust como reemplazo del desactualizado
CGminer.

Los dispositivos actualmente soportados son Antminer S9, S9i y S9j de Bitmain. Soporte para Antminer S17
está planificado en el futuro próximo.

***************
Características
***************

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

*********
Changelog
*********

20.03
---------------------------

Ver WHATSNEW.MD (Será publicado el 3/31 en github)

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
