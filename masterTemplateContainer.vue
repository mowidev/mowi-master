<template>
  <div>
    <LoadingComponent v-if="isLoading"></LoadingComponent>
    <!-- <div class="accordion" id="accordionExample"> -->
    <div class="card" style="padding-bottom: 0px;">
      <div class="card-header" style="padding-left: 15px;"  data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="false" aria-controls="collapseExample">
        <h2>{{headingTitleFinal}}</h2>
        <div class="clearfix">
        </div>
      </div>
      <div class="collapse" id="collapseExample">
        <div  class="card-body" >
          <MasterAdministrator
            :filters="filters"          
            :buttonFilter="buttonFilter"
          ></MasterAdministrator>
          <div class="card-header-actions">
            <button type="button"  @click="runSearch()" class="btn btn-success btn-xs">Buscar</button> 
          </div>
        </div>
      </div>
    </div>
    <!-- </div> -->

    <div v-if="flagUploadDta == true" class="card-header-actions">
      <button type="button"  data-toggle="modal" data-target=".bs-example-modal-test"   class="btn btn-success btn-xs">Subir data</button> 
    </div>    
    <TableMaf :header="header" :data="data" :tableTitle="tableTitle" ref="tableMaf" :useMassiveSelector="useMassiveSelector" ></TableMaf>
  
    <!-- Modal Rotación -->
    <div class="modal fade bs-example-modal-test" tabindex="-1" role="dialog" aria-hidden="true">
        <div class="modal-dialog modal-lg">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title" id="myModalLabel" >Detalle </h3>          
            </div>
            <div class="modal-body">
                  <div>
                    <vue-step :now-step="nowStep"  :step-list="stepList"></vue-step>
                  </div>
            </div>
            <div class="modal-footer">              
            </div>
        </div>
        </div>
    </div>
    <!-- Modal Rotación Fin -->
  
  
  
  </div>
</template>
<script>

//import consultServices from './../../utilities/consultServices.js';
import LoadingComponent from "./loadingComponentContainer.vue";
import MasterAdministrator from "./masterAdministratorContainer.vue";
import TableMaf from "../tableMaf.vue";
import Treeselect from '@riophae/vue-treeselect';
import vueStep from 'vue-step';

export default {
  name: "masterTemplate",
  components: {
    MasterAdministrator,
    TableMaf,
    Treeselect,
    LoadingComponent,
    vueStep,
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
    flagUploadDta:Boolean,
    headingTitle:String
  },

  data: () => ({
      //declarar variable que se va utilizar
      valueConsistsOf: 'ALL_WITH_INDETERMINATE',
      isLoading: false,  
      arrayOptions:[],
      valueTest:null,
      headingTitleFinal:'',
      nowStep: 0,
      stepList: ['Registro completo', 'Inscrito a charla', 'Evaluación aprobada', 'Participación confirmada'],
      flagShow:true
    
  }),



  created () {
    this.arrayOptions = this.tranforForSelectTree(this.filters) 
    this.headingTitle? this.headingTitleFinal = this.headingTitle : this.headingTitleFinal = 'Búsqueda'
    
  },
  methods: {

    test(){
      if(this.flagShow == true){document.getElementById('thediv').style.display = 'none';}
      //if(this.flagShow == false){document.getElementById('thediv').style.display = 'block'; this.flagShow=true}      
    },
    
    tranforForSelectTree(filters) {
      return _.map(filters,(item)=>{
        var response = {
          id: item.name,
          label: item.label,
          }
        return response;
      })
    },
    returnData(){
      for (let i = 0; i < this.filters.length; i++) {
       
       var item=null
       //var item  =_.filter(this.valueTest,this.filters[i].name);
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
        //Dar formato a los filtros generando objetos con los siguientes atributos: name, value, operator
        for (let index = 0; index <  this.filters.length; index++) {
            if(this.filters[index].isRequired == true && this.filters[index].vModel == undefined ){
                startSearch =false
            }else{
              var filter={}
                if(this.filters[index].selectField == true){
                    filter.name = this.filters[index].name
                    filter.value = this.filters[index].operators == true ? filter.value = this.filters[index].vModel.variable : this.filters[index].vModel
                    filter.operator = this.filters[index].operators == true ? filter.operator = this.filters[index].vModel.operator : filter.operator='F'                    
                    selectedFilters.push(filter)
                }  
            }          
        }
        //la búsqueda de registros solo se dará si el campo startSearch == true
        if(startSearch == true){
            //consultar servicio
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