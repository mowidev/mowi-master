<template>
  <div v-if="_isVisible  == true" :class="_styles.formGroup">
    <label :class="_styles.label" >{{_label}} <span v-if="_isRequired == true" > *</span> :</label>
    <div :class="_styles.container">

      <!-- <select class="form-control" :value="value"  v-validate="'required'">
        <option value="test0">Todos</option>
        <option
          v-for="(select,index) in _content"
          :key="index"
          v-on:input="returnData" 
          :value="select.value"          
        >{{select.label}}</option>
      </select> -->
        <treeselect     
          v-model="valueTest"
          v-on:input="returnData" 
          :value-consists-of="valueConsistsOf"
          :multiple="_multipleSelection"
          :options="arrayOptions" />
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import es from 'vee-validate/dist/locale/es'
// Importa VeeValidate y el Validator
import VeeValidate, { Validator } from "vee-validate";
Vue.use(VeeValidate);
// Indicar uso de idioma español
Validator.localize("es", es);

import Treeselect from '@riophae/vue-treeselect';


export default {
  name: "listComponent",
  props:['label','content','styles','isVisible','selectedValue', 'value','isRequired','multipleSelection']  ,
  components: {
      Treeselect,
  },
  
  data: () => ({
    //acá se definen todos los atributos customizables del componente en este objeto default
    default:{
      label: 'Lista',
      content:[{value:1, label:'test'}],
      styles:{ formGroup:'form-group col-md-12', label:'control-label col-md-3 col-sm-3 col-xs-12', container:'col-md-6 col-sm-6 col-xs-12'},
      selectedValue:'',
      isVisible: true,
      isRequired:false, 
    },
    //acá se definen todos los atributos usados en el html
    _label:'',
    _content:{},
    _styles:{},   
    _isVisible:true ,
    _selectedValue:'', 
    _isRequired:'', 
    //variables declaradas para el funcionamiento del componente
    variable:'',
    valueConsistsOf: 'ALL_WITH_INDETERMINATE',
    arrayOptions:[],
    valueTest:null,
    transicion:[],
    _multipleSelection: false,

  }),
  created () {
    //acá se validan los atributos customizables, de no existir un campo customizable, se asigna el valor por defecto
    this.label? this._label = this.label : this._label = this.default.label
    this.content? this._content = this.content : this._content = this.default.content
    this.styles? this._styles = this.styles : this._styles = this.default.styles  
    this.vModel? this._vModel = this.vModel : this._vModel = this.default.vModel
    this.isVisible? this._isVisible = this.isVisible : this._isVisible = this.default.isVisible
    this.selectedValue? this._selectedValue = this.selectedValue : this._selectedValue = this.default.selectedValue
    this.isRequired? this._isRequired = this.isRequired : this._isRequired = this.default.isRequired
    var transicion = this.content
    // this.arrayOptions = this.tranforForSelectTree(this.content)
    this.arrayOptions = this.tranforForSelectTree(transicion) 
    this.multipleSelection? this._multipleSelection = this.multipleSelection : this._multipleSelection = false

 },
  methods:{
    returnData(){
   
        this.$emit('input', this.valueTest);
    },
    updateContent (newContent){
      this.arrayOptions = this.tranforForSelectTree(newContent) 
      //this.content = newContent
    },
    tranforForSelectTree(content) {
      return _.map(content,(item)=>{
        var response = {
          id: item.value,
          label: item.label,
          }
        return response;
      })
    },

  }  
};
</script>