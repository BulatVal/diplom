<template>
    <div class="facilities" v-if="showfacility" @click="hideDialog">
        <my-button class="currentButton" @click="showDialog" >Добаить запись</my-button>
        <div class="scroll">
            <table cellspacing="0" cellpadding="0">
                <tbody>
                    <tr>
                        <th>Наименование</th>
                        <th>Тип НПО</th> 
                    </tr>
                    <tr v-for="facility in facilities" :key="facility.facility_id" >
                        <td>{{ facility.facility_name}}</td>
                        <td>{{ facility.facility_type_id.facility_type_name}}</td>
                    </tr>     
                </tbody>
            </table>
        </div>
        
        <dialog-window v-model:show="dialogVisble">
                <div class="exitBtn">
                    <my-button @click="closeDialog">х</my-button>
                </div>
                
                <my-input v-model="datafacility.facility_name" @input="(datafacility.facility_name = $event.target.value)" type="text" placeholder="Наименование объекта"></my-input>
                <select v-model="selectedTypeText" @change="this.selectedType=$event.target.options.selectedIndex">
                    <option disabled value="">Выберите из списка</option>
                    <option v-for="types in typeFacility" :key="types.facility_type_id">
                    {{ types.facility_type_name }}
                    </option>
                </select>
                <div class="saveBtn">
                    <my-button class="currentButton" @click="addFiclity">Сохранить</my-button>
                </div>
        </dialog-window>
    </div>
</template>

<script>
import axios from 'axios';
    export default {
        data(){
            return{
                facilities:[],
                typeFacility:[],
                dialogVisble: false,
                selectedType:0,
                selectedTypeText:'',
                datafacility:{
                    facility_name:'',
                },
            }
        },
        props:{
            showfacility:{
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
            addFiclity(){
                
                if (this.datafacility.facility_name === '' || this.selectedType === 0){
                        alert('Заполните все поля')
                
                }else{
                    this.addFacilityToDB();
                    this.closeDialog();
                    this.datafacility.facility_name = '';
                    this.selectedType = 0;
                    this.selectedTypeText = '';
                }
            },
            async getAllFacility(){
                try{
                    const response = await axios.get("http://localhost:8081/facility/all");
                    this.facilities = response.data;
                    console.log(this.typeFacility)
                    
                }catch(e){
                    alert('Error')
                }
                
            },
            async getAllFacilityType(){
                try{
                    const response = await axios.get("http://localhost:8081/facility_type/all");
                    this.typeFacility = response.data;
                    console.log(this.facilities)
                    
                }catch(e){
                    alert('Error')
                }
                
            },
            async addFacilityToDB(){
                try{
                    const response = await axios.post("http://localhost:8081/facility",this.datafacility,{
                        params:{
                            facilityTypeID: this.selectedType
                        }
                    });
                    alert('Объект добавлен');
                    
                }catch(e){
                    alert('Error');
                }
            }
        },
        mounted(){
            this.getAllFacility();
            this.getAllFacilityType();
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
select{
    border: 1px solid black;
    width: auto;
    margin-bottom: 5px;
    height: 30px;
}
.facilities{
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