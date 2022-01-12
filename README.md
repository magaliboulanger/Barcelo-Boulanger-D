# Trabajo Final Diseño de Sistemas de Software - Farmacy Food 

# Suposiciones
## Registro de compras y pagos (Control de stock)
Comunicación con 2 APIs (de heladeras y kiosco). Otra opción es el pago por la app (para ambas situaciones, heladera o kiosco). En ambos casos los sistemas deben interactuar en los dos sentidos.  
### Pago desde app
Heladera: aviso por seguridad para retirar el producto la persona adecuada.  
Kiosco: aviso al sistema del kiosco.  
### Pago en persona
Kiosco: API del kiosco.  
Heladera: API de la heladera (La heladera tiene el control del pago, no hay persona). 

* Agregado de stock lo hacen las APIs (heladera y kiosco) cuando reciben stock. 
* El eliminado es realizado por las APIs con las ventas.

## Objetivo de alcance inicial
Puesta en funcionamiento, se esperan 500 usuarios, 5 distribuidores, y en horarios pico 100 usuarios conectados simultáneamente. 
(Instagram de farmacyfood 230 seguidores, tomado como base).

# Casos de Uso
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

![Diagrama de Casos de Uso](/Images/DiagramaCasosDeUso.jpeg "Diagrama de casos de uso")

# Atributos de calidad 
Escenarios:  
* **Accesibilidad (movilidad accesible)**: Aplicación web y mobile.  
* **Accuracy y Disponibilidad:** Rápida recuperación/refresco ante fallas o actualizaciones.  
* **Performance y Confiabilidad:** Funcionamiento aceptable si disminuye ancho de banda. Siendo aceptable poder finalizar o abortar una transacción comenzada antes de la disminución informando al usuario en cada caso.  
* **Seguridad (en los pagos):** Registro completo de transacciones y protección de los datos.  
* **Escalabilidad:** Soporte para más distribuidores/usuarios.  
* **Facilidad de integración**   

# Restricciones 
* Aplicación web accesible desde móviles y diferentes plataformas (Windows, Linux, OsX).
* Los distribuidores y usuarios podrían tener bajo ancho de banda.
* Debe soportarse un mínimo de 100 usuarios simultáneos.
