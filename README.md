# Componente mowi-master

**1. Data general**
Component developed for search
Versión | 2.1.11
------------ | -------------
Install | npm i mowi-master
Descripción | Componente desarrollado para implementar propiedades CRUD de una entidad, incluye buscador con filtros, incluye función para importar registros desde un archivo .csv

**2. Modo de instalación**
* Copiar el comando de instalación en la consola del proyecto: `npm i mowi-master --save`
* Importar la librería en el archivo .vue:
`import MasterTemplate from 'mowi-master';`
* Declarar la variable en la sección de componentes:
`MasterTemplate`

**3. Propiedades del componente**
**3.1 Propiedades generales**
Propiedad | Tipo | Descripción | Subpropiedades
------------ | ------------- | ------------ | -------------
filters | Array | Arreglo donde se define los filtros para la búsqueda de registros | Si
setDataTable | Function |Brinda el formato definido por el programador para la tabla de resultados del componente. | No
dataLoadFunction | Function | Función donde se encuentra la lógica de búsqueda establecida por el programador. | No
header | Array | Arreglo donde se define la cabecera de la tabla del componente. |No
useMassiveSelector | Boolean | Booleano que define el uso de selectores en las columnas de la tabla. | No
data | Array | Arreglo que se utiliza para poblar la tabla del componente | No
customActions | String | Campo para agregar otras acciones a la tabla de resultado | No
tableTitle | String | Título de la tabla del componente | No
flagUploadData | Boolean | Booleano que define el uso de la función para importar data desde un archivo .csv | No
registerFields | Array | Arreglo donde el programador define los atributos del registro para la importación de datos | No
progressBarValue | Number | Valor de progreso de la barra de carga cuando se importan archivos | No
loadSummary | Array | Arreglo que define el resumen luego de importar los registros | Si
uploadFunction | Function | Función donde se encuentra la lógica | No
setContent | Function | Función para actualizar el contenido de un componente del tipo lista |No
showSearchSection | Boolean | Mostrar/ocultar sección de búsqueda | No
tableSearch | Boolean | Mostrar/ ocultar cabecera de búsqueda en la tabla | No

**3.2 Propiedades de los tipos de filtros**
La propiedad Filters admite 4 tipos de filtros para búsqueda
* Filtro tipo buscador

Propiedad | descripción
------------ | -------------
name | nombre de filtro
filterType | tipo de filtro
label  | Etiqueta que verá el usuario para el filtro
arrayData | Arreglo de datos que se consultan en el filtro
displayNameKey | nombre del atributo que se mostrará como etiqueta en el filtro
addNewValue | Atributo para permitir agregar nuevos valores en filtro de búsqueda (addNewValue =‘Y’ para agregar)
operators | Booleano para determinar si el filtro  necesita operadores (consultar lista de operadores)
selectField | Booleano para determinar si este filtro debe pertenecer a la sección de búsqueda del componente 
isVisible | Booleano para determinar si este filtro es visible en la sección de búsqueda del componente
isRequerid | Booleano para determinar si este filtro es obligatorio para la búsqueda del componente


* Filtro tipo input

Propiedad | descripción
------------ | -------------
name | Nombre del filtro
filterType | Tipo de filtro
type | Tipo de input para determinar los operadores (numeric o text)
valueInput | Valor capturado en el filtro
label | Etiqueta que verá el usuario para el filtro
validation | Objeto para definir validaciones al filtro (consultar lista de validaciones)
textArea | Booleano para determinar si el filtro es del tipo área de texto
operators | Booleano para determinar si el filtro  necesita operadores (consultar lista de operadores)
selectField | Booleano para determinar si este filtro debe pertenecer a la sección de búsqueda del componente 
isRequerid | Booleano para determinar si este filtro es obligatorio para la búsqueda del componente


* Filtro tipo list

Propiedad | descripción
------------ | -------------
name | Nombre del filtro
filterType | Tipo de filtro 
label | Etiqueta que verá el usuario para el filtro
selectedValue | Valor capturado en el filtro
isVisible | Booleano para determinar si la etiqueta del filtro debe ser visible 
content | Arreglo con las opciones del filtro,para cada opción se debe especificar el campo value(valor que se obtiene) y label(etiqueta de la opción en el filtro)
operators | Booleano para determinar si el filtro  necesita operadores (consultar lista de operadores)
selectField | Booleano para determinar si este filtro debe pertenecer a la sección de búsqueda del componente 
isRequerid | Booleano para determinar si este filtro es obligatorio para la búsqueda del componente


* Filtro tipo date

Propiedad | descripción
------------ | -------------
name | Nombre del filtro
filterType | Tipo de filtro
label | Etiqueta que verá el usuario para el filtro
dateRange | Atributo para establecer las fechas de inicio y fin de un rango
mode | range(capturar un rango de fechas), single(capturar solo una fecha)
selectedDate | Valor capturado por el filtro
operators | Booleano para determinar si el filtro  necesita operadores (consultar lista de operadores)
selectField | Booleano para determinar si este filtro debe pertenecer a la sección de búsqueda del componente 
isRequerid | Booleano para determinar si este filtro es obligatorio para la búsqueda del componente


**4. Modo de uso**
El componente presenta tres secciones generales:
* Sección 1: Búsqueda
* Sección 2: Botón para importar datos
* Sección 3: Tabla de resultados

En el archivo .vue, el componente mowi-master debe implementarse de la siguiente manera 

**4.1 Sección de búsqueda**
**4.1.1 Implementación**
Para implementar el buscador es necesario:
Figura Ejemplo. Ejemplo de implementación

* Paso 1: Setear la propiedad showSearchSection = true en el componente
En la Figura Ejemplo , en la línea 26, la propiedad this.showSearchSection = true

* Paso 2:
Los filtros del buscador deben ser definidos en la propiedad filters. En la Figura Ejemplo , en la línea 16 se asignan los filtros

* Paso 3:
Configurar la propiedad dataLoadFunction, se debe declarar una función para esta propiedad. 

El componente mowi-master retorna un arreglo con los filtros con la siguiente estructura:

`[
{
	name: //nombre del filtro
	value: //valor ingresado para el filtro
	operator: //si el tipo de filtro incluye operator
	type: //tipo numeric o text, si el filtro es de tipo inputComponent
}
]`


**4.1.2 Uso de operadores**
Los filtros del tipo inputComponent soportan el uso de componentes, la siguiente tabla presenta los operadores para los filtros del tipo inputComponent  para texto (text) y números (numeric)

* Operadores numéricos
  * {label:'Igual a',value:'='}
  * {label:'Mayor que', value:'>'}
  * {label:'Menor que', value:'<'}
  * {label:'Mayor igual', value:'>='}
  * {label:'Menor igual',value:'<=' }

* Operadores de texto
  * {label:'Contiene',value:'include'}
  * {label:'Es igual a', value:'equals'}
  * {label:'Empieza con', value:'%like'} 
  * {label:'Termina con', value:'like%'} 
  * {label:'Vacío',value:'null' }


**4.1.3 Ejemplo de función para búsqueda**
**4.2 Sección de importación**
Para la sección de importación, se deben configurar las siguientes propiedades del componente mowi-master

* flagUploadData
* registerFields
* loadSummary
* progressBarValue
* uploadFunction


**4.2.1 Pasos para implementar**
* Paso 1: Setear la propiedad flagUploadData con true
* Paso 2: Setear la propiedad con un arreglo de los campos del registro que se espera importar, cada objeto del arreglo debe tener la siguiente estructura
`registerFields:[
       {
               name://nombre del campo que se desea guardar
	   displayValue: //valor que se expone en la pantalla
       }
]
`
* Paso 3: Configurar la propiedad uploadFunction, se debe declarar una función para esta propiedad.
La propiedad deberá incluir el servicio que se consumirá para la subida masiva de los registros, en este paso se debe setear los atributos del arreglo que se envía en la propiedad loadSummary:
`
this.loadSummary[0].total = array.length //total de registros subidos
this.loadSummary[0].successful = //cantidad de registros subidos exitosos
this.loadSummary[0].errors = //cantidad de errores
this.loadSummary[0].detail = //detalle de errores
`
**4.2.2 Ejemplo de función uploadFunction**




