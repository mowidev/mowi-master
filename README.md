# Componente mowi-master

**1. Data general**
Component developed for search
Propiedad | Detalle
------------ | -------------
Versión | 2.1.17
Install | npm i mowi-master
Descripción | Componente desarrollado para implementar propiedades CRUD de una entidad, incluye buscador con filtros, incluye función para importar registros desde un archivo .csv



**2. Modo de instalación**
* Copiar el comando de instalación en la consola del proyecto: `npm i mowi-master --save`



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
loadingComponentLabel | String | Texto del mensaje en la pantalla de carga | No
loadingComponentClass | Strring | Nombre de la clase qeu se desea usar en la pantalla de carga | No

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

El componente presenta cuatro secciones generales:
* Sección 1: Búsqueda
* Sección 2: Botón para importar datos
* Sección 3: Tabla de resultados
* Sección 4: Personalización pantalla de carga


**Ejemplo básico de implementación:**


En el siguiente ejemplo se implementará el componente mowi-master para la gestión de registros llamados ‘noticias’. 
El ejemplo básico incluye la implementación de la sección de búsqueda de registros por filtros y la tabla de registros (Figura 1).


Figura 1: componente mowi implementado

**Pasos para la implementación del componente en un archivo .vue:**

* Paso 1 
Importar la librería en el archivo .vue: import MasterTemplate from 'mowi-master';

Img1.png

* Paso 2

Declarar la variable en la sección de componentes: MasterTemplate

Img2.png

* Paso 3

Implementar el componente con el tag <MasterTemplate />

Img3.png



**Customización de la sección de búsqueda por filtros:**


**Formato de la tabla:**


**Llenado de tabla:**


**Búsqueda de registros:**



En el archivo .vue, el componente mowi-master debe implementarse de la siguiente manera 

![GitHub Logo](/im4.png)

``` 
        <masterTemplate
          :filters="propsTemplateManangerOrder.filters"     
          :setDataTable="this.setDataTable"
          :dataLoadFunction="this.getOrders"
          :header="colum"
          :useMassiveSelector="useMassiveSelector"          
          :customActions="propsTemplateManangerOrder.customActions"  
          :data="orders"
          :tableTitle="titleTable"
          :flagUploadData="flagUploadData"
          :registerFields="registerFields"          
          :progressBarValue="progressBarValue"          
          :loadSummary ="loadSummary"
          :uploadFunction="this.uploadFunction"
          :setContent="this.setContentListComponent"
          :showSearchSection="this.showSearchSection"
          :tableSearch="this.searchOptionTableMaf"
          :loadingComponentLabel="this.loadingComponentLabel"
          :loadingComponentClass="this.loadingComponentClass"
        ></masterTemplate>      
      </div>
``` 

**4.1 Sección de búsqueda**

**4.1.1 Implementación**
Para implementar el buscador es necesario definir los filtros del buscador en la propiedad filters del componente:

Figura Ejemplo. Ejemplo de implementación

![GitHub Logo](/im5.png)

``` 
	[
	{
	name: //nombre del filtro
	value: //valor ingresado para el filtro
	operator: //si el tipo de filtro incluye operator
	type: //tipo numeric o text, si el filtro es de tipo inputComponent
	}
	]
``` 

* Paso 1: Setear la propiedad showSearchSelection = true en el componente
En la Figura Ejemplo , en la línea 26, la propiedad this.showSearchSection = true

* Paso 2:
Los filtros del buscador deben ser definidos en la propiedad filters. En la Figura Ejemplo , en la línea 16 se asignan los filtros

* Paso 3:
Configurar la propiedad dataLoadFunction, se debe declarar una función para esta propiedad. 

El componente mowi-master retorna un arreglo con los filtros con la siguiente estructura:

``` 
	[
	{
	name: //nombre del filtro
	value: //valor ingresado para el filtro
	operator: //si el tipo de filtro incluye operator
	type: //tipo numeric o text, si el filtro es de tipo inputComponent
	}
	]
``` 

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

![GitHub Logo](/im6.png)

```
        /**
        *  La función getParticipants() es la encargada de la búsqueda de registros
        *  se envía al componente <MasterTemplate> en la propiedad dataLoadFunction
        *  */ 
        async getParticipants(filters) {
          console.log('getClients: ', filters)
          var look =[]
          if(filters != null || filters != undefined){
               /**variables declaradas para el funcionamiento de la función */
              var filter={}
              /**declarar acá los parámetros necesarios para el servicio a utilizar
               * se filtra el arreglo recibido (selectFilters) para obtener el atributo con el nombre deseado
               */
              var fullName  =_.filter(filters,{'name':'fullName'});
              var documentNumber  =_.filter(filters,{'name':'documentNumber'});
              var email  =_.filter(filters,{'name':'email'});
              var phone  =_.filter(filters,{'name':'phone'});
              /**obtener los valores y agregarlos en el objeto filter */
              console.log('test', fullName[0])
              fullName.length > 0 ?  filter.fullName = fullName[0].value.variable : null
              documentNumber.length > 0 ?  filter.documentNumber = documentNumber[0].value.variable : null
              email.length > 0 ?  filter.email = email[0].value.variable : null
              phone.length > 0 ?  filter.phone = phone[0].value.variable : null
              filter.isClient = true

              console.log('filter obtenido ', filter)
              // /**consultar servicio con el objeto filter  */
              var response1 = await consultServices('searchClients','POST',filter);  
              response1.status == 200 ? look = response1.clients : null
          }else{
              look = await consultServices('listClients','POST',{});   
          }
          return look

        },
```

**4.2 Sección de importación**
Para la sección de importación, se deben configurar las siguientes propiedades del componente mowi-master

* flagUploadData
* registerFields
* loadSummary
* progressBarValue
* uploadFunction


**4.2.1 Pasos para implementar**
* Paso 1: Setear la propiedad flagUploadData con true
* Paso 2: Setear la propiedad con un arreglo de los campos del registro que se espera importar, cada objeto del arreglo debe tener la siguiente estructura:

```html
    registerFields:[
    {
    name://nombre del campo que se desea guardar
    displayValue: //valor que se expone en la pantalla
    }
    ]
```  
    


* Paso 3: Configurar la propiedad uploadFunction, se debe declarar una función para esta propiedad.
La propiedad deberá incluir el servicio que se consumirá para la subida masiva de los registros, en este paso se debe setear los atributos del arreglo que se envía en la propiedad loadSummary:

```html    
    this.loadSummary[0].total = array.length //total de registros subidos
    this.loadSummary[0].successful = //cantidad de registros subidos exitosos
    this.loadSummary[0].errors = //cantidad de errores
    this.loadSummary[0].detail = //detalle de errores
``` 

**4.2.2 Ejemplo de función uploadFunction**

![GitHub Logo](/im7.png)

```html 
    /**
     * uploadFunction() en esta función se realizará la subida masiva,
     * la lógica del consumo del servicio y los datos del resumen se debe desarrollar en esta funciónm
     */
    uploadFunction(array){
        this.progressBarValue = 0    
        var progress =100/array.length
        for (let index = 0; index < array.length; index++) {
            setTimeout(() => {
            //usar el servicio acá
            console.log('arreglo test', array[index])
            //luego de usar, sumar el progreso
            this.progressBarValue += progress    
            console.log('progressbar ', this.progressBarValue)        
            }, 1000);
        }
        var arrayDetail=['Este es un test','Este tambien']
        //cargar datos de resumen
        this.loadSummary[0].total = array.length
        this.loadSummary[0].successful = 2
        this.loadSummary[0].errors = 1
        this.loadSummary[0].detail = arrayDetail
        //retornar true
        return true     
        
    },
``` 

**4.2.3 Ejemplo de uso desde la pantalla**
* Paso 1: Clic en el botón Subir Data, se abrirá el siguiente modal con 4 pasos, seleccionar un archivo (solo .csv) y clic en continuar

![GitHub Logo](/im8.png)

* Paso 2:
Se cargará la primera fila (cabecera) y la segunda fila (primer registro) del archivo .csv
En el segundo paso se debe asignar qué campos de los registros del archivo .csv se importarán, para esto el usuario debe seleccionar el nombre del campo asignado a cada columna de la segunda fila del archivo .csv

Por ejemplo, el siguiente archivo .csv :
   ** tiene en la cabecera (primera fila): telefono, ,id,nombre
   ** tiene en el primer registro (segunda fila): 9902566,19,1,Jhon

![GitHub Logo](/im12.png)

Cada columna de la cabecera puede ser asignada para importar.
En este paso es posible cambiar campos (eliminar los campos seleccionados) o regresar al paso anterior (para seleccionar otro archivo)

![GitHub Logo](/im9.png)

En este ejemplo se asignaron sólo dos campos, clic en importar
* Paso 3: La barra de estado se cargará hasta finalizar la subida masiva

![GitHub Logo](/im10.png)

* Paso 4: El modal muestra el consolidado de la subida masiva

![GitHub Logo](/im11.png)

**4.3 Sección de tabla**

* data
* header
* setDataTable
* useMassiveSelector

**4.3.1 Ejemplo de implementación de sección**
* Paso 1:Configurar las columnas de la tabla, para esto setear la propiedad header del componente. Los atributos del arreglo que irá en la propiedad debe respetar el siguiente formato:

```html 
          this.colum = [
              {
	   name://nombre del atributo
	   label: //cabecera que se mostrará en la tabla 
               find:1, //Para activar la cabecera de búsqueda 1:true/ 2:false
               sort:1  //Para activar la opción de ordenar los registros 1:true/ 2:false
              },
          ]
```

* Paso 2: Configurar el formato de los objetos que se mostrarán en la tabla. Para esto, se debe setear la propiedad setDataTable con una función con la siguiente estructura:
```html 
        /**
*  La función setDataTable() es la encargada de darle el formato necesario al arreglo obtenido de la búsqueda
        *  el formato es definido por el programador
        *  se envía al componente <MasterTemplate> en la propiedad setDataTable
        *  */ 
        setDataTable(registers){
          var ctx = this;
          var formattedRegisters = [];
          suggestions.forEach(item => {
            var newRecord = {};
            var client= item.idClient
            newRecord.client = client.fullname;
            newRecord.phone= client.phone;
            newRecord.content = item.content;
            formattedRegisters.push(newRecord);
          }); 
          return formattedRegisters
        },
``` 

Los atributos de los objetos del arreglo formattedRegisters deben tener los mismos valores que los del campo value de los objetos de la propiedad header del componente

* Paso 3: Crear la función refreshData para actualizar los elementos de la tabla, utilizar las propiedad $refs para setear la data que se muestra en la tabla

```
        async refreshData(response){
          this.$refs.mowiMaster.setInitialData(response)
        },
```

**4.4 Sección de modal de loading**
Permite editar el testo del modal de carga y la clase que se aplicará al componente

Propiedad |  Descripción | Ejemplo
------------ | ------------- | ------------ 
loadingComponentClass | Permite asignar una propiedad al modal de carga |form_test
loadingComponentLabel | Permite asignar un mensaje al modal de carga |'Hola mundo'
