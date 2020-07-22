<template>
 <div v-if="_isVisible  == true" :class="_styles.formGroup">
   <label :class="_styles.label" >{{_label}} <span v-if="_isRequired == true" > *</span> :</label>
    <div :class="_styles.container">
      
      <input
      @focus="isFocus = true"
      @blur="onBlur()"
      v-on:input="onChange()"
      type="text"
      v-model="variable"     
      class="form-control col-md-12 col-xs-12" >
      <i class="fa fa-search"></i> 
      <div class="searchComponent-helper" v-if="isFocus" >
        <div @click="selectItem(item)" class="searchComponent-item" v-for="item in items">
          {{item[displayNameKey]}}
        </div>
      </div>
      
    </div>
 </div> 

</template>

<script>

import Vue from 'vue'
import _ from 'lodash';
import es from 'vee-validate/dist/locale/es'
// Importa VeeValidate y el Validator
import VeeValidate, { Validator } from "vee-validate";
Vue.use(VeeValidate);
// Indicar uso de idioma español
Validator.localize("es", es);

    export default {
        name: "searchComponent",
        props: ['label','arrayData','displayNameKey','callBackData','addNewValue', 'vModel','isRequired'],
        data: () => ({
          //valores internos para la lógica
          variable:'',
          items:[],
          isFocus: false,          
          timeout:null,
          //acá se definen todos los atributos customizables del componente en este objeto default
          default:{
            label:'Buscador',
            arrayData:[{value:1, label:'test'}],
            displayNameKey:'label',
            callBackData:'',
            addNewValue:'Y',
            vModel: "default",
            styles:{ formGroup:'form-group col-md-6', label:'control-label col-md-3 col-sm-3 col-xs-12', container:'col-md-6 col-sm-6 col-xs-12'},
            isVisible: true,
            isRequired:false, 
          },
          //acá se definen todos los atributos usados en el html
          _label:'',
          _value:{},
          _arrayData:[],
          _displayNameKey:'',
          _callBackData:'',
          _addNewValue:'',
          _vModel:'',
          _styles:{},
          _isRequired:'', 
          _isVisible:true ,
        }),
        watch: {
          arrayData(newValue){
            this.items = _.cloneDeep(newValue);
          }
        },
        created () {
           //acá se validan los atributos customizables, de no existir un campo customizable, se asigna el valor por defecto
           this.label? this._label = this.label : this._label = this.default.label
           this.arrayData? this._arrayData = this.arrayData : this._arrayData = this.default.arrayData
           this.displayNameKey? this._displayNameKey = this.displayNameKey : this._displayNameKey = this.default.displayNameKey
           this.callBackData? this._callBackData = this.callBackData : this._callBackData = this.default.callBackData
           this.addNewValue? this._addNewValue = this.addNewValue : this._addNewValue = this.default.addNewValue
           this.styles? this._styles = this.styles : this._styles = this.default.styles
           this.vModel? this._vModel = this.vModel : this._vModel = this.default.vModel
           this.isVisible? this._isVisible = this.isVisible : this._isVisible = this.default.isVisible           
           this.isRequired? this._isRequired = this.isRequired : this._isRequired = this.default.isRequired
           //cargar items para visualizarlos
           this.getItem();
           this.items = _.cloneDeep(this._arrayData);
        },
        methods: {
          onChange(){
            var ctx = this;
            clearTimeout(this.timeout);
            this.timeout = setTimeout(function(){ctx.getItem();}, 600);
          },
          async getItem() {    
             var ctx=this;
             this.items=_.filter(this._arrayData, function(o) {
                  return _.includes( _.lowerCase(o[ctx._displayNameKey]),  _.lowerCase(ctx.variable)) 
             });

             //Si se debe agregar un nuevo valor a la lista, la propiedad addNewValue debe ser igual a Y
             //entonces el nuevo valor tendrá por defecto el id 999999 y el texto como name para poder ser seleccionado   
             if(ctx.addNewValue == "Y" && this.items.length == 0){   
                this.items=[{
                  name: ctx.variable,
                  id:999999
                }]
             }         
          },

          onBlur(){
            var ctx = this;
            setTimeout(function(){ ctx.isFocus = false }, 600);
          },
          selectItem(item) {
            this.$emit('input', item.value);
            this.variable = item[this.displayNameKey]
            this.isFocus = false;
          },
          cleanText(){
             this.variable = '';
          },
          setText(name){
             this.variable = name;
          },
        }
    }
    
</script>



<style scoped>
  .searchComponent-helper {
      position: absolute;
      top: 100%;
      z-index: 10;
      background: white;
      border: 1px solid;
      width: calc(100% - 20px);
      max-height: 170px;
      overflow: auto;
  }
  .searchComponent-item {
    padding: 3px;
    border-bottom: 1px solid;
    padding-left: 10px;
    cursor: pointer;
  }

  .searchComponent-item:hover {
    background: #e2e8ee;
  }
  .fa {
    display: inline-block;
    font: normal normal normal 14px/1 FontAwesome;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    position: absolute;
    padding-right: 5px;
    position: absolute;
    right: 13px;
    top: 10px;
    -moz-osx-font-smoothing: grayscale;
}
</style>
