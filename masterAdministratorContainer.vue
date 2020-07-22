<template>
  <div> 
    <!--se renderizan los campos para la búsqueda -->
    <div v-for="(item, i) in filters" :key="`A-${i}`">
      <div v-if="item.filterType == 'dateComponent' && item.selectField == true"> <dateComponent :label="item.label" :isRequired="item.isRequired" :dateRange="item.dateRange"  :styles="item.styles" :mode="item.mode" v-model="item.vModel" :isVisible="item.isVisible" :selectDate="item.selectedDate" /> </div>
      <div v-if="item.filterType == 'listComponent' && item.selectField == true"> <listComponent :label="item.label" :isRequired="item.isRequired" :value="item.value" :content="item.content" :styles="item.styles" v-model="item.vModel" :isVisible="item.isVisible" :selectedValue="item.selectedValue" /> </div>
      <div v-if="item.filterType == 'searchComponent' && item.selectField == true"> <searchComponent :label="item.label" :isRequired="item.isRequired" :value="item.value" :arrayData="item.arrayData" :displayNameKey="item.displayNameKey" v-model="item.vModel" :service="item.service" :callBackData="item.callBackData" :addNewValue="item.addNewValue" /> </div>
      <div v-if="item.filterType == 'inputComponent' && item.selectField == true"> <inputComponent :type="item.type" :isRequired="item.isRequired" :valueInput="item.valueInput" :etiqueta="item.etiqueta" :label="item.label" :validation="item.validation"  :nameInput="item.nameImput" :callbackData="item.callbackData" 
       :textArea="item.textArea" :styles="item.styles" 
       v-model="item.vModel" /> </div>        
    </div>


  </div>
</template>

<script>
import dateComponent from "./dateComponentContainer.vue";
import listComponent from "./listComponentContainer.vue";
import inputComponent from "./inputComponentContainer.vue";
import searchComponent from "./searchComponentContainer.vue";
import moment from "moment";
const Value = ({ props }) => {
  //buton filter
  //return <div>{props.data}</div>;
};
export default {
  name: "masterAdministrator",
  components: {
    dateComponent,
    listComponent,
    inputComponent,
    searchComponent,
    Value,
  },
  props: ["filters","customActions"],
  created () {

  },
  methods: {

  }
};
</script>