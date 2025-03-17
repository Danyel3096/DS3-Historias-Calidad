# Documento de Arquitectura del Sistema de Gestión de Órdenes y Entregas

## 1. Introducción  
Este documento describe la arquitectura inicial del sistema de gestión de órdenes y entregas, incluyendo requisitos funcionales, requisitos de calidad y restricciones clave que deben ser consideradas en el diseño del software.

**Equipo:** 8  
**Integrantes:**   
| Nombre Completo           | Código de Estudiante     |
|---------------------------|--------------------------|
| Diego Alejandro Tolosa    | 2313023                  |
| Juan Esteban Lopez        | 2313026                  |
| Joan Sebastian Saavedra   | 2313025                  |
| Daniel Melendez Ramirez   | 2313024                  |
| Victor Manuel Alzate      | 2313022                  |
| Karen Grijalba            | 2259623                  |

**Fecha:** 20/02/2025  

---

## 2. Requisitos Funcionales  
Los requisitos funcionales se presentan en forma de **historias de usuario**, especificando los **criterios de aceptación**.

### **Historias de Usuario (Épica Usuarios)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero poder gestionar los datos de cualquier usuario, incluyendo la edición de sus información personal, cambio de rol y la opción de inactivar su cuenta. | - El administrador puede modificar el nombre, correo y rol de un usuario entre Repartidor y Cliente.<br>- Solo los administradores pueden realizar esta acción.<br>- Puede inactivar una cuenta, impidiendo que el usuario inicie sesión.<br>- El sistema debe registrar los cambios en un log de auditoría. |
| **US-02** | Como usuario, quiero ver mi perfil con mis datos personales y rol asignado. | - El usuario debe poder ver su información personal sin editarla.<br>- Debe mostrarse su rol y fecha de creación de cuenta.|
| **US-03** | Como usuario, quiero editar mi información personal para mantener mis datos actualizados. | - El usuario puede modificar su nombre y correo (pero no su rol).<br>- Se debe validar que el nuevo correo no esté registrado.<br>- Si la operación es exitosa, se debe mostrar un mensaje de confirmación.|
| **US-04** | Como administrador, quiero poder consultar la lista de usuarios registrados o buscar uno específico para verificar su información o estado. | - El administrador puede listar todos los usuarios registrados en el sistema.<br> - Puede buscar un usuario por correo, nombre o ID.<br> - Debe mostrarse el estado del usuario (activo/inactivo) y su rol asignado.<br> - Solo los administradores pueden realizar esta acción. |

### **Historias de Usuario (Épica Autenticación)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como usuario (Cliente, Administrador, Repartidor) de la aplicación, quiero registrar mi cuenta a través de un formulario, diligenciando mis datos personales en la plataforma, para obtener acceso al sistema. | - El usuario debe ingresar un correo y contraseña válidos.<br>- Si las credenciales son correctas, el usuario es redirigido a su perfil.<br>- Si las credenciales son incorrectas, se muestra un mensaje de error sin revelar detalles específicos.<br>- El usuario puede solicitar un restablecimiento de contraseña.<br>- El usuario debe especificar que tipo de rol tendrá “Repartidor, Cliente”.|
| **US-02** | Como usuario del sistema, quiero ingresar mis credenciales de acceso, para autenticarme y acceder a las funcionalidades según mi rol. | - Se debe ingresar un usuario o correo electrónico y una contraseña.<br>- Si las credenciales son correctas, el usuario debe acceder al sistema según su rol.<br>- Si las credenciales son incorrectas, debe mostrarse un mensaje de error sin especificar cuál de los datos es incorrecto.|
| **US-03** | Como usuario, quiero recuperar mi contraseña en caso de olvidarla para poder acceder a mi cuenta. | - El usuario debe ingresar su correo electrónico.<br>- Se debe enviar un enlace de restablecimiento de contraseña por email.<br>- El enlace debe expirar en 15 minutos.|
| **US-04** | Como usuario, quiero cerrar sesión en la plataforma para evitar accesos no autorizados. | - El sistema debe invalidar el token JWT actual.<br> - El usuario debe ser redirigido a la página de inicio.|

### **Historias de Usuario (Épica Productos)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero registrar nuevos productos en el sistema, para que estén disponibles en el inventario. | - Se debe validar que no existan productos duplicados con el mismo código.<br>- Los campos obligatorios deben ser requeridos antes de guardar.<br>- Debe mostrarse un mensaje de confirmación tras el registro exitoso.<br>- Los productos registrados deben aparecer en la lista de inventario.<br>- Solo los usuarios con permisos adecuados pueden registrar productos.<br>- Se debe registrar la fecha y usuario que realizó el registro.|
| **US-02** | Como administrado, quiero editar la información de un producto, para actualizar sus datos y mantener la información del inventario correctamente. | - Se debe validar que el producto a editar exista en el sistema.<br>- No se debe permitir duplicar códigos ya existentes.<br>- Solo los usuarios con permisos adecuados pueden editar productos.<br>- Se debe registrar la fecha y el usuario que realizó la modificación.|
| **US-03** | Como administrador, quiero eliminar productos del sistema, para mantener actualizado el inventario y eliminar aquellos que ya no sean necesarios. | - Solo los usuarios con permisos adecuados pueden eliminar productos.<br>- Se debe solicitar una confirmación antes de eliminar un producto.<br>- No se debe permitir la eliminación de productos con stock disponible o en uso en transacciones activas.<br>- Tras la eliminación, el producto debe desaparecer de la lista de inventario.<br>- Se debe mostrar un mensaje de confirmación tras una eliminación exitosa.|
| **US-04** | Como usuario del sistema, quiero buscar y filtrar productos por diferentes criterios y organizarlos en categorías, para encontrar fácilmente los productos en el inventario y gestionarlos de manera eficiente. | - La búsqueda debe ser en tiempo real o activarse al presionar un botón.<br> - Si no hay resultados, debe mostrarse un mensaje indicando que no se encontraron coincidencias.|
| **US-05** | Como administrador, quiero visualizar los productos, para  gestionar mejor los productos. | - El administrador debe poder ver una lista de todos los productos disponibles.<br> - Cada producto debe mostrar su nombre, precio y stock.|

### **Historias de Usuario (Épica Categorias Productos)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero registrar nuevas categorías de productos, para organizar mejor el inventario y facilitar la búsqueda y gestión de productos. | - No se debe permitir la creación de categorías duplicadas.<br>- Solo los usuarios con permisos adecuados pueden registrar categorías.<br>- Se debe mostrar un mensaje de confirmación tras un registro exitoso.|
| **US-02** | Como administrador, quiero editar las categorías de productos, para actualizar su información y mantener organizada la gestión del inventario. | - Solo los usuarios con permisos adecuados pueden editar categorías.<br>- Si se actualiza una categoría, los productos asociados deben mantener su asignación.<br>- Se debe mostrar un mensaje de confirmación tras una edición exitosa.|
| **US-03** | Como administrador del sistema, quiero consultar las categorías de productos, para visualizar y organizar los productos de manera eficiente dentro del inventario. | - El sistema debe mostrar una lista con todas las categorías registradas.<br>- Las categorías deben estar ordenadas alfabéticamente o por fecha de creación.|
| **US-04** | Como administrador, quiero eliminar categorías de productos, para mantener organizada la gestión del inventario y eliminar categorías innecesarias. | - Solo los usuarios con permisos adecuados pueden eliminar categorías.<br> - Antes de eliminar una categoría, se debe mostrar una advertencia de confirmación.|
| **US-05** | Como administrador, quiero visualizar las categorías de productos, para   gestionar mejor la categoría de productos. | - El administrador debe poder ver una lista de todas las categorías de productos disponibles.<br> - Cada categoría debe mostrar su nombre y una breve descripción.|

### **Historias de Usuario (Épica Ordenes)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como usuario autorizado, quiero crear una orden de compra, para registrar y gestionar las compras de productos de manera eficiente. | - No se puede crear una orden sin al menos un producto seleccionado.<br>- No se pueden agregar productos con cantidades en cero o negativas.<br>- El precio unitario debe ser mayor a cero.|
| **US-02** | Como usuario autorizado, quiero consultar las órdenes de compra, para visualizar el historial y dar seguimiento a cada orden. | - El sistema debe mostrar una lista de todas las órdenes de compra registradas.<br>- Los usuarios sin permisos solo podrán visualizar la información.|
| **US-03** | Como usuario autorizado, quiero eliminar una orden de compra, para corregir errores o descartar órdenes innecesarias antes de su aprobación. | - Solo los usuarios con permisos pueden eliminar órdenes de compra.<br>- Antes de eliminar una orden, el sistema debe solicitar una confirmación.|
| **US-04** | Como usuario autorizado, quiero editar una orden de compra, para corregir errores o actualizar información antes de su aprobación. | - No se pueden agregar productos con cantidad cero o negativa.|



### **Historias de Usuario (Épica Notificaciones)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero recibir notificaciones cuando el stock de un producto sea bajo, para evitar problemas en las ventas. | - Se debe configurar un nivel mínimo de stock para cada producto.<br>- Cuando el inventario de un producto alcance o baje del nivel mínimo, se debe generar una notificación.|
| **US-02** | Como usuario del sistema, quiero recibir notificaciones sobre los cambios en el estado de mis órdenes de compra, para estar informado y dar seguimiento a cada solicitud. | - El sistema debe enviar una notificación automática cuando una orden de compra cambie de estado.|


### **Historias de Usuario (Épica Detalles Ordenes)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como usuario autorizado, quiero agregar productos a una orden de compra, para incluir todos los artículos necesarios antes de su aprobación. | - El usuario autorizado debe poder buscar y seleccionar productos desde un listado disponible.<br>- Cada producto agregado debe mostrar su nombre, descripción, cantidad y precio unitario.|
| **US-02** | Como usuario autorizado, quiero consultar los ítems dentro de una orden de compra, para verificar los productos solicitados y su estado dentro del proceso de compra. | - El usuario autorizado debe poder visualizar todos los productos incluidos en una orden de compra.<br>- Cada ítem debe mostrar su nombre, descripción, cantidad, precio unitario y total.|
| **US-03** | Como usuario autorizado, quiero editar los ítems dentro de una orden de compra, para corregir cantidades, precios o detalles antes de su aprobación. | - Solo los usuarios con permisos pueden editar los ítems.<br>- No se pueden modificar ítems si la orden ya ha sido aprobada, rechazada o completada.|
| **US-04** | Como usuario autorizado, quiero eliminar ítems dentro de una orden de compra, para corregir errores o ajustar la lista de productos antes de su aprobación. | - Solo los usuarios con permisos pueden eliminar ítems.|


### **Historias de Usuario (Épica Entregas)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero poder registrar una nueva entrega con detalles como dirección de destino, nombre del cliente, tipo de paquete, repartidor asignado y hora estimada de entrega, para organizar y planificar las rutas de los repartidores de manera eficiente. | - El sistema debe validar que la dirección de destino sea válida (no esté vacía y cumpla con un formato estándar).<br>- Una vez creada, la entrega debe aparecer en la lista de entregas pendientes con su estado inicial como "Pendiente".|
| **US-02** | Como administrador, quiero poder ver el estado de una entrega (pendiente, en camino, entregada) y los detalles asociados como (repartidor, hora de salida, hora de llegada) para supervisar el progreso de las entregas. | - El sistema debe permitir consultar una entrega específica mediante su identificador único o filtrando por otros campos como nombre del cliente, repartidor asignado o estado de la entrega.<br>- Si no se encuentra una entrega con los criterios de búsqueda proporcionados, el sistema debe mostrar un mensaje claro que indique "No se encontraron resultados".|
| **US-03** | Como administrador, quiero poder modificar los detalles de una entrega (por ejemplo, cambiar el repartidor asignado o la dirección de destino) en caso de errores o cambios de última hora para no permitir imprevistos afectando la calidad del servicio. | - Después de realizar los cambios y hacer clic en "Guardar", el sistema debe mostrar un mensaje de confirmación que indique que la entrega se ha actualizado correctamente.<br>- Si se cambia el repartidor asignado, el sistema debe notificar al nuevo repartidor sobre la entrega asignada.|
| **US-04** | Como administrador, quiero poder cancelar una entrega si el cliente así lo solicita o si surge algún problema que impida su realización para evitar confusiones. | - Antes de proceder con la eliminación, el sistema debe mostrar un mensaje de confirmación que pregunte: "¿Está seguro de que desea eliminar esta entrega?" con opciones para "Confirmar" o "Cancelar".<br>- Después de eliminar la entrega, el sistema debe mostrar un mensaje de confirmación que indique: "La entrega ha sido eliminada correctamente".|
| **US-05** | Como administrador o cliente, quiero poder rastrear la ubicación del repartidor en tiempo real durante una entrega, para conocer el progreso de la entrega. | - Solo los administradores y clientes con pedidos en proceso deben tener acceso al rastreo de la entrega.<br>- Se debe garantizar la seguridad y privacidad de la ubicación del repartidor.|


### **Historias de Usuario (Épica Metodos de pago)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como usuario autorizado, quiero realizar pagos de manera segura y eficiente, para completar mis compras sin inconvenientes y asegurar que la transacción sea procesada correctamente. | - Cuando los datos sean correctos y cumplan con los estándares de seguridad, el sistema debe proceder con el procesamiento de la transacción.|


>  **Instrucciones:**  
> - Completar al menos **tres historias de usuario**.  
> - Asegurar que cada historia tenga criterios de aceptación claros y verificables.  

---

## 3. Requisitos de Calidad  
Los requisitos de calidad se presentan en forma de **historias de calidad**, siguiendo la estructura de Len Bass.

### **Historias de Calidad**
| **ID**   | **Fuente** | **Estímulo** | **Artefactos** | **Entorno** | **Respuesta** | **Medida de Respuesta** |
|----------|-----------|-------------|---------------|------------|-------------|-----------------|
| **RQ-01** | Cliente | Se solicita que los datos del cliente estén cifrados y protegidos contra acceso no autorizado. | Módulo de autenticación |El token JWT está visible y podria ser robado o utilizado después de su expiración.| El sistema deberá asegurar que el token JWT no sea inaccesible después de su expiración, y si un usuario intenta acceder tras el tiempo de expiración, se debe requerir reautenticación.| El token no deberá ser accesible tras haber pasado 24 horas desde que se generó y el sistema debe invalidarlo inmediatamente una vez que haya expirado. |
| **RQ-02** | Sistema de gestión de inventario. | Actualización del stock de un producto debido a una venta. | Microservicio de gestión de inventario y microservicio de gestión de pedidos. | Operación normal. | El sistema de gestión de pedidos debe comunicar la venta al sistema de inventario y actualizar el stock en tiempo real, sin errores de sincronización. | Tiempo de sincronización entre los sistemas y número de errores de inventario detectados. |
| **RQ-03** | Repartidores | Solicitud de la ruta de entrega optimizada para los próximos 10 pedidos. | Microservicio de gestión de entregas. | Operación normal, con acceso a datos de tráfico en tiempo real. | El sistema debe proporcionar la ruta optimizada en un tiempo no mayor a 2 segundos. | Tiempo de respuesta promedio para generar la ruta y la eficiencia de la ruta propuesta (distancia total, tiempo estimado). |

>  **Instrucciones:**  
> - Completar al menos **tres historias de calidad**, alineadas con atributos clave como **rendimiento, escalabilidad y seguridad**.  
> - Definir cómo se medirá la respuesta esperada ante la situación planteada.  

---

## 4. Restricciones del Sistema  
Las restricciones establecen **limitaciones** en la arquitectura del sistema, ya sean tecnológicas, de negocio, regulatorias o de infraestructura.

### **Lista de Restricciones**
| **Tipo de Restricción** | **Descripción** |
|------------------------|----------------|
| Tecnológica | El sistema debe desarrollarse utilizando **Spring Boot y PostgreSQL**, debido a la infraestructura actual de la empresa y su compatibilidad con otros sistemas internos. |
| Tecnológica | El sistema debe diseñarse utilizando una arquitectura de microservicios, donde cada servicio sea independiente y escalable. |
| Regulatorias | El nombre del producto debe ser único dentro de su categoría. El precio no puede ser menor o igual a cero. Los campos obligatorios deben completarse antes de guardar un producto. |
| De negocio | El cliente solo puede ver y gestionar las órdenes que ha realizado él mismo. |
| De negocio | El repartidor solo puede ver las entregas asignadas a él. |
| De negocio | El administrador tiene acceso total a todas las órdenes y entregas. |

>  **Tipos de restricciones:**  
> - **Tecnológicas:** Lenguajes, frameworks o herramientas que deben utilizarse.  
> - **De negocio:** Normativas o estándares de la empresa.  
> - **Regulatorias:** Cumplimiento de normativas legales o de seguridad.  
> - **De infraestructura:** Limitaciones en hardware, red o almacenamiento.  


---

## 5. Formato de Entrega  
- **Formato:** Markdown (`.md`) en el repositorio de Codelabs, El documento debe llamarse `Arquitectura.md`.  
- **Extensión máxima:** 3 páginas.  

---

## **Tiempo estimado para completar el ejercicio: 50 minutos**  
**15 min** → Definir **3 historias de usuario con criterios de aceptación**.  
**15 min** → Definir **3 historias de calidad alineadas con atributos clave**.  
**10 min** → Identificar **2 restricciones relevantes**.  
**10 min** → Revisión y ajustes finales del documento. 