<template>
  <div>
    <div class="card">
      <div class="card-header">
        <h2>Busqueda</h2>
        <div class="card-header-actions" align-items: center>
              <label class="control-label col-md-12 col-sm-12 col-xs-12">Campos seleccionados</label>
                <treeselect   
                v-model="valueTest"
                v-on:input="returnData" 
                :value-consists-of="valueConsistsOf"
                :multiple="true"
                :options="arrayOptions" />
               
        </div>
        <div class="clearfix"></div>
      </div>
      
      <div class="card-body">
        <masterAdministrator
          :filters="filters"          
          :buttonFilter="buttonFilter"
        ></masterAdministrator>
        <div class="card-header-actions">
          <button type="button"  @click="runSearch()" class="btn btn-success btn-xs">Buscar</button> 
        </div>
      </div>
    </div>
    <TableMaf :header="header" :data="data" :tableTitle="tableTitle" ref="tableMaf" :useMassiveSelector="useMassiveSelector" ></TableMaf>
  </div>
</template>
<script>


import masterAdministrator from "./masterAdministratorContainer.vue";
import TableMaf from "./tableMaf.vue";
import Treeselect from '@riophae/vue-treeselect';

export default {
  name: "masterTemplate",
  components: {
    masterAdministrator,
    TableMaf,
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
  },

  data: () => ({
      //declarar variable que se va utilizar
      valueConsistsOf: 'ALL_WITH_INDETERMINATE',  
      arrayOptions:[],
      valueTest:null
  }),



  created () {
    this.arrayOptions = this.tranforForSelectTree(this.filters)
  },
  methods: {

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

    async runSearch(){
        //esta función permite la búsqueda en función a los parámetros seleccionados  
        //definir variables
        var arrayData=[]
        var finalArray=[]
        var selectedFilters=[]
        var startSearch=true
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

    },
  }
};
</script>