<template>
  <div v-if="_isVisible  == true" :class="_styles.formGroup" class="mowi-master-item-search">
      <label :class="_styles.label">{{_label}}</label>
      <div :class="_styles.container">
          <treeselect v-model="valueTest" v-on:input="returnData" :value-consists-of="valueConsistsOf"
          :multiple="_multipleSelection" :options="arrayOptions"></treeselect>
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
      styles:{ formGroup:'form-group col-md-6 col-sm-12 col-xs-12', label:'control-label col-md-4 col-sm-4 col-xs-12', container:'col-md-8 col-sm-8 col-xs-12'},
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
    if(this.label){
        if(this._isRequired == true){
            this._label = this.label + "*:"
        }
        else{
            this._label = this.label + ":"
        }
    }
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
    async clearValue(){
        this.valueTest= undefined
        this.$emit('input', undefined);
    },

  }  
};
</script>