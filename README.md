**Documento de trabajo**

# **Buenas Prácticas Desarrollo de APIs-REST**

# **Asimov**

Versión [1.0] 

**Asimov Consultores®**   
Los Militares 5953   
Oficina 1906, Las Condes  
Santiago de Chile  
[contacto@asimov.cl](mailto:contacto@asimov.cl)  
[www.asimov.cl](http://www.asimov.cl)

# **Índice**

[**1\. Introducción**](#1.-introducción)

[2. Construcción de APIs](\#2.-construcción-de-apis)

[2.1. Formato de URLs](\#2.1-formato-de-urls)

[2.2 Encabezados HTTP](\#2.2-encabezados-http)

[2.3 Métodos HTTP](\#2.3-métodos-http)

[2.4 Respuestas](\#2.4-respuestas)

[2.5 Manejo de Errores](\#2.5-manejo-de-errores)

[2.6 Versiones](\#2.6-versiones)

[2.7 Límites de Registros](\#2.7-límites-de-registros)

[2.8 Codificación de caracteres](\#2.8-codificación-de-caracteres)

[2.9 JSONP](\#2.9-jsonp)

[3. Referencias](\#3.-referencias)


# **1. Introducción**

El presente documento tiene por objetivo describir buenas prácticas para el desarrollo de APIs-Rest.

La definición de estas recomendaciones responde a la experiencia en la ejecución de diferentes proyectos desarrollados.

# **2. Construcción de APIs**

A continuación, indicamos un conjunto de buenas prácticas semánticas y técnicas que deberían ser tomadas como base para la generación de APIs en un proyecto.

### **2.1 Formato de URLs**

1. El número de versión de la API debe ir incluida en la base de la URL Ej.: [https://ejemplo.cl/api/v2/solicitudes](https://ejemplo.cl/api/v2/solicitudes)  
2. Una URL debe identificar un recurso o una colección de recursos.  
3. La URL debe contener sustantivos, no verbos.  
4. Debe contener sustantivos en su forma plural.  
5. Se usan los métodos HTTP (GET, POST, PUT, DELETE) para operar sobre un recurso o una colección de recursos.  
6. La profundidad de una URL que describe un recurso no deberá ser más profunda que recurso/identificador/recurso.

Atributos opcionales:

1. Si cambian la lógica del resultado, deben ir en la url. Si son múltiples, deben ir separados por coma. Ej: [https://ejemplo.cl/api/v2/solicitudes?tema=salud,educacion\&sort=asc](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)  
2. Si no cambian la lógica del resultado, deben ir en el header HTTP. Ej: Autenticación OAuth.  
3. No utilizar acentos ni caracteres especiales en la URL.

Buenos ejemplos:

* Listado de solicitudes  
  GET [https://ejemplo.cl/api/v1/solicitudes](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)  
* Listado de solicitudes filtrado por tema y ordenado  
  GET [https://ejemplo.cl/api/v1/solicitudes?tema=salud,educacion\&sort=asc](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)

Malos ejemplos:

* Sustantivos en singular  
  GET [https://ejemplo.cl/api/v1/solicitud](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)  
  GET [https://ejemplo.cl/api/v1/solicitud/123](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)  
* Verbos en la URL  
  POST [https://ejemplo.cl/api/v1/solicitud/crear](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)  
* Atributos de filtro opcionales dentro de los segmentos de la URL  
  GET [https://ejemplo.cl/api/v1/solicitudes/asc](https://ejemplo.cl/api/v2/solicitudes?tema=salud\&sort=asc)

### **2.1.2 Encabezados HTTP** 

Dentro de los encabezados HTTP se deberá incluir el encabezado:

Content-type: application/json

**Tipos de datos**

El formato a usar será JSON.

JSON admite distintos tipos de datos estándar:

* Number: Un número que puede o no contener decimales.  
  Ej: 54, 55.3, 103.5  

* String: Una cadena de caracteres.  
  Ej: “hola mundo”, “auto”, “mesa”  

* Boolean: Indica verdadero o falso  
  Ej: true, false  

* Array: Un conjunto de elementos.  
  Ej: \[“hola mundo”, “mesa”, “auto”\]  

* Object: Se puede definir como un mapa de elementos.  
  Ej: {“id:” 2, “nombre”: “Arturo”, “apellido”: “zamorano”}

Algunos tipos de datos no son parte del estándar JSON, pero esta sería su forma recomendada de representarlos:

* Fechas: Se deberá usar un string con la fecha formateada usando el estándar [ISO 8601](http://www.w3.org/TR/NOTE-datetime)  
  Ej: “2001-08-22”, “18:30:45”, “1997-07-16T19:20:30”.

### **2.1.3 Métodos HTTP** 

Los métodos HTTP a usar deben ir en concordancia con lo definido en el [estándar HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). El método HTTP se aplicará sobre el recurso indicado en la URL.

La siguiente tabla muestra un resumen de los métodos HTTP y su uso:

| Método HTTP | Significado |
| ----- | ----- |
| GET | Obtiene un recurso. |
| POST | Crea un recurso. |
| PUT | Actualiza un recurso. |
| DELETE | Borra un recurso. |

*Tabla 17\. Métodos HTTP.*

Ejemplos:

| Recurso | GET | POST | PUT | DELETE |
| ----- | ----- | ----- | ----- | ----- |
| /mapaches | Obtiene el listado de mapaches. | Crea un mapache nuevo. | Actualización masiva de mapaches. | Borra todos los mapaches |
| /mapaches/123 | Obtiene el mapache identificado como 123 | Error | Si existe el mapache 123 lo actualiza, si no error. | Borra el mapache identificado como 123\. |

*Tabla 18\. Ejemplos de recursos.*

### 

### **2.1.4 Respuestas** 

Las respuestas deberán ir en formato JSON.

Si es una colección de elementos, deberán venir en un arreglo JSON.

Si es un elemento deberá venir en un objeto JSON.

En las llaves no deberán venir valores.

Si la respuesta requiere metadata adicional, esta metadata deberá incluir información sobre las propiedades de la respuesta. No sobre los miembros de la respuesta en sí.

**Buenos ejemplos**  
```js
{  
"solicitudes": \[  
 {"id": "125", "nombre": "Solicitud de acceso a ..."},  
 {"id": "834", "nombre": "Solicitud de acceso a ..."}  
\]  
}
```

**Malos ejemplos**

* El valor identificatorio se está usando como llave.
```js
{  
"solicitudes": \[  
 {"125": "Solicitud de acceso a ..."},  
 {"834": "Solicitud de acceso a ..."}  
\]  
}
```

### **2.1.5 Manejo de Errores** 

Los errores deberán ser devueltos utilizando los errores estándares HTTP en el header de respuesta.

| Código HTTP | Descripción |
| ----- | ----- |
| 200 | Ok |
| 400 | Parámetros incorrectos en la entrada |
| 401 | Token de acceso expirado o inválido |
| 403 | Autenticación OAuth incorrecta |
| 404 | Recurso no encontrado |
| 405 | Método HTTP no esperado. Por ej: se esperaba un HTTP GET y se recibió un HTTP POST. |
| 429 | Se están recibiendo muchos requests de parte de tu app. Se está limitando el acceso. |
| 500 | Error interno del servidor. |

*Tabla 19\. Códigos de error HTTP.*

Además, en el contenido de la respuesta se deberá incluir un objeto JSON con los siguientes campos:

| Llave | Valor |
| ----- | ----- |
| codigo | Código interno de error |
| mensaje | Descripción del mensaje de error |
| mas\_info\_url (Opcional) | URL donde se describe más información sobre el mensaje de error. |

*Tabla 20\. Estructura objeto JSON.*

### **2.1.6 Versiones**

Las APIs siempre deberán incluir un número de versión.

La numeración de las versiones solamente deberá incluir números enteros. No decimales.

Se debe anteponer el carácter ‘v’ antes del número de versión.

Buenos ejemplos: v1 v2 v3 v4

Malos ejemplos: v1,5 v1.5 1.8

### **2.1.7 Límites de Registros**

Si no se especifica un límite, entregar los resultados con un límite por defecto.

Para especificar límites utilizar parámetros opcionales limit y offset.

| Parámetro | Descripción |
| ----- | ----- |
| offset | Cuántos registros debe saltarse antes de comenzar. |
| limit | Cantidad de registros a desplegar. |

*Tabla 21\. Parámetros límite de registros.*

Ej.: para obtener los registros del 51 al 78

https://ejemplo.cl/v1/solicitudes?offset=50\&limit=28

### **2.1.8 Codificación de caracteres** 

La codificación a utilizar deberá ser UTF-8.

Los caracteres especiales pueden ser escapados según lo especificado en el [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt).

### **2.1.9 JSONP**

JSONP o JSON con padding es una técnica de comunicación utilizada en los programas JavaScript para realizar llamadas asíncronas a dominios diferentes. JSONP es un método concebido para suplir la limitación de AJAX entre dominios, que únicamente permite realizar peticiones a páginas que se encuentran bajo el mismo dominio y puerto por razones de seguridad.

Utilizando JSONP se puede permitir el uso de nuestra API desde un navegador web que esté visitando una web con un dominio distinto al de nuestra API.

Para habilitar JSONP, nos podemos guiar con el siguiente ejemplo.

Si el recurso original es:

[http://ejemplo.cl/v1/solicitudes](http://ejemplo.cl/v1/solicitudes)

Si la respuesta original es:
```js
{  
	solicitudes: \[  
		{"id":435, "nombre": "Solicitud ..."}  
\]  
}
```

Para solicitar el resultado con JSONP deberemos incluir el parámetro callback  
[http://ejemplo.cl/v1/solicitudes?callback=micallback](http://ejemplo.cl/v1/solicitudes?callback=micallback)

La respuesta que debería ser entregada es la siguiente:  

```js
micallback({  
	solicitudes: \[  
		{"id":435, "nombre": "Solicitud ..."}  
\]  
});
```

## 3. Referencias**

A continuación se listan algunas referencias adicionales asociadas:

* [Best Practices for a Pragmatic Restful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)  
* [json-p.org](http://json-p.org)  
* [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt)  
* [RFC 7159](http://tools.ietf.org/html/rfc7159)  
* [ISO 8601](http://www.w3.org/TR/NOTE-datetime)


