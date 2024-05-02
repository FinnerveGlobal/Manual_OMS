---
sidebar_label: 'Asignacion Pictet DVP'
sidebar_position: 2
---
# Documentación asignación PICTET DVP (coordenadas, etc)

En esta sección se ilustra el proceso de configuración de los campos para los distintos tipos de liquidación.

Los tipos de liquidación que existen actualmente en el sistema son: Fed, DTC, Euroclear, Clearstream e International.

## Configuración

Para poder configurar los campos Fed, DTC y Euroclear necesitamos ingresar al apartado de Mantenimiento -\> Instituciones -\> Dar doble clic en alguna institución de la lista

![](media/67d3b294304221fd2474bac1b6e70934.png)

Una vez demos doble clic en una de las instituciones, el sistema nos mostrará los campos de los tipos de liquidación que podemos llenar.

![](media/f987ad181239ce6a37860e6b6f9024db.png)

Para este caso completamos los campos de la siguiente manera:

Cuenta Fed: 66666

Cuenta DTC: 00433

Cuenta Euroclear: 98730

Una vez realizado ello le damos en SAVE y vamos a realizar unas pruebas para verificar que los campos se están mostrando correctamente en el formato de envío de asignación.

Esta evaluación se llevará a cabo con una orden consolidadora Fincome conformada por 3 órdenes unitarias y que tenga como Contraparte a CGCO para que pueda enrutar al Broker y seguir su flujo en Broker.

## Cuenta Fed

Seleccionamos y enviamos la orden consolidadora para recibirla en Broker.

Una vez estando en Broker, aceptamos la orden

![](media/482a06154689e6b5623cd8efddbb077c.png)

La enviamos

![](media/24a20991999920b41858c029428c4157.png)

Creamos su confirmación

Para colocar tipo de liquidación Fed, se selecciona la opción “Fed” en el desplegable del campo Settlement type.

![](media/246f32b02732cadc905cddef74a7d972.png)

La asignamos

![](media/68fb53f1176ea46e8a264e00109b5bca.png)

Enviamos el formato de envío de Asignación en Broker

![](media/a0602171f775bdfe95c90ddd9e2018ef.png)

Nos llega el siguiente formato de Pershing:

![](media/9cf433757b7e70ade4304dbdaf0d87fe.png)

Si regresamos a OMS Compass veremos que la orden se avanzó hasta Confirmada, procedemos a Asignarla

![](media/18a3f53675fb71b1f86e425a291a97de.png)

Y por último **enviamos el Formato de envío de Asignación**.

![](media/dc0b257cc167244c91f166f047f2d266.png)

El formato que recibimos es el siguiente y verificamos que el campo se está llenando correctamente.

![](media/4596ba6dca24d0fb80c1975a5495bb32.png)

## Cuenta DTC

Seleccionamos y enviamos la orden consolidadora para recibirla en Broker.

Una vez estando en Broker, aceptamos la orden

![](media/839577a576793253c419649a9e74fa26.png)

La enviamos y creamos su confirmación

Para colocar tipo de liquidación DTC, se selecciona la opción “DTC” en el desplegable del campo Settlement Type.

![](media/88ff4bb35063dec236809a0c71274763.png)

La asignamos

![](media/25de04a38bebb2c00daacfd11d2cbb62.png)

Enviamos el formato de envío de Asignación

![](media/1536b84b285709c09784b5c97c79954b.png)

En este caso nos llega el formato de Pershing

![](media/0b71ae13946ef714a4c38968f51c8ea2.png)

Si regresamos a OMS Compass veremos que la orden se avanzó hasta Confirmada, procedemos a Asignarla

![](media/f11e703c26e618d9b473d8c78b8b6a95.png)

Y por último enviamos el Formato de envío de Asignación.

![](media/e8f1d11479fa887ef0623e040efb044d.png)

El formato que recibimos es el siguiente y verificamos que el campo se está llenando correctamente.

![](media/8d66b856ee7811398754f9409314c42b.png)

## Cuenta EuroClear

Seleccionamos y enviamos la orden consolidadora para recibirla en Broker.

Una vez estando en Broker, aceptamos la orden

![](media/b6ad03286ce22ce84d60ac17b186397a.png)

La enviamos

![](media/f78ac83b9a6560a7c7bf1fe71ab0b175.png)

Creamos su confirmación

Para colocar tipo de liquidación Euroclear, se selecciona la opción “EuroClear” en el desplegable del campo Settlement Type.

![](media/ab300f10cdde56679109cce08a8ba228.png)

La asignamos

![](media/b3b60c09de3c1d8accbaf1793b7f2529.png)

Enviamos el formato de envío de Asignación

![](media/8c240de8c14d3954f4f942e933037c53.png)

En este caso nos llega el formato de Pershing

![](media/34acb5b085e51bb459e2bcc603a51e4d.png)

Si regresamos a OMS Compass veremos que la orden se avanzó hasta Confirmada, procedemos a Asignarla

![](media/4650e1fbebe2347853290b2c4dda8658.png)

Y por último enviamos el Formato de envío de Asignación.

![](media/99a3d9a28a63ba8384d0f29fc9713def.png)

El formato que recibimos es el siguiente y verificamos que el campo se está llenando correctamente.

![](media/7704e16e880062bf6ce295db149a7e45.png)
