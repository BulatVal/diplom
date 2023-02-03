<template>
    <div class="valueMsrValueType" v-if="showMsrValueType" @click="hideDialog">
        <my-button class="currentButton" @click="showDialog" >Добавить запись</my-button>
        
        <div class="scroll">
            <table cellspacing="0" cellpadding="0">
                <tbody>
                    <tr>
                        <th>Полное наименование</th>   
                        <th >Краткое наименование</th>
                    </tr>
                    <tr v-for="valueType in valueTypes" :key="valueType.value_type_id" >
                        <td>{{ valueType.value_type_name}}</td>
                        <td>{{ valueType.value_type_sname }} </td>
                    </tr>     
                </tbody>
            </table>
        </div>
        
        <dialog-window v-model:show="dialogVisble">
                <div class="exitBtn">
                    <my-button @click="closeDialog">х</my-button>
                </div>
                
                <my-input  @input="(objectForAdd.value_type_name = $event.target.value)" type="text" placeholder="Полное наименование"></my-input>
                <my-input  @input="(objectForAdd.value_type_sname = $event.target.value)" type="text" placeholder="Краткое наименование"></my-input>
                <div class="saveBtn">
                    <my-button class="currentButton" @click="addValueType">Сохранить</my-button>
                </div>
                
            
            
        </dialog-window>
    </div>
</template>

<script>
import axios from 'axios';
    export default {
        data(){
            return{
                valueTypes:[],
                
                dialogVisble: false,

                objectForAdd:{
                    value_type_name: '',
                    value_type_sname:''
                }
            }
        },
        props:{
            showMsrValueType:{
                type:Boolean,
                default:false
            }
        },
        methods:{
            hideDialog(){
                this.$emit('update:show',true);
            },
            closeDialog(){
                this.dialogVisble =false;
            },
            showDialog(){
                this.dialogVisble =true;
            },
            addValueType(){
                console.log(this.dataMsrValueType)
                if (this.objectForAdd.value_type_name === '' || this.objectForAdd.value_type_sname === ''){
                        alert('Заполните все поля');
                }else{
                    this.addvalueTypeToDB();
                    this.objectForAdd.value_type_name = '';
                    this.closeDialog();
                    this.objectForAdd.value_type_sname = '';
                }
            },
            async getAllValueTypes(){
                try{
                    const response = await axios.get("http://localhost:8081/msr_value_type/all");
                    this.valueTypes = response.data;
                    console.log(this.valueTypes)
                    
                }catch(e){
                    alert('Error')
                }
                
            },
            async addvalueTypeToDB(){
                try{
                    const response = await axios.post("http://localhost:8081/msr_value_type",this.objectForAdd);
                    alert('Тип значения добавлен');
                }catch(e){
                    alert('Error');
                }
            }
        },
        mounted(){
            this.getAllValueTypes();
        }
    }
</script>

<style  scoped>
table{
    width: 100%;
    font-family: "Lucida Sans Unicode", "Lucida Grande", Sans-Serif;
   font-size: 14px;
   border-radius: 15px;
   border-spacing: 0;
   text-align: center;
  
    
}
.saveBtn{
    text-align: center;
}
.exitBtn{
    text-align: right;
}
.currentButton{
    width: 150px;
}
.valueMsrValueType{
    margin-top: 30px;
}
form{
    display: flex;
    flex-direction: column;
}
.scroll{
    height: 500px;
    border: 2px solid black;
    overflow-x: scroll;
    overflow-y: scroll;
    border-radius: 10px;
    margin-right: 15px;
    margin-top: 10px;
}

table tr:last-child td:first-child{
   border-radius: 0 0 0 10px;
}
table tr:last-child td:last-child{
   border-radius: 0 0 10px 0;
}  

table tr:first-child th:last-child{
   border-radius: 0 10px 0 0;
}  
table tr:first-child th:first-child{
   border-radius: 10px 0 0 0;
}  

th{
    border: 1px solid black;
    margin: 0px;
    text-align: center;
    background-color: beige;
    font-size: 13pt;
}
td{
    border: 1px solid black;
    text-align: center;
}
</style>