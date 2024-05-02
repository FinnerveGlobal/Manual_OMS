---
sidebar_label: 'Generación Masiva de Pre Órdenes'
sidebar_position: 4
---

# Generación Masiva de Pre Órdenes

## Introducción

Con el objetivo de mejorar el control y registro de las órdenes generadas en el sistema OMS, se ha solicitado la implementación de una funcionalidad que permita la creación masiva de órdenes, en donde se manejen .

## Alcance

-   Las cuentas que conforman esta etapa están directamente relacionadas con aquellas gestionadas en Pivolt. Por lo tanto, las cuentas que se presentarán para la Generación Masiva estarán vinculadas a los portafolios que el usuario correspondiente puede ver en Pivolt.
-   Las órdenes ingresadas por el RIA, son órdenes que siguen un enrutamiento que se encuentra previamente configurado en el sistema.
-   Las órdenes ingresadas por el Broker, son órdenes que siguen un enrutamiento que se encuentra previamente configurado en el sistema.
-   Los activos y las instituciones (contrapartes) que se manejan en el sistema son aquellas gestionadas en Pivolt.
-   La creación de activos es de responsabilidad de las áreas de Middle Office de los RIAs, con un control exhaustivo por parte de GSU Riesgo para una correcta creación y clasificación.
-   Para los portafolios modelos que siguen una estrategia GSU, la responsabilidad de su creación es de GSU. Mientras que para las estrategias “Tailor Made”, es de responsabilidad de cada asesor su actualización en el sistema.
-   Los límites se encuentran configurados por el área de GSU Riesgo.
-   La integración de los datos entre Salesforce y Pivolt es de responsabilidad de Sistemas Compass.
-   Las posiciones son valorizadas por el equipo de Finix Group de manera diaria, a excepción de Pictet, para el cual se carga la información que envía directamente el custodio (esta integración la realiza Sistemas Compass).
-   La conciliación entre las posiciones de Pivolt y de los respectivos custodios es de responsabilidad de Finix Group.

## Glosario

| RIA          | Ejecutores que registran, enrutan, ingresen al custodio las órdenes de carteras manejadas offshore; para efecto del modelo operacional asociado, se consideran en este rol a las oficinas de: Miami y Uruguay |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pre órdenes  | Son solicitudes previas a la generación de las órdenes, estas muestran cómo se verán modificados las cuentas de los inversionistas al ser creada la órden.                                                    |
| Enrutamiento | Proceso por el cuál del sistema de OMS asociará a una órden una contraparte de acuerdo a ciertos criterios que se deben seguir según las rutas ingresadas a OMS.                                              |


## Configuración inicial

### Rutas

Para el correcto enrutamiento de las pre órdenes en el sistema, debe contar con ciertos parámetros que deben ser correctamente llenados en OMS para la creación de la ruta, los cuales son:

| Objeto    | Campo                 | Descripción                                                                                                         |
|-----------|-----------------------|---------------------------------------------------------------------------------------------------------------------|
| **Rutas** | Sociedad del Contrato | País de la sociedad del contrato                                                                                    |
|           | Tipo de Servicio      | Tipo de servicio (discrecional o no discrecional)                                                                   |
|           | Broker Original       | Custodio de la Cuenta es el Broker original el cuál será usado para la nueva ruta                                   |
|           | Tipo del Activo       | Es el tipo del Activo que se analizará para la ruta. Este puede ser Equity, Fincome, Fund, Option, Future o Divisa. |
|           | País del Activo       | País asociado al Activo.*.*                                                                                         |
|           | Mayor o Igual a       | Límite a ser cumplido dónde el monto de la cuenta debe ser mayor o igual a cierto número*.*                         |
|           | Menor a               | Límite a ser cumplido dónde el monto de la cuenta debe ser menor a cierto número.                                   |
|           | Broker Destino        | El resultado del enrutamiento. Será la contraparte a la que será enviada la órden.                                  |
|           | Cuenta DVP            | Cuenta DVP (Entrega contra pago) asociada a la ruta resultante.                                                     |

Campos relevantes por objetos, asociados a la ruta en OMS.![](media/1eedba077d28d7a68fc7dd16988ac08f.png)

| Objeto                  | Valores                                       | Comportamiento             |
|-------------------------|-----------------------------------------------|----------------------------|
| Account Manager Country | Lista de países                               | Se carga desde integración |
| Discretional            | SI/NO                                         | Se carga desde integración |
| Broker From             | Instituciones de Pivolt                       | Se carga desde integración |
| Asset Type              | Equity/ Fincome/ Fund/ Option/ Future/ Divisa | Se carga desde integración |
| Asset Country           | Lista de países                               | Se carga desde integración |
| Broker To               | Instituciones de Pivolt                       | Se carga desde integración |

Esta información se verá plasmada en la grilla de rutas.

![](media/cb5ff9c7e526d219f46a94436fcb8ec9.png)

### Definiciones de activos

**Equity:**

Para el cálculo del campo Amount de este tipo de activo, se toma en cuenta el Price y el Quantity.

Amount = Price \* Quantity

NOTA:

Para el caso de generar y consolidar una orden de tipo *Equity* que tenga Broker de México, a la orden consolidadora se le agrega en el precio el 1% del precio original. Las hijas u órdenes consolidadas mantienen el precio original.

**FIconme:**

Para el cálculo del campo Amount de este tipo de activo, se toma en cuenta el Price, Quantity, FactorPrice y el Nominal.

Amount = Price \* Quantity\*Nominal\* FactorPrice

**Option:**

Para el cálculo del campo Amount se toma en cuenta el ContractSize, Quantity y el Price.

Amount=ContractSize\*Quantity\*Price

**Fund:**

Para el cálculo del campo Amount de este tipo de activo, se toma en cuenta el Price y el Quantity. Tener el cuenta que para este caso el Quantity admite decimal.

Amount = Price \* Quantity

## Caja - Generación Masiva de Pre Órdenes

### Actualización de la Caja

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Caja.

Objetivo: Actualizar Caja con los últimos cambios de Pivolt y OMS.

Grilla para poder visualizar el estado actual de las cuentas de los inversionistas. Cada vez que sea posible seleccionar el botón de “Actualizar Caja” para actualizar las cuentas con los últimos cambios realizados.

![](media/f39b6d794b056af6a583a73d634a01b9.png)

-   Luego de presionar el botón “Actualizar Caja” debe aparecer un mensaje de “Caja Actualizada”

![](media/08f519cf9f710702784cea3d951c1d52.png)

Detalle de la información de la cuenta. D0, D1, D2 y DN se ven alterados de acuerdo a las fechas de liquidación de las órdenes generadas.

![](media/4506e255745db27ad15c5eb1b315e00c.png)

Para mover las columnas, debes seleccionar la cabecera y arrastrarla entre 2 columnas.

Ej: Para mover columna “Currency Account”, seleccionar y moverla hasta las columnas “Investor Currency” y “Cash in Account Manager”

![](media/9c0eee2f4f7c76bc6ea211d29330df06.png)

### Generación Masiva de Pre Órdenes

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Caja.

Objetivo: Crear pre órdenes de forma masiva con varias cuentas.

![](media/db7e28cbd7b4c7d80ebb5972280891b0.png)

#### Ingreso de pre órdenes masivas

![](media/f935a727beaa5dad32be2ef5fea4d80b.png)

| Campo                          | *Descripción*                                                                                                                                     | *Valores*                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| Transaction Type               | Tipo de transacción de la órden. Puede ser una compra, venta o compra/venta (llevar la caja a 0).                                                 | *Lista desplegable*                                         |
| Brokers                        | Es la contraparte que será usada para efectos de enrutamiento. Caso se deje vacío se usará el custodio de las cuentas                             | Lista desplegable y/o digitar                               |
| Asset                          | Es el activo de la pre órden                                                                                                                      | Lista desplegable y/o digitar                               |
| Precio Market                  | Es el precio del activo según el valor actual en el mercado. Puede ser modificado                                                                 | Valor numérico                                              |
| Ingresar por                   | Maneja cómo es que se deberá procesar las nuevas pre órdenes (por cantidad, por monto o por porcentaje establecido).                              | *Lista desplegable*: Cantidad Monto % Caja % NAV % Posición |
| Monto/ Cantidad/ Tasa          | Número correspondiente para el cálculo del monto final de la órden, y la cantidad solicitada. Va de acorde a lo seleccionado en el ítem anterior. | Valor numérico                                              |
| Distribuible o no Distribuible | Se debe seleccionar si el monto anterior va a ser distribuido entre las cuentas.                                                                  | Botón de selección                                          |
| Order Type                     | Se debe seleccionar si la orden será de tipo Market, Stop o Limit (tipo de ejecución)                                                             | *Lista desplegable* Mercado Limit  Stop                     |
| Precio Stop/ Limit             | Es el monto correspondiente si es de tipo Stop o Limit.                                                                                           | Valor numérico                                              |
| Tipo de Envío                  | Es el tipo de envío                                                                                                                               | *Lista desplegable* Cantidad Monto                          |
| Observación                    | Ingresar la nota asociada que se va a generar.                                                                                                    | Texto                                                       |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

Debe cerrarse y mostrar la pantalla anterior con un mensaje “Guardado”. Dependiendo de las cuentas seleccionadas, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación de cada una de las pre órdenes generadas

![](media/f98fed6e31bc8bfe2a1856844d96ede2.png)

### Generación Unitaria de Pre Órdenes

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Caja.

Objetivo: Crear pre órdenes con una cuenta.![](media/f1639f34ae5ad465627bb6b9ceabc3e1.png)

#### Ingreso de pre órden unitaria

![](media/83369feeef5b210682b6a115c4fa05c2.png)

| Campo                 | *Descripción*                                                                                                                                     | *Valores*                                                   |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| Transaction Type      | Tipo de transacción de la órden. Puede ser una compra, venta o compra/venta (llevar la caja a 0).                                                 | *Lista desplegable*                                         |
| Broker                | Es la contraparte que será usada para efectos de enrutamiento. Caso se deje vacío se usará el custodio de las cuentas                             | Lista desplegable y/o digitar                               |
| Asset                 | Es el activo de la pre órden                                                                                                                      | Lista desplegable y/o digitar                               |
| Ingresar por          | Maneja cómo es que se deberá procesar las nuevas pre órdenes (por cantidad, por monto o por porcentaje establecido).                              | *Lista desplegable*: Cantidad Monto % Caja % NAV % Posición |
| Monto/ Cantidad/ Tasa | Número correspondiente para el cálculo del monto final de la órden, y la cantidad solicitada. Va de acorde a lo seleccionado en el ítem anterior. | Valor numérico                                              |
| Order Type            | Se debe seleccionar si la orden será de tipo Market, Stop o Limit (tipo de ejecución)                                                             | *Lista desplegable* Mercado Limit  Stop                     |
| Precio Stop/ Limit    | Es el monto correspondiente si es de tipo Stop o Limit.                                                                                           | Valor numérico                                              |
| Tipo de Envío         | Es el tipo de envío                                                                                                                               | *Lista desplegable* Cantidad Monto                          |
| Precio Market         | Es el precio del activo según el valor actual en el mercado. Puede ser modificado                                                                 | Valor numérico                                              |
| Observación           | Ingresar la nota asociada que se va a generar.                                                                                                    | Texto                                                       |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

Debe cerrarse y mostrar la pantalla anterior con un mensaje “Guardado”. Dependiendo de las cuentas seleccionadas, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación de la pre órden generada

![](media/c0fd538f0cde422f0fc7d21f84da98cc.png)

### Enrutamiento

Luego del registro de las nuevas pre órdenes, van a seguir las reglas de enrutamiento establecidas de acuerdo al broker seleccionado en el formulario anterior o, por su defecto, el custodio de la cuenta.

Ej: En la tabla de rutas, si tenemos que para el caso de:

-   Tipo de Servicio: Discrecional
-   Broker de Origen: Pictet
-   Tipo de Activo: Equity
-   País del Activo: Estados Unidos

![](media/6820618c191c4e83282a6abf1faa20f5.png)

Y un monto mayor a 33,400 USD, entonces se enviará a CGCO, caso contrario, a Pictet.

Por lo tanto, si tenemos la pre órden que tienen como Broker a Pictet, con el activo seleccionado de tipo Equity cuyo país es Estados Unidos, la cuenta de los inversionistas es de Uruguay y el tipo de servicio es discrecional. Entonces el sistema evaluará las rutas para la pre órden a ser creada, si su monto sobrepasa los 33,400 USD entonces todas estas pre órdenes van a ser enviadas a CGCO. Caso contrario, se enviarán a Pictet.

## Posiciones - Generación Masiva de Pre Órdenes

### Actualización de los Activos

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Posiciones.

Objetivo: Actualizar Activos con los últimos cambios de Pivolt.

Grilla para poder visualizar, según un activo seleccionado, el estado de la cuenta de los inversionistas que tengan dicho activo en posición de sus portafolios. Cada vez que sea posible seleccionar el botón de “Actualizar Activo” para actualizar los activos con los últimos cambios realizados en Pivolt.

![](media/5074a5512dfa9fd2c44680dc1e80ae37.png)

-   Luego de presionar el botón “Actualizar Activos” debe aparecer un mensaje de “Activos Actualizados”.

![](media/45e34c2d3de58f257dd0e2893f654e4f.png)

#### Búsqueda de activos

![](media/0dbdefbd3e9434f0cbb34c3e67d15acd.png)

La búsqueda del activo mostrará todas las cuentas de los inversionistas que tengan al activo en posición.

### Generación Masiva de Pre Órdenes

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Posiciones.

Objetivo: Crear pre órdenes de forma masiva con varias cuentas.

![](media/9c17a68071e61560322943898e82d789.png)

#### Ingreso de pre órdenes masivas

![](media/f935a727beaa5dad32be2ef5fea4d80b.png)

| Campo                          | *Descripción*                                                                                                                                     | *Valores*                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| Transaction Type               | Tipo de transacción de la órden. Puede ser una compra, venta o compra/venta (llevar la caja a 0).                                                 | *Lista desplegable*                                         |
| Brokers                        | Es la contraparte que será usada para efectos de enrutamiento. Caso se deje vacío se usará el custodio de las cuentas                             | Lista desplegable y/o digitar                               |
| Asset                          | Es el activo de la pre órden                                                                                                                      | Lista desplegable y/o digitar                               |
| Precio Market                  | Es el precio del activo según el valor actual en el mercado. Puede ser modificado                                                                 | Valor numérico                                              |
| Ingresar por                   | Maneja cómo es que se deberá procesar las nuevas pre órdenes (por cantidad, por monto o por porcentaje establecido).                              | *Lista desplegable*: Cantidad Monto % Caja % NAV % Posición |
| Monto/ Cantidad/ Tasa          | Número correspondiente para el cálculo del monto final de la órden, y la cantidad solicitada. Va de acorde a lo seleccionado en el ítem anterior. | Valor numérico                                              |
| Distribuible o no Distribuible | Se debe seleccionar si el monto anterior va a ser distribuido entre las cuentas.                                                                  | Botón de selección                                          |
| Order Type                     | Se debe seleccionar si la orden será de tipo Market, Stop o Limit (tipo de ejecución)                                                             | *Lista desplegable* Mercado Limit  Stop                     |
| Precio Stop/ Limit             | Es el monto correspondiente si es de tipo Stop o Limit.                                                                                           | Valor numérico                                              |
| Tipo de Envío                  | Es el tipo de envío                                                                                                                               | *Lista desplegable* Cantidad Monto                          |
| Observación                    | Ingresar la nota asociada que se va a generar.                                                                                                    | Texto                                                       |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

![](media/c64e112a8891e96f93a8872dc7be0e84.png)

Regresando a la pantalla de caja Dependiendo de las cuentas seleccionadas, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación de cada una de las pre órdenes generadas.

![](media/9b134fbd7b4d356f35a157ec9748f44e.png)

### Generación Unitaria de Pre Órdenes

Ir al menú Órdenes 🡪 Generación Masiva de Órdenes 🡪 Caja.

Objetivo: Crear pre órdenes con una cuenta.

![](media/39f84221c2ab07b0cd8914172f82722a.png)

#### Ingreso de pre órden unitaria

![](media/83369feeef5b210682b6a115c4fa05c2.png)

| Campo                 | *Descripción*                                                                                                                                     | *Valores*                                                   |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| Transaction Type      | Tipo de transacción de la órden. Puede ser una compra, venta o compra/venta (llevar la caja a 0).                                                 | *Lista desplegable*                                         |
| Broker                | Es la contraparte que será usada para efectos de enrutamiento. Caso se deje vacío se usará el custodio de las cuentas                             | Lista desplegable y/o digitar                               |
| Asset                 | Es el activo de la pre órden                                                                                                                      | Lista desplegable y/o digitar                               |
| Ingresar por          | Maneja cómo es que se deberá procesar las nuevas pre órdenes (por cantidad, por monto o por porcentaje establecido).                              | *Lista desplegable*: Cantidad Monto % Caja % NAV % Posición |
| Monto/ Cantidad/ Tasa | Número correspondiente para el cálculo del monto final de la órden, y la cantidad solicitada. Va de acorde a lo seleccionado en el ítem anterior. | Valor numérico                                              |
| Order Type            | Se debe seleccionar si la orden será de tipo Market, Stop o Limit (tipo de ejecución)                                                             | *Lista desplegable* Mercado Limit  Stop                     |
| Precio Stop/ Limit    | Es el monto correspondiente si es de tipo Stop o Limit.                                                                                           | Valor numérico                                              |
| Tipo de Envío         | Es el tipo de envío                                                                                                                               | *Lista desplegable* Cantidad Monto                          |
| Precio Market         | Es el precio del activo según el valor actual en el mercado. Puede ser modificado                                                                 | Valor numérico                                              |
| Observación           | Ingresar la nota asociada que se va a generar.                                                                                                    | Texto                                                       |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

![](media/c64e112a8891e96f93a8872dc7be0e84.png)

Regresando a la pantalla de caja. Dependiendo de la cuenta seleccionada para la pre órden, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación. **![](media/2d29b75b467c00391d7023ea2740a452.png)**

# 

### Enrutamiento

Luego del registro de las nuevas pre órdenes, van a seguir las reglas de enrutamiento establecidas de acuerdo al broker seleccionado en el formulario anterior o, por su defecto, el custodio de la cuenta.

Ej: En la tabla de rutas, si tenemos que para el caso de:

-   Tipo de Servicio: Discrecional
-   Broker de Origen: Pictet
-   Tipo de Activo: Equity
-   País del Activo: Estados Unidos

![](media/6820618c191c4e83282a6abf1faa20f5.png)

Y un monto mayor a 33,400 USD, entonces se enviará a CGCO, caso contrario, a Pictet.

Por lo tanto, si tenemos la pre órden que tienen como Broker a Pictet, con el activo seleccionado de tipo Equity cuyo país es Estados Unidos, la cuenta de los inversionistas es de Uruguay y el tipo de servicio es discrecional. Entonces el sistema evaluará las rutas para la pre órden a ser creada, si su monto sobrepasa los 33,400 USD entonces todas estas pre órdenes van a ser enviadas a CGCO. Caso contrario, se enviarán a Pictet.

## Generación Masiva de Órdenes

### Consulta de Pre Órdenes

Ir a la pantalla Órdenes -\> Generación Masiva de Órdenes -\> Pre Órdenes.

Objetivo: Visualizar las pre órdenes creadas.

![](media/4ab942f157fd8c070591dad179bda2b3.png)

### Edición Masiva de Pre Órdenes

Ir a la pantalla Órdenes -\> Generación Masiva de Órdenes -\> Pre Órdenes.

Objetivo: Editar masivamente las pre órdenes antes de su envío.

![](media/b96a4752fe98b363eb5370cd05b18890.png)

Ingreso de edición pre órdenes masivas

![](media/1efe3fc6fc51c8b19a0a762bcd925325.png)

| Campo             | *Descripción*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | *Valores*                                        |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| Cantidad          | Ingresar la cantidad total de las pre órdenes, este modificará el valor de todas las pre órdenes a la nueva cantidad. Por lo tanto, va a generar un nuevo monto, teniendo en cuenta el valor del Precio ya establecido para cada activo o del precio modificado junto con la cantidad. El nuevo monto se mostrará en el tercer campo (Este nuevo valor generado para todas las pre ordenes se puede mostrar distinto en la grilla de preordenes dependiendo del activo como por ejemplo para los Fincome que considera en su cálculo factores de precio y nominales). | Valor numérico                                   |
| Precio            | Ingresar el precio total de las pre órdenes, este modificará el precio asociado al activo. Por lo tanto, va a generar un nuevo monto, teniendo en cuenta el valor de la cantidad ya establecida para cada activo o de la cantidad modificada junto con el precio. El nuevo monto se mostrará en el tercer campo (Este nuevo valor generado para todas las pre ordenes se puede mostrar distinto en la grilla de preordenes dependiendo del activo como por ejemplo para los Fincome que considera en su cálculo factores de precio y nominales).                      | Valor numérico                                   |
| Monto             | Ingresar el monto de las pre órdenes. De acuerdo al nuevo valor, se modificará la cantidad y el precio.                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Valor numérico                                   |
| Broker            | Es la contraparte que será usada para efectos de enrutamiento. Si no se llena, se usará el custodio de las cuentas. Cambiar la Contraparte va a sobreescribir el campo de “Settlement Date”.                                                                                                                                                                                                                                                                                                                                                                          | Lista desplegable y/o digitar                    |
| Settlement Date   | Es la nueva fecha en donde la pre órden va a liquidar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Fecha                                            |
| Tipo de Envío     | Seleccionar el tipo de envío de la futura órden                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | *Lista desplegable* Importe Títulos Subscripción |
| Tipo de ejecución | Seleccionar el tipo de ejecución de la futura órden.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | *Lista desplegable* Market Limit Stop            |
| Limit Price       | Si es de tipo Stop o Limit, ingresar el monto correspondiente.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Valor numérico                                   |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

![](media/5e4e0d25f230dda5f3753e9dd5632895.png)

Regresando a la pantalla de caja. Dependiendo de la cuenta seleccionada para la pre órden, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación.

![](media/089a888e6ba2d71e6bf9f7131b2ca941.png)

### Edición Unitaria de Pre Órden

Ir a la pantalla Órdenes -\> Generación Masiva de Órdenes -\> Pre Órdenes.

Objetivo: Editar masivamente las pre órdenes antes de su envío.

![](media/b2bbae4497e71e89eb089b851b66f44e.png)

#### Ingreso de edición de pre órden

![](media/eb7c027bf4cb8f0b3b8ff7542fd7200f.png)

| Campo             | *Descripción*                                                                                                                                                                                                                                                                                              | *Valores*                                        |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| Cantidad          | Ingresar la cantidad total de la pre órden, este modificará el valor de la pre orden seleccionada a la nueva cantidad. Por lo tanto, va a generar un nuevo monto, teniendo en cuenta el valor del Precio ya establecido o modificado junto con la cantidad. El nuevo monto se mostrará en el tercer campo. | Valor numérico                                   |
| Precio            | Ingresar el precio total de la pre órden, este modificará el precio asociado al activo. Por lo tanto se generará un nuevo monto de acuerdo a la cantidad establecida. El nuevo monto se mostrará en el tercer campo.                                                                                       | Valor numérico                                   |
| Monto             | Ingresar el monto de la pre orden. De acuerdo al nuevo valor ingresado se modificará el valor del campo Cantidad, teniendo en cuenta el precio establecido.                                                                                                                                                | Valor numérico                                   |
| Broker            | Es la contraparte que será usada para efectos de enrutamiento. Si no se llena, se usará el custodio de las cuentas. Cambiar la Contraparte va a sobreescribir el campo de “Settlement Date”.                                                                                                               | Lista desplegable y/o digitar                    |
| Settlement Date   | Es la nueva fecha en donde la pre órden va a liquidar.                                                                                                                                                                                                                                                     | Fecha                                            |
| Tipo de Envío     | Seleccionar el tipo de envío de la futura órden                                                                                                                                                                                                                                                            | *Lista desplegable* Importe Títulos Subscripción |
| Tipo de ejecución | Seleccionar el tipo de ejecución de la futura órden.                                                                                                                                                                                                                                                       | *Lista desplegable* Market Limit Stop            |
| Limit Price       | Si es de tipo Stop o Limit, ingresar el monto correspondiente.                                                                                                                                                                                                                                             | Valor numérico                                   |

-   Luego de presionar el botón “Guardar” debe aparecer un mensaje de “Guardado”.

![](media/67cc33f0db9d96a59a933704c2549ea5.png)

Regresando a la pantalla de caja. Dependiendo de la cuenta seleccionada para la pre órden, los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación.

![](media/31e2c5e02a4bc48a889b56ef1ab72ff1.png)

### Generación Masiva de Órdenes

Ir a la pantalla Órdenes -\> Generación Masiva de Órdenes -\> Pre Órdenes.

Objetivo: Generar las órdenes.

![](media/4c65b3444a8e16d0d9f7254b08942f62.png)

#### Confirmación

En caso no se seleccionen todas las órdenes, se mostrará una pantalla de confirmación.

![](media/4bba45493119e2dfd70bd37b2ccb90d1.png)

Finalmente serán generadas las órdenes y serán enviadas a los sistemas correspondientes.

### Envío de órdenes

Generada las pre órdenes y modificadas de acuerdo a las necesidades. Las **órdenes serán enviadas automáticamente** en dos sistemas.

-   Cuando las órdenes tienen como contraparte a CGCO se **enviará a OMS Broker** para su respectiva aprobación.
-   Cuando las órdenes no tienen como contraparte a CGCO se **enviará a OMS Ria**, en la pantalla de Cruzar / Consolidar Órdenes.

En cualquiera de las situaciones, todas las órdenes podrán ser consultadas.

Ir a la pantalla Órdenes -\> Generación Masiva de Órdenes -\> Pre Órdenes

![](media/7efe89d2ba92e9025e686830c601b510.png)

#### Envío a OMS Ria

Ir a la pantalla Órdenes -\> Cruzar / Consolidar/ Enviar Órdenes

![](media/ff674a4a0d36d9899aa6e79321880a47.png)

#### Envío a OMS Broker

Ir a la pantalla Órdenes -\> Aprobar Órdenes

![](media/622446f56c8aa1798baf3abe45109800.png)

## Generación Masiva de Pre-Ordenes con distintos activos seleccionados

El equipo de Compass ha solicitado que se pueda buscar activos por Portafolio. La finalidad de esta implementación es acotar la búsqueda de portafolios al momento de generar Pre-ordenes de manera masiva.

### Principales Cambios

#### Modal de Edición Masiva de Orden:

-   Se mantienen todos los campos que existían previamente pero ahora la lista de activos es un desplegable en el cual se puede buscar y seleccionar el activo que se desee y agregar en la grid que se muestra a continuación.

![](media/c3e23fd12f07209f97c4f85a01a7cb58.png)

-   Se ha agregado el campo Market Price al momento que se le da clic en el icono de editar de los activos seleccionados para así poder modificar el precio de los mismos.

![](media/fdaf8b39d77e16854d4be53bb8e98b31.png)

-   Se agrego una lista desplegable para el tipo de distribución de los activos:

    Tipos de distribución:

    **Distribuible por Posición**: Es una nueva funcionalidad. Esta forma de distribución se usa para generar liquidez en un portafolio vendiendo o comprando varios activos proporcional a las cantidades o montos en posicion de los activos seleccionados.

    **Distribuible por NAV:** Para el caso de Compra la distribución se hace proporcional al monto(campo D0) de cada portafolio. Para el caso de Venta se agrupa por activo y se distribuye proporcionalmente a las cantidades o montos en posicion de los activos seleccionados para los portafolios seleccionados.

    **No distribuible:** Permite generar órdenes con un monto y cantidad fijo para todos y cada uno de los activos seleccionados.

![](media/9e1e7d64258df576f22cbd7666416b81.png)

![](media/e7ea114609e9f711635227dd8bd33bf4.png)

\-Para el caso en que un activo que se quiera agregar para la edición Masiva de Orden, no tenga un precio actual, esto quiere decir, si el último precio registrado para este activo es de hace más de 190 días, se mostrará un mensaje y no se permitirá generar la preorden

![](media/3be9cdcc2351ae847d51dfb432c30bc3.png)

![](media/a41967d9372950db98791dd029898f14.png)

#### Vista de Posiciones:

-   En esta vista también se ha agregado una grid para poder visualizar los activos seleccionados y una vez realizada la búsqueda nos mostrará todas las carteras con los activos en posición.

![](media/7f2dda46e59423d16942fc522e751b7f.png)

-   Mismo caso con la búsqueda de Portafolios

![](media/34b012058d770a6bce3c77f21c978c7a.png)

-   Si buscamos por ambos tipos de búsqueda, es decir, búsqueda por Activos y búsqueda por Portafolios, el sistema hará un cruce entre los activos y portafolios que tienen posición.

![](media/8155e5e62cf91cc83a40e206917ebea0.png)

#### Vistas en general

**Edición unitaria de transacción:**

-   Se mantiene sin cambios y se puede generar una Pre-orden cuando ya se tiene un resultado de búsqueda y selección masiva de portafolios.

![](media/671a1de033a03aa6e44339d12b5f7807.png)

**Edición en la Vista de Pre-ordenes:**

-   Se mantiene la edición de las Pre-órdenes de manera masiva o de manera unitaria

![](media/761cb05b39bdd03c2b99ab202dc1dd5d.png)

![](media/30fc643a7c3d1d1f34a2db8cc7aa3203.png)

Cuando se edite de manera masiva, solo se guardaran los campos que hallan sido actualizados en el modal de Edición Masiva de Pre-ordenes, es decir, los demás campos se mantienen tal cual como fueron generados.

### Pestaña de Caja - Enrutamiento Agrupado y Cambiando el Precio de Mercado al Fund

Para la prueba de la Generación Masiva en Caja, se va a realizar con la creación de los activos desde cero. No habrá algún traslado de los activos seleccionados de una pantalla a otra. Esto se mostrará y entenderá mejor en la pantalla de Posiciones

#### Compra

Las pruebas se van a hacer con estas 4 Cuentas en la Caja![](media/56db675ddcff1c7c11ce525a4280d458.png)

##### Generación Masiva por Cantidad con 1 Equity, 1 Fund y 1 Renta Fija

#### ![](media/5fc625f99eec435c30e5cabb849b9668.png)

Vemos cómo se generan las Pre-ordenes.![](media/60eec259c4a57ad6ed4544b862b05ad2.png)

Finalmente, revisamos que se actualizaron los valores de la Caja. Los valores de DO, D1, D2 y DN se van actualizando de acuerdo a las fechas de liquidación de cada una de las pre órdenes generadas.![](media/3f6eeb86e28cd5e53d5c4363bfddf7d1.png)

##### Generación Masiva por Monto no Distribuible con 1 Equity, 1 Fund y 1 Renta Fija.

Para este caso, vamos a editar el precio del Fondo NIGIHUX a un valor de 25. El monto no distribuible significa que todas y cada una de las Preordenes se crearán con el monto o cantidad que se le asigne al momento de la generación masiva de Preórdenes.![](media/813fc49f07fb6415a7f828c170455be3.png)

Al darle clic al botón de Generar Pre-ordenes

![](media/a5628b69d38098adf82ab665d5eb4b0a.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/a4ef025539035ea1ddd216ab73118629.png)

Finalmente, revisamos los valores de la Caja.![](media/f055414048891258f2910ccdf3dada18.png)

##### Generación Masiva por Monto Distribuible Por Posición

El modo Distribuible por Posición, a diferencia del no distribuible, significa que el monto o cantidad será distribuido entre todas las Preordenes de forma proporcional a sus posiciones en la que se encuentran al momento de la generación masiva de Preórdenes. En este caso se distribuye el monto.

###### -Generar preordenes del mismo activo para diferentes portafolios

Al darle clic al botón de Generar Pre-ordenes![](media/0e309587f237f77258f4cebb1da88230.png)

Posiciones del activo para cada portafolio antes de generar las preordenes.

### ![](media/7094a9a80e3285327b088ba7b14beb4f.png)

![](media/fff8d66ff4edb1fd7a8cdd192a8fe2d4.png)

Al ser de diferentes portafolios se distribuye el monto colocado(10000) para cada preorden.

Podremos revisar las realidades de las Pre-ordenes.![](media/f9e1457b939ba21776505ec77d801460.png)

###### -Generar preordenes de activos FIncome, Fund, Equity y Option para diferentes portafolios

Al darle clic al botón de Generar Pre-ordenes![](media/b1967df60889ff05c00c611f57b3f6e9.png)

Posiciones de los activos para cada portafolio antes de generar las preordenes.

![](media/674ef7e82d0a685c3bbe7a6c8b427462.png)

![](media/c13aa8cd49299cd7bd1cbef6afab430e.png)

![](media/6837e92d2cf3b8c52b03f914974eaafa.png)

Se distribuye el monto colocado(20000) para los activos de cada portafolio. En este caso se tiene 3 portafolios a los cuales se le va a distribuir este monto de acuerdo a los montos en posición de sus activos.

Podremos revisar las realidades de las Pre-ordenes.

**![](media/ec18c87dc67c31cfb375dbf2270878ae.png)**

##### Generación Masiva por Monto Distribuible Por NAV

Para el caso de Compra la distribución se hace proporcional al monto(campo D0) de cada portafolio.

###### - Generar preordenes de diferentes activos

Para este caso, no se puede generar preordenes de distintos activos.

![](media/d1da6f47bf71d5a3dfc35d39391b11fb.png)

###### - Generar preordenes del mismo activo

![](media/656fcf16c88148c6f3d2951e0efbd031.png)

Se distribuye el monto de 50000 entre las 4 preordenes que son del mismo activo y de acuerdo a los D0 de cada portafolio:

![](media/d3e3ff60cb33fb6581da14fd557dbafe.png)

![](media/893ac7914936e033bc6b7567e3942e2d.png)

##### Generación Masiva por %Caja 1 Equity, 1 Fund y 1 Renta Fija.

El %Caja es el saldo disponible que tiene el inversionista. En este caso si se ingresa 10% Caja, se realizará la compra de ese activo con un monto del 10% de la caja disponible del inversionista.

Al darle clic al botón de Generar Pre-ordenes![](media/790bb89e847b527c939f27162af8273f.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/5bcf64c4ff16264dfdbd6cbc4518e067.png)

Finalmente, revisamos los valores de la Caja.![](media/d02970ec30726a383e3f53e1a2b7f61e.png)

##### Generación Masiva por %NAV 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/ae9f590b2328da7eb54582f2b290179d.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/7cea583f08d64aa7c0750bcbb5acb40a.png)

Finalmente, revisamos los valores de la Caja.![](media/26769a1938c7e4e0741b1b077d19a8e0.png)

##### Generación Unitaria por Cantidad

Seleccionamos el Activo a querer realizar la prueba.

![](media/831ee2f2e07711f3619fd95365ab3ff4.png)

Se muestra al Generar la Pre-orden![](media/db1c8632e7524436171f710da5e4d887.png)

Y finalmente la Caja actualizada![](media/de74c8d026e3e43cc0f87b1868733e54.png)

#### Venta

##### Generación Masiva por Cantidad con 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/4bc97d3f2683750e56b223157f27f84f.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/00e37208294b4533e839f7655fdc4392.png)

Finalmente, revisamos los valores de la Caja.![](media/94c0ce36a298312358fea7e80f20e01b.png)

##### Generación Masiva por Monto no Distribuible con 1 Equity, 1 Fund y 1 Renta Fija.

Para este caso, vamos a editar el precio del Fondo NIGIHUX![](media/2836576657c0d67d4f2182f6901e23ba.png)

Al darle clic al botón de Generar Pre-ordenes![](media/6575840a0fdc83cc84ce979e1db464d2.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/c9386660bfa6a6022c2f3310dce59d6f.png)

Finalmente, revisamos los valores de la Caja.![](media/dbc3c710aa18a8c84b96b54b98740432.png)

##### Generación Masiva por Monto Distribuible Por Posición

###### -Generar preordenes de diferentes activos para un mismo portafolio

Al darle clic al botón de Generar Pre-ordenes![](media/a82cb4dbc0a62c11535a0a95bd2b5e62.png)

Posiciones de los activos para el portafolio antes de generar las preordenes.

### ![](media/0369dd96a276871ae9ed1a1ac639a1fd.png)

Al ser de un solo portafolio se distribuye el monto colocado(50000) para cada preorden proporcional a sus montos en posición del portafolio.

Podremos revisar las realidades de las Pre-ordenes.

![](media/1c5051863bbd33749314edb77aef68f5.png)

##### Generación Masiva por Monto Distribuible Por NAV

Para el caso de Venta se agrupa por activo y se distribuye proporcionalmente a las cantidades o montos en posición de los activos seleccionados para los portafolios seleccionados.

###### - Generar preordenes del mismo activo

Se han seleccionado 3 portafolios para un mismo activo:

![](media/e3450583ed1db2ced4e47134638c710d.png)

Al darle clic al botón de Generar Pre-ordenes

![](media/ffe58d1d82a4865915a15f9b36b432f7.png)

Se distribuye el monto colocado(100000) para cada preorden proporcional a sus montos en posición del activo en cada portafolio.

Podremos revisar las realidades de las Pre-ordenes.

![](media/06a76ff9bcc40167f8d8a5a44a692c4b.png)

##### Generación Masiva por %Posición 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/03a91d4c392ca60949fe7b4980a0a181.png)

Podremos revisar las realidades de las Pre-ordenes. En este caso solo se crean las que sí tengan al activo en posición - previamente se mostrará un mensaje de aviso ante este caso.![](media/6ff1f356527cf7773a31690992aa4627.png)

Finalmente, revisamos los valores de la Caja.![](media/b94173b2f39038e9fe880281f54ec254.png)

##### Generación Unitaria por Cantidad

Seleccionamos el Activo a querer realizar la prueba.

![](media/0f40a033cea67ecce932ee9f7bd1ab02.png)

Se muestra al Generar la Pre-orden![](media/552ab20a3c5b1cc62fe068c68f5da9a8.png)

Y finalmente la Caja actualizada![](media/c1859a49f583b3586fe3215da38980e0.png)

#### Compra/Venta

##### Generación Masiva por 1 Equity

Con las mismas 4 cuentas que hemos fijado anteriormente, seleccionamos más de un activo y cambiamos el Broker a Pershing para efectos de enrutamiento agrupado. ![](media/7f3a7477cf0a7d05fb6d1193baa2ae8e.png)

Veremos que no nos dejará crear la Orden.![](media/a262a0559abb51a7e6587899f23577e9.png)

Por lo tanto, solo mantenemos AMZN US. El sistema no nos permitió crear las Pre-ordenes porque se tenía dos activos seleccionados y para esta operación de Compra/Venta se necesita seleccionar solo 1.![](media/06f1d780c30b41f6fb70de6f0dfb21ed.png)

Finalmente, al revisar las Pre-ordenes generadas.

La finalidad de las operaciones de Compra/Venta es llevar la caja del inversionista a cero(0), es decir, si tiene monto positivo tendrá que comprar y si tiene monto negativo tendrá que vender.![](media/cf2a0602b1eee02d553e7c0cbcad344c.png)

Y revisar las Cajas.![](media/fe1ff8364c024e998594679ffa8579b9.png)

##### Generación Unitaria

Seleccionamos el Activo a querer realizar la prueba. Le cambiamos el Broker a BBH para efectos de enrutamiento.

![](media/f181ba675305fb8f5d980adfe7c65b06.png)

Se muestra al Generar la Pre-orden![](media/0c4bc5b46c7bb6ddb85cf30e517f6def.png)

Y finalmente la Caja actualizada![](media/8d79433a01454fed03cd613abe496a31.png)

### Pestaña de Posición - Enrutamiento Agrupado y Cambiando el Precio de Mercado al Fund

Para la pestaña de Posición, se van a seleccionar los siguientes activos AMZN, TII y MFMLMAA LX.![](media/a8553b928bc074d2dcd6efe16998539d.png)

Adicionalmente, haremos un filtro de 4 portafolios que sí tengan estos activos en posición.![](media/56aff9a3a5658b6a6a11dfd4664ff442.png)

Finalmente, vamos a seleccionar los 4 resultados de la grilla que se muestra para poder realizar las pruebas.

#### Compra

##### Generación Masiva por Cantidad con 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/5b9faed4c9165ef26f274cd818e55ea3.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/21dfa21e6bc08261ce57724639dc433e.png)

Finalmente, revisamos los valores de la Caja.![](media/6a14742881efed8076956af017b7b208.png)![](media/36134a332cc4021bb2c1a442f9b3b7b9.png)![](media/04ad10fe91730e9e0801b1ed2885ec89.png)![](media/1d44845c29c7ef67fb4a5b37a3d36454.png)

##### Generación Masiva por Monto no Distribuible con 1 Equity, 1 Fund y 1 Renta Fija.

En este caso cambiamos el valor de Market Price para el Fund.![](media/5aacecccfc4b139116eaf2416eacf2e3.png)

Al darle clic al botón de Generar Pre-ordenes![](media/19a5b18687d6d1dcf33b099a933dff0d.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/92d2836bfeeca2a48d98cd9b5e696d77.png)

##### Generación Masiva por Monto Distribuible Por Posición:

###### -Generar preordenes del mismo activo para diferentes portafolios

Se tienen los siguientes posiciones para un activo:

### ![](media/7094a9a80e3285327b088ba7b14beb4f.png)

![](media/fff8d66ff4edb1fd7a8cdd192a8fe2d4.png)

Al darle clic al botón de Generar Pre-ordenes![](media/3e6b1189c5ab8ca8d4e4b3f5b5fd0831.png)

Al ser de diferentes portafolios se distribuye el monto colocado(10000) para cada preorden.

Podremos revisar las realidades de las Pre-ordenes.

![](media/623980a7c4ceb055db275660c8a5b2e9.png)

###### -Generar preordenes de activos FIncome, Fund, Equity y Option para diferentes portafolios

Se tiene las siguientes posiciones para los activos:

![](media/674ef7e82d0a685c3bbe7a6c8b427462.png)

![](media/c13aa8cd49299cd7bd1cbef6afab430e.png)

![](media/6837e92d2cf3b8c52b03f914974eaafa.png)

Al darle clic al botón de Generar Pre-ordenes![](media/a37ebed63cb547f9ba4d688f1d31d72e.png)

Se distribuye el monto colocado(20000) para los activos de cada portafolio. En este caso se tiene 3 portafolios a los cuales se le va a distribuir este monto de acuerdo a los montos en posición de sus activos.

Podremos revisar las realidades de las Pre-ordenes.

![](media/3a53327b89c440bdded83bd06eefe1ef.png)

##### Generación Masiva por Monto Distribuible Por NAV

###### - Generar preordenes de diferentes activos

No se permite seleccionar más de un activo para el tipo de transacción compra de distribución por NAV.

![](media/ec465553e38a2bd37347074ec606b1cd.png)

###### - Generar preordenes del mismo activo

Se tienen las siguientes posiciones:

![](media/1fe31551337aa15b039598c162040d54.png)

![](media/b4e20df7e42c44a997ebe241cd4bf9cd.png)

Se distribuye el monto de 50000 entre las 4 preordenes que son del mismo activo y de acuerdo a los D0 de cada portafolio:

![](media/bb8acd964614ac570b9ffe0d7b3405d3.png)

##### Generación Masiva por %Caja 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/bef3c518c29734a194020ad06b6d43ea.png)

Podremos revisar las realidades de las Pre-ordenes. Vemos que para este caso solo se crean Pre-ordenes para las cuentas que, si tengan un monto definido en su Caja, caso contrario sale un aviso al momento de generarlas.![](media/983c15394cddc89b478e4b13401c28cf.png)

##### Generación Masiva por %NAV 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/272a54637949163c1903d5fcdadcfb2e.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/c0de13af06d4e00b200c1594c0d138ea.png)

##### Generación Unitaria por Cantidad

Seleccionamos el Activo a querer realizar la prueba.![](media/3ed3b10b33e4bf70c18b711f44d8a57d.png)

Se muestra al Generar la Pre-orden![](media/231f205a2f3ecd2286995b9048887dd5.png)

Y finalmente la Caja actualizada![](media/1f53f05669ec5ad835f72e2349ab11a0.png)

#### Venta

##### Generación Masiva por Cantidad con 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/89ae5d4c40f98e2d6982bdcea89053cf.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/872efceaa5a8d491047e8b4a5b9af10e.png)

Finalmente revisamos las Cajas Actualizadas.![](media/56759f8920b1155f5ba6a358625d7ba0.png)![](media/1634bde44ed6495450230f15722406c5.png)![](media/c8aaf9e61f82cbdc16c3f0e5b900e6d7.png)![](media/2b14fc6f077b6a1dfeb6714cfc7f6d9d.png)

##### Generación Masiva por Monto no Distribuible con 1 Equity, 1 Fund y 1 Renta Fija.

Para este caso, vamos a actualizar el Fund.![](media/2ad7b670281a47cb2544c882ee9eb162.png)

Al darle clic al botón de Generar Pre-ordenes![](media/7a1cb4041d5797801e19bc4600f9d4e4.png)

Podremos revisar las realidades de las Pre-ordenes.![](media/8be10dcb6b3543162fb1a4ab4c51d557.png)

##### Generación Masiva por Monto Distribuible Por Posicion

###### -Generar preordenes de diferentes activos para un mismo portafolio

Se tienen las siguientes posiciones:

### ![](media/0369dd96a276871ae9ed1a1ac639a1fd.png)

Al darle clic al botón de Generar Pre-ordenes

![](media/e05f9909eb50900e6c023fd2ba558298.png)

Al ser de un solo portafolio se distribuye el monto colocado(50000) para cada preorden proporcional a sus montos en posición del portafolio.

Podremos revisar las realidades de las Pre-ordenes.

![](media/07bf3ad95658bf35f72373a8fb57740c.png)

##### Generación Masiva por Monto Distribuible Por NAV

###### - Generar preordenes del mismo activo

Se han seleccionado 3 portafolios para un mismo activo:

![](media/e3450583ed1db2ced4e47134638c710d.png)

Al darle clic al botón de Generar Pre-ordenes:

![](media/b3f9a9e0f3a56910f43c78a5dbb64bcd.png)

Podremos revisar las realidades de las Pre-ordenes. Solo se generarán las órdenes con los activos en posición.

![](media/aa9734736b21d86320f2a50865da0be7.png)

###### - Generar preordenes de activos Equity, FIncome, Fund y Option

Se tienen las siguientes posiciones de los activos:

![](media/7d6800ffd03500a6a0ad6b5ff27cf7ca.png)

![](media/e8b625f5ee19add194501e6f77e9e4e5.png)

![](media/7789425aa83b69fe978e0718fcebddfa.png)

Al darle clic al botón de Generar Pre-ordenes

![](media/3eb0ede0b3187ba8c7ef4284caf197f1.png)

Podremos revisar las realidades de las Pre-ordenes. Saldrá un mensaje de advertencia de los activos que no tienen posición para una cartera y estos no se generarán, pero si los que salen con mensaje guardado.![](media/8530814bc9278df2a5972d1a65540e45.png)

![](media/ed0c008f6642a60f61dc128158e04b18.png)

##### Generación Masiva por %Posición 1 Equity, 1 Fund y 1 Renta Fija.

Al darle clic al botón de Generar Pre-ordenes![](media/077b2f2f37d579410520183bc0a33d78.png)

Podremos revisar las realidades de las Pre-ordenes. Solo se generarán las órdenes con los activos en posición![](media/2cff47d0475d1a9d2e72eb6370a4fe79.png)

##### Generación Unitaria por Cantidad

Seleccionamos el Activo a querer realizar la prueba.![](media/16ff6eaa7a45b6ac0dd86173aad65d37.png)

Se muestra al Generar la Pre-orden![](media/4cfd344cd045386b1b5f37fc342a7849.png)

Y finalmente la Caja actualizada![](media/865e0242764f09a4e2a5bcc221669d83.png)

#### Compra/Venta

##### Generación Masiva por 1 Equity

Con las mismas 4 cuentas que hemos fijado anteriormente, seleccionamos más de un activo y cambiamos el Broker a Pershing para efectos de enrutamiento agrupado.![](media/6753564ab3f4cf30a03d840234341575.png)

Veremos que no nos dejará crear la Orden.![](media/a262a0559abb51a7e6587899f23577e9.png)

Por lo tanto, solo mantenemos AMZN US.![](media/b8f7b89382a68c404b97cdf3beb07cc5.png)

Finalmente, al revisar las Pre-ordenes generadas. Solo se van a generar las Pre-ordenes que tengan Caja asociada.![](media/21a66c42af50861ea0028403723b3c69.png)

Y revisar las Cajas.![](media/cd908592a735f3947e1a9b1c0bfabd0c.png)![](media/2a45d1ff84d256e199dcd0f11353d187.png)![](media/5b6f4a3123f03221bc437aaa6c3aad9d.png)

##### Generación Unitaria

Seleccionamos el Activo a querer realizar la prueba. Le cambiamos el Broker para ver los resultados![](media/5751ca9c281782247873cedd71d97824.png)

Se muestra al Generar la Pre-orden![](media/595dcdb81e9f29f52713578b62d1c2e1.png)

Y finalmente la Caja actualizada![](media/aefd7247043aca6aab41701902d701b5.png)

#### Pruebas de Usabilidad

##### Seleccionar más de un activo para la búsqueda.

Si seleccionamos un ACTIVO que no tenga posiciones se mostrará el siguiente mensaje.![](media/f967f42e484a2d77a2ecb98d73392945.png)

Caso contrario, vemos que si se obtienen resultados

![](media/ab8f1f6ebb66d031f5177b8005289809.png)

*Validar en PIVOLT la posición de los portafolios (API)* ![](media/dd51be847c33a54687397b360b5724fc.png)![](media/b914ae8a0e4377fc3d84d0234d1c975a.png)![](media/f946c13de4a816a76ee2198aed681fc5.png)

##### Seleccionar más de un portafolio para la búsqueda.

Si seleccionamos un portafolio sin posiciones, se muestra el siguiente mensaje.

![](media/9aa161328ebabda19015f8b39bcaeb00.png)

Caso contrario, vemos que si se obtienen resultados![](media/79b21d97cf2ec30baf206fe71b683e65.png)

*Validar en PIVOLT la posición de los portafolios (API)*

![](media/34c8a753df4b0b765b21bf661f044f5c.png)

![](media/56ac6b5dcec76e04f0e73bbee09f48d8.png)

![](media/e626043fdccbd1208b53a8e5a455ccd4.png)

##### Seleccionar más de un activo y más de un portafolio para la búsqueda.

![](media/4198bc27d53c591196af820a0edfa2f9.png)

*Validar en PIVOLT la posición de los portafolios (API)*

![](media/ab7bf46c4476082323a26496be819ca6.png)

![](media/6e6e3dd2ce09547216dbf8b1f5242f96.png)

![](media/699eb71c41e7961bf6dea532be060d28.png)
