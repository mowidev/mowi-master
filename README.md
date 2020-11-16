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

name | nombre de filtro
------------ | -------------
filterType | tipo de filtro
