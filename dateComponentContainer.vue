<template>
    <div v-if="_isVisible  == true" :class="_styles.formGroup">
      <label :class="_styles.label" >{{_label}} <span v-if="_isRequired == true" > *</span> :</label>
        <div :class="_styles.container">
          <v-date-picker
            format="dd/MM/yyyy"
            :mode="_mode"
            :value="_dateRange"
            v-model="variable"
            v-on:input="returnData"
            :columns="$screens({ default: 1, laptop: 2 })"
          ></v-date-picker>
        </div>    
    </div>      
</template>
    
<script>
import moment from "moment";

export default {
  name: "dateComponent",
 

  props:['label','dateRange','styles','mode','isVisible','isRequired']  ,
  data: () => ({
    //acá se definen todos los atributos customizables del componente en este objeto default
    default:{
      label: 'Fecha',
      dateRange:{start: moment().format('dddd, MMMM DD YYYY'), end: moment().format('dddd, MMMM DD YYYY')},
      styles:{ formGroup:'form-group col-md-6', label:'control-label col-md-3 col-sm-3 col-xs-12', container:'col-md-6 col-sm-6 col-xs-12'},
      mode: 'range',
      isVisible: true,
      isRequired:false, 
    },
    //acá se definen todos los atributos usados en el html
    _label:'',
    _dateRange:{},
    _styles:{formGroup:'',label:'',container:''},
    _mode: '',
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
    this.isVisible? this._isVisible = this.isVisible : this._isVisible = this.default.isVisible
    this.selectDate? this._selectDate = this.selectDate : this._selectDate = this.default.selectDate
    this.vModel? this._vModel = this.vModel : this._vModel = this.default.vModel
    this.isRequired? this._isRequired = this.isRequired : this._isRequired = this.default.isRequired
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
  }
};
</script>