
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Limitaciones
############

.. contents::
  :local:
  :depth: 2

La primera versión de Braiins Farm Proxy tiene un par de limitaciones conocidas que serán resueltas en versiones futuras.

 1.  Compatible con **Linux 64bit** y **ARM 64bit y 32bit**,
 2.  El requisito mínimo de HW es **Raspberry Pi 3** con 8GB de memoria,
 3.  Braiins Farm Proxy soporta solo **1 dominio de ruta**. Puede limitar una granja con necesidades amplias para varias reglas de enrutamiento. Esta limitación puede ser mitigada usando varias instancias de Braiins Farm Proxy. Por ejemplo, si tiene múltiples lugares/destinos. Por supuesto, puede combinar ambos enfoques,
 4.  Linux usualmente tiene un límite de **1.024** para el número de archivos abiertos. Puede revisar el límite con el comando ``ulimit -Hn``. Si planea conectar unos 1.000+ mineros es necesario aumentar este límite, se puede hacer con el comando ``ulimit -n 10000`` (incrementa a 10.000 archivos abiertos como un ejemplo),
 5.  No hay **GUI** para la configuración del enrutamiento de tasa de hash. La configuración debe hacerse en el archivo de configuración TOML,
 6.  Los tableros de Farm Monitor, dedicados a supervisar toda la granja en detalle, admiten actualmente mineros con el firmware Braiins OS+,
 7.  Los **mineros** probados fueron Antminers S9, X17, X19, Whatsminers M2x/M3x y Avalon miners con firmware de serie y Braiins OS+,
 8.  Hay un **intercambio** entre la agregación de la tasa de hash y el monitoreo de equipos individuales en el tablero del pool objetivo. Esto es relevante solo para casos donde se use el atributo *identity_pass_through = true* en el archivo de configuración TOML (dentro de la definición [target]). Un nivel mayor de agregación significa que los mineros individuales están mandando envíos mas lento debido a la alta dificultad aguas arriba (un proxy con alto factor de agregación obtiene un trabajo de dificultad alto desde el pool objetivo). En esos casos los mineros individuales pueden aparecer en el tablero del pool como inactivos. No es una limitación de Braiins Farm Proxy sino su característica lógica.
