<template>
  <div v-if="_isVisible  == true" :class="_styles.formGroup" class="mowi-master-item-search">
      <label :class="_styles.label">{{_label}}</label>
      <div :class="_styles.container">
          <!-- <v-date-picker 
          :mode="_mode" :value="_dateRange" v-model="variable"
          v-on:input="returnData" 
          format="MMMM-yyyy" value-format="MM-yyyy" type="month"
          ></v-date-picker> -->
          <!-- <Datepicker 
            :value="_dateRange"
            :mode="'range'"
            v-model="variable"
            :format="'MMMM-yyyy'"
            minimum-view="month"
            maximum-view="year"
            name="datepicker"
            input-class="input-class">
          </Datepicker> -->

          <v-date-picker 
            :format="_formatComp"
            :mode="_mode" 
            :value="_dateRange" 
            v-model="variable"            
            v-on:input="returnData" >
          </v-date-picker>

      </div>
  </div>     
</template>
    
<script>
import moment from "moment";
import Datepicker from 'vuejs-datepicker';

export default {
  name: "dateComponent",
  components: {
    Datepicker,
  }, 

  props:['label','dateRange','styles','mode','isVisible','isRequired', 'format']  ,
  data: () => ({
    //acá se definen todos los atributos customizables del componente en este objeto default
    default:{
      label: 'Fecha',
      dateRange:{start: moment().format('dddd, MMMM DD YYYY'), end: moment().format('dddd, MMMM DD YYYY')},
      styles:{ formGroup:'form-group col-md-6 col-sm-12 col-xs-12', label:'control-label col-md-4 col-sm-4 col-xs-12', container:'col-md-8 col-sm-8 col-xs-12'},
      mode: 'range',
      isVisible: true,
      isRequired:false, 
      formatComp: 'dd/MM/yyyy'
    },
    //acá se definen todos los atributos usados en el html
    _label:'',
    _dateRange:{},
    _styles:{formGroup:'',label:'',container:''},
    _mode: '',
    _formatComp: '',
    _isVisible:true ,
    _isRequired:'', 
    //valores internos para la lógica
    variable:'',

  }),

  created () {
    //acá se validan los atributos customizables, de no existir un campo customizable, se asigna el valor por defecto  
    this.label? this._label = this.label : this._label = this.default.label
    this.dateRange? this._dateRange = this.dateRange : this._dateRange = this.default.dateRange
    this.styles? this._styles = this.styles : this._styles = this.default.styles 
    this.mode? this._mode = this.mode : this._mode = this.default.mode
    this.format? this._formatComp = this.format : this._formatComp = this.default.formatComp
    this.isVisible? this._isVisible = this.isVisible : this._isVisible = this.default.isVisible
    this.selectDate? this._selectDate = this.selectDate : this._selectDate = this.default.selectDate
    this.vModel? this._vModel = this.vModel : this._vModel = this.default.vModel
    this.isRequired? this._isRequired = this.isRequired : this._isRequired = this.default.isRequired
    if(this.label){
        if(this._isRequired == true){
            this._label = this.label + "*:"
        }
        else{
            this._label = this.label + ":"
        }
    }
  },
  methods: {
    returnData(){
      //Función para retornar los valores seleccionados en el componente
      //este componente puede retornar una fecha o un rango compuesto por dos fechas 
      //es necesario validar y dar el formato
 
      //validar si se va a retornar una fecha o un rango de fechas
      if(this._mode=='single'){
        var selectedDate = moment(this.variable).valueOf()
        this.$emit('input', selectedDate);
      }
      if(this._mode=='range'){
        var selectedRange = {}
        selectedRange.start =  moment(this.variable.start).valueOf()
        selectedRange.end =  moment(this.variable.end).valueOf()    
        this.$emit('input', selectedRange);
      }
    },

    clearValue(){
        this.variable =undefined   
        this.$emit('input', undefined);
    }
  }
};
</script>