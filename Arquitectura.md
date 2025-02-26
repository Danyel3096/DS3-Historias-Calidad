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

### **Historias de Usuario (Cliente)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como cliente de la aplicación, quiero registrar mi cuenta a través de un formulario, diligenciando mis datos personales en la plataforma, para obtener mi historial de pedidos y realizar nuevas compras. | - El cliente debe ingresar un correo y contraseña válidos.<br>- Si las credenciales son correctas, el cliente es redirigido a su perfil.<br>- Si las credenciales son incorrectas, se muestra un mensaje de error sin revelar detalles específicos.<br>- El cliente puede solicitar un restablecimiento de contraseña. |
| **US-02** | Como cliente de la aplicación, quiero registrar mi cuenta vinculando mi cuenta de correo Gmail, para ingresar automáticamente mis datos personales en la plataforma, para obtener mi historial de pedidos y realizar nuevas compras. | - El cliente debe dar clic izquierdo en el botón “Registrarme con Gmail”.<br>- Si acepta la vinculación y las credenciales son correctas, el cliente es redirigido a su perfil.<br>-  Si se niega la vinculación o las credenciales son incorrectas, se muestra un mensaje de error sin revelar detalles específicos.<br>- El cliente puede solicitar un restablecimiento de contraseña.                                                                                                                                                           |
| **US-03** | Como cliente registrado, quiero iniciar sesión en la plataforma de manera segura utilizando OAuth2, para obtener un token JWT para acceder al sistema. | - El cliente debe ingresar un correo y contraseña válidos.<br>- Si las credenciales son correctas, el cliente es redirigido a la pagina principal del sistema.<br>- Si las credenciales son incorrectas, se muestra un mensaje de error.<br>- El cliente puede solicitar un restablecimiento de contraseña.                                                                                                                                                                                                                                                                             |


### **Historias de Usuario (Administrador)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como administrador, quiero ver todas las órdenes de compra para gestionarlas (aprobar, cancelar, etc.)                                                                                                   | - El administrador puede acceder a una lista de todas las órdenes de compra en el sistema.<br> - El administrador puede filtrar las órdenes por estado (en proceso, enviado, entregado, cancelado).<br> - El administrador puede cambiar el estado de una orden (por ejemplo, de “en proceso” a “enviado”).<br> - El administrador puede cancelar una orden, lo que notifica al cliente y libera el stock de los productos.                                                                                              |
| **US-02** | Como administrador, quiero crear nuevas categorías de productos, para organizar los productos de manera eficiente y mejorar la navegación del sistema                                                                                                   | - Debe existir un formulario con un campo obligatorio para ingresar el nombre de la nueva categoría.<br> - El nombre de la categoría debe ser único y no debe permitir duplicados.<br> - El sistema debe validar que el campo de nombre no esté vacío antes de guardar la categoría.<br> - Al crear una categoría correctamente, debe mostrarse un mensaje de confirmación como "Categoría creada exitosamente".<br> - Si la categoría ya existe, debe mostrarse un mensaje de error indicando que el nombre ya está en uso.                                                                                                                                                                                                                                                                              |
| **US-03** | _[Agregar historia de usuario]_                                                                                                   | _[Agregar criterios de aceptación]_                                                                                                                                                                                                                                                                                    |


### **Historias de Usuario (Repartidor)**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | _[Agregar historia de usuario]_                                                                                                   | _[Agregar criterios de aceptación]_                                                                                                       |
| **US-02** | _[Agregar historia de usuario]_                                                                                                   | _[Agregar criterios de aceptación]_                                                                                                                                                                                                                                                                                    |
| **US-03** | _[Agregar historia de usuario]_                                                                                                   | _[Agregar criterios de aceptación]_                                                                                                                                                                                                                                                                                    |


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
| Arquitectónica | El sistema debe diseñarse utilizando una arquitectura de microservicios, donde cada servicio sea independiente y escalable. |

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
