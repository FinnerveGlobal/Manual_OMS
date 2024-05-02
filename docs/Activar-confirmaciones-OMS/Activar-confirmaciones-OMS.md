---
sidebar_label: 'Activar Confirmaciones en OMS Compass'
sidebar_position: 2
---


# Activar confirmaciones en OMS Compass de las órdenes consolidadas en Broker

Hay órdenes consolidadas en Broker que no se les crea confirmaciones en Ria.

Además, al momento de crear las confirmaciones para las hijas, no toma el precio de la confirmación de la hija.

A continuación un ejemplo de ello:

![](media/871d5ce11d562f6580d3fbbfb8bdcbe4.png)

Detalle de las órdenes:

En OMS Compass:

(Consulta Confirmaciones)

![](media/96401b999cc43b8a57ec863e8e0f35a8.png)

(Información de orden)

![](media/ce074ac36d42f19c6dd75d948f004f88.png)

En OMS Broker:

(Consulta Confirmaciones)

![](media/fa4d7929474cfe2ba06cb3df29adfc88.png)

(Información de orden)

![](media/c5769db96441ed8509b584e3ca1a13f5.png)

## Configuración

El sistema cuando envía las confirmaciones órdenes consolidadas pasa por una condicional, la cual establece de que si el custodio (consultado en la tabla de instituciones) posee el campo “Send Custodian Order to OMS Broker” en False, no enviará las confirmaciones a RIA.

Este ajuste se encuentra en la gran mayoría de casos en False en Compass Broker, lo cual evita que la órdenes consolidadas de custodios con este ajuste en False no se repliquen en Compass.

Al activar este campo en el custodio que sea necesario, el sistema enviará efectivamente las confirmaciones desde Broker a Compass RIA de órdenes consolidadas.

### Equity:

Se activa el campo Send Custodian Order to OMS Broker en Pictet en Compass Broker:

![](media/72d01b51511b70bae74171c3eceda701.png)

![](media/dde1f8f7bb9913ec61ed70d1396c576b.png)

Órdenes en Compass RIA, “Cruzar Consolidar Enviar Órdenes”:

![](media/5238e641096aa5e2dcd2a0a0f8b7d982.png)

Se consultan sus confirmaciones:

![](media/94af1c01e4ada45468e51d59f80cfa15.png)![](media/72fc0d4020c91bbafac662acbb79b5cd.png)

Se envían a Broker

En Compass Broker se coloca “Aceptar Órdenes”:

![](media/97aee414d00bf2f800985210991f909d.png)

Se consolidan en Broker:

![](media/4d4179156ea2bfde372dff4356154417.png)

Se confirma la consolidadora en Broker:

![](media/eb5948ebf5e23611a3b8dbdb41053a38.png)

Se lleva hasta la pantalla Asignadas, creando las confirmaciones de las órdenes consolidadas.

Se consulta las confirmaciones de las órdenes en RIA:

![](media/cf833e74ee98a05ebfcc8e702321575d.png)

![](media/d09dcc78f04caab0032c55e11054f5a8.png)

### FIncome:

Se activa el campo Send Custodian Order to OMS Broker en Pictet en Compass Broker:

![](media/72d01b51511b70bae74171c3eceda701.png)

![](media/dde1f8f7bb9913ec61ed70d1396c576b.png)

Órdenes en Compass RIA, “Cruzar Consolidar Enviar Órdenes”:

![](media/144e5b0f2c972d1fdbd46d8c21ae3880.png)

Se envían.

En Compass Broker se coloca “Aceptar Órdenes”:

![](media/845550337563912dfe6cefd5d8229174.png)

![](media/1876bfc16b1a94181df1716e168661f5.png)

Se confirma la consolidadora en Broker:

![](media/217d011ae39bd71b7768f3a19a2703e5.png)

Se lleva hasta la pantalla Asignadas, creando las confirmaciones de las órdenes consolidadas.

Se consulta las confirmaciones de las órdenes en RIA:

![](media/0825f8939feb545d7f3103402213629c.png)

![](media/0a83ce5e6250a08485991d5307a497a3.png)
