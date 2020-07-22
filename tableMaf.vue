<template>
    <!--<div class="row">Content-->
        <div class="card">
            <div class="card-header" v-if="titleView != false">
                <h2>{{tableTitle}}</h2>
                <div class="clearfix"></div>
            </div>

            <div class="card-body">


                    <div class="row" v-if="datePerPageView != false">
                        <div class="col-md-12">
                            <div class="form-group form-inline pull-left VueTables__search">
                                <label> Datos:
                                    <select class="form-control" v-model="datePerPage">
                                        <option v-for="value in valPage" :value="value"  >{{value}}</option>
                                    </select>
                                    </label>
                            </div>
                        </div>
                        <div v-if="textView == true" class="col-md-12">
                            <div class="form-group form-inline pull-left VueTables__search">
                                <label> {{text}}</label>
                            </div>
                        </div>
                    </div><!--NroPaginas-->

                    <div class="row">
                        <div class="table-responsive">
                            <table class="table b-table table-bordered b-table-fixed">
                                <thead>
                                <tr>
                                    <th v-for="head in header">
                                        <div class="d-flex justify-content-between">
                                            <label>{{head.label}}  </label>
                                            <div style="
                                                width: 30px;
                                                min-width: 30px;
                                                margin-left: 15px;
                                            ">
                                                <span v-if="head.sort==1" @click="orderByAsc(head.name)"><i class="icon-arrow-up "></i></span>
                                                <span v-if="head.sort==1" @click="orderByDesc(head.name)"><i class="icon-arrow-down "></i></span>
                                                <input v-if="head.name == 'select'" @click="massiveSeletor()" type="checkbox" v-model="massiveSelect"  />
                                            </div>
                                        </div>
                                    </th>
                                </tr>
                                </thead>
                                <paginate name="dataTable" :list="filtered" :per="datePerPage" tag="tbody" ref="paginator" >
                                    <tr v-if="filterView != false" >
                                        <td v-for="(filter,index) in filSearch" :key="index" class="table-success" >
                                            
                                            <input v-if="filter.find==1" type="text" v-model="filter.value" class="form-control" :placeholder="filter.label">
                                            <label v-if="filter.find==0"> </label>
                                        </td>
                                    </tr>
                                    <tr v-for="object in paginated('dataTable')" v-bind:key="object.indexTableMaf" >
                                        <td v-for="(head, index) in header" :key="index" >
                                        <Value :data='object[head.name]'></Value></td>
                                    </tr>
                                </paginate>
                                
                            </table>
                        </div>
                    </div><!--Tabla-->

                <div class="row" v-if="paginationView != false">
                    <div class="dataTables_paginate paging_simple_numbers">
                        <paginate-links
                                    for="dataTable"
                                    :show-step-links="true"
                                    :limit="7"
                                    :step-links="{
                                        prev: 'Anterior',
                                        next: 'Siguiente'
                                    }"
                                    :classes="{
                                        'ul': 'pagination',
                                        'li': 'page-item',
                                        '.number': 'page-item',
                                        'a' : 'page-link'
                                    }"
                                    ></paginate-links>
                    </div>
                </div><!--Paginador-->

                <div class="col-sm-6 col-md-4">
            </div>
            </div> <!--Content-->

        </div> <!--Card-->
   <!--Content </div> Row-->
</template>

<script>
    
    import _ from 'lodash';
     const Value = ({ props }) => {
      return <div>{props.data}</div>
    }

    export default {

        props: ['header','data','tableTitle','searchOption','text','callbackData','title','datePerPageV','pagination','useMassiveSelector'],
         components: {
          Value,
        },
        data: () => ({
          allSelected:false,
          isLoading:false,
          idEvaluated:'',
          filSearch:{},
          datePerPage:10,
          paginate: ['dataTable'],
          dataTable:[],
          valPage:[5,10,25,50,100],
          filterView:true,
          textView:false,
          titleView:true,
          datePerPageView:true,
          paginationView:true,
          useMassiveSelectorFlag:false,
          massiveSelect:false

        }),
         created () {
             if(this.useMassiveSelector){this.useMassiveSelectorFlag=this.useMassiveSelector}
             if(this.useMassiveSelectorFlag == true){
                var selectionColumn={label: "Seleccionar todo", name: "select", find: 0, sort: 0 }
                this.header.splice(0, 0, selectionColumn)
                //this.header.push({ label: "Seleccionar todo", name: "select", find: 0, sort: 0 },)
             }
             
             console.log('valor inicial para la cabecera ', this.massiveSelect)
             this.search();
             this.formatData(this.data);
             if(this.title == false){this.titleView=false}
             if(this.datePerPageV == false){this.datePerPageView=false}
             if(this.pagination == false){this.paginationView=false}
             if(this.searchOption == false){this.filterView=false}
             if(this.text != ''){this.textView=true}
             

        },
        methods: {
            check_(e){
                
                if(e.target.checked == true)
                {    
                       for(let data of this.dataTable) 
                       {
                        
                           let d = this.hasChildElement(data.actionsCheck)
                              console.log("data",d)
                       }
                }
            },
            search(){
                this.filterView = this.searchOption;
                this.filSearch = _.map(this.header, (object) => {
                        return object;
                });
            },

           formatData(data){
                var ctx = this;
                if(this.useMassiveSelector == true){
                    
                    for (let index = 0; index < data.length; index++) {          
                        //data[index].isSelected = false
                        data[index].select=   <div> <input type="checkbox" checked={data[index].isSelected} onChange={()=>ctx.changeMassiveSelect(data[index].isSelected)}  /></div>     
                    }
                   
                }

                this.dataTable=[];
                this.dataTable = _.map(data, (item, i)=>{
                    item.indexTableMaf = i;
                    return item;
                });

            },
            orderByAsc(camp){

                var dataValue = _.toUpper(this.dataTable[1][camp]);
                 if (dataValue=='[OBJECT OBJECT]')
                {
                    this.dataTable = _.sortBy(this.dataTable, [(o)=> { return o[camp].elm.innerText; }]);
                    //this.dataTable = _.orderBy(this.dataTable,[camp].elm.innerText,['asc']);
                }
                else
                {
                    this.dataTable = _.orderBy(this.dataTable,[camp],['asc']);
                }
            },
            orderByDesc(camp){
                var dataValue = _.toUpper(this.dataTable[1][camp]);
                 if (dataValue=='[OBJECT OBJECT]')
                {
                    this.dataTable = _.sortBy(this.dataTable, [(o)=> { return o[camp].elm.innerText; }]).reverse();
                    //this.dataTable = _.orderBy(this.dataTable,[camp].elm.innerText,['asc']);
                }
                else
                {
                    this.dataTable =  _.orderBy(this.dataTable,[camp],['desc']);
                }

            },
            massiveSeletor(){    
                console.log('mira prueba ', this.massiveSelect)

                for (let index = 0; index < this.dataTable.length; index++) {
                    console.log('lo que está adentro', this.dataTable[index].isSelected)
                    this.massiveSelect == true?   this.dataTable[index].isSelected = false : this.dataTable[index].isSelected = true                     
                }
                console.log('prueba ', this.dataTable)
                this.formatData(this.dataTable);
            },
            changeMassiveSelect(valor){
               console.log('entró a changeMassiveSelect',valor) 
               this.massiveSelect == true? this.massiveSelect = false:null
            }


        },
        computed:{

            filtered () {

                var filteredData=this.dataTable;
                this.filSearch.forEach(element => {
                    filteredData = element.value ?
                    _.filter(filteredData, object =>{
                        var dataValue = _.toUpper(object[element.name]);
                        if (dataValue=='[OBJECT OBJECT]') {
                            if(object['_'+element.name]){
                                console.log('_'+element.name);
                                dataValue = _.toUpper(object['_'+element.name]);
                            }else{
                                console.log('_'+element.name+'--------------------FDSSSS------');
                                console.log(object);
                                dataValue = _.toUpper(object[element.name].elm.innerText);
                            }
                        };
                        var filterValue = _.toUpper(element.value);
                        return _.includes(dataValue,filterValue)
                    }) : filteredData ;
                })


                if (this.$refs.paginator)
                {
                    if(this.$refs.paginator.currentPage < 0 && filteredData.length >= 1 )
                    {
                        this.$refs.paginator.currentPage = 0 ;
                    }
                }


                return filteredData;
            },

        }

    }
</script>





<style>
ul.paginate-links.dataTable li.disabled a {
    color: #777;
    cursor: not-allowed;
    justify-content: center!important;
}

ul.paginate-links.dataTable li a {
    text-decoration: none;
    display: inline;
    background: #ddd;
    border-color: #ddd;
    width: auto;
    float: inherit;
    font-weight: normal;
    font-size: inherit;
    cursor:pointer;

}

ul.paginate-links>li:first-child>a{
    margin-left: 0;
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
}

ul.paginate-links>li:last-child>a{
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
}

ul.paginate-links>.active>a, ul.paginate-links>.active>a:focus, ul.paginate-links>.active>a:hover{
    z-index: 3;
    color: #fff;
    cursor: default;
    background-color: #337ab7;
    border-color: #337ab7;
}
  ul.paginate-links.dataTable li {
      display: inline;
      background: #ddd;
      border-color: #ddd;
      width: auto;
      float: inherit;
      font-weight: normal;
      font-size: inherit;
  }


  ul.paginate-links.dataTable {padding-top: 10px;}

  .scrolling-wrapper {
  overflow-x: scroll;
  overflow-y: hidden;
  white-space: nowrap;

}

</style>
