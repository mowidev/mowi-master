<template>
  <div>
    <LoadingComponent v-if="isLoading" 
    :label="loadingComponentLabel"
    :classLoading="loadingComponentClass"></LoadingComponent>

    <div  v-if="showSearchSection == true" class="card" id="accordion" style="padding-bottom: 0px;">
      
                <div class="card-header" style="padding-left: 15px;"  data-toggle="collapse" href="#collapseTag" role="button" aria-expanded="false" aria-controls="collapseExample">
                   <h2>{{headingTitleFinal}}</h2>
                   <div class="clearfix">
                   </div>
                </div>
                <div id="collapseTag" class="card-collapse collapse in">
                    <div class="card-body">                      
                       <div class="card-body" style="padding-bottom:5px; padding-top: 10px;">
                         
                          <div  class="card-body" style="border: 1px solid #E6E9ED;margin-bottom: 20px;" v-if="_flagSearchHistory == true"  >                             
                              <h3 style="padding-top: 10px;">Búsquedas guardadas </h3>
                              <div class="form-group" style="margin-top: 10px;">
                                <label class="control-label col-md-3 col-sm-3 col-xs-12">Selecciona un arreglo de filtros:</label>
                                <div class="col-md-6 col-sm-6 col-xs-12">
                                  <select class="form-control col-md-12 col-sm-12 col-xs-12" v-model="selectedArray" @change="useSearch()" >
                                    <option value="NEW">Nueva búsqueda</option>
                                    <option v-for="value in searchHistory" :value="value"  >{{value.label}}</option>
                                  </select>
                                </div>                               
                              </div>                              
                          </div>     

                          <div  class="card-body" style="border: 1px solid #E6E9ED;margin-bottom: 20px;" v-if="_flagListFilters == true"  >                             
                              <h3 style="padding-top: 10px;">Seleccione más filtros para la búsqueda </h3>
                              <div class="form-group" style="margin-top: 10px;">
                                <label class="control-label col-md-3 col-sm-3 col-xs-12">Selecciona un filtro:</label>
                                <div class="col-md-6 col-sm-6 col-xs-12">
                                  <treeselect v-model="arrayAddedFilters"                                
                                  :value-consists-of="valueConsistsOf"
                                  :multiple="true"
                                  :options="optionsFilters" />
                                </div> 
                                <button type="button"  @click="addFilter()" class="btn btn-success btn-xs float-right">Actualizar filtros</button>                               
                              </div>                                                            
                          </div>  

                          <div  class="card-body" style="border: 1px solid #E6E9ED;margin-bottom: 20px;" >
                            
                            <div class="form-group" style="margin-top: 20px; margin-bottom: 30px;" v-if="_flagSearchHistory ==true">
                                <h3 class="control-label col-md-3 col-sm-3 col-xs-12">Nombre de la búsqueda:</h3>
                                <div class="col-md-6 col-sm-6 col-xs-12">
                                    <input type="text"  v-if="showEditTitleSearch == true"  v-model="titleSearch"  class="form-control col-md-12 col-xs-12" >
                                    <input type="text"  v-if="showEditTitleSearch == false" v-model="titleSearch"  class="form-control col-md-12 col-xs-12" readonly>
                                </div>
                                <a @click="editTitleSearch()" class="btn btn-warning btn-xs"><i class="fa fa-pencil"></i></a>
                            </div>
                            

                            <MasterAdministrator
                              :filters="filtersMasterAdministrator"          
                              :buttonFilter="buttonFilter"                           
                              ref ="masterAdministartor"
                            ></MasterAdministrator>

                            <div class="card-body">
                              <button type="button"  @click="runSearch()" class="btn btn-success btn-xs float-right">Buscar</button> 
                              <button type="button"  @click="clearSearch()" class="btn btn-success btn-xs float-right">Limpiar campos</button>        
                              <button v-if="flagSaveSearch == true && _flagSearchHistory==true" type="button" @click="saveSearch()" class="btn btn-success btn-xs float-right">Guardar búsqueda</button>                   
                            </div>
                           
                          </div> 

                       </div>

                    </div>
                </div>      
    </div>

    <div v-if="flagUploadData == true" class="card-header-actions">
      <button type="button"  data-toggle="modal" data-target=".bs-example-modal-import"   class="btn btn-success btn-xs show-modal">Subir data</button> 
    </div>    
    <TableMaf 
    :header="header" 
    :data="data" 
    :tableTitle="tableTitle" 
    ref="tableMaf" 
    :useMassiveSelector="useMassiveSelector"  
    :searchOption="tableSearch"  ></TableMaf>
  
    <!-- Modal subida de archivos .CSV -->
    <div id="myModal" class="modal fade bs-example-modal-import" tabindex="-1" role="dialog" aria-hidden="true">
        <div class="modal-dialog modal-lg">
        <div class="modal-content">            
            <div class="modal-header">
                <h3 class="modal-title" id="myModalLabel" >Cargar registros desde un archivo .CSV </h3>          
            </div>
            <div class="modal-body">
                  <!--Mostrar pasos -->
                  <div>
                    <vue-step :now-step="nowStep"  :step-list="stepList"></vue-step>
                  </div>
                  <!--Paso 1 -->
                  <div class="card-body" v-if="nowStep == 1">
                    <div class="card-body" style="border-bottom-width: 20px; margin-bottom: 20px;">
                        <h1>Importar registros desde un archivo CSV</h1>
                        <h1>Esta herramienta te permite importar (o fusionar) datos de registros a tu intranet desde un archivo CSV.</h1>
                        <br>
                    </div>
                    <div class="card-body">
                      <label class="control-label col-md-3 col-sm-3 col-xs-12">Elegir un archivo CSV desde tu ordenador:</label> 
                      <div class="col-md-6 col-sm-6 col-xs-12">
                         <input type="file" id="csv_file"  name="csv_file" @change="loadCSV($event)" accept=".csv">
                      </div>
                    </div>
                   <!-- <div class="card-body">
                        <label class="control-label col-md-3 col-sm-3 col-xs-12">Actualizar registros existentes: </label>
                        <div class="col-md-6 col-sm-6 col-xs-12">
                            <input type="checkbox" id="updatedFlag" name="updated" value="Y">
                            <label for="updatedFlag">Registros con id coincidente se actualizarán, los regitros que no existan se omitirán </label><br>
                        </div>
                    </div>  -->
                  </div>
                  <!-- Paso 1 fin -->

                  <!--Paso 2 -->
                  <div class="card-body" v-if="nowStep == 2">
                    <div class="card-body" style="border-bottom-width: 20px; margin-bottom: 20px;">
                      <div v-if="flagColumnMapping == true">
                        <h1>Asignar campos CSV a los registros</h1>
                        <h1>Selecciona los campos de tu archivo CSV para asignarlos a los campos de producto, o para ignorarlos durante el proceso de importación</h1>
                      </div>  
                      <div v-else>
                         <h1>No es posible importar archivo CSV</h1>
                      </div>
                      <br>
                    </div>
                    <div class="card-body">
                          <div  class="card-body" v-if="flagColumnMapping == true">
                              <table id="searcher-datatable-Charla" class="table table-striped table-bordered dataTable no-footer dtr-inline" role="grid">
                              <thead>
                                  <tr>
                                      <th style="width: 20px;">Nombre de la columna</th>
                                      <th style="width: 20px;">Asignar al campo </th>
                                  </tr>
                              </thead>
                              <tbody>
                                  <tr v-for="(item,index) in dataForMapping" >
                                      <th style="width: 20px;">
                                        <label>{{item.header}} </label>
                                        <br>
                                        <label>Ejemplo: {{item.example}} </label>
                                      </th>
                                      <th style="width: 20px;">                                         
                                          <div>
                                               <select class="form-control" v-model="item.fieldName">
                                                <option selected="selected" value="">Selecciona un campo</option>
                                                <option v-for="item in registerFields" v-bind:value="item.name" >
                                                    {{item.displayValue}}
                                                </option>
                                              </select>
                                          </div>
                                      </th>
                                  </tr>
                              </tbody>
                              </table>
                          </div>
                          <div  class="card-body" v-else>                             
                              <h1>Los datos del archivo seleccionado no pueden ser importados.</h1>
                          </div>
                    </div>                    
                  </div>
                  <!-- Paso 2 fin -->
                
                  <!--Paso 3 -->
                  <div class="card-body" v-if="nowStep == 3">
                    <div class="card-body" style="border-bottom-width: 20px; margin-bottom: 20px;">
                        <h1>Importando</h1>
                        <h1>Los registros del archivo seleccionado </h1>
                        <br>
                    </div>
                    <div class="card-body">
                      <div class="progress">
                        <div id="progress" class="progress-bar" role="progressbar" style="background-color: rgb(31, 177, 29);" :style="{ width: progressBarValue+'%' }" ></div>
                      </div>
                    </div>                    
                  </div>
                  <!-- Paso 3 fin -->

                  <!--Paso 4 -->
                  <div class="card-body" v-if="nowStep == 4">
                    <div class="card-body" style="border-bottom-width: 20px; margin-bottom: 20px;">
                        <h1>Resúmen de carga de registros</h1>
                        <p>Información sobre la subida masiva de información</p>
                        <div class="card-body">
                          <div class="card-body">
                            <div class="form-group">
                              <label class="control-label col-md-3 col-sm-3 col-xs-12">Total de registros subidos:</label>
                              <div class="col-md-6 col-sm-6 col-xs-12">
                                <input readonly type="text" id="name" class="form-control" v-model="loadSummary[0].total">
                              </div>
                            </div>
                          </div>
                          <div class="card-body">
                            <div class="form-group">
                              <label class="control-label col-md-3 col-sm-3 col-xs-12">Enviados exitosamente:</label>
                              <div class="col-md-6 col-sm-6 col-xs-12">
                                <input readonly type="text" id="name" class="form-control" v-model="loadSummary[0].successful">
                              </div>
                            </div>
                          </div>
                          <div class="card-body">
                            <div class="form-group">
                              <label class="control-label col-md-3 col-sm-3 col-xs-12">No cargados:</label>
                              <div class="col-md-6 col-sm-6 col-xs-12">
                                <input readonly type="text" id="name" class="form-control" v-model="loadSummary[0].errors">
                              </div>
                            </div>
                          </div>
                          <div class="card" id="accordion" style="padding-bottom: 0px; " v-if="showDetailStep4 == true">
                            <div class="card card-default" >
                                    <div class="card-header" style="padding-left: 15px;"  data-toggle="collapse" href="#collapseConsole" role="button" aria-expanded="false" aria-controls="collapseExample">
                                      <h2>Detalle</h2>
                                      <div class="clearfix">
                                      </div>
                                    </div>
                                    <div id="collapseConsole" class="card-collapse collapse in" style="padding-top: 10px; max-height: 200px; overflow: auto;"> 
                                        <div class="card-body">                      
                                            <table id="searcher-datatable-Charla" class="table table-striped table-bordered dataTable no-footer dtr-inline" role="grid" >
                                              <tbody>
                                                  <tr v-for="(item,index) in loadSummary[0].detail" >
                                                      <th style="width: 20px;">
                                                        <label>{{item}} </label>                                                        
                                                      </th>
                                                  </tr>
                                              </tbody>
                                            </table>
                                        </div>
                                    </div>
                             </div>
                          </div>
                        </div>
                    </div>                 
                  </div>
                  <!-- Paso 4 fin -->


            </div>
            <div class="modal-footer">    
              <div v-if="nowStep == 1">
                  <button type="button"  class="btn btn-light" @click="cancelProccess">Cancelar</button>  
              </div>
              <div v-if="nowStep == 1">
                 <button type="button" class="btn btn-success btn-xs float-right" @click="showColumnMapping" >Continuar </button> 
              </div>    
              <div v-if="nowStep == 2">                               
                 <button type="button" class="btn btn-success btn-xs float-right" v-if="flagColumnMapping == true"  @click="importData()">Importar </button>
                 <button type="button" class="btn btn-light"  @click="actionsStep(2,'returnStep')">Volver </button>  
                 <button type="button" class="btn btn-light" v-if="flagColumnMapping == true" @click="actionsStep(2,'clear')">Limpiar campos </button> 
                 
              </div>       
              <div v-if="nowStep == 4">
                 <button type="button" data-dismiss="modal" class="btn btn-success" @click="actionsStep(4,'close')">Finalizar </button> 
              </div>    
            </div>
        </div>
        </div>
    </div>
    <!-- Modal subida de archivos .CSV Fin -->
 
  </div>
</template>
<script>

//import consultServices from './../../utilities/consultServices.js';
import LoadingComponent from "./loadingComponentContainer.vue";
import MasterAdministrator from "./masterAdministratorContainer.vue";
import listComponent from "./listComponentContainer.vue";
import TableMaf from "../tableMaf.vue";
import Treeselect from '@riophae/vue-treeselect';
import vueStep from 'vue-step';
import { string } from '../xlsx/jszip';
import { relativeTimeRounding } from 'moment';

export default {
  name: "masterTemplate",
  components: {
    MasterAdministrator,
    TableMaf,
    Treeselect,
    LoadingComponent,
    vueStep,
    listComponent,
    Treeselect,
    
  },

  props:{
    filters:Array,
    header:Array,
    data:Array,
    buttonFilter:Array,
    tableTitle: String,
    setDataTable:Function,
    dataLoadFunction:Function,
    useMassiveSelector:Boolean,
    flagUploadData:Boolean,
    headingTitle:String,
    registerFields:Array,
    progressBarValue: Number,
    loadSummary: Array,
    uploadFunction: Function,
    showSearchSection: Boolean,    
    tableSearch: Boolean,
    flagListFilters: Boolean,
    flagSearchHistory: Boolean,
    searchHistory: Array,
    loadingComponentLabel: String,
    loadingComponentClass: String,
  },



  data: () => ({
      //declarar variable que se va utilizar
      valueConsistsOf: 'ALL_WITH_INDETERMINATE',
      isLoading: false,  
      valueTest:null,
      headingTitleFinal:'',
      nowStep: 1,
      stepList: ['Subir archivo CSV', 'Asignación de columnas', 'Importar', 'Resúmen'],
      flagShow:true,
      //Arreglo donde se guardan los datos del archivo csv
      dataCSV:[],
      dataForMapping:[],
      //varible de porcentaje para el paso 3 
      dataPercentSent : 0,
      selectedField:'',
      //boleeanos para mostrar pasos
      flagColumnMapping: true,
      newCSV:false,
      fileName : '',
      showDetailStep4:false,
      //
      auxFilters:[],
      optionsFilters:[],
      filtersMasterAdministrator: [],
      arrayAddedFilters: [],
      selectedArray: 'NEW',
      flagSaveSearch: true, 
      titleSearch: 'Nueva Búsqueda',
      showEditTitleSearch: false,
      _flagListFilters: false,
      _flagSearchHistory: false,     
  }),


  async created () {
    this.headingTitle? this.headingTitleFinal = this.headingTitle : this.headingTitleFinal = 'Búsqueda'
    this.flagListFilters ? this._flagListFilters = this.flagListFilters :  this._flagListFilters = false
    this.flagSearchHistory ? this._flagSearchHistory = this.flagSearchHistory :  this._flagSearchHistory = false
    $(document).ready(function(){
        $(".show-modal").click(function(){
            $("#myModal").modal({
                backdrop: 'static',
                keyboard: false
            });
        });
    });
    this.filtersMasterAdministrator =  _.cloneDeep(this.filters)
    if(this.flagListFilters == true){
      await this.refreshContentListFilters('NEW')
    }
  },
  methods: {


    setContentListComponent(array, name){
      this.$refs.masterAdministartor.setContentListComponent(array, name);        
    },
    async refreshFilters(array){
        this.filtersMasterAdministrator = array
    },
    async refreshContentListFilters(type){

      this.arrayAddedFilters =[]
      if(type == 'NEW'){
        this.optionsFilters = []
        for (let index = 0; index < this.filters.length; index++) {
              var obj={}
              obj.id = this.filters[index].name
              obj.label = this.filters[index].label

              if(this.filters[index].selectField ==  true){
                this.arrayAddedFilters.push(this.filters[index].name)
                if(this.filters[index].isRequired ==  true){
                  obj.isDisabled = true  
                }
              }                
              
              this.optionsFilters.push(obj)     
              this.selectedArray = "NEW"     
        }
      }
    },
    clearSearch( ){
      console.log('filtros para revisar ', this.filters)
      for (let index = 0; index < this.filters.length; index++) {
        // if(this.filters[index].filterType == 'inputComponent' && this.filters[index].vModel ){
        //   this.$refs.masterAdministartor.clearValue(this.filters[index].name);
        // }
        //if(this.filters[index].filterType != 'inputComponent' ){
          this.$refs.masterAdministartor.clearValue(this.filters[index].name);
        //}
             
      }
         
    },
    returnData(){
      for (let i = 0; i < this.filters.length; i++) {
       
       var item=null
       for (let j = 0; j < this.valueTest.length; j++) {
        if(this.filters[i].name == this.valueTest[j]){
          item=this.valueTest[j]
        }         
       }
       
       if(item !=null){
          this.filters[i].selectField = true
       }else{
          this.filters[i].selectField = false
       }        
      }


    },
    saveSearch(){

      var arrayHistory = []
      //Obetener los nombres de los filtros seleccionados
      for (let index = 0; index < this.filtersMasterAdministrator.length; index++) {
        if(this.filtersMasterAdministrator[index].selectField == true){
            arrayHistory.push(this.filtersMasterAdministrator[index].name)
        }        
      }

      console.log('arrayHistory', arrayHistory)
      if(this.selectedArray == 'NEW'){
        //Si es una nueva búsuqeda, guardar
        var newSearch = {}
        newSearch.label = this.titleSearch
        newSearch.arrayInputs = arrayHistory
        this.searchHistory.push(newSearch)
      }else{
        //Si es una búsqueda anterior, reemplazar 
        for (let k = 0; k < this.searchHistory.length; k++) {
          if(this.searchHistory[k].label == this.selectedArray.label){
            this.searchHistory[k].arrayInputs = arrayHistory
            this.searchHistory[k].label = this.titleSearch
          }        
        }
      }

    },

    async useSearch(){
        console.log('selectedArray', this.selectedArray)
        var SelectedOption = _.cloneDeep(this.selectedArray)
        this.arrayAddedFilters = [] 
        var newArray = _.cloneDeep(this.filters)
        var finalArray = [] 
        this.showEditTitleSearch = false

        //NUEVA BÚSQUEDA
        if(SelectedOption == 'NEW'){
          console.log('entró a new')   
          //Asignar título       
          this.titleSearch = 'Nueva búsqueda'
          //Poblar sección 2 
          for (let k = 0; k < newArray.length; k++) {
           if(newArray[k].selectField == true ){
             this.arrayAddedFilters.push(newArray[k].name)
           }            
          }
          //Poblar sección 3
          this.filtersMasterAdministrator = _.cloneDeep(this.filters)
        }

        //HISTORIAL
        else{
          console.log('entró a useSearch')
          //Asignar título    
          this.titleSearch = SelectedOption.label
          //Poblar sección 2
          this.arrayAddedFilters = []
          var selected = SelectedOption.arrayInputs
          console.log('selected', selected)
          for (let i = 0; i < newArray.length; i++) {
            for (let j = 0; j < selected.length; j++) {
              if(selected[j] == newArray[i].name){
                // var obj ={}
                // obj.name = newArray[i].name
                // obj.label = newArray[i].label
                this.arrayAddedFilters.push(newArray[i].name)
              }
            }
          } 
          console.log('arrayAddedFilters', this.arrayAddedFilters)
          //Poblar sección 3
          for (let i = 0; i < newArray.length; i++) {
            for (let j = 0; j < SelectedOption.arrayInputs.length; j++) {
              if(SelectedOption.arrayInputs[j] == newArray[i].name){
                  var obj = newArray[i]
                  obj.selectField = true
                  finalArray.push(obj)
              }
            }
          }   
          this.filtersMasterAdministrator = finalArray       
        }
        
    },
    async addFilter(){
      this.isLoading = true
      console.log('arreglo de filtros en addfilter', this.arrayAddedFilters)
      if(this.arrayAddedFilters.length == 0){
        console.log('filter.length 0' )
        this.filtersMasterAdministrator  = _.cloneDeep(this.filters)
      }
      else{
        this.auxFilters = []
        var newArray = _.cloneDeep(this.filters)        
        for (let index = 0; index < this.arrayAddedFilters.length; index++) {
            for (let j = 0; j < newArray.length; j++) {
              if(newArray[j].name == this.arrayAddedFilters[index]){
                var obj = newArray[j]
                obj.selectField = true
                this.auxFilters.push(obj)
              }                
            }        
        }
        this.filtersMasterAdministrator = this.auxFilters
      }
      this.isLoading = false
    },
    /**
     * El método runSearch() permite darle el formato necesario a los filtros
     * para despues usar la propiedad del componente dataLoadFunction() y obtener 
     * el arreglo que poblará la tabla
     * 
     * Autor: Mowidev Desarrollo
     * Version: 1.2
     * 
     */
    async runSearch(){
        this.isLoading = true;      
        /**
         * Definición de variables
         * arrayData: Arreglo que recibe el resultado de la búsqueda
         * finalArray: Arreglo final que irá al componente TableMaf para poblar la tabla
         * selectedFilters: Arreglo con los filtros que seleccionados para iniciar la búsqueda
         * startSearch: Booleano que permite la Búsqueda con la función dataLoadFunction() si su valor es true
         */
        var arrayData=[]
        var finalArray=[]
        var selectedFilters=[]
        var startSearch=true
        console.log('check filteeeeers ', this.filtersMasterAdministrator)
        //Dar formato a los filtros generando objetos con los siguientes atributos: name, value, operator
        for (let index = 0; index <  this.filtersMasterAdministrator.length; index++) {
            
            if(this.filtersMasterAdministrator[index].isRequired == true && this.filtersMasterAdministrator[index].vModel == undefined ){
                startSearch =false
            }else{
              var filter={}
                if(this.filtersMasterAdministrator[index].selectField == true){
                    filter.name = this.filtersMasterAdministrator[index].name
                    if( this.filtersMasterAdministrator[index].vModel){
                      filter.value = this.filtersMasterAdministrator[index].operators == true ? filter.value = this.filtersMasterAdministrator[index].vModel.variable : this.filtersMasterAdministrator[index].vModel
                      filter.operator = this.filtersMasterAdministrator[index].operators == true ? filter.operator = this.filtersMasterAdministrator[index].vModel.operator : filter.operator='F'  
                      this.filtersMasterAdministrator[index].filterType == 'inputComponent'? filter.type = this.filtersMasterAdministrator[index].type : filter.type = null
                      selectedFilters.push(filter)
                    }         
                }  
            }          
        }
        //la búsqueda de registros solo se dará si el campo startSearch == true
        if(startSearch == true){
            //consultar servicio
            console.log('filter final:::::', selectedFilters)
            arrayData =await this.dataLoadFunction(selectedFilters)

            //dar el formato a los datos
            finalArray=this.setDataTable(arrayData)
            
            //refrescar la tabla
            this.$refs.tableMaf.formatData(finalArray);    
        }
        //notificar si no se inicia la búsqueda  
        else{
          alert('Debe completar los campos obligatorios(*) para inciar la búsqueda')
        }
        this.isLoading = false;
    },

    async setInitialData(data){
       this.$refs.tableMaf.formatData(this.setDataTable(data));
    },

    editTitleSearch(){
      this.showEditTitleSearch = true
    },

    /**Sección subida masiva de data
     * Contiene las siguientes funciones: loadCSV(),csvArray(),
     */

    /**
     * loadCSV(e) esta función carga el archivo ingresado en el modal de importación(paso 1)     
     */
    loadCSV(e) {      
      var vm=this  
      //capturar el nombre del archivo
      this.fileName = e.target.files[0].name
      if (window.FileReader) {
        this.newCSV = true
        var reader = new FileReader();
        reader.readAsText(e.target.files[0]);
        reader.onload = function(event) {
          var csv = event.target.result;
          vm.parse_csv = vm.csvArray(csv)          
        };
        reader.onerror = function(evt) {
          if(evt.target.error.name == "NotReadableError") {
            alert("No se pudo leer el archivo");
          }
        };
      } else {
        alert('Componente no soportado en este navegador.');
      }
    },

    /**
     * csvArray(csv) esta función permite transformar la data del csv en un arreglo con un sub-arreglo
     * por fila
     */
    csvArray(csv){
      //en esta variable se cargará la infomración del csv 
      this.dataCSV=[]
      //variables para ordenar la data
      var vm = this
      var lines = csv.split("\n")
      var result = []
      var headers = lines[0].split(",")
      vm.parse_header = lines[0].split(",")       
      //consolidar todo en un arreglo
      lines.map(function(line, indexLine){            
        var obj = []
        var currentline = line.split(",")        
        headers.map(function(header, indexHeader){
          obj.push(currentline[indexHeader])
        })       
        result.push(obj)
      })   
      //cargar el arreglo en dataCSV   
      this.dataCSV = result 
    },

    /**
     * showColumnMapping() esta funcion valida la lógica para mostrar el paso 2
     * en esta función, dataForMapping setea sus objetos en función ala data recogida del csv
     */
    showColumnMapping(){
      var extension = this.fileName.split('.').pop();
      if( this.newCSV == false || this.fileName == ''){
        //si no se ha seleccionado un archivo, no se puede ir al pasao 2 de la carga, notificar
        alert('Debe seleccionar un documento')
      }else if(extension != 'csv' ){
        //si no es el formato csv ,no se puede ir al pasao 2 de la carga, notificar
        alert('Debe seleccionar un documento de formato .csv')
      }
      else{
        //permitir mostrar el paso 2
            this.nowStep = 2
            if(this.dataCSV.length < 2 ){
              this.flagColumnMapping = false 
            }else{
              this.flagColumnMapping = true 
              var headers =  Object.values(this.dataCSV[0]) 
              var examples = Object.values(this.dataCSV[1])       
              this.dataForMapping =[]
              for (let index = 0; index < headers.length; index++) {
                  var obj={}
                  var objImport = {}
                  obj.header = headers[index]
                  obj.example = examples[index]
                  obj.index = index
                  obj.fieldName = ''
                  this.dataForMapping.push(obj)
                  obj={}          
              }      
            }
      }

    },
    /**
     * importData() en esta función se prepara el arreglo final con los registros asociados a la columna especificada
     */
    async importData(){
      var data = this.dataCSV
      var columnMaping = this.dataForMapping
      var numberHeaders = columnMaping.length
      //validar que al menos una cabecera de csv esté relacionado a los campos 
      var flagGroupedFields = false
      for (let index = 0; index < columnMaping.length; index++) {
        if(columnMaping[index].fieldName != ''){
          flagGroupedFields = true
        }      
      }

      if(flagGroupedFields == true){
            data.splice(0, 1);
            var finalArray =[]
            console.log('final columnMaping', columnMaping) 
            console.log('final registerFields',  this.registerFields) 
            //preparar el array que irá al servicio
            for (let i = 0; i < data.length; i++) {
                var obj ={}        
                for (let j = 0; j < this.registerFields.length; j++) { 
                  for (let k = 0; k < columnMaping.length; k++) {
                      if(columnMaping[k].fieldName == this.registerFields[j].name){
                          var test =[]
                          test = data[i]
                          obj[this.registerFields[j].name] = test[columnMaping[k].index]    
                      }              
                  }    
                }        
                finalArray.push(obj)
            }           
            this.nowStep =3     
            //this.$emit('update:importData', finalArray)     
            var flagFinal =await this.uploadFunction(finalArray)
            if(flagFinal == true){
              setTimeout(() => {
                  console.log('pasar a paso 4')
                  this.nowStep =4 
                  if(this.loadSummary[0].detail.length >0){
                     this.showDetailStep4 = true 
                  }
              }, 2000);
            }
            
      }else{
        alert('Debe asignar los campos que se incluirán en la migración')
      }
    },
    /**
     * actionsStep(step, acción) en esta función se define la lógica a aplicar en un respectivo paso y su acción
     */
    actionsStep(step,action){
      if(step == 2){
        if(action == 'clear'){
            for (let i = 0; i < this.dataForMapping.length; i++) {
                this.dataForMapping[i].fieldName = '' 
            }
        }
        if(action == 'returnStep' ){
            this.nowStep = 1
            this.dataForMapping = []
            this.dataCSV = []
            $("#csv_file").val('')
            this.fileName = ''
        }
      }
      if(step == 4){
        if(action == 'close'){
          this.nowStep = 1
          this.dataForMapping = []
          this.dataCSV = []
          $("#csv_file").val('')
          this.fileName = ''
          $('.bs-example-modal-import').modal('hide');
          this.showDetailStep4 = false
        }
      }
    },

    /**
     * cancelProccess() Cancela el proceso, disponible en los pasos 1 y 2
     */
    cancelProccess(){
      var flag= confirm('¿Cancelar proceso de importación?')
      console.log("flaaaaaaaa", flag)
      if(flag == true){
          this.nowStep = 1
          this.dataForMapping = []
          this.dataCSV = []
          $("#csv_file").val('')
          this.fileName = ''
          $('#myModal').modal('hide');
      }else{
        
        $('#myModal').modal('show');

      }
    },


  }
};
</script>

<style scoped>
.collapsing {
  -webkit-transition-delay: 0.2s;
  transition-delay: 0.2s;
  transition: height 0.2s ease;
}

</style>