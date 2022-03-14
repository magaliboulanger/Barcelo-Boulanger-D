# Trabajo Final Diseño de Sistemas de Software - Farmacy Food 

1. [Suposiciones](#id1)
2. [Casos de uso](#id2)
3. [ADD](#id3)


# Suposiciones<a name="id1"></a>
## Registro de compras y pagos (Control de stock)
Comunicación con 2 APIs (de heladeras y kiosco). Otra opción es el pago por la app (para ambas situaciones, heladera o kiosco). En ambos casos los sistemas deben interactuar en los dos sentidos.  
### Pago desde app
**Heladera:** aviso por seguridad para retirar el producto la persona adecuada.  
**Kiosco:** aviso al sistema del kiosco.  
### Pago en persona  
**Heladera:** API de la heladera (La heladera tiene el control del pago, no hay persona).  
**Kiosco:** API del kiosco.

* Agregado de stock lo hacen las APIs (heladera y kiosco) cuando reciben stock. 
* El eliminado es realizado por las APIs con las ventas.

## Objetivo de alcance inicial
Puesta en funcionamiento, se esperan 500 usuarios, 5 distribuidores, y en horarios pico 100 usuarios conectados simultáneamente. 
(Instagram de farmacyfood 230 seguidores, tomado como base).

# Casos de Uso<a name="id2"></a>
Usuarios:
* Cliente
* Distribuidor (actualmente heladeras y kioscos)
* Cocina  
Casos:
1. Comprar: El usuario puede seleccionar uno o más productos de un local y reservarlos pagando mediante la conexión con algún sistema de pago. El usuario podría aplicar promociones en la compra.
2. Vender : Un distribuidor selecciona uno o más productos a ser vendidos, registra la información del comprador y el monto de la venta. El stock de los productos vendidos es actualizado.
3. Puntuar y comentar : El usuario puede ver una lista de los productos que ha comprado y de los distribuidores a los que les ha comprado y hacer un comentario u otorgar una puntuación. Esta puntuación/comentario será asociada al producto o distribuidor con el nombre del usuario que puntúa.
4. Ver información de stock : El usuario tiene acceso a una lista de los productos disponibles pudiendo filtrar por varias características como tipo, ubicación del kiosco, marca, etc. El stock debe estar actualizado todo el tiempo.
5. Actualizar stock : El distribuidor puede agregar productos a su local pudiendo elegir entre los productos que ya ha vendido o agregar nuevos. 
6. Aplicar promociones : El usuario puede ingresar un código de promoción el cual, de estar en la lista de promociones, modifica el monto de la compra. Las promociones pueden ser un monto fijo que se descuenta, un porcentaje de la compra total o de un producto específico, etc.
7. Crear promociones : Agregar promociones a la lista de promociones del sistema.
8. Ver puntos de venta : El usuario puede ver una lista de todos los puntos de venta del sistema, con reseña, calificación promedio y ubicación. Se puede filtrar por productos en stock, distancia, tipo de comercio, etc.
9. Ver información de usuario : El usuario puede acceder a su información personal y su historial de compras.
10. Login: El usuario puede ingresar al sistema con un nombre de usuario y contraseña.
11. Registro: Un usuario puede registrarse como comprador o punto de venta, creando una cuenta con usuario, contraseña e información del perfil.

![Diagrama de Casos de Uso](/Images/DiagramaCasosDeUso.png "Diagrama de casos de uso")

# Atributos de calidad 
| Id | Atributo | Escenario | Casos de uso relacionados |
| ---- |-----------|-----------|------------------------|
| QA-1 | Accesibilidad (movilidad accesible) | Un usuario accede a la aplicación desde cualquier dispositivo con conexión a internet  | Todos |
| QA-2 | Disponibilidad | Ante una falla durante el normal funcionamiento del sistema, el sistema hace un rollback regresando al estado inicial. | Todos | 
| QA-3 | Accuracy | Ante una actualizacion en el stock, informacion o distribuidores, los usuarios obtienen la información correcta. | CU4 - CU5 - CU7 - CU8 - CU9 - CU11 |
| QA-4 | Confiabilidad | Ante una disminución en el ancho de banda, todas las transacciones se pueden finalizar o abortar informando al usuario en cada caso. | CU1 - CU2 - CU3 |
| QA-5 | Seguridad (en los pagos) | Un usuario realiza una transacción mediante el uso normal de la aplicación y puede conocerse quién hizo la operación y en qué momento, resguardando los datos confidenciales del usuario. En caso de ataques, los datos originales se restaurarán en un plazo máximo de 1 día. | CU1 - CU2 |
| QA-6 | Escalabilidad | Se agregan soporte para mayor cantidad de usuarios y distribuidores de forma satisfactoria y sin tener que realizar cambios en el CORE del sistema | CU11 |
| QA-7 | Facilidad de integración | Se agregan nuevos modulos o funcionalidades, o se integran herramientas externas al sistema sin tener que realizar cambios en el CORE del mismo | Todos |
| QA-8 | Performance | En condiciones normales de operacion el sistema deberá procesar las transacciones con una latencia promedio de 5 segundos. | Todos |

 

# Restricciones 
* Aplicación web accesible desde móviles y diferentes plataformas (Windows, Linux, OsX).
* Los distribuidores y usuarios podrían tener bajo ancho de banda.
* Debe soportarse un mínimo de 100 usuarios simultáneos.
* La aplicación debe comunicarse con las API de heladeras, kioscos y métodos de pago.

# Aspectos concernientes a la arquitectura
* Establecer una arquitectura inicial para la definicion general del sistema.

# ADD<a name="id3"></a>

1. [Step 1](#idS1)  
2. [Iteracion 1](#idI1)  
    1. [Step 2](#idS2)  
    2. [Step 3](#idS3)  
    3. [Step 4](#idS4) 
    4. [Step 5](#idS5) 
    5. [Step 6](#idS6) 
    6. [Step 7](#idS7) 
3. [Iteracion 2](#idI2)  
    1. [Step 2](#idS22)  
    2. [Step 3](#idS23)  
    3. [Step 4](#idS24) 
    4. [Step 5](#idS25)  
    5. [Step 6](#idS26)  
    6. [Step 7](#idS27) 
4. [Iteracion 3](#idI3)  
    1. [Step 2](#idS32)  
    2. [Step 3](#idS33)  
    3. [Step 4](#idS34) 
    4. [Step 5](#idS35)  
    5. [Step 6](#idS36)  
    6. [Step 7](#idS37)

## Step 1: Review Inputs<a name="idS1"></a>
| Categoria | Detalles |
| --------- | -------- |
| Propósito de diseño | Sistema a desarrollar desde cero con dominio conocido. Desarrollo Agil con iteraciones cortas para obtener feedback continuamente. Un primer diseño arquitectural es necesario como guia para evitar el doble esfuerzo. |
| Requerimientos funcionales primarios | De los casos de usos presentados los primarios son :  **CU1:** Comprar; **CU2:** Vender; **CU5:** Actualizar stock |
| Escenarios de QA | Los escenarios de atributos de calidad descritos son priorizados la tabla de prioridades. De estas, QA-1, QA-2, QA-3, QA-5 y QA-8 se seleccionan como drivers. |
| Restricciones | Todas las limitaciones se consideran como drivers. |
| Aspectos concernientes a la arquitectura | Este aspecto se considera un driver. |

### Tabla de prioridades de QA:
| ID Escenario | Importancia para el cliente | Dificultad de Implementación según el arquitecto |
| ------ | ------ | -------|
| QA-1 | Alta | Media |
| QA-2 | Alta | Alta |
| QA-3 | Alta | Media |
| QA-4 | Media | Media |
| QA-5 | Alta | Media |
| QA-6 | Media | Alta |
| QA-7 | Media | Media |

## *Iteración 1* <a name="idI1"></a>
## Step 2: Establecer objetivo de iteración mediante la selección de drivers<a name="idS2"></a>  
Se toma como objetivo de esta iteración el aspecto concerniente a la arquitectura **establecer una arquitectura inicial**.
Al tratarse de una definición general del sistema, el arquitecto deberá tener en mente todos los drivers, en particular:  
* QA-1: accesibilidad,  
* QA-2: disponibilidad,  
* QA-3: accuracy,  
* QA-5: seguridad,  
* QA-8: performance,
* Aplicación web accesible desde móviles y diferentes plataformas (Windows, Linux, OsX),  
* Los distribuidores y usuarios podrían tener bajo ancho de banda,  
* Debe soportarse un mínimo de 100 usuarios simultáneos,  
* La aplicación debe comunicarse con las API de heladeras, kioscos y métodos de pago,  
* Establecer una arquitectura inicial para la definicion general del sistema.  

## Step 3: Elegir uno o más elementos del sistema para refinar<a name="idS3"></a>  
Dado que es la primera iteración, el único elemento disponible a refinar es el sistema en su totalidad.  

## Step 4: Elegir conceptos de diseño que satisfagan los drivers seleccionados<a name="idS4"></a>  
| Decisiones de diseño y ubicación | Razón fundamental |
| -------------------------------- | ----------------- |
| Usar la arquitectura de referencia Web Application para la parte del cliente |La aplicación debe ser accesible mediante internet, no requiere una gran interfaz ni la instalación de ningún componente en la máquina del cliente y los recursos requeridos por parte del mismo son mínimos. |
| Usar la arquitectura de referencia Service Application para la parte del servidor | La aplicación es usada por otros sistemas únicamente, no requiere interfaz |
| Estructurar físicamente la aplicación utilizando el patrón de despliegue 2 tiers | Al ser una arquitectura cliente-servidor, usamos un nivel para cada uno. Además es una arquitectura conocida por las arquitectas. |  

**Alternativas descartadas**  
| Alternativa | Razón |
| ----------- | ----- |
| Rich Internet Application | Descartada por el uso de plugins que podrían no estar disponibles para todas las plataformas, lo que impactaría directamente en el driver QA-1. |
| Rich Client Application | Da soporte a conectividad nula y el sistema requiere conexión constante |
| Mobile Apllication | Cumple con los requisitos, pero limita el funcionamiento solo a dispositivos móviles |
| Load-Balanced cluster pattern | Descartada porque el alcance inicial de la aplicación no justifica más de un servidor, pero puede ser considerada una opción a futuro debido al crecimiento de la misma|

## Step 5: Crear instancias de elementos arquitectónicos, asignar responsabilidades y definir interfaces <a name="idS5"></a>
| Decisión de diseño y ubicación | Razón fundamental |
| ------------------------------ | ----------------- |
| El cliente se alojará en el Tier 1 | Responsable de la capa de presentación y persistencia de datos en caché. | 
| El servidor se encuentra en el Tier 2 | Ejecutando la capa de negocio, datos y servicio responsable de la comunicación con servicios externos al sistema. |
| Utilizar el lenguaje de programación Java para el desarrollo | Las alternativas consideradas fueron: Java, PHP, Python. El equipo posee conocimiento en las tres alternativas pero consideran para esta ocasión Java dado que es el lenguaje sobre el que mayor dominio poseen. |



## Step 6: Diagramas <a name="idS6"></a>
**Diagrama de Arquitectura Inicial** 
![Diagrama de Arquitectura Inicial](/Images/DiagramaInicial.png "Diagrama de arquitectura inicial.")
| Elemento | Responsabilidad |
| ---------- | ------------------- |
| Capa de presentación | Esta capa contiene módulos que controlan las interacciones del usuario. |
| Capa de negocios | Esta capa contiene módulos encargados del control de las reglas del negocio. |
| Entidades del negocio | Entidades que forman parte del modelo del dominio. |
| Componentes del negocio | Estos componentes implementan operaciones del negocio. |
| Capa de Datos | Esta capa es la encargada de la persistencia de los datos.|
| Acceso a datos | Este componente es el encargado de la persistencia de los datos en una base de datos relacional, utilizará patrones para abstraer el comportamiento de modo que la base puede ser fácilmente extendida o reemplazada.  |
| Componente transversal | Este componente contiene funcionalidad que atraviesa varias capas, como la seguridad, la recuperación de fallos y la comunicación. | 
| Capa de servicios | Esta capa contiene módulos para exponer servicios utilizados por el cliente y también para la comunicación con servicios externos |
| Cache | Conserva los últimos datos solicitados a la capa de datos del otro tier. |
   
**Diagrama de Deployment** 
![Diagrama de Deployment Inicial](/Images/DiagramaDeploymentInicial.jpeg "Diagrama de deployment inicial.")

  
| Elemento | Responsabilidad |
| ---------- | ------------------- |
| Client Server | Servidor donde reside la capa de presentación del sistema. |
| Application Server | Servidor que aloja la lógica del negocio, las conexiones con bases de datos y sistemas externos. |

| Relación | Descripción |
| ---------- | ------------------- |
| Entre Database y Application Server | La conexión se realizará mediante el API JDBC.|
| Entre Client Server y Application Server | La comunicación entre ambos se realizará mediante el protocolo HTTPS. |
| Entre Client Server y Browser | La comunicación entre ambos se realizará mediante el protocolo HTTPS. | 

## Step 7: Análisis y revisión de los objetivos de la iteración <a name="idS7"></a>
| No abordado | Parcialmente abordado | Completamente abordado | Decisiones de diseño tomadas durante la iteración |
| ------------- | ----------------------- | ------------------------- | -------------------------------------- |
| | | QA-1 (Accesibilidad) | La arquitectura de referencia seleccionada (Web Application) asegura la accesibilidad desde cualquier dispositivo con un browser. |
| QA-2 (Disponibilidad) | | | No abordado. |
| QA-3 (Accuracy) | | | No abordado.  |
| | QA-5 (Seguridad) | | La arquitectura de referencia seleccionada contiene módulos (dentro del módulo transversal) que satisfacen este driver pero auún resta detallarlos más. |
| QA-8 (Performance) | | | No abordado. |
| | | Restric 1 | La arquitectura de referencia, al ser una aplicación web, satisface este driver. |
| | | Restric 2  | La creación de un módulo transversal especializado en recuperación ante fallos satisface este driver. | 
| Restric 3 | | | No abordado aún. | 
| | Restric 4 | | Parcialmente cubierto por el diseño elegido. Existen módulos encargados de la comunicación con servicios externos que aún deben ser refinados. | 
| | | Restric 5 | Cubierto según el diseño presentado en el Diagrama de arquitectura inicial. | 



## *Iteración 2* <a name="idI2"></a>
## Step 2: Establecer objetivo de iteración mediante la selección de drivers <a name="idS22"></a>
El objetivo de esta iteración es que la funcionalidad primaria previamente definida se encuentre soportada por la arquitectura. En esta segunda iteración los drivers seleccionados son:  
* CU1: Comprar   
* CU2: Vender  
* CU5: Actualizar stock  
## Step 3: Elegir uno o más elementos del sistema para refinar <a name="idS23"></a>
Partiendo del diseño inicial, definido en la iteración anterior, los elementos a refinar seleccionados para esta iteración son los módulos ubicados en capas en el Servidor alojado en el Tier 2, el cual sigue las arquitecturas de referencia seleccionadas.  

## Step 4: Elegir conceptos de diseño que satisfagan los drivers seleccionados <a name="idS24"></a>
| Decisiones de diseño y ubicación | Razón fundamental |
| -------------------------------- | ----------------- |
| Creación del Modelo de Dominio. | Es necesario para poder realizar una descomposición funcional conocer el modelo de dominio del sistema con sus entidades más grandes y sus relaciones. |
| Identificar objetos del modelo y alojar funcionalidades. | Encapsular elementos funcionales en bloques para asegurarse de considerar todos los requerimientos. | 
| Descomponer los objetos del dominio en componentes o modulos. | La funcionalidad de los objetos del dominio puede estar relacionada con diferentes capas y por lo tanto "pertenecer" o asociarse con los elementos dentro de las capas. |
| Usar Data Access Object (DAO) para el acceso a la base de datos. | DAO Sera utilizado para separar la capa de persistencia del resto de las funcionalidades, proveyendo un acceso seguro a los datos. Fue elegida porque las arquitectas tienen conocimiento de este patrón y consideran que se adapta a los requerimientos. |  
| Elegir el framework Spring para el desarrollo web. | Se seleccionó este framework dado que es ampliamente utilizado para el lenguaje elegido y, por otro lado, la utilización de un framework facilitará el desarrollo, dado que en general la programación es más rápida y se automatizan las tareas. |

## Step 5: Crear instancias de elementos arquitectónicos, asignar responsabilidades y definir interfaces <a name="idS25"></a>
| Decisión de diseño y ubicación | Razón fundamental |
| --------------------------------------- | ----------------------- |
| Creación un modelo inicial. | Si bien el modelo podría crecer a medida que se implemente y a medida que el sistema tambien crece, se decide plantear unicamente un modelo inicial, con las entidades consideradas claves y asociando las responsabilidades necesarias para satisfacer los casos de uso considerados en esta iteración (Las responsabilidades de cada entidad pueden verse en los diagramas del paso siguiente). |  
| Descomposición de los objetos en módulos ubicados en diferentes capas. | En esta instancia, se asegura la identificación de módulos necesarios para los casos de uso principales considerados como drivers en esta iteración. A su vez se deberán crear pruebas unitarias para dichos módulos, esto quedará registrado como un nuevo aspecto conserniente a la arquitectura (ArqCon2). |
| Conexión con la base de datos. | En la capa de datos, existirán los módulos necesarios para la implementación del patron DAO para el acceso a los datos. La base de datos se encuentra en la nube y se decide utilizar los servicios de Oracle. | 

## Step 6: Diagramas <a name="idS26"></a>

**Diagrama del modelo inicial**

![Diagrama del Modelo Inicial](/Images/ModeloInicial.png "Diagrama del modelo inicial.")

**Diagrama del módulos inicial**
![Diagrama del Módulos Inicial](/Images/ModulosInicialesFuncionalidadPrimaria.png "Diagrama del módulos inicial.")  
Esta imagen muestra los módulos iniciales requeridos para soportar la funcionalidad primaria, ubicados en sus respectivas capas.
A continuación se detallan las responsabilidades de cada uno de ellos:
| Elemento | Responsabilidad |
| ---------- | ------------------- |
| TransaccionView | Muestra el estado de la compra o venta (tanto para el cliente como para el distribuidor respectivamente) y se actualiza en función del resultado (Pago aceptado-rechazado) informando al usuario. Este componente puede contener dentro más de un elemento perteneciente a la UI que resulten necesarios. |
| RequestManager | Encargado de comunicarse con el servidor. |
| RequestService | Componente que recibe los request de parte del cliente. |
| ConectorServExternos | Componente que maneja las solicitudes a los servicios externos requeridos como es el caso de los métodos de pago. |
| TransaccionController | Contiene la lógica del negocio necesaria para llevar a cabo una transacción, la misma puede ser tanto una compra ocmo una venta según el diseño presentado en el diagrama anterior. |
| Entidades del dominio | Contiene todas las entidades del dominio descriptas en el diagrama anterior.|
| Módulos DAO | Conexión con la base de datos siguiendo el patrón DAO para las operaciones CRUD. |


**Diagramas de secuencia de los casos de uso primarios**
![Diagrama de Secuencia](/Images/DiagramaSecuencia.png "Diagrama de Secuencia.") 
El diagrama de secuencia muestra el flujo para los tres casos de uso identificados como principales. 
El flujo de una compra como el de una venta a partir de que la misma se confirma es igual, la variación se da anteriormente donde es el Comprador quien añade los productos al carrito o el Distribuidor quien crea el mismo carro, tambien se ve en este diagrama reflejada la actualización de stock (cuando se realiza la actualizacion del distibuidor) luego de una compra o venta. Para el caso de la actualizacion de stock existe otro escenario que lo provoca y es cuando un distribuidor recibe nuevo stock.
Las actualizaciones a la persistencia se dan en caso de que la operación de pago haya sido exitosa (Actualizar diagrama).  
  
En el diagrama se pueden ver los métodos iniciales que surgen para los casos de uso primarios, se describen en la siguiente tabla:

| Elemento | Método | Descripción | 
| -------- | ------ | ----------- | 
| RequestService | Response sendRequest(Request) | Recibe el request con los datos de la transacción que se desea llevar a cabo.  |
| TransaccionController | comprar() | Genera una nueva transacción con los datos recibidos. |
| ConectorServExternos | pagar() | Conexion con serviciones externos que se comunica con el gestor de pagos y retorna el estado de | UsuarioDAO | updateUsuarioDAO() | Actualiza la informacion del Usuario en la persistencia, en este caso particular actualiza su lista de compras. | 
| DistribuidorDAO | updateDistribuidorDAO() | Actualiza la informacion del Distribuidor en la persistencia, en este caso particular actualiza su lista de ventas y su stock disponible. |
| TransaccionDAO | addTransaccionDAO() | Registra la ultima operación efectuada. |

## Step 7: Análisis y revisión de los objetivos de la iteración <a name="idS27"></a>
| No abordado | Parcialmente abordado | Completamente abordado | Decisiones de diseño tomadas durante la iteración |
| ----------- | --------------------- | ---------------------- | ------------------------------------------------- |
| | CU1 - Comprar || Se encuentra parcialemnte abordado dado que la conexión con el servicio externo de pagos no esta completamente definida. |
| | CU2 - Vender || Se encuentra parcialemnte abordado dado que la conexión con el servicio externo de pagos no esta completamente definida. |
| | | CU3 - Actualizar Stock | Se considera completamente abordado con la interfaz de CRUD de la base de datos. |

## *Iteración 3* <a name="idI3"></a>
## Step 2: Establecer objetivo de iteración mediante la selección de drivers<a name="idS32"></a>  

## Step 3: Elegir uno o más elementos del sistema para refinar <a name="idS33"></a>
## Step 4: Elegir conceptos de diseño que satisfagan los drivers seleccionados <a name="idS34"></a>
## Step 5: Crear instancias de elementos arquitectónicos, asignar responsabilidades y definir interfaces <a name="idS35"></a>
## Step 6: Diagramas <a name="idS66"></a>

## Step 7: Análisis y revisión de los objetivos de la iteración <a name="idS37"></a>