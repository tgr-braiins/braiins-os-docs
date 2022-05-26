
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Terminología
############

.. contents::
  :local:
  :depth: 2

Braiins Farm proxy es una aplicación de software que acepta tasa de hash a través de múltiples puertos de escucha **[Servers]** [#f1]_ transfiriéndola a los múltiples puntos finales **[Targets]** siguiendo reglas definidas **[Routing goals]**. Los objetivos se refieren a una lista de puntos finales **[Routing goal levels]**. Una colección de Servidores, Objetivos de ruta y Niveles de objetivos de ruta se denomina como el **Routing domain**.

Las conexiones desde los mineros al Braiins Farm Proxy son definidas como conexiones **Downstream** (aguas abajo). Las conexiones desde Braiins Farm Proxy al pool de minería son definidas como conexiones **Upstream** (aguas arriba).

La Terminología utilizada se pone en contexto con el uso de los siguientes diagramas.

**Diagrama Del Enrutamiento de Tasa de hash**

  .. |pic1| image:: ../_static/routing_diagram.png
      :width: 100%
      :alt: Diagrama de Enrutamiento

  |pic1|

**Interpretación del Diagrama**

  .. |pic2| image:: ../_static/diagram_interpretation.png
      :width: 100%
      :alt: Interpretación del Diagrama

  |pic2|


.. rubric:: Notas a pie de página

.. [#f1] Los servidores son puerto de escucha en los términos de Braiins Farm Proxy, no confundir con el servidor clásico.
