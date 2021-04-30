# Desafio para Backend Software Engineer


## Descripción del problema

En **La Haus** trabajamos con `propiedades` de personas que están vendiendo su inmueble con nosotros, y sobre `usuarios` que tienen interés en comprar esas `propiedades`. El objetivo de este desafío es realizar una aplicación la cual exponga un _API_ que permita realizar algunas operaciones de _CRUD_ para cada una de esas dos entidades con algunas reglas de negocio sobre ellas.


### Propiedades

Una propiedad tiene la información básica sobre el inmueble que será anunciado por nosotros. 

#### Representación de una propiedad

A continuación un ejemplo de la representación _JSON_ de una propiedad, por ejemplo para representar una propiedad:

```json
{
	"id": 10,
	"title": "Apartamento cerca a la estación de transmilenio",
	"description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Chapinero",
	"location": {
		"longitude": -74.0665887,
		"latitude": 4.6371593
	},
	"pricing": {
		"salePrice": 450000000,
		"administrativeFee": 250000
	},
	"propertyType": "HOUSE",
	"bedrooms": 3,
	"bathrooms": 2,
	"parkingSpots": 1,
	"area": 60,
	"photos": [
		"https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg",
		"https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
	],
	"createdAt": "2020-05-10T04:20:33Z",
	"updatedAt": "2020-05-10T05:30:00Z",
	"status": "INACTIVE"
}
```

#### Reglas sobre propiedades

1. Los _ids_ deben ser numéricos generados automáticamente a partir del número 1.
2. Los campos `title`, `location`, `pricing.salePrice`, `bedrooms`, `bathrooms`, `area`, `propertyType` son obligatórios.
3. Los campos `createdAt`, `updatedAt` son automáticamente generados por el sistema. La API no debería permitir modificaciones sobre ellos.
4. El campo `propertyType` puede tener los valores {`HOUSE`, `APARTMENT`}

Para propósito del ejercicio vamos a tener algunas reglas sobre si una propiedad es válida o no dependiendo su tipo.

5. Caso la propiedad sea una Casa `"propertyType": "HOUSE"`
	
    5.1. El campo `bedrooms` tiene como valor mínimo 1 y valor máximo 14.
	
    5.2. El campo `bathrooms` tiene como valor mínimo 1 y valor máximo 12.
	
    5.3. El campo `area` tiene como valor mínimo 50 y valor máximo 3000.
	
    5.4. El campo `parkingSpots` es opcional, pero caso tenga valor tiene como valor mínimo 0.

6. Caso la propiedad sea una Apartamento `"propertyType": "APARTMENT"`
   
	6.1. El campo `bedrooms` tiene como valor mínimo 1 y valor máximo 6.

	6.2. El campo `bathrooms` tiene como valor mínimo 1 y valor máximo 4.

	6.3. El campo `area` tiene como valor mínimo 40 y valor máximo 400.

	6.4. El campo `parkingSpots` es opcional, pero caso tenga valor tiene como valor mínimo 1.

7. El campo `status` puede tener los siguientes valores: 
 - `ACTIVE`: Un inmueble que cumple con todas las reglas anteriormente descritas y está listo para ser anunciado
 - `INACTIVE`: Un inmueble que cumple con todas las reglas anteriormente descritas, pero que no va a ser anunciado. 

Las propiedades que no pueden ser anunciadas porque no cumplen con las reglas de validación, las consideraremos como inválidas. Parte del ejercicio es ver como usted lidia con estas propiedades.

8. Nosotros tenemos inmuebles en varias ciudades de Colombia y México. Para este ejercicio solo vamos a tener inmuebles con estado activo `"status":"ACTIVE"`, aquellos anuncios de inmuebles que están localizados en Ciudad de México y cumplen con las otras reglas de validación. Para simplificar este ejercicio, los inmuebles de Ciudad de México van a ser aquellos que estén dentro de este [Bounding Box](http://bboxfinder.com/#19.296134,-99.296741,19.661237,-98.916339) . Más información sobre qué es un Bounding Box en este [link](https://wiki.openstreetmap.org/wiki/Bounding_Box). Inmuebles fuera de ese bounding box se entenderán como propiedades en Colombia; y para el proposito de este ejercicio, estaran inactivas.


9. Para Ciudad de México, el valor del precio de venta puede ser entre 1 y 15 millones de pesos mexicanos. En las otras ciudades colombianas, el valor del precio de venta puede ser entre 50 millones y 3.500 millones de pesos colombianos.

#### Desafío

Usando las siguientes estructuras de `Request`, `Body` y `Response` permita las siguientes funcionalidades. Para crear o alterar propiedades, tenga en cuenta las reglas descritas anteriormente.


1. **Cree nuevas propiedades.**

_Request_:
```
POST v1/properties
```
_Body_:

```json
{
	"title": "Apartamento cerca a la estación de transmilenio",
	"location": {
		"longitude": -74.0665887,
		"latitude": 4.6371593
	},
	"pricing": {
		"salePrice": 450000000,
	},
	"propertyType": "HOUSE",
	"bedrooms": 3,
	"bathrooms": 2,
	"parkingSpots": 1,
	"area": 60,
	"photos": [
		"https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg",
		"https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
	]
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

2. **Actualice una propiedad**
 
_Request_:

```
PUT v1/properties/{id}
```

_Body_:

```json
{
	"id": 52,
	"title": "Apartamento en Chapinero, cerca a la estación de transmilenio",
	"description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Chapinero",
	"location": {
		"longitude": -74.0665887,
		"latitude": 4.6371593
	},
	"pricing": {
		"salePrice": 450000000,
		"administrativeFee": 250000
	},
	"propertyType": "HOUSE",
	"bedrooms": 3,
	"bathrooms": 2,
	"parkingSpots": 1,
	"area": 60,
	"photos": [
		"https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg",
		"https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
	],
	"createdAt": "2020-05-10T04:20:33Z",
	"updatedAt": "2020-05-10T05:30:00Z",
	"status": "INACTIVE"
}
```

_Response_:
  
Usted define la respuesta, hace parte del desafío

3. **Buscar propiedades**

Retorna las propiedades filtradas por un _BoundingBox_ (opcional). Los resultados vienen paginados y organizados por fecha de actualización del más reciente al más antiguo. Si no tiene fecha de actualización, debe usar la fecha de creación para ordenar.

_Request_:
  
```
GET v1/properties?status={status}&bbox={bbox}&page={pageNumber}&pageSize={pageSize}
```

Donde:

- `status`: Es el filtro por el estado de la propiedad; es un parámetro opcional y su valor default es `ALL`. Puede tener los siguientes valores:
	- `ALL`: Retorna todas las propiedades sin importar su valor en el campo el estado
	- `ACTIVE`: Retorne las propiedades activas
	- `INACTIVE`: Retorne las propiedades inactivas
	- `INVALID`: Retorne las propiedades inválidas
- `bbox`: Es un [BoundingBox](https://wiki.openstreetmap.org/wiki/Bounding_Box) para filtrar propiedades. Es un parámetro opcional, su valor default es `null`, es decir debe retornar todas las propiedades. El formato del parámetro esperado en la url es `bbox={minLongitude},{minLatitude},{maxLongitude},{maxLatitude}`. Si en el *request* no hay especificado un BoundingBox, retorne las propiedades sin importar su localización
- `page`: Es el número de la página; es un parámetro opcional y su valor default es 1
- `pageSize`: Es el tamaño solicitado de resultados en la página. Es un parámetro opcional, su valor default es 10, y su valor máximo es 20.


_Response_:

La respuesta debe seguir la siguiente estructura de campos:

- `page`: La página actual de los resultados
- `pageSize`: El máximo número de resultados retornado por página
- `total`: El número total de propiedades
- `totalPages`: El número total de páginas que contienen resultados para la búsqueda hecha.
- `data`: Un array con los objetos conteniendo las propiedades solicitadas en el request. Para ver en detalle la estructura de una `propiedad`, por favor avance a la siguiente sección.

```json
{
	"page": 1,
	"pageSize": 10,
	"totalPages": 1,
	"total": 1,
	"data": [
		{
			"id": 52,
			"title": "Apartamento en Chapinero, cerca a la estación de transmilenio",
			"description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Chapinero",
			"location": {
				"longitude": -74.0665887,
				"latitude": 4.6371593
			},
			"pricing": {
				"salePrice": 450000000,
				"administrativeFee": 250000
			},
			"propertyType": "HOUSE",
			"bedrooms": 3,
			"bathrooms": 2,
			"parkingSpots": 1,
			"area": 60,
			"photos": [
				"https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg",
				"https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
			],
			"createdAt": "2020-05-10T04:20:33Z",
			"updatedAt": "2020-05-10T05:30:00Z",
			"status": "INACTIVE"
		}
	]
}
```

### Usuarios

Tenemos usuarios que guardan como favoritas algunas propiedades. Para propósito de este ejercicio, la única información con la que vamos a contar sobre el usuario es su email. Y los usuarios pueden tener algunas propiedades marcadas como favoritas.

Solo si el usuario está autenticado puede marcar como favoritas sus propiedades, y listar sus propiedades favoritas. 

Queda a su criterio escoger una forma para autenticar el usuario; para facilitar las pruebas sobre este ejercicio, solicitamos que el valor de la contraseña de cada usuario sea su email.

#### Desafío

Usando las siguientes estructuras de `Request`, `Body` y `Response` permita las siguientes funcionalidades. Para crear o alterar propiedades, tenga en cuenta las reglas descritas anteriormente.


1. **Cree un usuario**
 
_Request_:

```
POST v1/users/{id}
```

_Body_:

```json
{
	"email": "code-challenge-lahaus@test.lh"
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

2. **Autentique un usuario**

_Request_:

```
POST v1/users/login
```

_Body_:
```json
{
	"email": "code-challenge-lahaus@test.lh",
	"password": "code-challenge-lahaus@test.lh"
}
```

_Response_:

Usted define la respuesta, hace parte del desafío


3. **Marque como favorita la propiedad de un usuario autenticado**

_Request_:

Adapte su *request* para enviar los datos de autenticación del usuario

```
POST v1/users/me/favorites
```

_Body_:
```json
{
	"propertyId": 52
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

4. **Retorna las propiedades favoritas del usuario autenticado**

Para este ejercicio solo retorne las propiedades favoritas del usuario que están activas.

_Request_:

Adapte su *request* para enviar los datos de autenticación del usuario

```
GET v1/users/me/favorites
```

_Body_:
```json
{
	"page": 1,
	"pageSize": 10,
	"totalPages": 1,
	"total": 1,
	"data": [
		{
			"id": 52,
			"title": "Apartamento en Chapinero, cerca a la estación de transmilenio",
			"description": "Apartamento con 3 cuartos y 2 baños localizado a 100 metros de la avenida caracas en la zona de Chapinero",
			"location": {
				"longitude": -74.0665887,
				"latitude": 4.6371593
			},
			"pricing": {
				"salePrice": 450000000,
				"administrativeFee": 250000
			},
			"propertyType": "HOUSE",
			"bedrooms": 3,
			"bathrooms": 2,
			"parkingSpots": 1,
			"area": 60,
			"photos": [
				"https://cdn.pixabay.com/photo/2014/08/11/21/39/wall-416060_960_720.jpg",
				"https://cdn.pixabay.com/photo/2016/09/22/11/55/kitchen-1687121_960_720.jpg"
			],
			"createdAt": "2020-05-10T04:20:33Z",
			"updatedAt": "2020-05-10T05:30:00Z",
			"status": "ACTIVE"
		}
	]
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

## Criterios de calificación

Esperamos que el código que usted va a crear sea considerado por usted como _"Production Ready"_; por favor use las buenas prácticas a las cuáles usted está acostumbrado en su rutina de desarrollo de código.

Para la evaluación de su código, esperamos que su código sea portable. Esperamos que usted nos provea un comando para correr fácilmente en el ambiente local, la solución del problema.

Para el desarrollo del desafío usted puede utilizar alguna de las siguientes lenguajes de programación:

- Ruby, 
- Python, 
- Golang, 
- Java

Dentro de los criterios que vamos a tener en cuenta a la hora de revisar su código, revisaremos:

- Resuelve el problema propuesto
- Organización y estructura del proyecto
- Mantenibilidad
- Facilidad para hacer tests
- Valoraremos adicionalmente si usa alguna arquitectura limpia.

## Entrega del proyecto

Para revisitar la descripción de cómo entregar el proyecto, por favor continúe leyendo la documentación principal a partir de [aquí](../README.md#formato-para-entrega-de-la-prueba).
