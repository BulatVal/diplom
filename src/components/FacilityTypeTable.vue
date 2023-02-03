<template>
    <div class="facilityTypes" v-if="showfacilityType" @click="hideDialog">
        <my-button class="currentButton" @click="showDialog" >Добавить запись</my-button>
        <div class="scroll">
            <table cellspacing="0" cellpadding="0">
                <tbody>
                    <tr>
                        <th>Наименование</th> 
                    </tr>
                    <tr v-for="facilitytype in facilityTypes" :key="facilitytype.facility_type_id" >
                        <td>{{ facilitytype.facility_type_name}}</td>
                    </tr>     
                </tbody>
            </table>
        </div>
        
        <dialog-window v-model:show="dialogVisble">
                <div class="exitBtn">
                    <my-button @click="closeDialog">х</my-button>
                </div>
                <my-input v-model="datafacilityType.facility_type_name" @input="(datafacilityType.facility_type_name = $event.target.value)" type="text" placeholder="Тип НПО"></my-input>
                <div class="saveBtn">
                    <my-button class="currentButton" @click="addFiclityType">Сохранить</my-button>
                </div>
                
            
            
        </dialog-window>
    </div>
</template>

<script>
import axios from 'axios';
    export default {
        data(){
            return{
                facilityTypes:[],
                dialogVisble: false,
                datafacilityType:{
                    facility_type_name:'',
                },
            }
        },
        props:{
            showfacilityType:{
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
            addFiclityType(){
                
                if (this.datafacilityType.facility_type_name === ''){
                        alert('Заполните все поля')
                
                }else{
                    this.addFacilityTypeToDB();
                    this.closeDialog(); 
                    this.datafacilityType.facility_type_name = '';
                }
            },
            async getAllFacilityTypes(){
                try{
                    const response = await axios.get("http://localhost:8081/facility_type/all");
                    this.facilityTypes = response.data;
                    
                    
                }catch(e){
                    alert('Тут')
                }
                
            },
            async addFacilityTypeToDB(){
                try{
                    const response = await axios.post("http://localhost:8081/facility_type",this.datafacilityType);
                    alert('Тип НПО добавлен');
                }catch(e){
                    alert('Error');
                }
            }
        },
        mounted(){
            this.getAllFacilityTypes();
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
.facilityTypes{
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